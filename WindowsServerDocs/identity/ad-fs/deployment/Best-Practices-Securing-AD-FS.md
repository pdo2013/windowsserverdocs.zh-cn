---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: "广告金融服务和 Web 应用程序代理服务器的安全的最佳实践"
description: "本文档提供安全的规划和部署 Active Directory 联合身份验证服务 (广告 FS) 和 Web 应用程序代理服务器的最佳实践。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6026246228b22fdea6001528ab7621a1704f1983
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>保护 Active Directory 联合身份验证服务的最佳实践

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本文档提供安全的规划和部署 Active Directory 联合身份验证服务 (广告 FS) 和 Web 应用程序代理服务器的最佳实践。  它包含有关默认行为的这些组件和建议特定的用例和安全要求的组织的其他安全配置的信息。

本文适用于广告金融服务和 Windows Server 2012 R2 和 Windows Server 2016 （预览） 中的 WAP。  是否在打开的本地网络或例如 Microsoft Azure 云托管环境中部署基础结构，则可以使用这些建议。

## <a name="standard-deployment-topology"></a>标准部署拓扑
对于本地环境中部署，我们建议包含内部公司的网络，一个或多个广告 FS 服务器 DMZ 或外部网络中的一个或多个 Web 应用程序代理 (WAP) 服务器的标准部署拓扑。  每一层，广告金融服务和 WAP，硬件或软件负载平衡位于放服务器电场的日落，并处理路由交通。  防火墙位于放每个 （金融服务和代理） 场按需在前的负载平衡外部 IP 地址。

![广告 FS 标准拓扑](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>所需端口
以下图表显示了必须为启用状态，以及之间十分广告金融服务和 WAP 部署的组件防火墙端口。  如果部署不包括 Azure AD / Office 365、 同步要求可以忽略。

>请注意，只是端口 49443 所需如果使用用户证书身份验证，它是可选的 Azure AD 和 Office 365。

![广告 FS 标准拓扑](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD 连接和联合身份验证的服务器/WAP
此表描述端口和所需的 Azure AD 连接服务器和联盟/WAP 服务器之间进行通信的协议。  

协议 |端口 |描述
--------- | --------- |---------
HTTP|80 (TCP/UDP)|用于下载 Crl （证书吊销列表） 以验证 SSL 证书。
HTTPS|443(TCP/UDP)|用于使用 Azure AD 同步。
WinRM|5985| WinRM 的聆听者

### <a name="wap-and-federation-servers"></a>WAP 和联合身份验证的服务器
此表描述端口和所需的联合身份验证的服务器和 WAP 服务器之间进行通信的协议。

协议 |端口 |描述
--------- | --------- |---------
HTTPS|443(TCP/UDP)|用于身份验证。

### <a name="wap-and-users"></a>WAP 和用户
此表描述端口和所需的用户和 WAP 服务器之间进行通信的协议。

协议 |端口 |描述
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|用于设备身份验证。
TCP|49443 (TCP)|用于证书身份验证。

所需的端口和所需的混合部署看到该文档协议的其他信息[此处](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-ports/)。

有关详细信息端口和所需的 Azure AD 协议和 Office 365 部署，请参阅文档[此处](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)。

### <a name="endpoints-enabled"></a>端点启用

安装广告金融服务和 WAP 时联合身份验证服务和代理服务器上，可启用一组默认的广告 FS 端点。  已基于最常所需常用的方案选择这些的默认值，并不需要进行更改。  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[可选]分钟套端点为 Azure AD 启用的代理 / Office 365
仅针对 Azure AD 部署广告金融服务和 WAP 组织和 Office 365 方案可以限制进一步广告 FS 端点启用的代理服务器上，以实现更多最少的攻击 surface 的数量。
下面是在这些方案中代理服务器，必须启用的端点的列表：

|端点|目的
|-----|-----
|/ adfs 月 1 ！|基于浏览器的身份验证的版本流和当前版本的 Microsoft Office 用于 Azure AD 此端点和 Office 365 身份验证
|/adfs/services/trust/2005/usernamemixed|用于 Exchange Online 与 Office 客户端早于 2015 Office 2013 年 5 月更新。  更高版本的客户端使用被动 \adfs\ls 端点。
|/adfs/services/trust/13/usernamemixed|用于 Exchange Online 与 Office 客户端早于 2015 Office 2013 年 5 月更新。  更高版本的客户端使用被动 \adfs\ls 端点。
|/ adfs/oauth2|使用此一个任何现代应用 （在本地或在云中） 你已经将配置直接向 （即不是通过 AAD) 的广告 FS 进行身份验证
|/adfs/services/trust/mex|用于 Exchange Online 与 Office 客户端早于 2015 Office 2013 年 5 月更新。  更高版本的客户端使用被动 \adfs\ls 端点。
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |对于任何被动流; 要求和使用的 Office 365 / Azure AD 检查广告 FS 证书


使用以下 PowerShell cmdlet 代理服务器上，可以禁用广告 FS 端点：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

例如：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>扩展的保护进行身份验证
身份验证的扩展的保护是一项功能在中间 (MITM) 攻击的男子针对缓解了，默认情况下与广告 FS 处于启用状态。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>验证设置，请执行以下操作：
可验证设置，使用以下 PowerShell commandlet。  
    
   `PS:\>Get-ADFSProperties`

属性`ExtendedProtectionTokenCheck`。  默认设置是允许，以便没有不支持的功能的浏览器的兼容性问题可以实现安全权益。  

### <a name="congestion-control-to-protect-the-federation-service"></a>拥挤控制保护联合身份验证服务
联合身份验证服务代理 （WAP 的一部分） 提供了大量的请求从保护广告 FS 服务的拥挤控制。  Web 应用程序代理将拒绝外部身份验证射如果检测到的 Web 应用程序代理和联盟服务器之间延迟重载联合身份验证的服务器。  默认情况下使用推荐的延迟 threshold 级别配置此功能。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>验证设置，请执行以下操作：
1.  你 Web 应用程序代理计算机上，启动提升了权限的 command 窗口。
2.  导航到 ADFS 目录 %windir%\adfs\config。
3.  更改拥挤控制设置为其默认值即可从<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />。
4.  保存并关闭该文件。
5.  通过运行网络停止 adfssrv，然后按网络开始 adfssrv，重新启动广告 FS 服务。
此功能的指南可以找到您，以供参考[此处](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx )。

### <a name="standard-http-request-checks-at-the-proxy"></a>在代理检查标准的 HTTP 请求。
代理还执行以下标准检查防御的所有通信：

- 本身 FS P 向广告 FS 短期证书通过验证身份。  在可疑危害 dmz 服务器方案，广告 FS 可以"取消代理信任"，使其不再信任的任何传入请求潜在威胁代理服务器。 撤销代理信任吊销每个代理自己证书，以使其无法成功进行身份验证用于任何目的广告 FS 服务器
- FS P 终止所有连接，然后在内部网络上创建新 HTTP 连接到广告 FS 服务。 这样，外部设备和广告 FS 服务之间的会话级别缓冲区。 外部设备永远不会直接连接到广告 FS 服务。
- FS P 执行专门筛选出不需要的广告 FS 服务的 HTTP 标头的 HTTP 请求验证。

## <a name="recommended-security-configurations"></a>推荐的安全配置
确保所有广告 FS 和 WAP 服务器接收最新的更新您的广告 FS 基础结构的最重要的安全推荐是都确保你有一种方法来使你的广告金融服务和 WAP 服务器保持最新的所有安全更新，以及在此页面上的广告 fs 指定为重要这些可选更新。

Azure AD 客户能够监视和保持最新的推荐的方式他们的基础结构是通过 Azure AD 连接健康广告 fs，Azure AD 高级的一项功能。  Azure AD 连接健康包括监视器和警报触发如果广告 FS 或 WAP 计算机专为广告金融服务和 WAP 缺少重要的更新之一。

有关广告 FS 可以找到有关安装 Azure AD 连接健康信息[此处](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-health-agent-install/)。

## <a name="additional-security-configurations"></a>其他安全配置
以下附加功能可以或者配置提供其他对这些默认部署中提供的保护。

### <a name="extranet-soft-lockout-protection-for-accounts"></a>帐户联网"柔软"锁定保护
使用外部锁定功能在 Windows Server 2012 R2，广告 FS 管理员可以设置最大数目的身份验证失败的请求 (ExtranetLockoutThreshold) 和 ' 观察窗口的时段 (ExtranetObservationWindow)。 达到此最大数目 (ExtranetLockoutThreshold) 的身份验证请求后，广告 FS 停止尝试进行提供的帐户凭据针对广告 FS 身份验证的设置的时间 (ExtranetObservationWindow)。 此操作会将该帐户防止 AD 帐户锁定，换句话说，它可防止此帐户依靠广告 FS 的用户身份验证的企业资源失去访问权限。 这些设置适用于所有广告 FS 服务可以进行身份验证的域。

你可以使用下面的 Windows PowerShell 命令设置广告 FS 联网锁定 （如）： 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

此功能的公用文档，以供参考是[此处](https://technet.microsoft.com/en-us/library/dn486806.aspx )。 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>区分访问策略的 intranet 和外部网络的访问权限
广告 FS 能够区分访问策略源自从 internet 通过代理推出的本地，公司网络 vs 请求中的请求。  这可以完成每个应用程序或全球。  对于高业务值应用程序或应用程序的敏感或个人身份的信息，请考虑需要多因素身份验证。  这可以通过广告 FS 管理单元。  

### <a name="require-multi-factor-authentication-mfa"></a>要求多因素身份验证 (MFA)
广告 FS 可以配置为需要强的身份验证 （如多因素身份验证） 专门为通过代理传入请求、 个别应用程序和条件访问这两个 Azure AD / Office 365 和本地资源。  受支持的方法的 MFA 包括 Microsoft Azure MFA 和第三方提供商。  提示用户可以提供其他信息 (如包含一个短信短时间代码)，以及广告 FS 的工作方式的提供商特定插件，以允许访问。  

受支持的外部 MFA 提供商包括中列出的这些[这](https://technet.microsoft.com/en-us/library/dn758113.aspx)页上，以及 HDI 全球。

### <a name="hardware-security-module-hsm"></a>硬件安全模块 (HSM)
在其默认配置，键广告 FS 用来登录标记从未的 intranet 上保留联合身份验证的服务器。  它们是永远不会显示在 DMZ 或代理计算机上。  （可选） 若要提供额外的保护，这些键可以保护在附加到广告 FS 硬件安全模块。  Microsoft 不会产生 HSM 产品，但也有几个市场支持广告 FS 上。  为了实现该建议，请按照供应商指导，以便创建 X509 证书签名和加密，然后使用广告 FS 安装 powershell commandlets，指定你自定义的证书，如下所示：

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

位置：


- `CertificateThumbprint` 是 SSL 证书
- `SigningCertificateThumbprint` 是你签名证书 （带有 HSM 保护键）
- `DecryptionCertificateThumbprint` 是你的加密证书 （带有 HSM 保护键）



