---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: 保护 AD FS 和 Web 应用程序代理的最佳做法
description: 本文档提供 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理的安全规划和部署的最佳实践。
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 15b0c721b620e2891f4452fd54501f4970b7c177
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359997"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>保护 Active Directory 联合身份验证服务的最佳实践


本文档提供 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理的安全规划和部署的最佳实践。  它包含有关这些组件的默认行为的信息, 以及针对具有特定用例和安全要求的组织的其他安全配置的建议。

本文档适用于 Windows Server 2012 R2 和 Windows Server 2016 (预览版) 中的 AD FS 和 WAP。  无论基础结构部署在本地网络中, 还是部署在 Microsoft Azure 的云托管环境中, 都可以使用这些建议。

## <a name="standard-deployment-topology"></a>标准部署拓扑
对于本地环境中的部署, 建议使用一个标准部署拓扑, 该拓扑包含内部企业网络中的一个或多个 AD FS 服务器, 其中包含一个或多个 Web 应用程序代理 (WAP) 服务器。  在每个层, AD FS 和 WAP, 硬件或软件负载平衡器置于服务器场的前面, 并处理流量路由。  在每个 (FS 和代理) 场前面的负载均衡器的外部 IP 地址前面放置防火墙。

![AD FS 标准拓扑](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>需要的端口
下图描述了在 AD FS 和 WAP 部署的组件之间必须启用的防火墙端口。  如果部署不包括 Azure AD/Office 365, 则可忽略同步要求。

>请注意, 仅当使用用户证书身份验证时才需要端口 49443, 对于 Azure AD 和 Office 365 是可选的。

![AD FS 标准拓扑](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> 端口 808 (Windows Server 2012R2) 或端口 1501 (Windows Server 2016 +) 是 Net.tcp 端口 AD FS 用于本地 WCF 终结点, 以将配置数据传输到服务进程和 Powershell。 可以通过运行 Set-adfsproperties | 来查看此端口选择 NetTcpPort。 这是一个本地端口, 无需在防火墙中打开, 但会在端口扫描中显示。 

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect 和联合服务器/WAP
下表描述了 Azure AD Connect 服务器和联合/WAP 服务器之间通信所需的端口和协议。  

Protocol |端口 |描述
--------- | --------- |---------
HTTP|80 (TCP/UDP)|用于下载 Crl (证书吊销列表) 以验证 SSL 证书。
HTTPS|443 (TCP/UDP)|用于与 Azure AD 同步。
WinRM|5985| WinRM 侦听器

### <a name="wap-and-federation-servers"></a>WAP 和联合服务器
此表描述了联合服务器与 WAP 服务器之间通信所需的端口和协议。

Protocol |端口 |描述
--------- | --------- |---------
HTTPS|443 (TCP/UDP)|用于身份验证。

### <a name="wap-and-users"></a>WAP 和用户
下表描述了用户与 WAP 服务器之间通信所需的端口和协议。

Protocol |端口 |描述
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)|用于设备身份验证。
TCP|49443 (TCP)|用于证书身份验证。

有关混合部署所需的端口和协议的其他信息, 请参阅[此处](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/)的文档。

有关 Azure AD 和 Office 365 部署所需的端口和协议的详细信息, 请参阅[此处](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)的文档。

### <a name="endpoints-enabled"></a>终结点已启用

安装 AD FS 和 WAP 后, 联合身份验证服务和代理上会启用一组默认的 AD FS 终结点。  这些默认值是根据最常用的方案选择的, 并且不需要更改它们。  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>可有可无为 Azure AD/Office 365 启用的终结点代理的最小集
仅对 Azure AD 和 Office 365 方案部署 AD FS 和 WAP 的组织可以进一步限制在代理上启用的 AD FS 终结点数, 以实现更小的攻击面。
下面是在这些情况下, 必须在代理上启用的终结点列表:

|终结点|用途
|-----|-----
|/adfs/ls|基于浏览器的身份验证流和当前版本的 Microsoft Office 将此终结点用于 Azure AD 和 Office 365 身份验证
|/adfs/services/trust/2005/usernamemixed|用于与 Office 2013 以前版本的 Office 客户端进行的 Exchange Online 2015 更新。  更高版本的客户端使用被动 \adfs\ls 终结点。
|/adfs/services/trust/13/usernamemixed|用于与 Office 2013 以前版本的 Office 客户端进行的 Exchange Online 2015 更新。  更高版本的客户端使用被动 \adfs\ls 终结点。
|/adfs/oauth2|此帐户用于已配置为直接 AD FS (即不通过 AAD) 进行身份验证的任何现代应用 (即本地或云中)
|/adfs/services/trust/mex|用于与 Office 2013 以前版本的 Office 客户端进行的 Exchange Online 2015 更新。  更高版本的客户端使用被动 \adfs\ls 终结点。
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |任何被动流的要求;并由 Office 365/Azure AD 用于检查 AD FS 证书


可以使用以下 PowerShell cmdlet 在代理上禁用 AD FS 终结点:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

例如：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>身份验证的扩展保护
针对身份验证的扩展保护是一项功能, 可减轻中间人 (MITM) 攻击, 并 AD FS 默认启用。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要验证设置, 可以执行以下操作:
可以使用以下 PowerShell commandlet 验证设置。  
    
   `PS:\>Get-ADFSProperties`

属性为`ExtendedProtectionTokenCheck`。  默认设置为 "允许", 因此无需考虑与不支持功能的浏览器的兼容性问题即可实现安全优势。  

### <a name="congestion-control-to-protect-the-federation-service"></a>用于保护联合身份验证服务的拥塞控制
联合身份验证服务代理 (WAP 的一部分) 提供拥塞控制以保护 AD FS 服务免受大量请求的攻击。  如果通过 Web 应用程序代理与联合服务器之间的延迟检测到, 则 Web 应用程序代理将拒绝外部客户端身份验证请求。  默认情况下, 此功能配置为建议的延迟阈值级别。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要验证设置, 可以执行以下操作:
1.  在您的 Web 应用程序代理计算机上, 启动一个提升的命令窗口。
2.  导航到 ADFS 目录，网址为%WINDIR%\adfs\config。
3.  将拥塞控制设置从其默认值更改为 "<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />"。
4.  保存并关闭该文件。
5.  通过运行 "net stop adfssrv" 和 "net start adfssrv" 来重新启动 AD FS 服务。
有关参考, 可在[此处](https://msdn.microsoft.com/library/azure/dn528859.aspx )找到有关此功能的指南。

### <a name="standard-http-request-checks-at-the-proxy"></a>在代理中检查标准 HTTP 请求
代理还对所有流量执行以下标准检查:

- FS-P 本身通过短期证书对 AD FS 进行身份验证。  在怀疑外围服务器泄露的情况下, AD FS 可以 "撤消代理信任", 使其不再信任来自可能泄露的代理的任何传入请求。 撤消代理信任会吊销每个代理的证书，使其无法成功地针对 AD FS 服务器的任何目的进行身份验证
- FS-P 终止所有连接, 并创建与内部网络上的 AD FS 服务的新 HTTP 连接。 这会在外部设备与 AD FS 服务之间提供会话级缓冲区。 外部设备从不直接连接到 AD FS 服务。
- FS-P 执行 HTTP 请求验证, 该验证专门筛选出 AD FS 服务不需要的 HTTP 标头。

## <a name="recommended-security-configurations"></a>推荐的安全配置
确保所有 AD FS 和 WAP 服务器都能接收最新的更新, 为你的 AD FS 基础结构提供最重要的安全建议, 确保你有一种方法来使你的 AD FS 和 WAP 服务器最新, 并提供所有安全更新以及这些可选指定为此页上 AD FS 的重要更新。

Azure AD 客户监视和保持当前基础结构的推荐方式是通过 Azure AD Connect Health AD FS Azure AD Premium 的一项功能。  Azure AD Connect Health 包括 AD FS 或 WAP 计算机是否缺少专门用于 AD FS 和 WAP 的重要更新之一时触发的监视器和警报。

有关为 AD FS 安装 Azure AD Connect Health 的信息, 请参阅[此处](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/)。

## <a name="additional-security-configurations"></a>其他安全配置
可以根据需要配置以下附加功能, 为默认部署中提供的这些功能提供额外的保护。

### <a name="extranet-soft-lockout-protection-for-accounts"></a>适用于帐户的 Extranet "软" 锁定保护
使用 Windows Server 2012 R2 中的 extranet 锁定功能, AD FS 管理员可以设置允许的最大失败身份验证请求数 (ExtranetLockoutThreshold) 和 "观察时段的时间段 (ExtranetObservationWindow)"。 当达到身份验证请求的最大数量 (ExtranetLockoutThreshold) 时, AD FS 将停止尝试针对设置的时间段 (ExtranetObservationWindow) 的 AD FS 验证提供的帐户凭据。 此操作可防止此帐户受到 AD 帐户锁定, 换言之, 它可以防止此帐户失去对依赖于用户身份验证 AD FS 的公司资源的访问权限。 这些设置适用于 AD FS 服务可以进行身份验证的所有域。

你可以使用以下 Windows PowerShell 命令来设置 AD FS extranet 锁定 (例如): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

[此处](https://technet.microsoft.com/library/dn486806.aspx )提供了此功能的公开文档以供参考。 

### <a name="disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet"></a>在代理上禁用 WS-TRUST Windows 终结点, 即从 extranet

WS-TRUST Windows 终结点 ( */adfs/services/trust/2005/windowstransport*和 */adfs/services/trust/13/windowstransport*) 仅适用于使用 HTTPS 上的 WIA 绑定的面向 intranet 的终结点。 向 extranet 公开它们可能会允许对这些终结点的请求绕过锁定保护。 应在代理上禁用这些终结点 (即从 extranet 禁用), 以使用以下 PowerShell 命令保护 AD 帐户锁定。 在代理上禁用这些终结点不会影响已知的最终用户。

    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/2005/windowstransport -Proxy $false
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/windowstransport -Proxy $false
    
### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>区分 intranet 和 extranet 访问的访问策略
AD FS 能够区分源自本地企业网络的请求的访问策略, 以及通过代理从 internet 传入的请求。  这可以按应用程序或全局执行。  对于具有敏感或个人身份信息的高业务价值应用程序或应用程序, 请考虑需要多重身份验证。  可以通过 "AD FS 管理" 管理单元完成此操作。  

### <a name="require-multi-factor-authentication-mfa"></a>需要多重身份验证 (MFA)
AD FS 可以配置为要求强身份验证 (例如多重身份验证), 专用于通过代理传入的请求、单个应用程序以及对 Azure AD/Office 365 和本地资源的条件访问。  MFA 支持的方法包括 Microsoft Azure MFA 和第三方提供程序。  系统将提示用户提供附加信息 (例如包含一段时间代码的短信文本), AD FS 与提供程序的特定插件结合使用以允许访问。  

支持的外部 MFA 提供程序包括[此](https://technet.microsoft.com/library/dn758113.aspx)页中列出的那些提供程序以及 HDI Global。

### <a name="hardware-security-module-hsm"></a>硬件安全模块 (HSM)
在默认配置中, AD FS 用来对令牌进行签名的密钥永远不会将联合服务器保留在 intranet 上。  它们永远不会出现在 DMZ 或代理计算机上。  可以选择提供其他保护, 可以在附加到 AD FS 的硬件安全模块中保护这些密钥。  Microsoft 不会生成 HSM 产品, 但是有几个在市场上支持 AD FS。  若要实现此建议, 请按照供应商指南创建用于签名和加密的 X509 证书, 然后使用 AD FS 安装 powershell commandlet, 指定自定义证书, 如下所示:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

其中：


- `CertificateThumbprint`是你的 SSL 证书
- `SigningCertificateThumbprint`签名证书 (带有 HSM 保护的密钥)
- `DecryptionCertificateThumbprint`是加密证书 (带有 HSM 保护的密钥)



