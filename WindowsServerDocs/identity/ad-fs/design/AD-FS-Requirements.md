---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: 使用 SQL Server 的联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c6aa91956f4a90b32b82e6c970e68b3164c732f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191706"
---
# <a name="ad-fs-requirements"></a>AD FS 要求

部署 AD FS 时必须遵守的各种要求如下：  
  
-   [证书要求](AD-FS-Requirements.md#BKMK_1)  
  
-   [硬件要求](AD-FS-Requirements.md#BKMK_2)  
  
-   [软件要求](AD-FS-Requirements.md#BKMK_3)  
  
-   [AD DS 要求](AD-FS-Requirements.md#BKMK_4)  
  
-   [配置数据库要求](AD-FS-Requirements.md#BKMK_5)  
  
-   [浏览器要求](AD-FS-Requirements.md#BKMK_6)  
  
-   [Extranet 要求](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [网络要求](AD-FS-Requirements.md#BKMK_7)  
  
-   [属性存储要求](AD-FS-Requirements.md#BKMK_8)  
  
-   [应用程序的要求](AD-FS-Requirements.md#BKMK_9)  
  
-   [身份验证要求](AD-FS-Requirements.md#BKMK_10)  
  
-   [Workplace join 要求](AD-FS-Requirements.md#BKMK_11)  
  
-   [加密要求](AD-FS-Requirements.md#BKMK_12)  
  
-   [权限要求](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>证书要求  
在保护服务器、 Web 应用程序代理、 声明的联合身份验证之间的通信中最重要的作用，证书着\-识别应用程序和 Web 客户端。 有关证书要求可能有所不同，具体取决于是否您设置了联合身份验证服务器或代理的计算机，如本部分中所述。  
  
**联合身份验证服务器证书**  
  
|||  
|-|-|  
|**证书类型**|**要求、 支持和须知**|  
|**安全套接字层\(SSL\)证书：** 这是用于保护联合服务器和客户端之间的通信的标准 SSL 证书。|-此证书必须是公开受信任\*X509 v3 证书。<br />-所有访问任何 AD FS 终结点的客户端必须信任此证书。 强烈建议使用由公共颁发的证书\(第三个\-方\)证书颁发机构\(CA\)。 可以使用自\-签名 SSL 证书已成功在联合身份验证服务器，在测试实验室环境中的。 但是，对于生产环境中，我们建议从公共 CA 获取证书。<br />-支持的 SSL 证书支持 Windows Server 2012 R2 的任何密钥大小。<br />-执行不支持使用 CNG 密钥的证书。<br />-与工作区加入一起使用时\/设备注册服务使用者可选名称的 SSL 证书的 AD FS 服务必须包含值 enterpriseregistration，后跟用户主体名称\(UPN\)你的组织后缀，例如 enterpriseregistration.contoso.com。<br />的支持通配符证书。 在创建 AD FS 场时，系统会提示提供 AD FS 服务的服务名称\(例如， **adfs.contoso.com**。<br />-强烈建议为 Web 应用程序代理使用相同的 SSL 证书。 但是这是**必需**支持 Windows 集成身份验证终结点通过 Web 应用程序代理和启用扩展保护身份验证时为相同\(默认设置\).<br />的此证书使用者名称用于表示每个实例的部署的 AD FS 的联合身份验证服务名称。 出于此原因，您可能要考虑选择任何新的 CA 上的使用者名称\-颁发的证书最符合需求的公司或伙伴组织名称。<br />    将证书的身份必须与联合身份验证服务名称匹配\(例如 fs.contoso.com\)。标识是类型为 dNSName 任一的使用者可选名称扩展，或者，如果没有使用者可选名称项，使用者名称指定为公用名。 提供的其中一个与联合身份验证服务名称匹配，可以在证书中，出现多个使用者可选名称条目。<br />-   **重要说明：** 强烈建议在 AD FS 场的所有节点，以及 AD FS 场中的所有 Web 应用程序代理中使用相同的 SSL 证书。|  
|**服务通信证书：** 此证书用于保护联合服务器之间的通信安全以确保 WCF 消息的安全性。|-默认情况下，SSL 证书用作服务通信证书。  但您还可以选择以另一个证书配置为服务通信证书。<br />-   **重要说明：** 如果 SSL 证书过期时，要用作服务通信证书，SSL 证书，请务必将续订的 SSL 证书配置为服务通信证书。 这不会自动发生。<br />-此证书必须受 AD FS 使用的 WCF 消息安全的客户端。<br />-我们建议你使用的服务器身份验证证书的颁发由公共\(第三个\-方\)证书颁发机构\(CA\)。<br />-服务通信证书不能使用 CNG 密钥的证书。<br />-可以使用 AD FS 管理控制台管理此证书。|  
|**令牌\-签名证书：** 这是一种用于安全地对联合服务器颁发的所有令牌进行签名的标准 X509 证书。|-默认情况下，AD FS 创建自\-使用 2048 位密钥签名证书。<br />-颁发的 CA 证书也受到支持，可以使用 AD FS 管理管理单元更改\-中<br />-CA 颁发的证书必须存储和访问通过 CSP 加密提供程序。<br />-令牌签名证书不能使用 CNG 密钥的证书。<br />-AD FS 不需要外部已注册的证书进行令牌签名。<br />    AD FS 会自动续订这些自助\-签名证书在过期之前，首先配置新证书作为辅助证书，使合作伙伴可以使用它们，则为主要副本的过程中翻转调用自动证书滚动更新。我们建议使用默认情况下，自动生成的令牌签名证书。<br />    如果你的组织的需要不同配置的证书进行令牌签名的策略，则可以在安装时使用 Powershell 指定证书\(使用安装的 – SigningCertificateThumbprint 参数\-AdfsFarm cmdlet\)。  安装完成后，你可以查看和管理令牌签名证书使用 AD FS 管理控制台或 Powershell cmdlet 集\-AdfsCertificate 和 Get\-AdfsCertificate。<br />    当使用外部已注册的证书进行令牌签名时，AD FS 不执行自动证书续订或滚动更新。  此过程必须由管理员执行。<br />    若要允许证书滚动更新一个证书即将过期时，可以在 AD FS 中配置辅助令牌签名证书。 默认情况下，所有的令牌签名证书在发布中联合身份验证元数据，但只有主令牌\-签名证书由 AD FS 来进行实际签名令牌。|  
|**令牌\-解密\/加密证书：** 这是标准 X509 证书用于解密\/加密任何传入令牌。 该证书也在联合元数据中发布。|-默认情况下，AD FS 创建自\-使用 2048 位密钥签名证书。<br />-颁发的 CA 证书也受到支持，可以使用 AD FS 管理管理单元更改\-中<br />-CA 颁发的证书必须存储和访问通过 CSP 加密提供程序。<br />-令牌\-解密\/加密证书不能使用 CNG 密钥的证书。<br />-默认情况下，AD FS 生成，并使用其自己，在内部生成的和自\-签名的令牌解密证书。  AD FS 不需要实现此目的的外部已注册的证书。<br />    此外，AD FS 会自动续订这些自助\-签名证书在过期之前。<br />    **我们建议使用默认情况下，自动生成的令牌解密证书。**<br />    如果你的组织的需要进行配置的不同证书进行令牌解密的策略，则可以在安装时使用 Powershell 指定证书\(使用 – DecryptionCertificateThumbprint 参数安装\-AdfsFarm cmdlet\)。  安装完成后，你可以查看和管理令牌解密证书使用 AD FS 管理控制台或 Powershell cmdlet 集\-AdfsCertificate 和 Get\-AdfsCertificate。<br />    **当使用外部已注册的证书进行令牌解密时，AD FS 不会执行自动证书续订。此过程必须由管理员执行**。<br />-将 AD FS 服务帐户必须有权访问令牌\-签名证书的私钥在本地计算机的个人存储中。 这是由负责安装程序。 此外可以使用 AD FS 管理管理单元\-以确保此访问权限，如果随后更改令牌\-签名证书。|  
  
> [!CAUTION]  
> 用于令牌的证书\-签名和令牌\-解密\/加密对联合身份验证服务的稳定性至关重要。 管理他们自己的令牌的客户\-签名和令牌\-解密\/加密证书应确保这些证书进行备份，并且可独立恢复事件期间。  
  
> [!NOTE]  
> 在 AD FS 中，您可以更改安全哈希算法\(SHA\)用于任一 sha 的数字签名的级别\-1 或 SHA\-256\(更安全\)。 AD FS 不支持与其他哈希方法，如 MD5 证书的使用\(与 Makecert.exe 命令一起使用的默认哈希算法\-行工具\)。 作为安全性最佳实践，我们建议使用 SHA\-256\(这默认情况下设置\)所有签名。 SHA\-我们建议仅在其中你必须与进行互操作不支持使用 SHA 通信的产品的方案中使用\-256，如非\-Microsoft 产品或旧版本的 AD FS。  
  
> [!NOTE]  
> 从 CA 收到证书后，请确保将所有的证书导入到本地计算机的个人证书存储中。 可以证书导入到具有在证书 MMC 管理单元的个人存储区\-中。  
  
## <a name="BKMK_2"></a>硬件要求  
以下的最低和推荐硬件要求适用于 Windows Server 2012 R2 中的 AD FS 联合身份验证服务器：  
  
||||  
|-|-|-|  
|**硬件要求**|**最低要求**|**建议的要求**|  
|CPU 速度|1.4 GHz 64\-位处理器|四核\-核 2 GHz|  
|RAM|512 MB|4 GB|  
|磁盘空间|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>软件要求  
以下的 AD FS 要求适用于 Windows Server® 2012 R2 操作系统中内置的服务器功能：  
  
-   必须将 Web 应用程序代理角色服务部署为 extranet 访问\-Windows Server® 2012 R2 远程访问服务器角色的一部分。 Windows Server® 2012 R2 中 AD FS 不支持联合身份验证服务器代理的以前版本。  
  
-   不能在同一台计算机上安装联合身份验证服务器和 Web 应用程序代理角色服务。  
  
## <a name="BKMK_4"></a>AD DS 要求  
**域控制器要求**  
  
将所有用户域和 AD FS 服务器所加入的域中的域控制器必须运行 Windows Server 2008 或更高版本。  
  
> [!NOTE]  
> 使用 Windows Server 2003 域控制器的环境的所有支持将在扩展支持都结束日期用于 Windows Server 2003 的后都结束。 强烈建议客户尽快升级其域控制器。 请访问[本页](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)有关 Microsoft 支持生命周期的其他信息。 对于发现的问题，是特定于 Windows Server 2003 的域控制器环境，修补程序将会仅为颁发的安全问题，如果可以在扩展支持适用于 Windows Server 2003 的到期时间之前发出修补程序。  
  
**域功能\-要求**  
  
所有用户帐户域和 AD FS 服务器所加入的域必须运行在域功能级别的 Windows Server 2003 或更高版本。  
  
大多数 AD FS 功能不需要 AD DS 功能\-级别修改，以便成功运行。 但是，如果证书明确映射到 AD DS 中的用户帐户，则 Windows Server 2008 域功能级别或更高版本是要成功运行客户端证书身份验证所必需的。  
  
**架构要求**  
  
-   AD FS 不需要架构更改或功能\-级别对 AD DS 的修改。  
  
-   若要使用工作区加入的功能，AD FS 服务器将加入到林的架构必须设置为 Windows Server 2012 R2。  
  
**服务帐户要求**  
  
-   任何标准的服务帐户可以用作 AD FS 服务帐户。 此外支持组托管服务帐户。 这要求至少一个域控制器\(建议部署两个或多个\)的正在运行 Windows Server 2012 或更高版本。  
  
-   为 Kerberos 身份验证的域之间正常\-加入客户端和 AD FS 中，主机\/< adfs\_服务\_名称 > 必须注册为服务帐户的 SPN。 默认情况下，AD FS 将配置此创建新的 AD FS 场，如果它具有足够权限来执行此操作时。  
  
-   必须包含对 AD FS 服务的用户进行身份验证的每个用户域中受信任的 AD FS 服务帐户。  
  
**域要求**  
  
-   所有 AD FS 服务器都必须加入到 AD DS 域。  
  
-   必须在单个域中部署所有 AD FS 服务器场中。  
  
-   AD FS 服务器将加入到域必须信任包含用户到 AD FS 服务进行身份验证的每个用户帐户域。  
  
**多林要求**  
  
-   AD FS 服务器将加入到域必须信任每个用户帐户域或林中包含对 AD FS 服务进行身份验证的用户。  
  
-   必须包含对 AD FS 服务的用户进行身份验证的每个用户域中受信任的 AD FS 服务帐户。  
  
## <a name="BKMK_5"></a>配置数据库要求  
应用限制的基于配置存储区的类型和要求如下：  
  
**WID**  
  
-   如果有 100 个或更少的信赖方信任，则 WID 场有 30 个联合身份验证服务器的限制。  
  
-   WID 配置数据库中不支持 SAML 2.0 中的项目解析配置文件。  WID 配置数据库中不支持令牌重放检测。 仅在 AD FS 作为联合身份验证提供程序并且使用的安全令牌的外部声明提供程序的情况下仅使用此功能。  
  
-   部署 AD FS 服务器在不同的数据中心的故障转移或地理负载平衡支持，只要服务器数不超过 30。  
  
下表提供有关使用 WID 场的摘要。  使用它来计划你的实现。  
  
||||  
|-|-|-|  
||1 \- 100 RP 信任|超过 100 个 RP 信任|  
|1 \- 30 AD FS 节点|WID 支持|不支持使用 WID\-所需的 SQL|  
|30 个以上 AD FS 节点|不支持使用 WID\-所需的 SQL|不支持使用 WID\-所需的 SQL|  
  
**SQL Server**  
  
对于 Windows Server 2012 R2 中的 AD FS，可以使用 SQL Server 2008 和更高版本  
  
## <a name="BKMK_6"></a>浏览器要求  
通过浏览器或浏览器控件执行 AD FS 身份验证时，你的浏览器必须符合以下要求：  
  
-   必须启用 JavaScript  
  
-   必须启用 cookie  
  
-   服务器名称指示\(SNI\)必须支持  
  
-   用户证书和设备证书身份验证\(工作区加入功能\)，浏览器必须支持 SSL 客户端证书身份验证  
  
多个密钥的浏览器和平台已进行了验证下面列出了其中的详细信息的呈现和功能。 如果它们符合上面列出的要求，仍支持浏览器和未涵盖此表中的设备：  
  
|||  
|-|-|  
|**浏览器**|**平台**|  
|IE 10.0|Windows 7，Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012 中，Windows Server 2012 R2|  
|IE 11.0|Windows7、 Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012 R2|  
|Windows Web 身份验证代理|Windows 8.1|  
|Firefox \[v21\]|Windows 7，Windows 8.1|  
|Safari \[v7\]|iOS 6，Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7、 Windows 8.1、 Windows Server 2012，Windows Server 2012 R2、 Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> 已知问题\-Firefox:工作区联接功能，用于标识使用设备证书的设备不在 Windows 平台上起作用。 Firefox 当前不支持使用证书预配到 Windows 客户端上的用户证书存储的执行 SSL 客户端证书身份验证。  
  
**Cookie**  
  
AD FS 创建会话\-都必须存储在客户端计算机来登录的基于和永久 cookie\-中，登录\-out，单一登录\-上\(SSO\)，和其他功能。 因此，必须将客户端浏览器配置为接受 Cookie。 用于进行身份验证的 cookie 始终是安全超文本传输协议\(HTTPS\)为原始服务器编写的会话 cookie。 如果未将客户端浏览器配置为允许使用这些 Cookie，则 AD FS 不能正常工作。 永久 Cookie 用于保留用户选择的声明提供方。 你可以通过在 AD FS 登录的配置文件中使用的配置设置中禁用它们\-页中。 支持 TLS\/均出于安全原因需要 SSL。  
  
## <a name="BKMK_extranet"></a>Extranet 要求  
若要提供对 AD FS 服务的 extranet 访问，必须部署 Web 应用程序代理角色服务作为 extranet 面向角色该代理身份验证请求中安全的方式为 AD FS 服务。 这提供了隔离的 AD FS 服务终结点以及所有的安全密钥隔离\(如令牌签名证书\)从源自 internet 的请求。 此外，如软 Extranet 帐户锁定功能都需要 Web 应用程序代理的使用。 有关 Web 应用程序代理的详细信息，请参阅[Web 应用程序代理](https://technet.microsoft.com/library/dn584107.aspx)。  
  
如果你想要使用第三个\-这第三方代理进行 extranet 访问\-方代理必须支持中定义的协议[http:\/\/download.microsoft.com\/下载\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bms\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>网络要求  
正确配置以下网络服务对于成功部署你的组织中的 AD FS 至关重要：  
  
**配置企业防火墙**  
  
这两个防火墙位于 Web 应用程序代理和联合服务器场和客户端和 Web 应用程序代理之间的防火墙之间必须具有 TCP 端口 443 启用入站。  
  
此外，如果客户端用户证书身份验证\(进行 clientTLS 身份验证使用 X509 用户证书\)是必需的 Windows Server 2012 R2 中的 AD FS 要求启用 TCP 端口 49443 上之间的防火墙入站客户端和 Web 应用程序代理。 这不需要 Web 应用程序代理和联合身份验证服务器之间的防火墙上\)。  
  
**配置 DNS**  
  
-   对于 intranet 访问，所有客户端访问 AD FS 服务内部企业网络中\(intranet\)必须能够解析 AD FS 服务名称\(提供的 SSL 证书名称\)负载为 AD FS 服务器或 AD FS 服务器的均衡器。  
  
-   为 extranet 访问，所有客户端访问 AD FS 服务从公司网络外部\(extranet\/internet\)必须能够解析 AD FS 服务名称\(提供的 SSL 证书名\)到 Web 应用程序代理服务器或 Web 应用程序代理服务器的负载均衡器。  
  
-   为 extranet 访问才能正常工作，在外围网络中的每个 Web 应用程序代理服务器必须能够解析 AD FS 服务名称\(提供的 SSL 证书名称\)到 AD FS 服务器或 AD FS 服务器的负载均衡器。 这可以实现使用备用 DNS 服务器在外围网络中或通过更改使用主机文件的本地服务器解析。  
  
-   若要运行在网络内部和外部网络中的 Web 应用程序代理通过公开终结点的子集的 Windows 集成身份验证，必须使用 A 记录\(不是 CNAME\)为指向负载均衡器。  
  
有关配置公司 DNS 中的为联合身份验证服务和设备注册服务的信息，请参阅[配置公司 DNS 中为联合身份验证服务和 DRS](https://technet.microsoft.com/library/dn486786.aspx)。  
  
为 Web 应用程序代理配置公司 DNS 中的信息，请参阅中的"配置 DNS"一节[步骤 1:配置 Web 应用程序代理基础结构](https://technet.microsoft.com/library/dn383644.aspx)。  
  
有关如何配置群集 IP 地址或群集 FQDN 使用 NLB 的信息，请参阅指定群集参数在[http:\/\/go.microsoft.com\/fwlink\/？LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
## <a name="BKMK_8"></a>属性存储要求  
AD FS 要求至少一个属性存储用于进行用户身份验证以及为这些用户提取安全声明。 有关 AD FS 支持的存储属性的列表，请参阅[The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
> [!NOTE]  
> AD FS 会自动创建"Active Directory"属性存储，默认情况下。 属性存储要求取决于你的组织充当帐户合作伙伴\(承载联合的用户\)或资源伙伴\(承载联合应用程序\)。  
  
**LDAP 属性存储**  
  
当您使用其他轻型目录访问协议\(LDAP\)\-基于属性存储，必须连接到 LDAP 服务器支持 Windows 集成身份验证。 必须采用如 RFC 2255 中所述的 LDAP URL 格式编写 LDAP 连接字符串。  
  
它也是所需的 AD FS 服务的服务帐户有权检索 LDAP 属性存储中的用户信息。  
  
**SQL Server 属性存储**  
  
若要成功运行的 Windows Server 2012 R2 中 AD fs，承载 SQL Server 属性存储的计算机必须运行 Microsoft SQL Server 2008 或更高版本。 当使用 SQL\-基于属性存储，你还必须配置连接字符串。  
  
**自定义属性存储**  
  
你可以开发自定义属性存储来启用高级方案。  
  
-   内置于 AD FS 的策略语言可以引用自定义属性存储，以便可以增强以下任何方案：  
  
    -   创建本地身份验证的用户的声明  
  
    -   补充外部身份验证的用户的声明  
  
    -   授权用户获取令牌  
  
    -   授权服务，以获取用户行为的令牌  
  
    -   在 AD fs 颁发给信赖方的安全令牌中颁发的其他数据。  
  
-   基于.NET 4.0 或更高版本，必须生成所有自定义属性存储。  
  
使用自定义属性存储时，可能还需要配置的连接字符串。 在这种情况下，可以输入可以连接到自定义属性存储所选的自定义代码。 在此情况下的连接字符串是一组的名称\/值对，将解释为自定义属性存储的开发人员实现。有关开发和使用自定义属性存储的详细信息，请参阅[属性存储概述](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="BKMK_9"></a>应用程序的要求  
AD FS 支持声明\-识别应用程序，使用以下协议：  
  
-   WS\-联合身份验证  
  
-   WS\-信任  
  
-   SAML 2.0 协议使用 IDPLite、 SPLite 和 eGov1.5 配置文件。  
  
-   OAuth 2.0 授权授予配置文件  
  
AD FS 还支持身份验证和授权的任何非\-声明\-识别应用程序支持的 Web 应用程序代理。  
  
## <a name="BKMK_10"></a>身份验证要求  
**AD DS 身份验证\(主要身份验证\)**  
  
对于 intranet 访问，支持以下标准身份验证机制适用于 AD DS:  
  
-   Windows 集成身份验证使用 Kerberos 和 NTLM 协商  
  
-   窗体身份验证使用的用户名\/密码  
  
-   使用映射到 AD DS 中的用户帐户的证书的证书身份验证  
  
为 extranet 访问，支持以下身份验证机制：  
  
-   窗体身份验证使用的用户名\/密码  
  
-   使用映射到 AD DS 中的用户帐户的证书的证书身份验证  
  
-   Windows 集成身份验证使用 Negotiate\(仅 NTLM\) ws\-信任接受 Windows 集成身份验证的终结点。  
  
对于证书身份验证：  
  
-   将扩展到可以是受保护的 pin 的智能卡。  
  
-   用户输入其 pin 的 GUI 不由 AD FS 提供和需要使用客户端 TLS 时，将显示客户端操作系统的一部分。  
  
-   读取器和加密服务提供程序\(CSP\)智能卡必须在浏览器所在的计算机上工作。  
  
-   智能卡证书必须链接到所有 AD FS 服务器和 Web 应用程序代理服务器上受信任的根。  
  
-   必须通过以下方法之一将该证书映射到 AD DS 中的用户帐户：  
  
    -   证书使用者名称对应于 AD DS 中的用户帐户的 LDAP 可分辨名称。  
  
    -   证书使用者 altname 扩展具有用户主体名称\(UPN\)的 AD DS 中的用户帐户。  
  
对于无缝 Windows 集成身份验证使用 Kerberos 在 intranet 中  
  
-   它是必需的服务名称为受信任的站点或本地 Intranet 站点的一部分。  
  
-   此外，主机\/< adfs\_服务\_名称 > 必须在 AD FS 场在运行的服务帐户设置 SPN。  
  
**多\-重身份验证**  
  
AD FS 支持其他身份验证\(超出了支持的 AD DS 的主要身份验证\)使用提供程序模型，由此供应商\/客户构建他们自己多\-身份验证适配器管理员可以注册和登录期间使用。  
  
每个 MFA 适配器必须基于.NET 4.5。  
  
有关 MFA 的详细信息，请参阅[使用针对敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
**设备身份验证**  
  
AD FS 支持使用证书在加入其设备的最终用户工作区的操作期间由设备注册服务设置设备身份验证。  
  
## <a name="BKMK_11"></a>Workplace join 要求  
最终用户可以加入工作区使用 AD FS 的组织到其设备。 这被支持 AD FS 中的设备注册服务。 因此，最终用户获取在 AD FS 支持的应用程序的 SSO 的另一个优点。 此外，管理员可以通过限制对仅限为已加入到组织的工作区的设备的应用程序访问管理风险。 下面是以下要求，才能启用此方案。  
  
-   AD FS 支持加入工作区适用于 Windows 8.1 和 iOS 5\+设备  
  
-   若要使用工作区加入的功能，AD FS 服务器将加入到林的架构必须是 Windows Server 2012 R2。  
  
-   使用者可选名称的 SSL 证书的 AD FS 服务必须包含后跟用户主体名称值 enterpriseregistration \(UPN\)你的组织后缀，例如，enterpriseregistration.corp.contoso.com。  
  
## <a name="BKMK_12"></a>加密要求  
下表提供有关 AD FS 令牌签名、 令牌加密的其他加密支持信息\/解密功能：  
  
||||  
|-|-|-|  
|**算法**|**密钥长度**|**协议\/应用程序\/注释**|  
|TripleDES – 默认 192\(支持 192 – 256\) \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|受支持的算法来对安全令牌进行解密。 不支持加密的安全令牌使用此算法。|  
|AES128 \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#aes128\-cbc|128|受支持的算法来对安全令牌进行解密。 不支持加密的安全令牌使用此算法。|  
|AES192 \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#aes192\-cbc|192|受支持的算法来对安全令牌进行解密。 不支持加密的安全令牌使用此算法。|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|“默认”  。 受支持的算法来加密的安全令牌。|  
|TripleDESKeyWrap \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#kw\-tripledes|支持的.NET 4.0 的所有密钥大小\+|受支持的加密对安全令牌进行加密的对称密钥的算法。|  
|AES128KeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|受支持的加密对安全令牌进行加密的对称密钥的算法。|  
|AES192KeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|受支持的加密对安全令牌进行加密的对称密钥的算法。|  
|AES256KeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|受支持的加密对安全令牌进行加密的对称密钥的算法。|  
|RsaV15KeyWrap \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#rsa\-1\_5|1024|受支持的加密对安全令牌进行加密的对称密钥的算法。|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|默认值。 受支持的加密对安全令牌进行加密的对称密钥的算法。|  
|SHA1\-http:\/\/www.w3.org\/PICS\/DSig\/SHA1\_1\_0 html|N\/A|在项目 SourceId 生成中使用 AD FS 服务器：在此方案中，STS 使用 SHA1\(每个 SAML 2.0 标准中的建议\)创建项目 sourceiD 短 160 位值。<br /><br />也使用 ADFS web 代理\(WS2003 时间范围中的旧组件\)以确定在"上次更新时间"的时间内更改值，以便它知道何时更新 STS 中的信息。|  
|SHA1withRSA\-<br /><br />http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|AD FS 服务器验证的 SAML AuthenticationRequest 签名时，则使用在情况下，登录项目分辨率请求或响应中，创建令牌\-签名证书。<br /><br />在这些情况下，SHA256 是默认情况下，，如果仅使用 SHA1 合作伙伴\(信赖方\)无法支持 SHA256，并且必须使用 SHA1。|  
  
## <a name="BKMK_13"></a>权限要求  
执行安装和初始配置的 AD FS 管理员必须具有域管理员权限在本地域中\(换而言之，联合身份验证服务器所加入到域。\)  
  
## <a name="see-also"></a>请参阅  
[Windows Server 2012 R2 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

