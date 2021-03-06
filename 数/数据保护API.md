{{NoteTA
|G1=IT
}}
'''数据保护API'''（全称：{{lang|en|'''Data Protection Application Programming Interface'''}}，缩写'''DPAPI'''）是一个简单的[[密码学|密码学]][[应用程序接口|应用程序接口]] ，作为一个组件内置在[[Windows_2000|Windows 2000]]及之后版本的[[Microsoft_Windows|Microsoft Windows]][[操作系统|操作系统]]中。理论上，数据保护API可以实现任何类型的数据对称加密；在实践中，其在Windows操作系统中的主要用途是执行[[非对称加密|非对称]]私钥的[[对称加密|对称加密]]，使用用户或系统的秘密信息作为[[熵|熵]]的重要来源。

对于几乎所有密码系统来说，最困难的挑战之一是“密钥管理”——其中部分即是，如何安全地存储解密密钥。如果密钥以纯文本存储，则可以访问密钥的任何用户都可以访问加密的数据。如果密钥被加密，则又需要另一个密钥，周而复始。DPAPI允许开发者使用从用户的登录私钥导出的对称密钥来加密密钥，或者在系统加密的情况下使用系统的域验证私钥来加密密钥。

用于加密用户RSA密钥的DPAPI密钥存储在<code>%APPDATA%\Microsoft\Protect\{SID}</code>目录，其中{SID}为该用户的[[安全标识符|安全标识符]]。DPAPI密钥存储在与保护用户私钥的主密钥相同的文件中。它通常为64字节的随机数据。

2010年，{{tsl|en|Elie Bursztein}}和{{tsl|en|Jean-Michel Picod}}在Black Hat DC 2010提出了Reversing DPAPI and Stealing Windows Secrets Offline。<ref>https://www.blackhat.com/html/bh-dc-10/bh-dc-10-briefings.html</ref>除了他们的简报，Bursztein和Picod发布了能离线解密DPAPI加密数据的DPAPIck。2012年，Passcape Software在他们的博客中发布了介绍DPAPI内部逻辑的更详细文章<ref>http://passcape.com/index.php?section=blog&cmd=details&id=20</ref>及一个完全离线解密和分析DPAPI的工具<ref>http://passcape.com/windows_password_recovery_dpapi_decoder</ref>。与上一个不同，该工具利用了一些旧有Windows的缺陷（例如，你可以无需获知所有者的登录密码就解密Windows 2000的DPAPI数据体）并完全兼容Windows 8的DPAPI数据结构。在Windows 8中，微软改变了DPAPI逻辑的工作方式，现在可以使用多个用户的密钥来导出加密密钥以解密用户的主密钥，然后用于解码单个DPAPI数据体（blob）。

== 安全属性 ==
DPAPI不为自己存储任何持久性数据。相反，它只接受明文并返回密文（反之亦然）。

DPAPI安全性依赖于Windows操作系统保护主密钥和[[RSA加密演算法|RSA]]私钥免受攻击的能力。这在大多数攻击情形中高度依赖最终用户凭据的安全性。主加密/解密密钥通过{{tsl|en|PBKDF2}}函数从用户密码导出。<ref>{{Cite web|url=http://www.passcape.com/windows_password_recovery_dpapi_master_key|title=Windows Password Recovery - DPAPI Master Key analysis|accessdate=2013-05-06}}</ref>特定数据的[[二進位大型物件|二進位大型物件]]可以添加[[盐_(密码学)|盐]]和/或询问外部用户提供额外密码（也称强密码保护）来加密。盐的使用由各实现的选项控制，即由应用程序开发者控制，不能由最终用户或系统管理员控制。

通过使用[[组件对象模型|COM+]]对象可以授予密钥的委托访问权限。这使[[網際網路資訊服務|IIS]][[網頁伺服器|網頁伺服器]]能够使用DPAPI。

== 微软软件对DPAPI的使用 ==
虽然并非所有微软产品都在使用，但微软产品对DPAPI的使用随着每个Windows版本在增加。不过，出自微软或第三方开发人员的许多应用程序仍倾向于使用自己的保护方式，或者最近才切换为使用DPAPI。例如[[Internet_Explorer|Internet Explorer]]的4.0-6.0版本、[[Outlook_Express|Outlook Express]]和{{tsl|en|MSN Explorer}}使用较旧的保护存储（PStore）API来存储保存的凭据（例如密码）。[[Internet_Explorer_7|Internet Explorer 7]]则开始使用DPAPI保护已存储的用户凭据。<ref>{{Cite web|url=http://www.symantec.com/connect/articles/password-management-concerns-ie-and-firefox-part-one|title=Password Management Concerns with IE and Firefox, part one|accessdate=2010-03-28|author=Mikhael Felker|date=December 8, 2006|publisher={{tsl|en|SecurityFocus.com}}, [[Symantec.com|Symantec.com]]}}</ref>
* [[Windows_8|Windows 8]]中的图片密码、PIN和指纹
* Windows 2000及之后版本中的[[加密文件系统|加密文件系统]]
* SQL Server {{tsl|en|Transparent Data Encryption|透明数据加密}}（TDE）服务的主加密密钥<ref>https://msdn.microsoft.com/en-us/library/ms189586(v=sql.110).aspx</ref>
* [[Internet_Explorer_7|Internet Explorer 7]]，无论是适用于[[Windows_XP|Windows XP]]的独立版本，还是[[Windows_Vista|Windows Vista]]和[[Windows_Server_2008|Windows Server 2008]]中集成的版本
* [[邮件_(Windows)|Windows Mail]]和[[Windows_Live_Mail|Windows Live Mail]]
* Outlook的[[S/MIME|S/MIME]]
* [[網際網路資訊服務|Internet Information Services]]的[[傳輸層安全協議|SSL/TLS]]
* Windows {{tsl|en|Rights Management Services|权利管理服务}}客户端v1.1及之后版本
* [[Windows_2000|Windows 2000]]及之后版本对[[扩展认证协议|EAP/TLS]]（[[虛擬私人網路|VPN]]身份验证）和802.1x（[[Wi-Fi|Wi-Fi]]身份验证）
* Windows XP及之后版本对存储的用户名和密码）<ref>http://technet.microsoft.com/en-us/library/bb457059.aspx</ref>（也称凭据管理器）
* [[.NET框架|.NET Framework]] 2.0及之后版本对System.Security.Cryptography.ProtectedData<ref>http://msdn2.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx</ref>
* Microsoft.Owin（Katana） Cookie身份验证（当自托管时）<ref>{{Cite web|url=http://msdn.microsoft.com/en-us/library/microsoft.owin.security.cookies.cookieauthenticationoptions.ticketdataformat(v=vs.113).aspx|title=CookieAuthenticationOptions.TicketDataFormat Property  (Microsoft.Owin.Security.Cookies)|accessdate=2015-01-15}}</ref>

== 参考资料 ==
{{reflist|2}}

== 外部链接 ==
* [http://go.microsoft.com/fwlink/?LinkId=89993 Windows Data Protection API (DPAPI) white paper by NAI Labs]
* [http://www.codeproject.com/KB/system/protected_data.aspx Data encryption with DPAPI]
* [http://www.obviex.com/samples/dpapi.aspx Use DPAPI to encrypt and decrypt data]
* [http://msdn.microsoft.com/library/aa302404.aspx How To: Use DPAPI (User Store) from ASP.NET 1.1 with Enterprise Services]
* [http://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx System.Security.Cryptography.ProtectedData in .NET Framework 2.0 and later]
* [http://msdn.microsoft.com/library/cc201324.aspx Discussion of the use of MS BackupKey Remote Protocol by DPAPI to protect user secrets]
* [http://msdn.microsoft.com/library/bb432403.aspx The Windows PStore]
* [http://www.nirsoft.net/utils/dpapi_data_decryptor.html DataProtectionDecryptor]，NirSoft出品的免费软件

{{Microsoft APIs}}

[[Category:加密软件|Category:加密软件]]
[[Category:微軟API|Category:微軟API]]