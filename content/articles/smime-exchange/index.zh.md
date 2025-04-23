---
title: "Implementing S/MIME with Exchange Online and OWA"
summary: "Implementing S/MIME with Exchange Online and OWA for secure email communications"
date: 2024-11-14

authors:
  - admin

tags:
  - Microsoft
  - Azure

categories:
  - M365
---

啊，电子邮件，发明于20世纪末，至今仍是沟通的基石。然而，随着安全需求的增加，我们必须找到新的方法来确保电子邮件通信的安全。为什么？因为电子邮件服务器使用SMTP协议来交换邮件，这是一个未加密的协议，因此我们无法保证机密性和真实性。虽然我们可以通过SMTPS或STARTTLS将其包装在TLS中，但如果您使用的是**Exchange Online**，而收件方的邮件服务器不支持TLS，那么通信将是[简单的未加密SMTP](https://learn.microsoft.com/en-us/purview/exchange-online-uses-tls-to-secure-email-connections#tls-basics-for-microsoft-365-and-exchange-online)。

这就是像[S/MIME](https://en.wikipedia.org/wiki/S/MIME)（安全/多用途互联网邮件扩展）这样的标准来拯救我们的地方！使用这个标准，您可以在发送邮件之前使用您的私钥对邮件进行签名，收件人可以通过您的公钥验证签名（公钥需要通过其他安全方式与收件人交换）。您还可以使用收件人的公钥加密邮件，以确保只有该收件人可以打开它。

然后您可能会说，这个小故事很有趣，但我该如何实现呢？您可以在每个支持它的电子邮件客户端上实现S/MIME，比如旧版Outlook，但这样您需要在每个电子邮件客户端上添加联系人公钥，这可能有点繁琐，但确实可行！另外需要注意的是，新版Outlook尚不支持S/MIME，但预计将在[2024年11月](https://www.microsoft.com/en-us/microsoft-365/roadmap?filters=Outlook%2CDesktop%2CWeb&searchterms=s%2Fmime)内支持它。然而，如果您使用的是Exchange Online，您可以为所有联系人一次性在服务器上设置公钥！但这并不是魔法，每个用户仍然需要在其设备上本地安装自己的私钥。

在本文中，我们将看到如何在Exchange Online和Outlook on the Web（OWA）上实现它。

## 在本地机器上安装私钥
### 为OWA安装S/MIME控件
首先，您需要在浏览器上安装S/MIME控件（截至目前，它仅支持Edge和Chrome），以便OWA可以调用本地机器的Windows API来使用安装在其上的私钥加密/签名邮件。
1. 打开`outlook.office.com`，点击右上角的齿轮图标进入设置
![gear](smime/smime_control1.png)
2. 点击`Mail > SMIME`，然后点击链接`click here`安装浏览器扩展
![click link](smime/smime_control2.png)
3. 安装浏览器扩展后，返回设置页面，点击链接`click here`下载并安装控件扩展`SmimeOutlookWebChrome.msi`。
4. 完成后，重启浏览器，确保这些选项框不再灰显。

### 安装您的S/MIME证书
如果您还没有证书，您需要申请一个。您可以向公共CA（证书颁发机构）申请，但除了[Actalis](https://extrassl.actalis.it/portal/uapub/freemail?lang=en)，我不知道其他免费的公共CA提供S/MIME证书。您也可以向自己的私有CA申请（例如使用openssl），但随后您需要将私有CA的公证书导入`crtmngr`。

然后，您需要在浏览器的机器上安装您的证书：
1. 双击您的`.p12`证书，按照向导操作并提供证书密码：
![wizard store](smime/cert_install1.png)
![wizard location](smime/cert_install2.png)
![wizard pwd](smime/cert_install3.png)
![wizard auto](smime/cert_install4.png)
![wizard finish](smime/cert_install5.png)
2. 安装完成后，从Windows搜索栏打开`Manage User Certificate`工具：
![certmgr](smime/cert_install6.png)
3. 转到证书安装的文件夹，应该是`Personal > Certificates`：
![cert store](smime/cert_install7.png)
4. 右键点击您的证书，选择`All Tasks > Export`：
![export cert](smime/cert_install8.png)
5. 按照向导操作，将证书保存为`.der`格式，稍后需要将公钥上传到Exchange Online。
![export wizard](smime/cert_install9.png)
![export wizard no key](smime/cert_install10.png)
![export wizard der](smime/cert_install11.png)
![export wizard location](smime/cert_install12.png)
![export wizard finish](smime/cert_install13.png)

## 将信任链上传到Exchange Online
与Windows不同，Exchange Online没有预装的证书颁发机构。您需要将每个可以验证最终用户和联系人证书的证书颁发机构推送到Exchange Online。
1. 再次打开`usercrtmngr`，双击最终用户证书
2. 在`Certification Path`选项卡中，您将看到验证最终用户证书的所有证书，这称为信任链
3. 确保所有这些证书在同一个文件夹中，或者将不在当前文件夹中的证书复制到当前文件夹，以便稍后打包
4. 对所有最终用户和联系人重复上述步骤
5. 使用`ctrl+左键点击`选择信任链中的每个证书（如果信任链中只有一个证书，请再包含另一个证书，以便能够将列表导出为`.sst`）
6. 右键点击选中的一个证书，选择`All Tasks > Export`，按照向导操作并选择`.sst`格式
7. 使用Powershell 7（低于7会出现错误`unable to find type [uint]`）：
```ps1
Install-Module ExchangeOnlineManagement
Import-Module ExchangeOnlineManagement
Connect-ExchangeOnline -UserPrincipalName <exchangeadmin_for_yourdomain>
Set-SmimeConfig -SMIMECertificateIssuingCA ([System.IO.File]::ReadAllBytes('C:\Users\Gold\Downloads\chainOfTrust.sst'))
```
8. 等待一段时间以便修改传播

太好了！我们现在已经可以在OWA中签名和解密邮件了，那么如何加密或验证邮件签名呢？

## 在Exchange Online上安装公钥
这一部分文档不多，尤其是为Exchange Online外部联系人添加公钥的用例，我没有找到相关文档，但它确实可行！那么让我们开始吧。

1. 收集您的联系人公钥，格式为DERv3（如[安装您的S/MIME证书](#install-your-smime-certificate)第5步所述）
2. 使用以下Powershell代码将每个DERv3打包为`.sst`：
```ps1
$certArray = New-Object System.Collections.ArrayList
$cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("D:\Gold\Documents\VM-apps\baptiste@cravaterouge.com.cer") <- DERv3格式
$certArray.Insert(0, $cert.GetRawCertData())
```
3. 如果联系人属于同一个Exchange Online，使用以下Powershell命令：
```ps1
Set-Mailbox -Identity baptiste -UserCertificate $certArray
```
4. 如果联系人是此Exchange Online外部的，使用以下命令：
```ps1
# 如果联系人尚未创建，请使用此命令
New-MailContact -Name CravateRouge_SMIME -ExternalEmailAddress baptiste@cravaterouge.com
Set-MailContact -Identity CravateRouge_SMIME -UserCertificate $certArray
```
5. 确保证书已正确更新，通过验证`Get-Mailbox/Get-MailContact -Identity CravateRouge | select UserCertificate`返回的二进制数据与`$cert.GetRawCertData()`相同
6. 要向这些联系人发送邮件，您必须从GAL联系人中选择它们，并使用您在上述Powershell命令中指定的确切标识名称。例如，要向__Baptiste__发送邮件，我必须使用名为__CravateRouge_SMIME__的联系人，而不是直接输入__baptiste@cravaterouge.com__或使用其他联系人卡片，即使它具有相同的电子邮件地址。

## 结果
- 您现在可以签署每封邮件，以向收件人证明您是原始发送者，并解密发送给您的邮件
![sign everything](smime/sign_everything.png)
- 您还可以加密/验证您添加到Exchange Online的联系人的邮件
![encrypt selected](smime/encrypt_whitelisted.png)

{{% callout note %}}
即使一切正常，您在向外部客户端发送加密邮件时可能仍会收到错误消息。别担心，这只是微软的一个bug，因为此功能尚未完全支持。
![encryption error](smime/error_encrypted.png)
{{% /callout %}}