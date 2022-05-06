---
layout: post
title:  "bloodyAD and Kerberos"
date:   2022-05-05 14:16:51 +0200
categories: "Active Directory" "Privilege Escalation"
---
Most of the time I use NTLM authentication, but in some situations, we only have a kerberos TGT or ST and it would be a shame to not use it to attempt to elevate our privileges in the AD. So let's see how we can do this with bloodyAD.
