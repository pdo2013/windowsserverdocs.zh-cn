---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: 使用 SQL Server 的联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 720c20437f7e6da875b809b2816f0d4df5d210d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359187"
---
# <a name="ad-fs-requirements"></a>AD FS 要求

部署 AD FS 时，必须遵守以下各种要求：  
  
-   [证书要求](AD-FS-Requirements.md#BKMK_1)  
  
-   [硬件要求](AD-FS-Requirements.md#BKMK_2)  
  
-   [软件要求](AD-FS-Requirements.md#BKMK_3)  
  
-   [AD DS 要求](AD-FS-Requirements.md#BKMK_4)  
  
-   [配置数据库要求](AD-FS-Requirements.md#BKMK_5)  
  
-   [浏览器要求](AD-FS-Requirements.md#BKMK_6)  
  
-   [Extranet 要求](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [网络要求](AD-FS-Requirements.md#BKMK_7)  
  
-   [属性存储要求](AD-FS-Requirements.md#BKMK_8)  
  
-   [应用程序要求](AD-FS-Requirements.md#BKMK_9)  
  
-   [身份验证要求](AD-FS-Requirements.md#BKMK_10)  
  
-   [工作区加入要求](AD-FS-Requirements.md#BKMK_11)  
  
-   [加密要求](AD-FS-Requirements.md#BKMK_12)  
  
-   [权限要求](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>证书要求  
证书在保护联合服务器、web 应用程序代理、声明\-感知应用程序和 Web 客户端之间的通信方面起着最重要的作用。 证书要求因你设置的是联合服务器还是代理计算机而异，具体取决于此部分中所述。  
  
**联合服务器证书**  
  
|||  
|-|-|  
|**证书类型**|**要求、支持 & 须知**|  
|**安全套接字层\(SSL\)证书：** 这是标准的 SSL 证书，用于保护联合服务器和客户端之间的通信。|-此证书必须是公开信任\*的 X509 v3 证书。<br />-访问任何 AD FS 终结点的所有客户端都必须信任此证书。 强烈\(建议使用由公共第三\-方\)证书颁发机构\(CA\)颁发的证书。 在测试实验室环境中\-，可以在联合服务器上成功使用自签名 SSL 证书。 但是，对于生产环境，建议从公共 CA 获取证书。<br />-支持 Windows Server 2012 R2 for SSL 证书支持的任何密钥大小。<br />-不支持使用 CNG 密钥的证书。<br />-与 Workplace Join\/设备注册服务一起使用时，AD FS 服务的 SSL 证书的使用者可选名称必须包含后跟用户主体名称\(enterpriseregistration的值组织\)的 UPN 后缀，例如，enterpriseregistration.contoso.com。<br />-支持通配符证书。 当你创建 AD FS 场时，系统将提示你提供 AD FS 服务\(的服务名称，例如**adfs.contoso.com**。<br />-强烈建议为 Web 应用程序代理使用相同的 SSL 证书。 但在通过 Web 应用程序代理支持 Windows 集成身份验证终结点以及在\(默认设置\)下启用扩展保护身份验证时，这是必需的。<br />-此证书的使用者名称用于表示部署 AD FS 的每个实例的联合身份验证服务名称。 出于此原因，你可能需要考虑在任何新 CA\-颁发的证书上选择一个使用者名称，该证书最好表示你的公司或组织对合作伙伴的名称。<br />    证书的标识必须与联合身份验证服务名称\(（例如 fs.contoso.com\)）匹配。标识是类型为 dNSName 的使用者可选名称扩展，如果没有使用者可选名称项，则为指定为公用名的使用者名称。 证书中可以存在多个使用者可选名称项，前提是其中一项与联合身份验证服务名称匹配。<br />-   **重要说明：** 强烈建议在 AD FS 场的所有节点以及 AD FS 场中的所有 Web 应用程序代理上使用相同的 SSL 证书。|  
|**服务通信证书：** 此证书用于保护联合服务器之间的通信安全以确保 WCF 消息的安全性。|-默认情况下，将 SSL 证书用作服务通信证书。  但您也可以选择将其他证书配置为服务通信证书。<br />-   **重要提示：** 如果使用 ssl 证书作为服务通信证书，当 ssl 证书过期时，请确保将续订的 SSL 证书配置为服务通信证书。 此操作不会自动发生。<br />-此证书必须受使用 WCF 消息安全的 AD FS 的客户端信任。<br />- \(建议使用由公共第三\-方\)证书颁发机构\(CA\)颁发的服务器身份验证证书。<br />-服务通信证书不能是使用 CNG 密钥的证书。<br />-此证书可使用 AD FS 管理控制台进行管理。|  
|**令牌\-签名证书：** 这是一种用于安全地对联合服务器颁发的所有令牌进行签名的标准 X509 证书。|-默认情况下，AD FS 创建具有\-2048 位密钥的自签名证书。<br />-也支持 CA 颁发的证书，并且可以使用中的 AD FS 管理 "\-管理单元进行更改<br />-CA 颁发的证书必须存储 & 通过 CSP 加密提供程序进行访问。<br />-令牌签名证书不能是使用 CNG 密钥的证书。<br />-AD FS 不需要外部注册的令牌签名证书。<br />    AD FS 在这些自\-签名证书过期之前自动续订这些证书，请首先将新证书配置为辅助证书，以便合作伙伴可以使用这些证书，然后在名为 "自动" 的进程中翻转到主证书证书滚动更新。建议使用默认的自动生成的令牌签名证书。<br />    如果你的组织有需要为令牌签名配置不同证书的策略，你可以在安装时使用 Powershell \(指定证书。请使用安装的– SigningCertificateThumbprint 参数。\-Install-adfsfarm cmdlet\)。  安装完成后，可以使用 AD FS 管理控制台或 Powershell cmdlet 设置\-get-adfscertificate 并获取\-get-adfscertificate 来查看和管理令牌签名证书。<br />    如果使用外部注册的证书进行令牌签名，AD FS 不会执行自动证书续订或滚动更新。  此过程必须由管理员执行。<br />    若要允许在一个证书即将过期时使用证书滚动更新，可以在 AD FS 中配置辅助令牌签名证书。 默认情况下，所有令牌签名证书都在联合元数据中发布，但 AD FS\-仅使用主要令牌签名证书来对令牌进行实际签名。|  
|**令牌\-解密\/加密证书：** 这是用于解密\/任何传入令牌的标准 X509 证书。 该证书也在联合元数据中发布。|-默认情况下，AD FS 创建具有\-2048 位密钥的自签名证书。<br />-也支持 CA 颁发的证书，并且可以使用中的 AD FS 管理 "\-管理单元进行更改<br />-CA 颁发的证书必须存储 & 通过 CSP 加密提供程序进行访问。<br />-令牌\-解密\/加密证书不能是使用 CNG 密钥的证书。<br />-默认情况下，AD FS 生成并使用其自己的、内部生成\-的自签名证书进行令牌解密。  AD FS 不需要外部注册的证书即可实现此目的。<br />    此外，AD FS 会在证书过期之前\-自动续订这些自签名证书。<br />    **建议使用默认的自动生成的证书进行令牌解密。**<br />    如果你的组织有需要为令牌解密配置不同证书的策略，你可以在安装时使用 Powershell \(指定证书。安装\-install-adfsfarm cmdlet\)。  安装后，可以使用 AD FS 管理控制台或 Powershell cmdlet 设置\-get-adfscertificate 并获取\-get-adfscertificate 来查看和管理令牌解密证书。<br />    **当外部注册的证书用于令牌解密时，AD FS 不会执行自动证书续订。此过程必须由管理员**执行。<br />-AD FS 服务帐户必须有权访问本地计算机个人\-存储中的令牌签名证书的私钥。 这由安装程序执行。 如果随后更改令牌\-\-签名证书，还可以使用 AD FS 管理 "管理单元来确保此访问权限。|  
  
> [!CAUTION]  
> 用于令牌\-签名和令牌\-解密\/加密的证书对于联合身份验证服务的稳定性至关重要。 如果客户管理其自己\-的令牌签名\-&\/令牌对加密证书进行解密，则应确保这些证书已备份，并在恢复事件期间单独提供。  
  
> [!NOTE]  
> 在 AD FS 可以将用于数字签名的安全\(哈\)希算法 SHA 级别更改为 sha\-1 或 sha\-256 \(更安全\)。 AD FS 不支持将证书用于其他哈希方法（如 MD5 \(）和 Makecert 命令\-行工具\)一起使用的默认哈希算法。 作为安全性最佳做法，我们建议使用默认情况下\-\)为所有\(签名设置的 SHA 256。 仅\-在必须与不支持使用 sha\-256 通信的产品（如非\-Microsoft 产品或旧版本的 AD FS）互操作的情况下，才建议使用 SHA 1。  
  
> [!NOTE]  
> 从 CA 收到证书后，请确保将所有的证书导入到本地计算机的个人证书存储中。 可以通过证书 mmc 管理单元\-将证书导入到个人存储中。  
  
## <a name="BKMK_2"></a>硬件要求  
以下最低要求和建议的硬件要求适用于 Windows Server 2012 R2 中的 AD FS 联合服务器：  
  
||||  
|-|-|-|  
|**硬件要求**|**最低要求**|**建议的要求**|  
|CPU 速度|1.4 GHz 64\-位处理器|四\-核，2 GHz|  
|RAM|512 MB|4 GB|  
|磁盘空间|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>软件要求  
以下 AD FS 要求适用于 Windows Server® 2012 R2 操作系统中内置的服务器功能：  
  
-   对于 extranet 访问，你必须部署 Windows Server® 2012 R2 远程\-访问服务器角色的 "Web 应用程序代理" 角色服务部分。 Windows Server® 2012 R2 中的 AD FS 不支持早期版本的联合服务器代理。  
  
-   不能在同一台计算机上安装联合服务器和 Web 应用程序代理角色服务。  
  
## <a name="BKMK_4"></a>AD DS 要求  
**域控制器要求**  
  
所有用户域中的域控制器和 AD FS 服务器加入到的域中的域控制器必须运行 Windows Server 2008 或更高版本。  
  
> [!NOTE]  
> Windows server 2003 域控制器的环境的所有支持将在 Windows Server 2003 的扩展支持结束日期之后结束。 强烈建议客户尽快升级域控制器。 有关 Microsoft 支持部门生命周期的其他信息，请访问[此页](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)。 对于发现的特定于 Windows Server 2003 域控制器环境的问题，将仅针对安全问题和修补程序发出修补程序，前提是在延长对 Windows Server 2003 的支持之前。  
  
**域功能\-级别要求**  
  
必须在 Windows Server 2003 或更高版本的域功能级别上运行所有用户帐户域和 AD FS 服务器加入到的域。  
  
大多数 AD FS 功能不需要 AD DS 功能\-级别修改即可成功运行。 但是，如果证书明确映射到 AD DS 中的用户帐户，则 Windows Server 2008 域功能级别或更高版本是要成功运行客户端证书身份验证所必需的。  
  
**架构要求**  
  
-   AD FS 不需要对 AD DS 进行架构更改\-或功能级别修改。  
  
-   若要使用 Workplace Join 功能，必须将 AD FS 服务器的林的架构设置为 Windows Server 2012 R2。  
  
**服务帐户要求**  
  
-   任何标准服务帐户都可用作 AD FS 的服务帐户。 还支持组托管服务帐户。 这需要至少一个域控制器， \(建议部署两台或更多\)运行 Windows Server 2012 或更高版本的域控制器。  
  
-   要使 Kerberos 身份验证在加入\-域的客户端和 AD FS 之间正常工作，\_必须\_在服务帐户上将 "HOST\/< adfs 服务名称 >" 注册为 SPN。 默认情况下，如果新的 AD FS 场具有执行此操作的足够权限，则 AD FS 将配置此配置。  
  
-   每个用户域中都必须有 AD FS 的服务帐户，该帐户包含对 AD FS 服务进行身份验证的用户。  
  
**域要求**  
  
-   所有 AD FS 服务器都必须加入 AD DS 域。  
  
-   在场中的所有 AD FS 服务器都必须部署在单个域中。  
  
-   AD FS 服务器加入到的域必须信任包含向 AD FS 服务进行身份验证的用户的每个用户帐户域。  
  
**多林要求**  
  
-   AD FS 服务器加入到的域必须信任包含向 AD FS 服务进行身份验证的用户的每个用户帐户域或林。  
  
-   每个用户域中都必须有 AD FS 的服务帐户，该帐户包含对 AD FS 服务进行身份验证的用户。  
  
## <a name="BKMK_5"></a>配置数据库要求  
下面是根据配置存储的类型应用的要求和限制：  
  
**WID**  
  
-   如果有100个或更少的信赖方信任，则 WID 场的限制为30个联合服务器。  
  
-   WID 配置数据库中不支持 SAML 2.0 中的项目解析配置文件。  WID 配置数据库不支持令牌重播检测。 此功能仅在 AD FS 充当联合身份验证提供程序并使用来自外部声明提供程序的安全令牌的情况下才使用。  
  
-   只要服务器的数量不超过30个，就支持在不同的数据中心部署 AD FS 服务器进行故障转移或地理负载平衡。  
  
下表简要介绍了如何使用 WID 场。  使用它来规划你的实现。  
  
||||  
|-|-|-|  
||1 \- 100 RP 信任|超过 100 RP 信任|  
|1 \- 30 个 AD FS 节点|支持 WID|不支持使用 WID \- SQL|  
|超过30个 AD FS 节点|不支持使用 WID \- SQL|不支持使用 WID \- SQL|  
  
**SQL Server**  
  
对于 Windows Server 2012 R2 中的 AD FS，可以使用 SQL Server 2008 及更高版本  
  
## <a name="BKMK_6"></a>浏览器要求  
AD FS 通过浏览器或浏览器控件执行身份验证时，浏览器必须遵守以下要求：  
  
-   必须启用 JavaScript  
  
-   Cookie 必须打开  
  
-   必须\(支持\)服务器名称指示 SNI  
  
-   要使用户证书 & 设备证书\(身份验证工作\)区加入功能，浏览器必须支持 SSL 客户端证书身份验证  
  
几个关键浏览器和平台已经完成了呈现和功能的验证，详细信息如下所示。 如果未在此表中包含的浏览器和设备满足上面列出的要求，则仍受支持：  
  
|||  
|-|-|  
|**器**|**适用**|  
|IE 10。0|Windows 7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2|  
|IE 11。0|Windows7，Windows 8.1，Windows Server 2008 R2，Windows Server 2012，Windows Server 2012 R2|  
|Windows Web 身份验证代理|Windows 8.1|  
|Firefox \[v21\]|Windows 7、Windows 8。1|  
|Safari \[v7\]|iOS 6，Mac OS\-X 10。7|  
|Chrome \[v27\]|Windows 7、Windows 8.1、windows server 2012、windows server 2012 R2、Mac OS\-X 10。7|  
  
> [!IMPORTANT]  
> 已知问题\- ：使用设备证书标识设备的 Workplace Join 功能在 Windows 平台上不起作用。 Firefox 当前不支持使用预配到 Windows 客户端上的用户证书存储的证书来执行 SSL 客户端证书身份验证。  
  
**Cookie**  
  
AD FS 创建必须\-存储在客户端计算机上的基于会话的 cookie 和永久 cookie\-，以提供\-登录、注销、\-单一\(登录\)SSO 和其他功能。 因此，必须将客户端浏览器配置为接受 Cookie。 用于身份验证的 cookie 始终是为原始服务器编写的\(安全\)超文本传输协议 HTTPS 会话 cookie。 如果未将客户端浏览器配置为允许使用这些 Cookie，则 AD FS 不能正常工作。 永久 Cookie 用于保留用户选择的声明提供方。 你可以使用配置文件中 AD FS 登录\-页的配置设置来禁用它们。 出于安全考虑\/，需要支持 TLS SSL。  
  
## <a name="BKMK_extranet"></a>Extranet 要求  
若要提供对 AD FS 服务的 extranet 访问，必须将 Web 应用程序代理角色服务部署为面向公众的角色，以安全方式将身份验证请求代理到 AD FS 服务。 这将提供 AD FS 服务终结点的隔离，并将所有安全密钥\(（例如来自 internet 的请求的令牌签名证书\) ）隔离起来。 此外，软 Extranet 帐户锁定等功能需要使用 Web 应用程序代理。 有关 Web 应用程序代理的详细信息，请参阅[Web 应用程序代理](https://technet.microsoft.com/library/dn584107.aspx)。  
  
如果要使用第三个 @ no__t-0party 代理进行 extranet 访问，则此第三个 @ no__t-1party 代理必须支持 http 中定义的协议[： \/\/download.microsoft.com @ no__t-5download @ no__t-69 @ no__t-no__t](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf)@ 8E-no__t @ 995EF66AF-no__t @ 109026-no__t @ 114BB0-no__t @ 12A41D。  
  
## <a name="BKMK_7"></a>网络要求  
适当地配置以下网络服务对于在贵组织中成功部署 AD FS 至关重要：  
  
**配置企业防火墙**  
  
位于 Web 应用程序代理和联合服务器场之间以及客户端和 Web 应用程序代理之间的防火墙的防火墙必须启用 TCP 端口443入站。  
  
此外，如果需要使用 X509 \(\)用户证书的客户端用户证书身份验证 clientTLS authentication，Windows Server 2012 R2 中 AD FS 需要在防火墙上启用 TCP 端口49443，客户端和 Web 应用程序代理。 在 Web 应用程序代理和联合服务器\)之间的防火墙上不需要这样做。  
  
**配置 DNS**  
  
-   对于 intranet 访问，访问内部公司\(网络 intranet\)内 AD FS 服务的所有客户端必须能够将 SSL 证书\)提供的 AD FS \(服务名称名称解析为负载AD FS 服务器或 AD FS 服务器的均衡器。  
  
-   对于 extranet 访问，从企业\(网络 extranet\/internet\)外部访问 AD FS 服务的所有客户端必须能够解析 SSL 证书\(提供的ADFS服务名称名称\)用于 Web 应用程序代理服务器或 Web 应用程序代理服务器的负载均衡器。  
  
-   为了使 extranet 访问正常运行，DMZ 中的每个 Web 应用程序代理服务器都必须能够将 SSL 证书\(\)提供 AD FS 服务名称名称解析为 AD FS 服务器或 AD FS 服务器的负载均衡器。 这可以通过使用外围网络中的备用 DNS 服务器或通过使用 HOSTS 文件更改本地服务器解析来实现。  
  
-   若要使 Windows 集成身份验证在网络内部和网络外部使用，以通过 Web 应用程序代理公开的终结点子集，必须使用 a 记录\(而不\)是 CNAME 来指向负载均衡器。  
  
有关为联合身份验证服务和设备注册服务配置企业 DNS 的信息，请参阅为[联合身份验证服务和 DRS 配置企业 dns](https://technet.microsoft.com/library/dn486786.aspx)。  
  
有关为 Web 应用程序代理配置企业 DNS 的信息，请参阅[步骤1：配置 Web 应用程序代理基础](https://technet.microsoft.com/library/dn383644.aspx)结构。  
  
有关如何使用 NLB 配置群集 IP 地址或群集 FQDN 的信息，请参阅在[\/http：\/go.microsoft.com\/fwlink\/中指定群集参数？LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
## <a name="BKMK_8"></a>属性存储要求  
AD FS 要求至少使用一个属性存储来对用户进行身份验证，并为这些用户提取安全声明。 有关 AD FS 支持的属性存储的列表，请参阅[属性存储的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
> [!NOTE]  
> 默认情况下，AD FS 会自动创建 "Active Directory" 特性存储。 属性存储要求取决于你的组织是否\(作为托管联合用户\)的帐户伙伴或托管联合应用程序\)的\(资源伙伴。  
  
**LDAP 特性存储**  
  
使用其他轻型目录访问协议\(LDAP\)\-的属性存储时，必须连接到支持 Windows 集成身份验证的 LDAP 服务器。 必须采用如 RFC 2255 中所述的 LDAP URL 格式编写 LDAP 连接字符串。  
  
还要求 AD FS 服务的服务帐户有权在 LDAP 属性存储中检索用户信息。  
  
**SQL Server 属性存储**  
  
若要使 Windows Server 2012 R2 中的 AD FS 成功运行，承载 SQL Server 属性存储的计算机必须 Microsoft SQL Server 2008 或更高版本运行。 使用基于 SQL\-的属性存储时，还必须配置连接字符串。  
  
**自定义属性存储**  
  
你可以开发自定义属性存储来启用高级方案。  
  
-   内置于 AD FS 的策略语言可以引用自定义属性存储，以便可以增强以下任何方案：  
  
    -   创建本地身份验证的用户的声明  
  
    -   补充外部身份验证的用户的声明  
  
    -   授权用户获取令牌  
  
    -   授权服务，以获取用户行为的令牌  
  
    -   在 AD FS 颁发给信赖方的安全令牌中发出额外的数据。  
  
-   所有自定义属性存储都必须在 .NET 4.0 或更高版本之上构建。  
  
使用自定义属性存储时，可能还需要配置连接字符串。 在这种情况下，可以输入所选的自定义代码，以便能够连接到自定义属性存储。 此情况下的连接字符串是一组名称\/值对，这些值对被解释为自定义属性存储的开发人员实现的。有关开发和使用自定义属性存储的详细信息，请参阅[属性存储概述](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="BKMK_9"></a>应用程序要求  
AD FS 支持使用\-以下协议的声明感知应用程序：  
  
-   WS\-联合身份验证  
  
-   WS\-信任  
  
-   SAML 2.0 协议，使用 IDPLite、SPLite & eGov 1.5 配置文件。  
  
-   OAuth 2.0 授权授权配置文件  
  
AD FS 还支持对 Web 应用程序代理支持\-的\-任何非声明感知应用程序进行身份验证和授权。  
  
## <a name="BKMK_10"></a>身份验证要求  
**AD DS 身份\(验证主要身份验证\)**  
  
对于 intranet 访问，支持以下 AD DS 标准身份验证机制：  
  
-   使用协商的 Windows 集成身份验证 & NTLM  
  
-   使用用户名\/密码进行 Forms 身份验证  
  
-   使用映射到 AD DS 中的用户帐户的证书进行证书身份验证  
  
对于 extranet 访问，支持以下身份验证机制：  
  
-   使用用户名\/密码进行 Forms 身份验证  
  
-   使用映射到 AD DS 中的用户帐户的证书进行证书身份验证  
  
-   Windows 集成身份验证仅\(\)对接受 Windows 集成\-身份验证的 WS 信任终结点使用协商 NTLM。  
  
对于证书身份验证：  
  
-   扩展到可固定受保护的智能卡。  
  
-   用于用户输入其 pin 的 GUI 不是由 AD FS 提供的，并且必须是使用客户端 TLS 时所显示的客户端操作系统的一部分。  
  
-   智能卡的读取器和\(加密\)服务提供程序 CSP 必须在浏览器所在的计算机上工作。  
  
-   智能卡证书必须链接到所有 AD FS 服务器和 Web 应用程序代理服务器上的受信任的根。  
  
-   必须通过以下方法之一将该证书映射到 AD DS 中的用户帐户：  
  
    -   证书使用者名称对应于 AD DS 中的用户帐户的 LDAP 可分辨名称。  
  
    -   证书使用者 altname 扩展在 AD DS 中具有用户\(帐户\)的用户主体名称 UPN。  
  
对于 intranet 中使用 Kerberos 的无缝 Windows 集成身份验证，  
  
-   服务名称必须是受信任的站点或本地 Intranet 站点的一部分。  
  
-   此外，必须在运行\/AD FS 场\_的\_服务帐户上设置主机 < adfs 服务名称 > SPN。  
  
**多重\-身份验证**  
  
AD FS 支持除主要\(身份验证之外的其他身份\)验证，该身份验证是使用提供程序模型提供的\-AD DS，因此供应商\/客户可以构建自己的多重身份验证适配器在登录过程中，管理员可以注册并使用。  
  
每个 MFA 适配器都必须构建在 .NET 4.5 之上。  
  
有关 MFA 的详细信息，请参阅[利用适用于敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
**设备身份验证**  
  
AD FS 支持设备身份验证，使用设备注册服务在加入其设备的最终用户工作区的操作过程中设置的证书。  
  
## <a name="BKMK_11"></a>工作区加入要求  
最终用户可以通过工作区将其设备加入到使用 AD FS 的组织。 AD FS 中的设备注册服务支持此项。 因此，最终用户可以在 AD FS 所支持的应用程序中获得 SSO 的额外权益。 此外，管理员还可以通过将对应用程序的访问权限限制为已加入到组织的工作区的设备来管理风险。 若要启用此方案，需要满足以下要求。  
  
-   AD FS 支持 Windows 8.1 和 iOS 5\+设备的工作区加入  
  
-   若要使用 Workplace Join 功能，AD FS 服务器联接到的林的架构必须为 Windows Server 2012 R2。  
  
-   AD FS 服务的 SSL 证书的使用者可选名称必须包含值 enterpriseregistration，后跟组织的用户主体名称\(UPN\)后缀，例如enterpriseregistration.corp.contoso.com。  
  
## <a name="BKMK_12"></a>加密要求  
下表提供了有关 AD FS 令牌签名、令牌加密\/解密功能的其他加密支持信息：  
  
||||  
|-|-|-|  
|**算法**|**密钥长度**|**协议\/应用\/程序注释**|  
|TripleDES –默认 192 \(支持192– 256\) \- [http：\/\/www.w3.org\/200104\/xmlenc\#TripleDES\/cbc\-](http://www.w3.org/2001/04/xmlenc#tripledes-cbc)|>\=192|支持对安全令牌进行解密的算法。 不支持通过此算法加密安全令牌。|  
|AES128 \- [http：\/www.w3.org\/200104\/xmlencAES128\#cbc\/\/\-](http://www.w3.org/2001/04/xmlenc#aes128-cbc)|128|支持对安全令牌进行解密的算法。 不支持通过此算法加密安全令牌。|  
|AES192 \- [http：\/www.w3.org\/200104\/xmlencAES192\#cbc\/\/\-](http://www.w3.org/2001/04/xmlenc#aes192-cbc)|192|支持对安全令牌进行解密的算法。 不支持通过此算法加密安全令牌。|  
|AES256 \- [http：\/www.w3.org\/200104\/xmlencAES256\#cbc\/\/\-](http://www.w3.org/2001/04/xmlenc#aes256-cbc)|256|“默认”。 支持对安全令牌进行加密的算法。|  
|TripleDESKeyWrap \- [http：\/www.w3.org\/200104\/xmlenckw\#tripledes\/\/\-](http://www.w3.org/2001/04/xmlenc#kw-tripledes)|.NET 4.0 支持的所有密钥大小\+|支持对加密安全令牌的对称密钥进行加密的算法。|  
|AES128KeyWrap \- [http：\/www.w3.org\/200104\/xmlenckw\#aes128\/\/\-](http://www.w3.org/2001/04/xmlenc#kw-aes128)|128|支持对加密安全令牌的对称密钥进行加密的算法。|  
|AES192KeyWrap \- [http：\/www.w3.org\/200104\/xmlenckw\#aes192\/\/\-](http://www.w3.org/2001/04/xmlenc#kw-aes192)|192|支持对加密安全令牌的对称密钥进行加密的算法。|  
|AES256KeyWrap \- [http：\/www.w3.org\/200104\/xmlenckw\#aes256\/\/\-](http://www.w3.org/2001/04/xmlenc#kw-aes256)|256|支持对加密安全令牌的对称密钥进行加密的算法。|  
|RsaV15KeyWrap \- [http：\/www.w3.org\/200104\/xmlencrsa1\-5\/\/\#\_](http://www.w3.org/2001/04/xmlenc#rsa-1_5)|1024|支持对加密安全令牌的对称密钥进行加密的算法。|  
|RsaOaepKeyWrap \- [http：\/www.w3.org\/200104\/xmlencrsaoaep\-rsa-oaep-mgf1p\/\/\#\-](http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p)|1024|默认值。 支持对加密安全令牌的对称密钥进行加密的算法。|  
|Sha1\-[http：\/www.w3.org\/图片DSig\/SHA110\_.html\/\/\_](http://www.w3.org/PICS/DSig/SHA1_1_0.html)|N\/A|由项目 SourceId 生成中的 AD FS 服务器使用：在此方案中，STS 按照 SAML \(2.0 标准\)中的建议使用 SHA1，为项目 sourceiD 创建一个较小的160位值。<br /><br />还由 ADFS web 代理\(旧版组件在 WS2003 时间范围\)内使用，以识别 "上次更新时间" 值中的更改，以便它知道何时从 STS 更新信息。|  
|SHA1withRSA\-<br /><br />[http：\/\/www.w3.org图片DSig\/RSA SHA1 10\_.html\/\-\/\_](http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html)|N\/A|用于 AD FS 服务器验证 SAML AuthenticationRequest 的签名、对项目解析请求或响应进行签名、创建令牌\-签名证书的情况下使用。<br /><br />在这些情况下，SHA256 是默认值，仅当合作伙伴\(信赖方\)无法支持 SHA256 并且必须使用 sha1 时，才使用 sha1。|  
  
## <a name="BKMK_13"></a>权限要求  
执行安装和初始配置 AD FS 的管理员必须具有本地域\(中的域管理员权限，即联合服务器加入到的域。\)  
  
## <a name="see-also"></a>请参阅  
[Windows Server 2012 R2 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

