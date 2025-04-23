---
title: "Implémenter S/MIME avec Exchange Online et OWA"
summary: "Implémenter S/MIME avec Exchange Online et OWA pour des communications email sécurisées"
date: 2024-11-14

authors:
  - admin

tags:
  - Microsoft
  - Azure

categories:
  - M365
---

Aaah l'email, inventé à la fin du 20ème siècle et toujours la pierre angulaire de la communication d'aujourd'hui. Cependant, les besoins en sécurité augmentent et nous avons dû trouver de nouvelles façons de garantir la sécurité de nos communications email. Pourquoi ? Parce que pour échanger des emails, les serveurs email utilisent SMTP, un protocole non chiffré, donc on peut oublier la garantie de confidentialité et d'authenticité. Il est vrai que nous pouvons l'encapsuler dans TLS en utilisant SMTPS ou STARTTLS mais si vous utilisez **Exchange Online** et que le serveur de messagerie du destinataire ne prend pas en charge TLS, alors la communication sera [simplement en SMTP non chiffré](https://learn.microsoft.com/en-us/purview/exchange-online-uses-tls-to-secure-email-connections#tls-basics-for-microsoft-365-and-exchange-online).

C'est là que des standards comme [S/MIME](https://en.wikipedia.org/wiki/S/MIME) (Secure/Multipurpose Internet Mail Extensions) viennent à notre secours !  
En utilisant ce standard, vous pouvez signer votre email avec votre clé privée avant de l'envoyer au destinataire qui peut le vérifier avec votre clé publique (clé publique que vous échangez avec le destinataire par un autre moyen sécurisé). Vous pouvez également chiffrer un email avec la clé publique du destinataire pour garantir que seul ce destinataire peut l'ouvrir.

Et là vous allez me dire que toute cette petite histoire est très intéressante mais comment fait-on pour l'implémenter ? Vous pourriez implémenter S/MIME sur chacun de vos clients email s'ils le prennent en charge comme Outlook classic, mais alors vous devez ajouter la clé publique de vos contacts sur chaque client email, ce qui peut être un peu fastidieux mais fonctionne ! Il est également bon de noter que le Nouvel Outlook ne prend pas encore en charge S/MIME mais devrait le prendre en charge durant ce mois de [novembre 2024](https://www.microsoft.com/en-us/microsoft-365/roadmap?filters=Outlook%2CDesktop%2CWeb&searchterms=s%2Fmime).  
Cependant, si vous avez Exchange Online, vous pourriez configurer les clés publiques de tous les contacts une seule fois pour tout le monde sur votre serveur ! Mais ce n'est pas magique, chaque utilisateur devra toujours installer sa propre clé privée localement sur son appareil.

Dans cet article, nous verrons comment l'implémenter sur Exchange Online avec Outlook sur le Web (OWA).

## Installer la clé privée sur votre machine locale
### Installer le contrôle S/MIME pour OWA
Tout d'abord, vous devrez installer le contrôle S/MIME sur votre navigateur (à ce jour, il n'est disponible que sur Edge et Chrome) afin qu'OWA puisse appeler l'API Windows de votre machine locale pour chiffrer/signer les emails en utilisant la clé privée installée dessus.
1. Ouvrez `outlook.office.com` et allez dans les paramètres en cliquant sur l'engrenage en haut à droite
![gear](smime/smime_control1.png)
2. Cliquez sur `Mail > SMIME` et sur le lien `cliquez ici` pour installer l'extension du navigateur
![click link](smime/smime_control2.png)
3. Une fois l'extension du navigateur installée, retournez à la page des paramètres et cliquez sur le lien `cliquez ici` pour télécharger et installer l'extension de contrôle `SmimeOutlookWebChrome.msi`.
4. Une fois cela fait, vous pouvez redémarrer votre navigateur et vous assurer que ces cases ne sont plus grisées.

### Installer votre certificat S/MIME
Si vous n'avez pas encore de certificat, vous devrez en demander un. Vous pouvez en demander un à une CA publique (Autorité de Certification) mais à part [Actalis](https://extrassl.actalis.it/portal/uapub/freemail?lang=en), je ne connais pas d'autres CA publiques gratuites pour les certificats S/MIME. Vous pouvez également en demander un à votre propre CA privée (par exemple en utilisant openssl) mais vous devrez alors importer le certificat public de la CA privée dans `crtmngr`.

Vous devrez ensuite installer votre certificat sur la machine de votre navigateur :
1. Double-cliquez sur votre certificat `.p12`, suivez l'assistant et fournissez le mot de passe du certificat :
![wizard store](smime/cert_install1.png)
![wizard location](smime/cert_install2.png)
![wizard pwd](smime/cert_install3.png)
![wizard auto](smime/cert_install4.png)
![wizard finish](smime/cert_install5.png)
2. Une fois installé, ouvrez l'utilitaire `Gérer les certificats utilisateur` depuis la barre de recherche Windows :
![certmgr](smime/cert_install6.png)
3. Allez dans le dossier où votre certificat est installé. Cela devrait être `Personnel > Certificats` :
![cert store](smime/cert_install7.png)
4. Faites un clic droit sur votre certificat et sélectionnez `Toutes les tâches > Exporter` :
![export cert](smime/cert_install8.png)
5. Suivez l'assistant pour enregistrer votre certificat au format `.der`, vous en aurez besoin pour télécharger la clé publique sur Exchange Online plus tard.
![export wizard](smime/cert_install9.png)
![export wizard no key](smime/cert_install10.png)
![export wizard der](smime/cert_install11.png)
![export wizard location](smime/cert_install12.png)
![export wizard finish](smime/cert_install13.png)

## Télécharger la chaîne de confiance sur Exchange Online
Contrairement à Windows, Exchange Online n'a pas d'autorités de certification préinstallées. Vous devrez y ajouter toutes les autorités de certification qui peuvent vérifier vos certificats d'utilisateur final et de contacts.
1. Ouvrez `usercrtmngr` à nouveau et double-cliquez sur le certificat de l'utilisateur final
2. Dans l'onglet `Chemin de certification`, vous verrez tous les certificats vérifiant le certificat de l'utilisateur final, c'est ce qu'on appelle la chaîne de confiance
3. Assurez-vous que tous ces certificats sont dans le même dossier ou copiez ceux qui sont en dehors dans le dossier actuel pour pouvoir les concaténer plus tard
4. Répétez les étapes ci-dessus pour tous vos utilisateurs finaux et contacts
5. Utilisez `ctrl+clic gauche` pour sélectionner chaque certificat de la chaîne de confiance (s'il n'y a qu'un seul certificat dans la chaîne de confiance, incluez un autre certificat avec pour pouvoir exporter la liste au format `.sst`)
6. Faites un clic droit sur l'un des certificats sélectionnés et sélectionnez `Toutes les tâches > Exporter`, suivez l'assistant et sélectionnez le format `.sst`
7. En utilisant Powershell 7 (en dessous de 7, vous aurez l'erreur `unable to find type [uint]`) :
```ps1
Install-Module ExchangeOnlineManagement
Import-Module ExchangeOnlineManagement
Connect-ExchangeOnline -UserPrincipalName <exchangeadmin_for_yourdomain>
Set-SmimeConfig -SMIMECertificateIssuingCA ([System.IO.File]::ReadAllBytes('C:\Users\Gold\Downloads\chainOfTrust.sst'))
```
8. Attendez un peu de temps pour que la modification soit propagée

Super ! Nous avons maintenant tout ce qu'il faut pour signer et déchiffrer nos emails dans OWA, qu'en est-il du chiffrement ou de la vérification de la signature des emails ?

## Installer la clé publique sur Exchange Online
Cette partie n'est pas bien documentée, en particulier le cas d'utilisation pour ajouter un contact en dehors de notre Exchange Online où je n'ai trouvé aucune documentation à ce sujet, mais cela fonctionne ! Alors commençons.

1. Rassemblez les clés publiques de vos contacts au format DERv3 (comme mentionné à l'étape 5 de [Installer votre certificat S/MIME](#installer-votre-certificat-smime))
2. Utilisez le code powershell suivant pour concaténer chaque DERv3 en `.sst` :
```ps1
$certArray = New-Object System.Collections.ArrayList
$cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("D:\Gold\Documents\VM-apps\baptiste@cravaterouge.com.cer") <- DERv3 format
$certArray.Insert(0, $cert.GetRawCertData())
```
3. Si le contact appartient au même Exchange Online, utilisez la commande powershell suivante :
```ps1
Set-Mailbox -Identity baptiste -UserCertificate $certArray
```
4. Si le contact est externe à cet Exchange Online, utilisez la commande suivante :
```ps1
# Utilisez cette commande si le contact n'est pas déjà créé
New-MailContact -Name CravateRouge_SMIME -ExternalEmailAddress baptiste@cravaterouge.com
Set-MailContact -Identity CravateRouge_SMIME -UserCertificate $certArray
```
5. Assurez-vous que le certificat a été correctement mis à jour en vérifiant que `Get-Mailbox/Get-MailContact -Identity CravateRouge | select UserCertificate` renvoie le même binaire que `$cert.GetRawCertData()`
6. Pour envoyer un email à ces contacts, vous devez les sélectionner dans les contacts GAL avec le nom d'identité exact que vous avez donné dans la commande powershell ci-dessus. Par exemple, pour envoyer un email à __Baptiste__, je dois utiliser le contact nommé __CravateRouge_SMIME__ et non en tapant directement __baptiste@cravaterouge.com__ ou en utilisant une autre carte de contact même si elle contient la même adresse email.

## Résultat
- Vous pouvez maintenant signer chacun de vos emails pour prouver aux destinataires que vous êtes l'expéditeur original et déchiffrer les emails qui vous sont envoyés
![sign everything](smime/sign_everything.png)
- Et vous pouvez également chiffrer/vérifier les emails des contacts que vous avez ajoutés à Exchange Online
![encrypt selected](smime/encrypt_whitelisted.png)

{{% callout note %}}
Même si tout fonctionne bien, vous pouvez toujours avoir un message d'erreur lors de l'envoi de messages chiffrés à des clients externes. Ne vous inquiétez pas, c'est juste un bug de Microsoft car cette fonctionnalité n'est pas encore bien prise en charge.
![encryption error](smime/error_encrypted.png)
{{% /callout %}}