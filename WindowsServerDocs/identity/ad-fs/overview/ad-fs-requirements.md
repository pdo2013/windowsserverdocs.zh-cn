---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: "广告 FS 2016 要求"
description: "安装 Active Directory 联合身份验证服务要求。"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81b51c751d5f54436b14450ef21bf49feb864290
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-requirements"></a>广告 FS 要求

>适用于：Windows Server 2016

以下是用于部署广告 FS 要求：  
  
-   [证书要求](ad-fs-requirements.md#BKMK_1)  
  
-   [硬件要求](ad-fs-requirements.md#BKMK_2)  
  
-   [代理要求](ad-fs-requirements.md#BKMK_3)  
  
-   [广告 DS 要求](ad-fs-requirements.md#BKMK_4)  
  
-   [配置数据库要求](ad-fs-requirements.md#BKMK_5)  
  
-   [浏览器要求](ad-fs-requirements.md#BKMK_6)  

-   [网络要求](ad-fs-requirements.md#BKMK_7)  
  
-   [权限要求](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>证书要求  
  
### <a name="ssl-certificates"></a>SSL 证书

每个广告金融服务和 Web 应用程序代理服务器具有 SSL 证书服务 HTTPS 请求到联合身份验证服务。  Web 应用程序代理产生服务请求发布的应用到其他 SSL 证书。

**建议：**用于所有广告 FS 联合身份验证的服务器和 Web 应用程序代理 SSL 同一个证书。 

**要求：**

SSL 服务器联合身份验证的证书必须满足以下要求
- 证书为公开受信任（用于生产部署）
- 证书含有服务器身份验证增强密钥使用量 (EKU) 值
- 证书包含联合身份验证服务名称，如"fs.contoso.com"主题或主题替代名称 (SAN)
- 对于用户端口 443 上证书身份验证，证书含有"certauth。\ < 联合身份验证服务 name\ >"，如"certauth.fs.contoso.com"SAN
- 对于设备注册或现代的身份验证本地资源使用预的 Windows 10 客户端，圣必须包含"enterpriseregistration。\ < upn suffix\ >"的每个 UPN 职务在使用你的组织中。

在 Web 应用程序代理 SSL 证书必须满足以下要求
- 如果该代理使用到使用 Windows 集成验证的代理 SSL 证书的代理广告 FS 请求必须相同（使用同一个键）联合身份验证的服务器 SSL 证书
- 如果广告 FS 属性"ExtendedProtectionTokenCheck"已启用（设置中广告 FS 默认），代理 SSL 证书必须相同（使用同一个键）联合身份验证的服务器 SSL 证书
- 否则，代理 SSL 证书要求都相同的联合身份验证的服务器 SSL 证书

### <a name="service-communication-certificate"></a>服务通信证书
不需要大多数广告 FS 方案包括 Azure AD 和 Office 365 该证书。 默认情况下，广告 FS 配置为服务通信证书初始配置时提供 SSL 证书。

**建议：**
- 当你使用用于 SSL，则使用同一个证书。  

### <a name="token-signing-certificate"></a>令牌签名证书
该证书是常用的符号颁发标记信赖方，因此信赖的方应用必须识别的证书及其关联作为已知的且受信任的键。 当令牌签名证书更改，例如当到期时，并且你配置了一个新的证书时，必须先更新所有信赖的合作伙伴。

**建议：**使用广告 FS 默认情况下，内部生成、自签名的令牌签名证书。  

**要求：**
- 如果你的组织需要用于令牌签名证书企业 PKI 从，这可以使用的 Install-AdfsFarm cmdlet SigningCertificateThumbprint 参数。
- 使用默认内部生成证书还是外部已注册的证书，更改令牌签名证书后你必须确保所有信赖的合作伙伴的更新的新证书信息。  否则，将无法登录到任何依赖方不会更新。

### <a name="token-encryptingdecrypting-certificate"></a>标记加密解密证书
索赔提供商加密标记颁发给广告 FS 使用该证书。

**建议：**使用广告 FS 默认情况下，内部生成、自签名的令牌解密证书。  

**要求：**
- 如果你的组织需要用于令牌签名证书企业 PKI 从，这可以使用的 Install-AdfsFarm cmdlet DecryptingCertificateThumbprint 参数。
- 使用默认内部生成证书还是外部已注册的证书，当解密证书令牌更改你必须确保所有索赔提供程序的都更新的新证书信息。  否则，将无法登录时使用任何索赔不更新的提供商。
  
> [!CAUTION]  
> 用于 token\ 签名和加密 token\ decrypting\/证书至关重要联合身份验证服务的稳定性。 管理自己 token\ 签名和加密 token\ decrypting\/证书客户应确保这些证书备份好并处于空闲状态独立恢复事件。  

### <a name="user-certificates"></a>用户证书
- 当使用的 x509 与广告 FS，所有用户证书用户证书身份验证必须链接到受信任的广告金融服务和 Web 应用程序代理服务器根证书颁发机构。

## <a name="BKMK_2"></a>硬件要求  
广告金融服务和 Web 应用程序代理服务器硬件要求（物理或虚拟）上 CPU、网关，因此你应大小用于处理容量你电场的日落。  
- 使用[广告 FS 2016 容量规划电子表格](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)以确定广告金融服务和 Web 应用程序代理服务器，你将需要的数量。

广告 FS 的内存和磁盘要求相当固定，请参阅下表：


|**硬件要求**|**最低要求。**|**推荐的要求**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|磁盘空间|为 32 GB|100 GB |

**SQL Server 硬件要求**

如果使用的为您的广告 FS 配置数据库的 SQL Server、SQL Server 的最基本的 SQL Server 建议根据的大小。  广告 FS 数据库大小很小，并且广告 FS 不会将重要的处理加载的数据库实例。  广告 FS，但是，在连接到数据库多次身份验证，因此应强大的网络连接。  遗憾的是，SQL Azure 不支持广告 FS 配置数据库中。
  
## <a name="BKMK_3"></a>代理要求  
  
-   必须将 Web 应用程序代理角色服务部署外部网络的访问，\-远程访问服务器角色的一部分。 

-   第三方代理必须支持[MS ADFSPIP 协议](https://msdn.microsoft.com/en-us/library/dn392811.aspx)作为广告 FS 代理支持。  有关第三方的列表供应商看到[常见问题](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip)。

-   广告 FS 2016 需要在 Windows Server 2016 的 Web 应用程序代理服务器。  无法运行的 2016 年场行为级别广告 FS 2016 场配置下层代理。
  
-   无法在同一台计算机上安装的联合身份验证的服务器和 Web 应用程序代理角色服务。  
  
## <a name="BKMK_4"></a>广告 DS 要求  
**域控制器要求**  
  
- 广告 FS 需要运行 Windows Server 2008 或更高版本的域控制器。

- 在至少有一个 Windows Server 2016 域控制器，才能进行 Microsoft Passport 的工作。
  
> [!NOTE]  
> 环境与 Windows Server 2003 该域控制器的所有支持已都结束。 访问[本页](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)以获取有关 Microsoft 支持生命周期。  
  
**域 functional\ 级别要求**  
  
 - 在 Windows Server 2003 或更高版本的功能域级别必须运行域中的所有用户帐户和广告 FS 服务器加入的域。  
  
 - Windows Server 2008 域功能级别或更高版本时所必需的客户端证书身份验证证书明确映射到一个用户帐户在广告 DS。  
  
**方案要求**  
  
-   新安装的广告 FS 2016 需要 Active Directory 2016 架构（最低版本 85）。

-   提升到 2016 年级别的广告 FS 电场的日落行为级别 (FBL) 需要 Active Directory 2016 架构（最低版本 85）。  

  
**服务帐户要求**  
  
-   任何标准域帐户可用作广告 FS 服务帐户。 组托管服务帐户也会受支持。 配置广告 FS 时，将自动添加所需在运行时的权限。

-   组托管服务帐户需要运行 Windows Server 2012 的域控制器在至少有一个或更高版本。  GMSA 必须 live 下默认值 CN = 托管服务帐户的容器。  

-   对于 Kerberos 身份验证，service 主要名称`HOST/<adfs\_service\_name>`' 必须广告 FS 服务帐户上注册。 默认情况下，广告 FS 将配置此创建一个新的广告 FS 场时。  如果无法正常工作，如就而言冲突或者没有足够的权限，你将看到一条警告，你应手动添加。  
   
**域要求**  
  
-   所有广告 FS 服务器必须都是加入广告 DS 域。  
  
-   必须在同一域部署电场的日落中的所有广告 FS 服务器。  
   
**多森林要求**  
  
-   广告 FS 服务器加入的域必须信任每域或森林包含用户身份验证广告 FS 服务。  

-   林中广告 FS 服务帐户的成员，必须信任所有用户登录森林。 
  
-   广告 FS 服务帐户必须具有权限阅读包含用户身份验证广告 FS 服务每域中的用户属性。  
  
## <a name="BKMK_5"></a>配置数据库要求  
本部分介绍要求和限制用作分别 Windows 内部数据库 (WID) 或 SQL Server 数据库的广告 FS 场：  
  
**WID**  
  
-   在 WID 电场的日落，不支持 SAML 2.0 的项目分辨率配置文件。    

-   标记重播检测不受支持 WID 场。 （此功能仅用仅在其中广告是充当联盟提供商和对消耗安全令牌来自外部索赔提供商的方案。）  
  
下表提供了多少广告 FS 服务器摘要 WID vs SQL Server 农场里的支持。    
  
  
|| 1-100 依赖方 (RP) 信任配置广告 FS 在 | 超过 100 RP 信任配置  |
| --- |--- | --- |
|广告 1-30 FS 服务器|受支持的 WID|不支持使用 WID-所需的 SQL Server |
|超过 30 年广告 FS 服务器|不支持使用 WID-所需的 SQL Server|不支持使用 WID-所需的 SQL Server  
  
**SQL Server**  
  
- 对于在 Windows Server 2016 的广告 FS，支持 SQL Server 2008 和更高版本。

- SQL Server 场支持 SAML 项目分辨率和令牌重播检测。  
  
## <a name="BKMK_6"></a>浏览器要求  
通过浏览器或浏览器控件执行广告 FS 身份验证，当你的浏览器必须遵守以下要求：  
  
-   JavaScript 必须为启用状态  
  
-   对于单一登录，必须配置客户端浏览器允许 cookie  
  
-   必须支持服务器名称指示 \(SNI\)  
  
-   对于用户证书和设备证书身份验证，浏览器必须支持 SSL 客户证书身份验证  

-   为在使用 Windows 的集成身份验证的无缝符号，必须在本地 intranet 或受信任的站点区域配置联合身份验证服务名称（例如 https:\/\/fs.contoso.com)。
## <a name="BKMK_7"></a>网络要求  
 
**防火墙要求**  
  
必须 TCP 端口 443 启用了防火墙位于 Web 应用程序代理和联合身份验证的服务器场之间和客户端和 Web 应用程序代理之间防火墙站。  
  
此外，如果客户端用户证书身份验证 \ (clientTLS 身份验证使用 X509 用户 certificates\)，才能和不启用上端口 443 certauth 端点、广告 FS 2016 要求启用 TCP 端口 49443 客户端和 Web 应用程序代理之间防火墙上的入站。 这不是需要的 Web 应用程序代理和联盟 servers\ 之间防火墙）。 

有关其他信息混合端口要求，请参阅[混合身份端口和协议](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports)。 

有关其他信息，请参阅[的最佳做法保护 Active Directory 联合身份验证服务](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**DNS 要求**  
  
-   Intranet 访问所有客户访问公司内部网络 \(intranet\) 内的广告 FS 服务都必须无法解析为负载平衡广告 FS 服务器或广告 FS 服务器的广告 FS 服务名称。  
  
-   外部网络的访问，所有客户访问广告 FS 服务的企业网络 \(extranet\/Internet\) 之外都必须无法解决的 Web 应用程序代理服务器或的 Web 应用程序代理服务器负载平衡广告 FS 服务名称。  
  
-   每个 DMZ 中的 Web 应用程序代理服务器必须是能够解决负载平衡广告 FS 服务器或广告 FS 服务器广告 FS 服务的名称。 这可以实现在 DMZ 网络或通过更改使用该主机文件的本地服务器分辨率使用备用 DNS 服务器。  
  
-   有关 Windows 的集成身份验证，你必须使用联合身份验证服务名称 DNS A 记录 \(not CNAME\)。  

-   用户在端口 443 证书身份验证，必须在 DNS 解决联合身份验证的服务器或 web 应用程序代理配置"certauth。\ < 联合身份验证服务 name\ >"。

-   对于设备注册或现代的身份验证本地资源使用预的 Windows 10 客户端，必须配置"enterpriseregistration。\ < upn suffix\ >"，为你的组织中使用的每个 UPN 职务解析为联合身份验证的服务器或 web 应用程序代理。

**负载平衡要求**
- 不必须负载平衡终止 SSL。 广告 FS 支持多个证书身份验证，这将时终止 SSL 中断的用例。 任何用例不支持在负载平衡终止 SSL。 
- 建议使用支持 SNI 负载平衡。 在事件不是，请在你的广告 FS 上使用 0.0.0.0 回退绑定 / Web 应用程序代理服务器应提供一种解决方法。
- 建议使用 HTTP (而不是 HTTPS) 健康探测端点执行负载平衡健康检查路由交通。 这将避免相关 SNI 的任何问题。 这些探测端点响应 HTTP 200 确定，并且不具有依赖后端服务与本地提供服务。 可以通过使用路径 '/ adfs/探测' HTTP 访问 HTTP 探测
    - http://&lt;Web 应用程序代理名称&gt;/adfs/探测
    - http://&lt;ADFS 服务器名称&gt;/adfs/探测
    - http://&lt;Web 应用程序代理 IP 地址&gt;/adfs/探测
    - http://&lt;ADFS IP 地址&gt;/adfs/探测
- 不建议使用 DNS 循环作为加载余额的方法。 使用负载平衡这种不提供自动化的方式使用健康探测负载平衡中删除节点。 
- 不建议使用基于 IP 会话关联或便笺会话到负载平衡内的广告 FS 身份验证交通。 使用传统的身份验证的邮件客户端的协议连接到 Office 365 邮件服务 (Exchange Online) 时，这可能导致重载某些节点。 

## <a name="BKMK_13"></a>权限要求  
安装和广告 FS 初始配置执行管理员必须具有广告 FS 服务器上的本地管理员权限。  如果本地管理员没有创建 Active Directory 对象的权限，它们首次必须具有域管理员创建所需的广告对象，然后使用 AdminConfiguration 参数广告 FS 场配置。  
  
  

