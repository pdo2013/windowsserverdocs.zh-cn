---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: AD FS 2016 要求
description: 安装 Active Directory 联合身份验证服务的要求。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c8ab160699bc6a961f4fbed6c58cf072a395a313
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407419"
---
# <a name="ad-fs-requirements"></a>AD FS 要求



部署 AD FS 的要求如下：  
  
-   [证书要求](ad-fs-requirements.md#BKMK_1)  
  
-   [硬件要求](ad-fs-requirements.md#BKMK_2)  
  
-   [代理要求](ad-fs-requirements.md#BKMK_3)  
  
-   [AD DS 要求](ad-fs-requirements.md#BKMK_4)  
  
-   [配置数据库要求](ad-fs-requirements.md#BKMK_5)  
  
-   [浏览器要求](ad-fs-requirements.md#BKMK_6)  

-   [网络要求](ad-fs-requirements.md#BKMK_7)  
  
-   [权限要求](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>证书要求  
  
### <a name="ssl-certificates"></a>SSL 证书

每个 AD FS 和 Web 应用程序代理服务器都有一个 SSL 证书，用于向联合身份验证服务发送 HTTPS 请求。  Web 应用程序代理可以有其他 SSL 证书来向发布的应用程序发送请求。

**推荐**对所有 AD FS 联合服务器和 Web 应用程序代理使用相同的 SSL 证书。 

**要求**

联合服务器上的 SSL 证书必须满足以下要求
- 证书已公开信任（适用于生产部署）
- 证书包含服务器身份验证增强型密钥用法（EKU）值
- 证书包含联合身份验证服务名称，如使用者或使用者备用名称（SAN）中的 "fs.contoso.com"
- 对于端口443上的用户证书身份验证，证书包含 "certauth"。联合身份验证\>服务名称 "，如 SAN 中的" certauth.fs.contoso.com " \<
- 若要使用 Windows 10 客户端在本地资源上注册设备或使用新式身份验证，SAN 必须包含 "enterpriseregistration"。\<upn后缀\>"，以便在组织中使用的每个 upn 后缀。

Web 应用程序代理上的 SSL 证书必须满足以下要求
- 如果代理用于代理使用 Windows 集成身份验证的 AD FS 请求，则代理 SSL 证书必须与联合服务器 SSL 证书相同（使用相同密钥）
- 如果启用了 AD FS 属性 "ExtendedProtectionTokenCheck" （AD FS 中的默认设置），则代理 SSL 证书必须与联合服务器 SSL 证书相同（使用相同的密钥）
- 否则，代理 SSL 证书的要求与联合服务器 SSL 证书的要求相同

### <a name="service-communication-certificate"></a>服务通信证书
大多数 AD FS 方案（包括 Azure AD 和 Office 365）都不需要此证书。 默认情况下，AD FS 会将初始配置提供的 SSL 证书配置为服务通信证书。

**推荐**
- 使用与用于 SSL 相同的证书。  

### <a name="token-signing-certificate"></a>令牌签名证书
此证书用于向信赖方签署颁发的令牌，因此信赖方应用程序必须识别证书，并将其关联密钥识别为已知和可信。 当令牌签名证书更改时（例如，当证书过期且你配置新证书时），必须更新所有信赖方。

**推荐**使用 AD FS 默认的、内部生成的自签名令牌签名证书。  

**要求**
- 如果你的组织要求使用企业 PKI 中的证书进行令牌签名，则可以使用 Install-adfsfarm cmdlet 的 SigningCertificateThumbprint 参数来完成此操作。
- 无论你使用默认的内部生成的证书还是外部注册的证书，当令牌签名证书更改时，你必须确保使用新的证书信息更新所有信赖方。  否则，将无法登录到任何未更新的信赖方。

### <a name="token-encryptingdecrypting-certificate"></a>令牌加密/解密证书
此证书由加密颁发给 AD FS 的令牌的声明提供程序使用。

**推荐**使用 AD FS 默认的内部生成的自签名令牌解密证书。  

**要求**
- 如果你的组织要求使用企业 PKI 中的证书进行令牌签名，则可以使用 Install-adfsfarm cmdlet 的 DecryptingCertificateThumbprint 参数来完成此操作。
- 无论你使用默认的内部生成的证书还是外部注册的证书，当令牌解密证书发生更改时，你必须确保使用新证书信息更新所有声明提供程序。  否则，使用未更新的任何声明提供程序的登录将会失败。
  
> [!CAUTION]  
> 用于令牌\-签名和令牌\-解密\/加密的证书对于联合身份验证服务的稳定性至关重要。 如果客户管理其自己\-的令牌签名\-&\/令牌对加密证书进行解密，则应确保这些证书已备份，并在恢复事件期间单独提供。  

### <a name="user-certificates"></a>用户证书
- 将 x509 用户证书身份验证用于 AD FS 时，所有用户证书必须链接到 AD FS 和 Web 应用程序代理服务器信任的根证书颁发机构。

## <a name="BKMK_2"></a>硬件要求  
AD FS 和 Web 应用程序代理硬件要求（物理或虚拟）都是在 CPU 上封闭的，因此，你应该为你的场提供处理能力。  
- 使用[AD FS 2016 容量规划电子表格](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)来确定所需的 AD FS 和 Web 应用程序代理服务器的数量。

AD FS 的内存和磁盘要求非常静态，请参阅下表：


|**硬件要求**|**最低要求**|**建议的要求**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|磁盘空间|32 GB|100 GB |

**SQL Server 硬件要求**

如果为 AD FS 配置数据库使用 SQL Server，请根据最基本的 SQL Server 建议调整 SQL Server 大小。  AD FS 的数据库大小非常小，AD FS 不会对数据库实例进行大量处理负载。  但 AD FS 会在身份验证过程中多次连接到数据库，因此网络连接应是可靠的。  遗憾的是，AD FS 配置数据库不支持 SQL Azure。
  
## <a name="BKMK_3"></a>代理要求  
  
-   对于 extranet 访问，你必须部署远程访问服务器角色的 " \- Web 应用程序代理" 角色服务部分。 

-   第三方代理必须支持将[ADFSPIP 协议](https://msdn.microsoft.com/library/dn392811.aspx)作为 AD FS 代理来支持。  有关第三方供应商的列表，请参阅[常见问题解答](AD-FS-FAQ.md)。

-   AD FS 2016 需要 Windows Server 2016 上的 Web 应用程序代理服务器。  不能为在2016场行为级别运行的 AD FS 2016 场配置下级代理。
  
-   不能在同一台计算机上安装联合服务器和 Web 应用程序代理角色服务。  
  
## <a name="BKMK_4"></a>AD DS 要求  
**域控制器要求**  
  
- AD FS 要求域控制器运行 Windows Server 2008 或更高版本。

- Microsoft Passport for Work 至少需要一个 Windows Server 2016 域控制器。
  
> [!NOTE]  
> 对 Windows Server 2003 域控制器环境的所有支持已结束。 有关 Microsoft 支持部门生命周期的其他信息，请访问[此页](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)。  
  
**域功能\-级别要求**  
  
 - 必须在 Windows Server 2003 或更高版本的域功能级别上运行所有用户帐户域和 AD FS 服务器加入到的域。  
  
 - 如果在 AD DS 中将证书显式映射到用户的帐户，则客户端证书身份验证需要 Windows Server 2008 域功能级别或更高版本。  
  
**架构要求**  
  
-   AD FS 2016 的新安装需要 Active Directory 2016 架构（最低版本85）。

-   将 AD FS 场行为级别（FBL）提升到2016级别需要 Active Directory 2016 架构（最低版本85）。  

  
**服务帐户要求**  
  
-   任何标准域帐户都可以用作 AD FS 的服务帐户。 还支持组托管服务帐户。 当你配置 AD FS 时，将自动添加运行时所需的权限。

-   AD 服务帐户所需的用户权限分配是 "作为服务登录"

-   "NT Service\adfssrv" 和 "NT Service\drs" 所需的用户权限分配是 "生成安全审核" 和 "作为服务登录"。

-   组托管服务帐户需要至少一个运行 Windows Server 2012 或更高版本的域控制器。  GMSA 必须位于默认的 "CN = 托管服务帐户" 容器下。  

-   对于 Kerberos 身份验证，服务主体名称 "`HOST/<adfs\_service\_name>`" 必须在 AD FS 服务帐户上进行注册。 默认情况下，在创建新的 AD FS 场时，AD FS 将配置此设置。  如果此操作失败（例如，在冲突或权限不足的情况下），你将看到一条警告，你应该手动添加它。  
   
**域要求**  
  
-   所有 AD FS 服务器都必须加入 AD DS 域。  
  
-   在场中的所有 AD FS 服务器都必须部署在同一域中。  
   
**多林要求**  
  
-   AD FS 服务器加入的域必须信任包含向 AD FS 服务进行身份验证的用户的每个域或林。  

-   AD FS 服务帐户所属的林必须信任所有用户登录林。 
  
-   AD FS 服务帐户必须有权读取每个域中包含对 AD FS 服务进行身份验证的用户的用户属性。  
  
## <a name="BKMK_5"></a>配置数据库要求  
本部分介绍分别使用 Windows 内部数据库（WID）或 SQL Server 数据库的 AD FS 场的要求和限制：  
  
**WID**  
  
-   在 WID 场中不支持 SAML 2.0 的项目解析配置文件。    

-   不支持将令牌重播检测作为 WID 场。 （此功能仅在 AD FS 充当联合身份验证提供程序并使用来自外部声明提供程序的安全令牌的情况下使用。）  
  
下表汇总了 WID 与 SQL Server 场中支持的 AD FS 服务器数。    
  
  
|| 1-100 AD FS 中配置的信赖方（RP）信任 | 配置了100个以上的 RP 信任  |
| --- |--- | --- |
|1-30 AD FS 服务器|支持 WID|不支持使用 WID-SQL Server 必需 |
|超过30个 AD FS 服务器|不支持使用 WID-SQL Server 必需|不支持使用 WID-SQL Server 必需  
  
**SQL Server**  
  
- 对于 Windows Server 2016 中的 AD FS，支持 SQL Server 2008 和更高版本。

- SQL Server 场中支持 SAML 项目解析和令牌重播检测。  
  
## <a name="BKMK_6"></a>浏览器要求  
AD FS 通过浏览器或浏览器控件执行身份验证时，浏览器必须遵守以下要求：  
  
- 必须启用 JavaScript  
  
- 对于单一登录，必须将客户端浏览器配置为允许 cookie  
  
- 必须\(支持\)服务器名称指示 SNI  
  
- 对于用户证书 & 设备证书身份验证，浏览器必须支持 SSL 客户端证书身份验证  

- 若要使用 Windows 集成身份验证进行无缝登录，必须在 "本地 intranet 区域\/" 或 "受信任的站点" 区域中配置联合身份验证服务名称（如 https：\/fs.contoso.com）。
  ## <a name="BKMK_7"></a>网络要求  
 
**防火墙要求**  
  
位于 Web 应用程序代理和联合服务器场之间以及客户端和 Web 应用程序代理之间的防火墙的防火墙必须启用 TCP 端口443入站。  
  
此外，如果需要使用 X509 \(\)用户证书的客户端用户证书身份验证 clientTLS authentication 并且未启用端口443上的 certauth 终结点，AD FS 2016 要求启用 TCP 端口49443客户端和 Web 应用程序代理之间的防火墙上的入站。 在 Web 应用程序代理和联合服务器\)之间的防火墙上不需要这样做。 

有关混合端口要求的其他信息，请参阅[混合标识端口和协议](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports)。 

有关其他信息，请参阅[保护 Active Directory 联合身份验证服务的最佳做法](../deployment/Best-Practices-Securing-AD-FS.md)
  
**DNS 要求**  
  
-   对于 intranet 访问，访问内部公司网络\(intranet\)内 AD FS 服务的所有客户端必须能够将 AD FS 服务名称解析为 AD FS 服务器或 AD FS 服务器的负载均衡器。  
  
-   对于 extranet 访问，从企业网络\(extranet\/internet\)外部访问 AD FS 服务的所有客户端必须能够将 AD FS 服务名称解析为 Web 应用程序代理服务器的负载均衡器，或Web 应用程序代理服务器。  
  
-   DMZ 中的每个 Web 应用程序代理服务器都必须能够将 AD FS 服务名称解析为 AD FS 服务器或 AD FS 服务器的负载均衡器。 这可以通过使用外围网络中的备用 DNS 服务器或通过使用 HOSTS 文件更改本地服务器解析来实现。  
  
-   对于 Windows 集成身份验证，必须使用 DNS a 记录\(而不是 CNAME\)作为联合身份验证服务名称。  

-   对于端口443上的用户证书身份验证，"certauth"。必须在 DNS\>中配置联合身份验证服务名称 "以解析为联合服务器或 web 应用程序代理。 \<

-   对于设备注册或使用 Windows 10 之前的客户端 "enterpriseregistration" 对本地资源进行新式身份验证。\<upn后缀\>"（对于组织中使用的每个 upn 后缀），必须配置为解析为联合服务器或 web 应用程序代理。

**负载均衡器要求**
- 负载均衡器不得终止 SSL。 AD FS 支持具有证书身份验证的多用例，这会在终止 SSL 时中断。 对于任何用例，都不支持在负载平衡器上终止 SSL。 
- 建议使用支持 SNI 的负载均衡器。 如果不是这样，则在 AD FS/Web 应用程序代理服务器上使用0.0.0.0 回退绑定应提供一种解决方法。
- 建议使用 HTTP （而非 HTTPS）运行状况探测终结点来执行路由流量的负载均衡器运行状况检查。 这样可以避免与 SNI 相关的任何问题。 对这些探测终结点的响应是 HTTP 200 OK，并在本地提供服务，而不依赖于后端服务。 可使用路径 "/adfs/probe" 通过 HTTP 访问 HTTP 探测
    - http://&lt;Web 应用程序代理&gt;名称/adfs/probe
    - http://&lt;ADFS 服务器名称&gt;/adfs/probe
    - http://&lt;Web 应用程序代理 IP&gt;地址/adfs/probe
    - http://&lt;ADFS IP 地址&gt;/adfs/probe
- 不建议使用 DNS 轮循机制作为负载平衡的方式。 使用这种类型的负载均衡并不提供使用运行状况探测从负载均衡器中删除节点的自动方式。 
- 不建议在负载平衡器中使用基于 IP 的会话相关性或用于身份验证流量的粘滞会话 AD FS。 当使用旧的身份验证协议（邮件客户端连接到 Office 365 邮件服务（Exchange Online））时，这可能会导致某些节点的过载。 

## <a name="BKMK_13"></a>权限要求  
执行安装和初始配置 AD FS 的管理员必须具有 AD FS 服务器上的本地管理员权限。  如果本地管理员没有在 Active Directory 中创建对象的权限，则必须先让域管理员创建所需的 AD 对象，然后使用 AdminConfiguration 参数配置 AD FS 场。  
  
  

