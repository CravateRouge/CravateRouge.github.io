---
layout: post
title:  "bloodyAD and Certificate Authentication"
author: CravateRouge
date:   2022-05-09 11:32:00 +0200
categories: ad privesc
tags: ad privesc bloodyad certificate authentication
---
A few days ago I read a [great article](https://offsec.almond.consulting/authenticating-with-certificates-when-pkinit-is-not-supported.html) from Yannick Méheut of Almond about certificate authentication in Active Directory environment. It is especially useful when PKINIT is not supported and thus you can't use your certificate to request a TGT.
This is why I wanted to extend the capabilities of [bloodyAD](https://github.com/CravateRouge/bloodyAD) by allowing certificate authentication.
Here is an example on how to use it (the first part show how to get a certificate if you just want to try the functionality):
{% highlight powershell %}
# Grab the cert

## Get the CA Authority name
## -debug is required in my env or it doesn't work
(venv) PS > certipy.exe find bloody/Administrator:passw0rd@192.168.10.2 -debug
Certipy v2.0.9 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[+] Authenticating to LDAP server
[+] Bound to ldaps://192.168.10.2:636 - ssl
[+] Default path: DC=bloody,DC=local
[+] Configuration path: CN=Configuration,DC=bloody,DC=local
[*] Found 33 certificate templates
[*] Finding certificate authorities
[+] Trying to resolve 'WIN-IJ5B521UO5L.bloody.local' at '192.168.10.2'
[*] Trying to get CA configuration for 'bloody-WIN-IJ5B521UO5L-CA' via CSRA
[+] Target system is 192.168.10.2 and isFQDN is False
[+] StringBinding: \\\\WIN-IJ5B521UO5L[\\pipe\\cert]
[+] StringBinding: WIN-IJ5B521UO5L[49702]
[*] Got CA configuration for 'bloody-WIN-IJ5B521UO5L-CA'
[+] Resolved 'WIN-IJ5B521UO5L.bloody.local' from cache: 192.168.10.2
[+] Connecting to 192.168.10.2:80
[*] Found 11 enabled certificate templates
[*] Saved text output to '20220506173005_Certipy.txt'
[*] Saved JSON output to '20220506173005_Certipy.json'
[*] Saved BloodHound data to '20220506173005_Certipy.zip'. Drag and drop the file into the BloodHound GUI

## Get the PFX
(venv) PS > certipy.exe req bloody/Administrator:passw0rd@192.168.10.2 -ca bloody-WIN-IJ5B521UO5L-CA -debug

[*] Requesting certificate
[+] Trying to connect to endpoint: ncacn_np:192.168.10.2[\pipe\cert]
[+] Connected to endpoint: ncacn_np:192.168.10.2[\pipe\cert]
[*] Successfully requested certificate
[*] Request ID is 4
[*] Got certificate with UPN 'Administrator@bloody.local'
[*] Saved certificate and private key to 'administrator.pfx'

## Convert it to pem
(venv) PS > openssl.exe pkcs12 -in administrator.pfx -out administrator.pem -nodes

# Use cert authentication
(venv) PS > python bloodyAD.py -c ":administrator.pem" -d bloody.local -u Administrator --host WIN-IJ5B521UO5L.bloody.local getObjectAttributes  'DC=bloody,DC=local' msDS-Behavior-Version
{
    "msDS-Behavior-Version": "DS_BEHAVIOR_WIN2016"
}
{% endhighlight %}