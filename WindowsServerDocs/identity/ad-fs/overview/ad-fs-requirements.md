---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: AD FS 2016 要求
description: 安装 Active Directory 联合身份验证服务的要求。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e62032533b15ec3d93896d242273612faafdca58
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444100"
---
# <a name="ad-fs-requirements"></a>AD FS 要求



有关部署 AD FS 的要求如下：  
  
-   [证书要求](ad-fs-requirements.md#BKMK_1)  
  
-   [硬件要求](ad-fs-requirements.md#BKMK_2)  
  
-   [代理服务器要求](ad-fs-requirements.md#BKMK_3)  
  
-   [AD DS 要求](ad-fs-requirements.md#BKMK_4)  
  
-   [配置数据库要求](ad-fs-requirements.md#BKMK_5)  
  
-   [浏览器要求](ad-fs-requirements.md#BKMK_6)  

-   [网络要求](ad-fs-requirements.md#BKMK_7)  
  
-   [权限要求](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>证书要求  
  
### <a name="ssl-certificates"></a>SSL 证书

每个 AD FS 和 Web 应用程序代理服务器具有 SSL 证书，为服务联合身份验证服务的 HTTPS 请求。  Web 应用程序代理可以为发布的应用程序请求提供服务的其他 SSL 证书。

**建议：** 所有 AD FS 联合身份验证服务器和 Web 应用程序代理都使用相同的 SSL 证书。 

**要求：**

联合身份验证服务器上的 SSL 证书必须满足以下要求
- 证书是公开的受信任 （适用于生产部署）
- 证书包含服务器身份验证增强型密钥用法 (EKU) 值
- 证书包含联合身份验证服务名称，例如"fs.contoso.com"中的使用者或使用者可选名称 (SAN)
- 端口 443 上的用户证书身份验证，证书包含"certauth。\<联合身份验证服务名称\>"，如 SAN 中的"certauth.fs.contoso.com"
- SAN 设备注册或使用 windows 10 之前的客户端的本地资源到新式身份验证，必须包含"enterpriseregistration。\<upn 后缀\>"为你的组织中使用每个 UPN 后缀。

在 Web 应用程序代理服务器上的 SSL 证书必须满足以下要求
- 如果为使用的代理使用 Windows 集成身份验证，代理 SSL 证书的 AD FS 代理请求必须相同 （使用相同的密钥） 与联合身份验证服务器 SSL 证书
- 如果 AD FS 属性"ExtendedProtectionTokenCheck"是启用 （默认设置中的 AD FS），代理 SSL 证书必须相同 （使用相同的密钥） 与联合身份验证服务器 SSL 证书
- 否则，代理 SSL 证书的要求将相同的联合身份验证服务器 SSL 证书

### <a name="service-communication-certificate"></a>服务通信证书
此证书不需要用于大多数 AD FS 方案，包括 Azure AD 和 Office 365。 默认情况下，AD FS 配置在初始配置与服务通信证书时提供的 SSL 证书。

**建议：**
- 使用相同的证书，如使用的 SSL。  

### <a name="token-signing-certificate"></a>令牌签名证书
使用此证书进行签名的信赖方颁发的令牌，以便信赖方应用程序必须能够识别证书及其关联的密钥作为已知和受信任。 当令牌签名证书更改，例如当它过期时和在配置新的证书时，必须更新所有信赖方。

**建议：** 使用 AD FS 默认情况下在内部生成的自签名令牌签名证书。  

**要求：**
- 如果你的组织需要从企业 PKI 的证书可用于令牌签名，这可以使用 Install-adfsfarm cmdlet 的 SigningCertificateThumbprint 参数。
- 使用在内部生成的默认证书还是外部已注册的证书，当您更改令牌签名证书时必须确保使用新的证书信息更新所有信赖方。  否则，不会更新任何依赖方的登录将失败。

### <a name="token-encryptingdecrypting-certificate"></a>令牌加密/解密证书
由加密令牌颁发给 AD FS 声明提供程序使用此证书。

**建议：** 使用 AD FS 默认情况下在内部生成的自签名令牌解密证书。  

**要求：**
- 如果你的组织需要从企业 PKI 的证书可用于令牌签名，这可以使用 Install-adfsfarm cmdlet 的 DecryptingCertificateThumbprint 参数。
- 使用在内部生成的默认证书还是外部已注册的证书，当您更改令牌解密证书时必须确保使用新的证书信息更新所有声明提供程序。  否则，登录使用任何声明提供程序不会更新将失败。
  
> [!CAUTION]  
> 用于令牌的证书\-签名和令牌\-解密\/加密对联合身份验证服务的稳定性至关重要。 管理他们自己的令牌的客户\-签名和令牌\-解密\/加密证书应确保这些证书进行备份，并且可独立恢复事件期间。  

### <a name="user-certificates"></a>用户证书
- 当使用的 x509 用户证书身份验证使用 AD FS 时，所有用户证书必须链接到受 AD FS 和 Web 应用程序代理服务器的根证书颁发机构。

## <a name="BKMK_2"></a>硬件要求  
AD FS 和 Web 应用程序代理硬件要求 （物理或虚拟） 上 CPU、 网关，因此你应该调整你的处理能力的场的大小。  
- 使用[AD FS 2016 容量规划电子表格](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)来确定你将需要的 AD FS 和 Web 应用程序代理服务器的数目。

适用于 AD FS 的内存和磁盘要求是相当静态的请参阅下表：


|**硬件要求**|**最低要求**|**建议的要求**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|磁盘空间|32 GB|100 GB |

**SQL Server 的硬件要求**

如果将 SQL Server 用于你的 AD FS 配置数据库，大小的最基本的 SQL Server 建议根据 SQL Server。  AD FS 数据库大小为非常小，并且 AD FS 不会将数据库实例上的大量的处理负载。  AD FS，但是，在连接到数据库多次身份验证，因此应可靠的网络连接。  遗憾的是，SQL Azure 不支持 AD FS 配置数据库。
  
## <a name="BKMK_3"></a>代理服务器要求  
  
-   必须将 Web 应用程序代理角色服务部署为 extranet 访问\-远程访问服务器角色的一部分。 

-   第三方代理必须支持[MS ADFSPIP 协议](https://msdn.microsoft.com/en-us/library/dn392811.aspx)作为 AD FS 代理支持。  列表的第三方供应商请参阅[常见问题解答](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip)。

-   AD FS 2016 要求 Windows Server 2016 上的 Web 应用程序代理服务器。  下层代理不能配置为在 2016年场行为级别运行的 AD FS 2016 场。
  
-   不能在同一台计算机上安装联合身份验证服务器和 Web 应用程序代理角色服务。  
  
## <a name="BKMK_4"></a>AD DS 要求  
**域控制器要求**  
  
- AD FS 要求运行 Windows Server 2008 或更高版本的域控制器。

- Microsoft Passport for Work 需要至少一个 Windows Server 2016 域控制器。
  
> [!NOTE]  
> 使用 Windows Server 2003 域控制器的环境的所有支持已都结束。 请访问[本页](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)有关 Microsoft 支持生命周期的其他信息。  
  
**域功能\-要求**  
  
 - 所有用户帐户域和 AD FS 服务器所加入的域必须运行在域功能级别的 Windows Server 2003 或更高版本。  
  
 - Windows Server 2008 域功能级别或更高版本才需要客户端证书身份验证的证书显式映射到用户的帐户在 AD DS 中。  
  
**架构要求**  
  
-   AD FS 2016 的全新安装需要 Active Directory 2016 架构 （最低版本 85）。

-   到 2016年级别提高 AD FS 场行为级别 (FBL) 需要 Active Directory 2016 架构 （最低版本 85）。  

  
**服务帐户要求**  
  
-   任何标准的域帐户可以用作 AD FS 服务帐户。 此外支持组托管服务帐户。 配置 AD FS 时，将自动添加在运行时所需的权限。

-   用户权限分配所需的 AD 服务帐户是作为服务登录

-   NT Service\adfssrv 和 NT Service\drs 所需的用户权限分配为生成安全审核和作为服务登录。

-   组托管服务帐户需要至少一个域控制器运行 Windows Server 2012 或更高版本。  GMSA 必须 live 下默认值 CN = 托管服务帐户的容器。  

-   对于 Kerberos 身份验证，服务主体名称`HOST/<adfs\_service\_name>`必须在 AD FS 服务帐户上注册。 默认情况下，AD FS 时将要配置这创建新的 AD FS 场。  如果此操作失败，如对于冲突或没有足够的权限，您将看到一条警告，并应将其手动添加。  
   
**域要求**  
  
-   所有 AD FS 服务器都必须加入到 AD DS 域。  
  
-   必须在同一个域中部署所有 AD FS 服务器场中。  
   
**多林要求**  
  
-   AD FS 服务器所加入的域必须信任每个域或林中包含对 AD FS 服务进行身份验证的用户。  

-   AD FS 服务帐户是的成员的林必须信任所有用户登录林状结构。 
  
-   AD FS 服务帐户必须有权读取每个域都包含对 AD FS 服务的用户进行身份验证中的用户属性。  
  
## <a name="BKMK_5"></a>配置数据库要求  
本部分介绍的要求和 AD FS 场，分别使用 Windows 内部数据库 (WID) 或 SQL Server 作为数据库的限制：  
  
**WID**  
  
-   WID 场中不支持 SAML 2.0 的项目解析配置文件。    

-   令牌重放检测是不受支持的 WID 场。 （此功能仅用于仅在 AD FS 作为联合身份验证提供程序并且使用的安全令牌的外部声明提供程序的情况下。）  
  
下表提供了多少个 AD FS 服务器的摘要在 WID vs 中支持的 SQL Server 场。    
  
  
|| 1-100 的信赖方 (RP) 信任 AD FS 配置 | 超过 100 个 RP 信任配置  |
| --- |--- | --- |
|1-30年个 AD FS 服务器|WID 支持|不支持使用 WID 的所需的 SQL Server |
|30 个以上 AD FS 服务器|不支持使用 WID 的所需的 SQL Server|不支持使用 WID 的所需的 SQL Server  
  
**SQL Server**  
  
- 对于 Windows Server 2016 中的 AD FS，支持 SQL Server 2008 和更高版本。

- 在 SQL Server 场中支持 SAML 项目解析和令牌重放检测。  
  
## <a name="BKMK_6"></a>浏览器要求  
通过浏览器或浏览器控件执行 AD FS 身份验证时，你的浏览器必须符合以下要求：  
  
- 必须启用 JavaScript  
  
- 对于单一登录，必须配置客户端浏览器为允许 cookie  
  
- 服务器名称指示\(SNI\)必须支持  
  
- 对于用户证书和设备证书身份验证，在浏览器必须支持 SSL 客户端证书身份验证  

- 以进行无缝登录使用 Windows 集成身份验证，联合身份验证服务名称 (例如 https:\/\/fs.contoso.com) 必须在本地 intranet 区域或受信任的站点区域中配置。
  ## <a name="BKMK_7"></a>网络要求  
 
**防火墙要求**  
  
这两个防火墙位于 Web 应用程序代理和联合服务器场和客户端和 Web 应用程序代理之间的防火墙之间必须具有 TCP 端口 443 启用入站。  
  
此外，如果客户端用户证书身份验证\(进行 clientTLS 身份验证使用 X509 用户证书\)是必需的和未启用端口 443 上的 certauth 终结点，AD FS 2016 要求启用 TCP 端口 49443在客户端和 Web 应用程序代理之间的防火墙上的入站。 这不需要 Web 应用程序代理和联合身份验证服务器之间的防火墙上\)。 

有关其他信息混合端口要求，请参阅[混合标识端口和协议](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports)。 

有关其他信息，请参阅[保护 Active Directory 联合身份验证服务的最佳做法](../deployment/Best-Practices-Securing-AD-FS.md)
  
**DNS 要求**  
  
-   对于 intranet 访问，所有客户端访问 AD FS 服务内部企业网络中\(intranet\)必须能够解析 AD FS 服务名称为 AD FS 服务器或 AD FS 服务器的负载均衡器。  
  
-   为 extranet 访问，所有客户端访问 AD FS 服务从公司网络外部\(extranet\/internet\)必须能够解析 AD FS 服务名称为 Web 应用程序代理服务器的负载均衡器或Web 应用程序代理服务器。  
  
-   外围网络中的每个 Web 应用程序代理服务器必须能够解析 AD FS 服务名称为 AD FS 服务器或 AD FS 服务器的负载均衡器。 这可以实现使用备用 DNS 服务器在外围网络中或通过更改使用的主机文件的本地服务器解析。  
  
-   对于 Windows 集成身份验证，必须使用一条 DNS A 记录\(不是 CNAME\)联合身份验证服务名称。  

-   在端口 443，"certauth 用户证书身份验证。\<联合身份验证服务名称\>"必须在 DNS 中解析为联合身份验证服务器或 web 应用程序代理配置。

-   为设备注册或为进行新式验证到本地资源使用 windows 10 之前的客户端，"enterpriseregistration。\<upn 后缀\>"，必须配置你的组织中正在使用的每个 UPN 后缀将解析为联合身份验证服务器或 web 应用程序代理。

**负载均衡器要求**
- 负载均衡器都不能终止 SSL。 AD FS 支持使用证书身份验证，这将中断时终止 SSL 的多个用例。 任何用例不支持在负载平衡器处终止 SSL。 
- 建议使用负载均衡器支持 SNI。 在事件不是，请使用回退中的 AD FS 上的绑定 0.0.0.0 / Web 应用程序代理服务器应提供一种解决方法。
- 建议使用 HTTP (不是 HTTPS) 运行状况探测终结点来路由流量执行负载均衡器运行状况检查。 这样可以避免与 SNI 相关的任何问题。 对这些探测终结点的响应为 HTTP 200 正常和不依赖于后端服务与本地提供服务。 可以通过使用路径 / adfs/探测的 HTTP 访问 HTTP 探测
    - http://&lt;Web 应用程序代理名称 &gt; /adfs/探测
    - http://&lt;ADFS 服务器名称 &gt; /adfs/探测
    - http://&lt;Web 应用程序代理 IP 地址 &gt; /adfs/探测
    - http://&lt;ADFS IP 地址 &gt; /adfs/探测
- 不建议使用 DNS 轮循机制作为一种方式进行负载平衡。 使用此类型的负载平衡不提供自动化的方式来从负载均衡器使用运行状况探测中删除节点。 
- 建议不要对负载均衡器中的 AD fs 身份验证流量使用基于 IP 会话相关性或粘性会话。 使用传统的身份验证协议的邮件客户端连接到 Office 365 邮件服务 (Exchange Online）) 时，这可能会导致某些节点的重载。 

## <a name="BKMK_13"></a>权限要求  
执行安装和初始配置的 AD FS 管理员必须在 AD FS 服务器上具有本地管理员权限。  如果本地管理员不具有在 Active Directory 中创建对象的权限，他们首先必须具有域管理员创建所需的 AD 对象，然后配置 AD FS 场使用 AdminConfiguration 参数。  
  
  

