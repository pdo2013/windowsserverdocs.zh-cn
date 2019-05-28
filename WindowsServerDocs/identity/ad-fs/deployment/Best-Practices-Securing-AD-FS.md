---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: 保护 AD FS 和 Web 应用程序代理的最佳做法
description: 本文档提供有关安全规划和部署 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理的最佳实践。
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 958bf8455d03ddc04395fafe83e70a49c7659c96
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192441"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>保护 Active Directory 联合身份验证服务的最佳做法


本文档提供有关安全规划和部署 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理的最佳实践。  它包含有关默认行为的这些组件和具有特定用例和安全要求的组织的其他安全配置建议的信息。

本文档适用于 AD FS 和 WAP 中 Windows Server 2012 R2 和 Windows Server 2016 （预览版）。  可以使用这些建议，无论是基础结构部署在本地网络中还是在 Microsoft Azure 等云托管环境中。

## <a name="standard-deployment-topology"></a>标准部署拓扑
对于部署在本地环境中，我们建议包含内部企业网络上一个或多个 AD FS 服务器与外围网络或 extranet 网络中的一个或多个 Web 应用程序代理 (WAP) 服务器的标准部署拓扑。  在每个层上，AD FS 和 WAP，硬件或软件负载均衡器放置在服务器场的前面，并处理流量路由。  防火墙位于每个 （FS 和代理） 场前面的负载均衡器的外部 IP 地址前所需的方式。

![AD FS 标准拓扑](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>所需端口
下图描绘了之间和在 AD FS 和 WAP 部署内的组件之间必须启用防火墙端口。  如果部署不包括 Azure AD / Office 365，同步要求可忽略。

>请注意，端口 49443 才需要如果使用用户证书身份验证，是可选的 Azure ad 和 Office 365。

![AD FS 标准拓扑](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect 和联合身份验证服务器 /WAP
下表介绍的端口和协议所需的 Azure AD Connect 服务器和联合服务器 /WAP 服务器之间的通信。  

Protocol |端口 |描述
--------- | --------- |---------
HTTP|80 (TCP/UDP)|用于下载 Crl （证书吊销列表） 来验证 SSL 证书。
HTTPS|443(TCP/UDP)|用于与 Azure AD 同步。
WinRM|5985| WinRM 侦听器

### <a name="wap-and-federation-servers"></a>WAP 和联合身份验证服务器
下表介绍的端口和协议所需的联合身份验证服务器和 WAP 服务器之间的通信。

Protocol |端口 |描述
--------- | --------- |---------
HTTPS|443(TCP/UDP)|用于身份验证。

### <a name="wap-and-users"></a>WAP 和用户
下表介绍的端口和协议所需的用户与 WAP 服务器之间的通信。

Protocol |端口 |描述
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|用于设备身份验证。
TCP|49443 (TCP)|使用证书身份验证。

有关其他信息所需的端口和协议所需的混合部署，请参阅文档[此处](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/)。

有关端口和协议所需的 Azure AD 的详细信息和 Office 365 部署，请参阅文档[此处](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)。

### <a name="endpoints-enabled"></a>启用终结点

安装 AD FS 和 WAP，联合身份验证服务和代理服务器上都启用一组默认的 AD FS 终结点。  已根据最常需要和已用的情况下选择这些默认值并不需要更改它们。  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[可选]最小值组的终结点代理服务器启用 Azure ad / Office 365
组织仅为 Azure AD 中部署 AD FS 和 WAP 和 Office 365 方案可以限制甚至更多的 AD FS 终结点代理服务器上启用以实现更少的攻击面数。
下面是必须在这些情况下在代理启用的终结点的列表：

|终结点|用途
|-----|-----
|/adfs/ls|基于浏览器的身份验证流和当前版本的 Microsoft Office 使用此终结点适用于 Azure AD 和 Office 365 身份验证
|/adfs/services/trust/2005/usernamemixed|与 Office 客户端早于 Office 2013 2015 年 5 月更新一起使用为 Exchange Online。  更高版本的客户端使用被动 \adfs\ls 终结点。
|/adfs/services/trust/13/usernamemixed|与 Office 客户端早于 Office 2013 2015 年 5 月更新一起使用为 Exchange Online。  更高版本的客户端使用被动 \adfs\ls 终结点。
|/adfs/oauth2|使用此一个为任何新型应用程序 （在本地或云中），已配置为直接向 AD FS （即，不通过 AAD) 进行身份验证
|/adfs/services/trust/mex|与 Office 客户端早于 Office 2013 2015 年 5 月更新一起使用为 Exchange Online。  更高版本的客户端使用被动 \adfs\ls 终结点。
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |必需的任何被动模式的过程;并由 Office 365 / Azure AD，检查 AD FS 证书


可以使用以下 PowerShell cmdlet 在代理上禁用 AD FS 终结点：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

例如：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>身份验证扩展的保护
身份验证扩展的保护是一项功能，可针对中间人 (MITM) 攻击缓解并与 AD FS 默认情况下启用。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要验证设置，请执行以下操作：
可以验证设置，使用以下 PowerShell commandlet。  
    
   `PS:\>Get-ADFSProperties`

该属性是`ExtendedProtectionTokenCheck`。  默认设置是允许，以便可以实现而产生与不支持的功能的浏览器的兼容性问题的安全优势。  

### <a name="congestion-control-to-protect-the-federation-service"></a>拥塞控制以保护联合身份验证服务
联合身份验证服务代理 （WAP 的一部分） 提供了拥塞控制来保护从大量请求的 AD FS 服务。  如果检测到的 Web 应用程序代理与联合身份验证服务器之间的延迟重载函数的联合身份验证服务器，Web 应用程序代理将拒绝外部客户端身份验证请求。  此功能配置默认情况下建议的延迟阈值级别。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要验证设置，请执行以下操作：
1.  您的 Web 应用程序代理计算机上启动提升的命令窗口。
2.  导航到 ADFS 目录，请在 %windir%\adfs\config。
3.  从其默认值来更改拥塞控制设置<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />。
4.  保存并关闭该文件。
5.  通过运行 net stop adfssrv，然后 net start adfssrv 重新启动 AD FS 服务。
可供你参考，找到此功能的指导[此处](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx )。

### <a name="standard-http-request-checks-at-the-proxy"></a>在代理处检查标准 HTTP 请求
代理还执行以下标准检查对所有流量：

- FS-P 本身的生存期短的证书通过 AD FS 进行身份验证。  在怀疑被泄露的外围网络服务器的方案中，AD FS 可以"撤销代理信任"，使其无法再信任来自的任何传入请求可能会泄露代理。 代理信任撤消后，每个代理的证书，以便它不能成功进行身份验证出于任何目的与 AD FS 服务器
- FS-P 终止所有连接，并在内部网络上创建新的 HTTP 连接到 AD FS 服务。 这样，外部设备和 AD FS 服务之间的会话级别缓冲区。 外部设备永远不会直接连接到 AD FS 服务。
- FS-P 执行专门筛选出不需要的 AD FS 服务的 HTTP 标头的 HTTP 请求验证。

## <a name="recommended-security-configurations"></a>推荐的安全配置
确保所有 AD FS 和 WAP 服务器都接收到最新的 AD FS 基础结构的最重要的安全建议是为了确保您有一种方式到位，以使你的 AD FS 和 WAP 服务器保持最新与所有安全更新，以及这些可选的更新为此页上的 AD FS 指定为重要的更新。

用于 Azure AD 用户能够监视和保持最新的推荐方法是通过 Azure AD Connect Health AD fs，Azure AD Premium 的一项功能其基础结构。  Azure AD Connect Health 包括监视器和 AD FS 或 WAP 计算机专门为 AD FS 和 WAP 缺少其中一个重要的更新的情况下触发的警报。

安装 Azure AD Connect Health AD FS 可以找到有关的信息[此处](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/)。

## <a name="additional-security-configurations"></a>其他安全配置
可以根据需要配置以下附加功能，以提供到默认部署中提供额外的保护。

### <a name="extranet-soft-lockout-protection-for-accounts"></a>对帐户的"soft"extranet 锁定保护
使用 Windows Server 2012 R2 中的 extranet 锁定功能中，AD FS 管理员可以设置最大数目的失败的身份验证请求 (ExtranetLockoutThreshold) 和一个观察窗口的时间段 (ExtranetObservationWindow)。 当达到此最大数目 (ExtranetLockoutThreshold) 的身份验证请求时，就会停止 AD FS 尝试对针对 AD FS 对提供的帐户凭据进行身份验证设置的时间段内 (ExtranetObservationWindow)。 此操作从 AD 帐户锁定保护此帐户，换而言之，它可防止此帐户的公司资源的依赖于 AD FS 进行身份验证的用户会失去访问权限。 这些设置适用于 AD FS 服务可以进行身份验证的所有域。

可以使用以下 Windows PowerShell 命令来设置 AD FS extranet 锁定 （例如）： 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

有关参考，是此功能的公共文档[此处](https://technet.microsoft.com/library/dn486806.aspx )。 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>区分 intranet 和 extranet 访问的访问策略
AD FS 具有能够区分在本地的公司网络 vs 传来的请求，从 internet 通过代理发出的请求的访问策略。  这可以每个应用程序或全局范围内。  对于具有高业务价值的应用程序或应用程序对于敏感或个人身份信息，请考虑需要多因素身份验证。  这可以通过 AD FS 管理管理单元。  

### <a name="require-multi-factor-authentication-mfa"></a>需要多重身份验证 (MFA)
可以配置 AD FS 为需要强身份验证 （例如多重身份验证），专用于通过代理传入请求、 单个应用程序，并对这两个 Azure AD 条件性访问 / Office 365 和本地资源。  MFA 的受支持的方法包括 Microsoft Azure MFA 和第三方提供程序。  系统会提示用户提供的其他信息 (如包含一个短信代码的时间)，AD FS 配合插件，以允许访问特定于提供程序。  

支持外部的 MFA 提供程序包括中列出的内容[这](https://technet.microsoft.com/library/dn758113.aspx)页上，以及全局 HDI。

### <a name="hardware-security-module-hsm"></a>硬件安全模块 (HSM)
在默认配置中，密钥 AD FS 使用的令牌进行签名永远不会保留在 intranet 上联合身份验证服务器。  它们永远不会存在于外围网络或代理计算机上。  （可选） 若要提供额外的保护，这些密钥可以进行保护的硬件安全模块连接到 AD FS。  Microsoft 不会生成 HSM 产品，但有几个支持 AD FS 市场上。  若要实现此建议，请按照供应商的指南创建 X509 证书进行签名和加密，然后使用 AD FS 安装 powershell commandlet，指定自定义证书，如下所示：

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

其中：


- `CertificateThumbprint` 是你的 SSL 证书
- `SigningCertificateThumbprint` 是 （使用受 HSM 保护密钥） 签名证书
- `DecryptionCertificateThumbprint` 为您的加密证书 （带有受 HSM 保护密钥）



