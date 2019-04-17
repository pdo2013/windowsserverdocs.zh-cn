---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: "联合 Server 场使用 SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e23154b20dfcd642178a5ac0de17f1b6a490f91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-requirements"></a>广告 FS 要求

>适用于： Windows Server 2012 R2

以下是时部署广告 FS 必须符合的各种要求：  
  
-   [证书要求](AD-FS-Requirements.md#BKMK_1)  
  
-   [硬件要求](AD-FS-Requirements.md#BKMK_2)  
  
-   [软件要求](AD-FS-Requirements.md#BKMK_3)  
  
-   [广告 DS 要求](AD-FS-Requirements.md#BKMK_4)  
  
-   [配置数据库要求](AD-FS-Requirements.md#BKMK_5)  
  
-   [浏览器要求](AD-FS-Requirements.md#BKMK_6)  
  
-   [联网要求](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [网络要求](AD-FS-Requirements.md#BKMK_7)  
  
-   [特性官方商城要求](AD-FS-Requirements.md#BKMK_8)  
  
-   [应用程序要求](AD-FS-Requirements.md#BKMK_9)  
  
-   [身份验证要求](AD-FS-Requirements.md#BKMK_10)  
  
-   [工作区加入要求](AD-FS-Requirements.md#BKMK_11)  
  
-   [密码系统要求](AD-FS-Requirements.md#BKMK_12)  
  
-   [权限要求](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>证书要求  
证书播放保护通信之间联合身份验证的服务器、 Web 应用程序代理、 claims\ 注意到的应用程序和 Web 客户端中的最重要角色。 证书要求有所不同，具体取决于是否你设置的联合身份验证的服务器或代理计算机上，在此部分中所述。  
  
**联合身份验证的服务器证书**  
  
|||  
|-|-|  
|**证书类型**|**要求、 支持和事项需要你**|  
|**安全套接字层 \(SSL\) 证书：**这是用于保护联合身份验证的服务器和客户端之间进行通信的标准 SSL 证书。|-此证书必须公开 trusted\ * X509 v3 证书。<br />-所有客户访问的任何广告 FS 端点必须都信任该证书。 强烈建议使用证书颁发的公用 \(third\-party\) 证书颁发机构 \(CA\)。 你可以使用 SSL self\ 签名证书成功上联盟服务器的测试实验环境中。 但是，对于生产环境中，我们建议你从公共认证获得证书。<br />-支持 SSL 证书受 Windows Server 2012 R2 的任何键大小。<br />不支持证书的使用 CNG 键的功能。<br />-在工作区设备 Join\/注册服务一起使用时，主题替代名称 SSL 证书广告 FS 服务必须包含跟的你的组织，例如，enterpriseregistration.contoso.com 用户主要名称 \(UPN\) 职务值 enterpriseregistration。<br />的支持通配符证书。 当您创建广告 FS 场时，你将提示您提供服务名为广告 FS 服务 \ (例如， **adfs.contoso.com**。<br />-强烈建议 Web 应用程序代理为使用同一 SSL 证书。 这是但是**需要**支持 Windows 的集成身份验证端点通过 Web 应用程序代理和外延保护身份验证已打开 \(default setting\) 时会相同。<br />-此证书名称主题用于代表你部署的广告 FS 的每个实例联合身份验证服务名称。 出于此原因，你可能希望考虑选择任何新的 CA\ 颁发的证书上的最佳表示你的公司或组织向合作伙伴的名称的对象名称。<br />    证书身份必须匹配联合身份验证服务名称 \ (例如，fs.contoso.com\)。身份是类型 dNSName 任一的主题替代名称扩展，或如果没有主题名称替代项，主题名称指定为通用名称。 提供其中一个匹配的联合身份验证服务的名称，可以证书中, 存在多个主题替代名称条目。<br />-   **重要提示：**具有强烈建议使用同一 SSL 证书跨所有节点的广告 FS 场以及所有广告 FS 场中的 Web 应用程序代理服务器。|  
|**服务通信证书：**该证书允许 WCF 消息安全的保护联合身份验证的服务器之间进行通信。|-默认情况下，SSL 证书用作服务通信证书。  但你还可以选择要将另一个证书配置为服务通信证书。<br />-   **重要提示：**如果你使用的 SSL 证书作为服务通信证书，SSL 证书到期时，请确保将续订的 SSL 证书配置为你的服务通信证书。 这不会自动发生。<br />-使用 WCF 邮件安全的广告 FS 的客户必须信任该证书。<br />-我们建议你使用的公用 \(third\-party\) 证书颁发机构颁发服务器身份验证证书 \(CA\)。<br />-服务通信证书不能使用 CNG 键的证书。<br />-可以使用广告 FS 管理控制台管理该证书。|  
|**Token\ 签名证书：**这是第标准 X509 证书用于安全地登录联合身份验证的服务器问题的所有标记。|-默认情况下，广告 FS 带有 2048 位键创建 self\ 签名证书。<br />-证书颁发 CA 也支持和使用广告 FS 管理 snap\ 中进行更改<br />-必须存储 CA 颁发的证书，和访问通过 CSP 加密提供商。<br />-不能使用 CNG 键的证书令牌签名证书。<br />广告 FS 不需要的令牌签名外部注册的证书。<br />    广告 FS 自动续订这些 self\ 签名证书过期之前, 第一次配置新证书，作为第二证书，以使合作伙伴合作，以使用它们，然后翻转到主称为自动证书翻转的过程中。我们建议你使用默认值，自动生成令牌签名证书。<br />    如果你的组织有需要不同的证书可用于令牌签名的策略，你可以在安装时，使用 Powershell 指定证书 \ （使用 Install\ AdfsFarm cmdlet\ 的 – SigningCertificateThumbprint 参数）。  安装完成后，你可以查看和管理使用广告 FS 管理控制台或 Powershell cmdlet Set\ AdfsCertificate 以及 Get\ AdfsCertificate 的令牌签名证书。<br />    使用外部注册的证书令牌签名，广告 FS 不自动续订证书或翻转执行。  由管理员，必须执行此过程。<br />    以使证书翻转一个证书靠近过期的通知时，可以在广告 FS 配置辅助令牌签名证书。 默认情况下，发布联合元数据中所有令牌签名证书，但只有主要 token\ 签名证书由广告 FS 用于实际上登录标记。|  
|**Token\ decryption\/加密证书：**这是一种标准 X509 证书，它是用于加密 decrypt\/任何传入标记。 它还会发布联盟元数据中。|-默认情况下，广告 FS 带有 2048 位键创建 self\ 签名证书。<br />-证书颁发 CA 也支持和使用广告 FS 管理 snap\ 中进行更改<br />-必须存储 CA 颁发的证书，和访问通过 CSP 加密提供商。<br />不能使用 CNG 键的证书 token\ decryption\/加密证书。<br />-默认情况下，广告 FS 生成，并使用用于令牌解密自己的、 内部生成和 self\ 签名证书。  广告 FS 不需要外部注册的证书实现此目的。<br />    此外，广告 FS 自动续订这些 self\ 签名证书之前到期。<br />    **我们建议你使用的默认情况下，用于令牌解密自动生成证书。**<br />    如果你的组织有需要不同的证书可用于令牌解密的策略，你可以在安装时，使用 Powershell 指定证书 \ （使用 Install\ AdfsFarm cmdlet\ 的 – DecryptionCertificateThumbprint 参数）。  安装完成后，你可以查看和管理使用广告 FS 管理控制台或 Powershell cmdlet Set\ AdfsCertificate 以及 Get\ AdfsCertificate 令牌解密证书。<br />    **当外部注册的证书用于令牌解密时，广告 FS 不执行证书自动续订。此过程都必须由管理员**。<br />-广告 FS 服务帐户必须在本地计算机个人应用商店中访问 token\ 签名证书专用密钥。 这是关注安装程序。 你还可以使用 snap\ 中广告 FS 管理以确保这访问，如果你以后改变 token\ 签名证书。|  
  
> [!CAUTION]  
> 用于 token\ 签名和加密 token\ decrypting\/证书至关重要联合身份验证服务的稳定性。 管理自己 token\ 签名和加密 token\ decrypting\/证书客户应确保这些证书备份好并处于空闲状态独立恢复事件。  
  
> [!NOTE]  
> 广告 FS 中，你可以更改数字签名 SHA\ 1 或 SHA\ 256 \(more secure\) 使用安全哈希算法 \(SHA\) 级别。 广告 FS 不支持使用其他哈希方法，如 MD5 证书的使用 \ （默认哈希算法，可与 Makecert.exe command\ 行 tool\）。 作为安全的最佳做法，我们建议你使用 SHA\ 256 \（这由设置 default\）所有签名。 仅在你必须交互产品不支持使用 SHA\-256，如广告 FS non\ Microsoft 产品或传统版本的通信的情况下使用建议 SHA\ 1。  
  
> [!NOTE]  
> 你收到证书从 CA 后，确保所有证书导都入到本地计算机个人证书应用商店。 你可以将证书导入到个人证书 MMC snap\ 在与应用商店。  
  
## <a name="BKMK_2"></a>硬件要求  
以下最低和推荐的硬件要求适用于 Windows Server 2012 R2 的广告 FS 联盟服务器中：  
  
||||  
|-|-|-|  
|**硬件要求**|**最低要求。**|**推荐的要求**|  
|CPU 速度|1.4 GHz 64\ 位处理器|Quad\ 核 2 g h z|  
|RAM|512 MB|4 GB|  
|磁盘空间|为 32 GB|100 GB|  
  
## <a name="BKMK_3"></a>软件要求  
以下广告 FS 要求是内置于 Windows Server® 2012 R2 操作系统服务器功能：  
  
-   必须将 Web 应用程序代理角色服务部署外部网络的访问，\-Windows Server® 2012 R2 远程访问服务器角色的一部分。 以前版本的联合身份验证的服务器代理均不支持使用在 Windows Server® 2012 R2 的广告 FS。  
  
-   无法在同一台计算机上安装的联合身份验证的服务器和 Web 应用程序代理角色服务。  
  
## <a name="BKMK_4"></a>广告 DS 要求  
**域控制器要求**  
  
所有用户域和广告 FS 服务器加入的域中的域控制器必须将运行 Windows Server 2008 或更高版本。  
  
> [!NOTE]  
> 环境与 Windows Server 2003 该域控制器的所有支持将在外延支持都结束日期为 Windows Server 2003 都结束。 强烈建议客户升级与其域控制器尽快。 访问[本页](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)以获取有关 Microsoft 支持生命周期。 发现的问题，可以是特定于 Windows Server 2003 域控制器环境中，修复将仅针对安全问题并修复如果可以发出之前的外延支持的 Windows Server 2003 到期。  
  
**域 functional\ 级别要求**  
  
在 Windows Server 2003 或更高版本的功能域级别必须运行域中的所有用户帐户和广告 FS 服务器加入的域。  
  
大多数广告 FS 功能确实需要广告 DS functional\ 级别修改成功运营。 但是，Windows Server 2008 域功能层，或更高版本，才能进行客户端证书身份验证的证书显式映射到广告 DS 中的用户帐户如果成功运营。  
  
**方案要求**  
  
-   广告 FS 不需要方案更改或广告 DS functional\ 级别改动。  
  
-   若要使用加入工作区的功能，必须为 Windows Server 2012 R2 设置的广告 FS 服务器将加入森林模式。  
  
**服务帐户要求**  
  
-   任何服务标准帐户可用作广告 FS 服务帐户。 组托管服务帐户也会受支持。 这要求在至少有一个域控制器 \ (建议你部署的两个或 more\) 运行 Windows Server 2012 或更高版本。  
  
-   为 Kerberos 到 domain\ 加入客户端与广告 FS 之间的身份验证 HOST\ / < adfs\_service\_name > 必须注册服务帐户 SPN 为。 默认情况下，广告 FS 时将配置这创建新的广告 FS 电场的日落，如果有足够的权限来执行此操作。  
  
-   广告 FS 服务帐户必须信任包含用户身份验证广告 FS 服务每个用户域。  
  
**域要求**  
  
-   所有广告 FS 服务器必须都是加入广告 DS 域。  
  
-   必须在单个域部署电场的日落中的所有广告 FS 服务器。  
  
-   广告 FS 服务器将加入的域必须信任包含用户身份验证广告 FS 服务的每个用户帐户域。  
  
**多森林要求**  
  
-   每个用户帐户域或森林包含用户对广告 FS 服务进行身份验证，则必须信任广告 FS 服务器将加入的域。  
  
-   广告 FS 服务帐户必须信任包含用户身份验证广告 FS 服务每个用户域。  
  
## <a name="BKMK_5"></a>配置数据库要求  
以下是要求和限制适用根据配置官方商城的类型：  
  
**WID**  
  
-   如果你有 100 或更少信赖的方信任，WID 场具有最多 30 联合身份验证的服务器。  
  
-   在 SAML 2.0 项目分辨率配置文件不支持 WID 配置数据库中。  标记重播检测 WID 配置数据库中不支持。 仅在其中广告是充当联盟提供商和对消耗安全令牌来自外部索赔提供商的方案仅使用此功能。  
  
-   部署广告 FS 服务器，在不同的数据中心故障转移或地理负载平衡只要服务器数不超过 30 受支持。  
  
下表提供了使用 WID 场摘要。  使用该计划实现。  
  
||||  
|-|-|-|  
||1 \-100 RP 信任|超过 100 RP 信任|  
|1 \-30年广告 FS 节点|受支持的 WID|不支持使用 WID \-所需的 SQL|  
|超过 30年广告 FS 节点|不支持使用 WID \-所需的 SQL|不支持使用 WID \-所需的 SQL|  
  
**SQL Server**  
  
对于在 Windows Server 2012 R2 的广告 FS，你可以使用 SQL Server 2008 和更高版本  
  
## <a name="BKMK_6"></a>浏览器要求  
通过浏览器或浏览器控件执行广告 FS 身份验证，当你的浏览器必须遵守以下要求：  
  
-   JavaScript 必须为启用状态  
  
-   Cookie 必须处于打开状态  
  
-   必须支持服务器名称指示 \(SNI\)  
  
-   为用户证书和设备证书身份验证 \ (工作区加入功能 \，) 在浏览器必须支持 SSL 客户证书身份验证  
  
几个键的浏览器和平台已经过验证呈现和功能的详细信息，其中如下所示。 如果他们满足上面列出的要求，仍支持浏览器和不涵盖此表中的设备：  
  
|||  
|-|-|  
|**浏览器**|**平台**|  
|IE 10.0|Windows 7、 Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012 R2|  
|IE 11.0|Windows7 作为、 Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012 R2|  
|Windows Web 身份验证的代理|Windows 8.1|  
|Firefox \[v21\]|Windows 7、 Windows 8.1|  
|Safari \[v7\]|iOS 6 Mac OS\ X 10.7|  
|Chrome \[v27\]|Windows 7、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2、 Mac OS\ X 10.7|  
  
> [!IMPORTANT]  
> 已知问题 \-Firefox： 标识使用设备的证书设备的工作区加入功能不能在 Windows 平台上正常工作。 Firefox 当前不支持使用证书预配到 Windows 客户端用户证书应用商店的性能 SSL 客户证书身份验证。  
  
**Cookie**  
  
广告 FS 创建 session\ 基于和持续必须存储 sign\ 中提供的客户端计算机、 sign\ 出、 单个 sign\ 上 \(SSO\)，以及其他功能的 cookie。 因此，客户端浏览必须配置接受 cookie。 用于身份验证的 cookie 始终都目标读者为起始服务器的安全超文本传输协议 \(HTTPS\) 会话 cookie。 如果没有配置客户端浏览器，允许这些 cookie，广告 FS 无法正常工作。 永久性 cookie 用于保留的索赔提供商的用户选择。 您可以通过使用页面中 sign\ 广告 FS 配置文件中的配置设置来禁用它们。 SSL TLS\ 月的支持，才能出于安全原因。  
  
## <a name="BKMK_extranet"></a>联网要求  
为了提供外部广告 FS 服务的访问，你必须部署 Web 应用程序代理角色服务以联网的面向角色该代理服务器身份验证请求广告 FS 服务安全的方式。 这将提供的广告 FS 服务端点隔离以及的所有安全密钥隔离 \ （如令牌签名 certificates\) 从来自 internet 的请求。 此外，功能，例如软外部帐户锁定需要使用的 Web 应用程序代理。 有关 Web 应用程序代理服务器的详细信息，请参阅[Web 应用程序代理](https://technet.microsoft.com/library/dn584107.aspx)。  
  
如果你想要使用 third\ 方代理外部网络访问，此 third\ 方代理必须支持中定义的协议[http:///\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf)。  
  
## <a name="BKMK_7"></a>网络要求  
相应地配置以下网络服务至关重要的成功部署在你的组织的广告 FS:  
  
**配置公司防火墙**  
  
必须 TCP 端口 443 启用了防火墙位于 Web 应用程序代理和联合身份验证的服务器场之间和客户端和 Web 应用程序代理之间防火墙站。  
  
此外，如果客户端用户证书身份验证 \ (clientTLS 身份验证使用 X509 用户 certificates\) 是在 Windows Server 2012 R2 的广告 FS 要求，需要启用 TCP 端口 49443 客户端和 Web 应用程序代理之间防火墙上的入站。 这不是需要的 Web 应用程序代理和联盟 servers\ 之间防火墙）。  
  
**配置 DNS**  
  
-   必须 intranet 访问公司内部网络 \(intranet\) 内的广告 FS 服务的所有客户端将无法解决广告 FS 服务名称 \ （SSL certificate\ 所提供的姓名） 到负载平衡广告 FS 服务器或广告 FS 服务器。  
  
-   必须外部网络的访问，访问广告 FS 服务的企业网络 \(extranet\/internet\) 之外的所有客户端将无法解决广告 FS 服务名称 \ （SSL certificate\ 所提供的姓名） 到负载平衡 Web 应用程序代理服务器或的 Web 应用程序代理服务器。  
  
-   外部网络访问权限才能正常工作，每个 DMZ 中的 Web 应用程序代理服务器必须是可解决广告 FS 服务名称 \ （SSL certificate\ 所提供的姓名） 到负载平衡广告 FS 服务器或广告 FS 服务器。 这可以实现在 DMZ 网络或通过更改使用主机文件的本地服务器分辨率使用备用 DNS 服务器。  
  
-   以供验证 Windows 集成，才能网络内外子集端点公开通过 Web 应用程序代理服务器的网络，你必须使用 A 记录 \(not CNAME\) 指向负载平衡。  
  
配置为联合身份验证服务的公司 DNS 和设备注册服务的信息，请参阅[联合身份验证服务和 DRS 配置公司 DNS](https://technet.microsoft.com/library/dn486786.aspx)。  
  
为 Web 应用程序代理配置公司 DNS 的信息，请参阅"配置 DNS"部分中的[第 1 步： 将 Web 应用程序代理基础结构配置](https://technet.microsoft.com/library/dn383644.aspx)。  
  
有关如何群集 FQDN 使用 NLB 或配置群集 IP 地址的信息，请参阅指定在群集参数[http:///\/go.microsoft.com\/fwlink\/？LinkId\ = 75282](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
## <a name="BKMK_8"></a>特性官方商城要求  
广告 FS 需要至少一个特性官方商城的用户身份验证，并提取安全提起的索赔这些用户使用。 有关的特性列表存储广告 FS 支持，请参阅[角色的特性存储](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
> [!NOTE]  
> 默认情况下，广告 FS 会自动创建"Active Directory"特性应用商店。 特性官方商城要求取决于你的组织正在充当帐户合作伙伴 \ （举办联盟的 users\） 或资源合作伙伴 \ （举办联盟的 application\）。  
  
**LDAP 特性官方商城**  
  
当你使用其他便捷目录访问协议 \ (LDAP\) \-based 特性官方商城，您必须连接到支持集成 Windows 身份验证 LDAP 服务器。 RFC 2255 中所述，还必须 LDAP URL，格式编写 LDAP 连接字符串。  
  
这种鼠类还需要广告 FS 服务的服务帐户有权检索 LDAP 特性应用商店中的用户信息。  
  
**SQL Server 特性官方商城**  
  
在 Windows Server 2012 R2 成功运营广告 FS，对于主机 SQL Server 特性官方商城的计算机必须运行任一 Microsoft SQL Server 2008 或更高版本。 当你使用的 SQL\ 基于属性官方商城时，您还必须配置连接字符串。  
  
**自定义特性官方商城**  
  
你可以开发自定义特性官方商城启用高级的方案。  
  
-   内置广告 FS 策略语言可以参考自定义特性官方商城，以便可以增强任何以下情形：  
  
    -   创建本地经过身份验证的用户的声明  
  
    -   补充进行外部身份验证的用户的声明  
  
    -   在授权的用户，若要获取标记  
  
    -   在授权服务获取的用户的行为标记  
  
    -   发出安全令牌广告 FS 发给信赖方中的其他数据。  
  
-   必须内置所有自定义特性官方商城，在.NET 4.0 顶部或更高版本。  
  
当你使用的自定义特性应用商店中时，你可能需要配置连接字符串。 在此情况下，你可以输入自定义你选择允许你自定义特性官方商城的连接的代码。 在此情况下连接字符串是 name\ 中的值对解释实现开发人员的自定义特性官方商城的一组。有关开发和使用自定义特性官方商城的详细信息，请参阅[属性官方商城概述](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="BKMK_9"></a>应用程序要求  
广告 FS 支持 claims\ 感知使用以下协议的应用程序：  
  
-   WS\ 联盟  
  
-   WS\ 信任  
  
-   使用 IDPLite、 SPLite 和 eGov1.5 配置文件 SAML 2.0 协议。  
  
-   OAuth 2.0 授权授予配置文件  
  
广告 FS 还支持身份验证和授权的任何 non\ claims\ 注意的应用程序受 Web 应用程序代理。  
  
## <a name="BKMK_10"></a>身份验证要求  
**广告 DS \(Primary Authentication\) 身份验证**  
  
支持以下标准身份验证机制广告 ds intranet 访问：  
  
-   Windows 使用适用于 Kerberos 和 NTLM 协商集成身份验证  
  
-   使用 username\ 中的密码窗体身份验证  
  
-   使用证书映射到广告 DS 中的用户帐户的身份验证证书  
  
支持以下身份验证机制外部网络的访问:  
  
-   使用 username\ 中的密码窗体身份验证  
  
-   使用证书映射到广告 DS 中的用户帐户的身份验证证书  
  
-   Windows 使用接受 Windows 的集成身份验证的 WS\ 信任端点协商 \(NTLM only\) 的身份验证集成。  
  
为证书身份验证：  
  
-   适用于可 pin 受保护的智能卡。  
  
-   用户可以输入其 pin GUI 不由广告 FS 提供，并且需要使用客户端 TLS 时，将显示客户端操作系统的一部分。  
  
-   阅读器和加密的服务提供商的智能卡 \(CSP\) 必须工作浏览器的计算机上。  
  
-   智能卡证书必须为受信任的所有广告 FS 服务器和 Web 应用程序代理服务器根链接。  
  
-   证书必须映射到广告 DS 中的用户帐户，通过以下方法之一：  
  
    -   证书主题名称与广告 DS 中的用户帐户 LDAP 辨别名称相对应。  
  
    -   证书主题 altname 扩展已用户主要命名 \(UPN\) 广告 DS 的用户帐户。  
  
无缝 Windows 的集成身份验证的 intranet，在使用 Kerberos  
  
-   这是必需的服务名为受信任的站点或本地 Intranet 站点的一部分。  
  
-   此外，HOST\ / < adfs\_service\_name > SPN 必须广告 FS 场上运行的服务帐户设置。  
  
**Multi\ 双因素身份验证**  
  
广告 FS 支持额外的身份验证 \ （超出依靠广告 DS\ 支持的主要身份验证） 使用提供程序型号，vendors\ 中的客户可以版本管理员可以注册并登录时使用他们自己 multi\ 双因素身份验证适配器的由此。  
  
在顶部.NET 4.5，必须生成的每个 MFA 适配器。  
  
MFA 的详细信息，请参阅[管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
**设备身份验证**  
  
广告 FS 支持使用在加入他们的设备最终用户工作区法案通过注册设备服务预配证书的设备身份验证。  
  
## <a name="BKMK_11"></a>工作区加入要求  
最终用户可以使用广告 FS 组织为其设备的工作区加入。 设备注册服务广告 FS 中支持此功能。 因此，最终用户获取 SSO 跨应用程序受广告 FS 的其他好处。 此外，管理员可以通过限制对仅向已加入组织的工作区的设备的应用程序访问管理风险。 以下是启用此项 scenario 以下要求。  
  
-   广告 FS 支持的 Windows 8.1 和 iOS 设备 5\ + 工作区加入  
  
-   若要使用加入工作区的功能，将加入广告 FS 服务器森林架构必须是 Windows Server 2012 R2。  
  
-   者备用名称 SSL 证书广告 FS 服务必须包含跟的你的组织，例如，enterpriseregistration.corp.contoso.com 用户主要名称 \(UPN\) 职务值 enterpriseregistration。  
  
## <a name="BKMK_12"></a>密码系统要求  
下表提供有关广告 FS 令牌签名，标记的 encryption\ 解密功能的其他加密技术支持信息：  
  
||||  
|-|-|-|  
|**算法**|**关键长度**|**Protocols\ 月 Applications\/评论**|  
|TripleDES – 默认 192 \ (受支持的 192 – 256\) \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|加密安全标记的算法支持。|  
|AES128 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes128\-cbc|128|加密安全标记的算法支持。|  
|AES192 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes192\-cbc|192|加密安全标记的算法支持。|  
|AES256 \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**默认**。 加密安全标记的算法支持。|  
|TripleDESKeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-tripledes|.NET 4.0\+ 支持的所有键大小|支持算法加密安全标记进行加密密钥。|  
|AES128KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|支持算法加密安全标记进行加密密钥。|  
|AES192KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|支持算法加密安全标记进行加密密钥。|  
|AES256KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|支持算法加密安全标记进行加密密钥。|  
|RsaV15KeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-1\_5|1024|支持算法加密安全标记进行加密密钥。|  
|RsaOaepKeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|默认情况。 支持算法加密安全标记进行加密密钥。|  
|SHA1\ http: / / \ / www.w3.org \/PICS\/DSig\/SHA1\_1\_0.html|N\/A|在项目源 Id 代由广告 FS 服务器： 在此情况下，STS 使用 SHA1 \ （每个在 SAML 2.0 standard\ 建议） 创建项目源 Id 的简短 160 位值。<br /><br />ADFS web 代理还使用 \ （传统组件 WS2003 timeframe\ 从） 以在"最近更新"的时间内发现更改值，以使它知道时更新 STS 从的信息。|  
|SHA1withRSA\-<br /><br />http:///\/ www.w3.org \/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|当广告 FS 服务器验证 SAML AuthenticationRequest 签名，则使用情况下，登录项目分辨率请求或响应、 创建 token\ 签名证书。<br /><br />在这些情况下，SHA256 默认情况下，并且如果合作伙伴 \(relying party\) 不支持 SHA256，必须使用 SHA1 仅用于 SHA1。|  
  
## <a name="BKMK_13"></a>权限要求  
执行安装和广告 FS 初始配置管理员必须域管理员权限在地域 \ (换句话说的联合身份验证的服务器加入的域。 \)  
  
## <a name="see-also"></a>请参阅  
[在 Windows Server 2012 R2 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

