---
title: 始终启用 VPN 的高级功能
description: 超出此部署中提供的部署方案，可以添加其他高级的 VPN 功能，以提高安全性和 VPN 连接的可用性。
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 5f43d64dc7642ef67da03fec989909bc4f2f14ae
ms.sourcegitcommit: a3c9a7718502de723e8c156288017de465daaf6b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67263026"
---
# <a name="advanced-features-of-always-on-vpn"></a>Always On VPN 的高级的功能

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

- [**上一：** 了解有关 Always On VPN 技术](../always-on-vpn-technology-overview.md)
- [**下一步：** 开始规划 Always On VPN 部署](always-on-vpn-deploy-planning.md)

除了提供的部署方案，可以添加其他高级的 VPN 功能，以提高安全性和 VPN 连接的可用性。 例如，此类组件可帮助确保在允许连接之前连接的客户端处于正常状态。

## <a name="high-availability"></a>高可用性

以下是用于高可用性的其他选项。

|Option  |描述  |
|---------|---------|
|服务器恢复能力和负载平衡     |在环境中需要高可用性或支持大量请求，你可以提高性能和复原能力的远程访问通过使用运行网络策略服务器 (NPS) 并启用远程的多个服务器之间进行负载均衡访问服务器群集。<p>相关的文档：<ul><li>[NPS 代理服务器负载平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[在群集中部署远程访问](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|地理站点恢复     |对于基于 IP 的地理位置，可以使用 Windows Server 2016 中 DNS 使用全局流量管理器。 对于更强大的地理负载平衡，可以使用全局服务器负载平衡解决方案，如 Microsoft Azure 流量管理器。<p>相关的文档：<ul><li>[流量管理器概述](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>高级身份验证

以下是用于身份验证的其他选项。

|Option  |描述  |
|---------|---------|
|Windows Hello 企业版     |在 Windows 10 中，Windows Hello 企业版在电脑和移动设备上使用强大的双因素身份验证替换密码。 此身份验证包含新的用户凭据，绑定到设备并使用生物识别或个人标识号 (PIN) 类型。<p>Windows 10 VPN 客户端是与 Windows hello 企业版兼容。 用户使用手势登录后，VPN 连接用于在 Windows hello 企业版证书基于证书的身份验证。<p>相关的文档：<ul><li>[Windows Hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>技术案例研究：[启用对 Windows 10 中的业务的远程访问使用 Windows Hello](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure 多重身份验证 (MFA)     |Azure MFA 有云和内部版本可以与 Windows VPN 身份验证机制集成。<p>有关此机制的工作原理的详细信息，请参阅[集成 RADIUS 身份验证和 Azure 多重身份验证服务器](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)。         |

## <a name="advanced-vpn-features"></a>高级的 VPN 功能

以下是高级功能的其他选项。

|Option  |描述  |
|---------|---------|
|流量筛选     |如果需要强制执行客户端可以访问哪些应用程序 VPN，则可以启用 VPN 流量筛选器。<p>有关详细信息，请参阅[VPN 安全功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)。         |
|应用程序触发的 VPN     |你可以配置某些应用程序或类型的应用程序启动时自动连接的 VPN 配置文件。<p>有关此设置和其他触发选项的详细信息，请参阅[VPN 自动触发的配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)。         |
|VPN 条件性访问   |条件性访问和设备符合性可以要求要符合标准，才能连接到 VPN 的托管的设备。 VPN 条件性访问的高级功能之一，可限制为仅 VPN 连接的客户端身份验证证书，包含 AAD 条件性访问的对象 ID 的"1.3.6.1.4.1.311.87。<p>若要限制 VPN 连接，需要：<ol><li>在 NPS 服务器上，打开**网络策略服务器**管理单元中。</li><li>展开**策略** > **网络策略**。</li><li>右键单击**虚拟专用网络 (VPN) 连接**网络策略，然后选择**属性**。</li><li>选择**设置**选项卡。</li><li>选择**供应商特定**，然后选择**添加**。</li><li>选择**允许证书 OID**选项，然后选择**添加**。</li><li>粘贴的 AAD 条件性访问 OID **1.3.6.1.4.1.311.87**作为属性值，然后选择**确定**两次。</li><li>选择**关闭**，然后**应用**。<p>现在当 VPN 客户端尝试使用生存期较短的云证书以外的任何证书进行连接，则连接将失败。</li></ol>有关条件性访问的详细信息，请参阅[VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)。   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>阻止使用吊销的证书的 VPN 客户端
  
安装更新后，RRAS 服务器可以使用 IKEv2 的 vpn 强制执行证书吊销和计算机证书进行身份验证，如设备处理始终在 Vpn 隧道。 这意味着，此类 Vpn RRAS 服务器可以拒绝对 VPN 连接到尝试使用已吊销的证书的客户端。

**可用性**

下表列出了每个版本的 Windows 修补程序的近似发行日期。

|操作系统版本 |发布日期 * |
|---------|---------|
|Windows Server 版本 1903  |第 2 季度 2019  |
|Windows Server 2019<br />Windows Server 版本 1809  |第 3 季度 2019  |
|Windows Server 版本 1803  |第 3 季度 2019  |
|Windows Server 版本 1709  |第 3 季度 2019  |
|Windows Server 2016 中，版本 1607  |第 2 季度 2019  |
  
\* 日历季度列出了所有的发布日期。 日期是近似和更改，恕不另行通知。

**如何配置系统必备组件** 

1. 可用时安装的 Windows 更新。
1. 请确保所有的 VPN 客户端和您使用的 RRAS 服务器证书包含 CDP 的条目，并在 RRAS 服务器可访问各自的 Crl。
1. 在 RRAS 服务器上，使用**集 VpnAuthProtocol** PowerShell cmdlet 来配置**RootCertificateNameToAccept**参数。<br /><br />
   下面的示例列出了要执行此操作的命令。 在示例中， **CN = Contoso 根证书颁发机构**表示根证书颁发机构的可分辨的名称。 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority,*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**如何配置 RRAS 服务器，以强制实施基于 IKEv2 计算机证书的 VPN 连接的证书吊销**

1. 在命令提示符窗口中，运行以下命令： 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. 重新启动**路由和远程访问**服务。
  
若要禁用这些 VPN 连接的证书吊销，设置**CertAuthFlags = 2**或删除**CertAuthFlags**值，并重新启动**路由和远程访问**服务。 

**如何撤消基于 IKEv2 计算机证书的 VPN 连接的 VPN 客户端证书**
1. 撤消 VPN 客户端证书从证书颁发机构。
1. 发布新的 CRL 从证书颁发机构。
1. 在 RRAS 服务器上，打开管理命令提示符窗口，并运行以下命令：
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**如何验证该证书吊销的 IKEv2 计算机基于证书的 VPN 连接正常**  
>[!Note]  
> 使用此过程之前，请确保启用 CAPI2 操作事件日志。
1. 按照前面的步骤，若要撤消 VPN 客户端证书。
1. 尝试使用具有吊销的证书的客户端连接到 VPN。 RRAS 服务器应拒绝连接并显示一条消息，如"IKE 身份验证凭据是不可接受。"
1. 在 RRAS 服务器上，打开事件查看器并导航到**应用程序和服务日志/Microsoft/Windows/CAPI2**。 
1. 搜索事件，其中包含以下信息：
   * 日志名称：**Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational**
   * 事件ID：**41** 
   * 该事件包含以下文本： **subject ="*客户端 FQDN*"** (*客户端 FQDN*表示已吊销的客户端的完全限定的域名证书。） 

   **<Result>** 的事件数据的字段应包括**吊销了证书**。 例如，请参阅以下部分内容摘自一个事件：
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
    <UserData>  
     <CertVerifyRevocation>  
      <Certificate fileRef="C97AE73E9823E8179903E81107E089497C77A720.cer" subjectName="client01.corp.contoso.com" />  
      <IssuerCertificate fileRef="34B1AE2BD868FE4F8BFDCA96E47C87C12BC01E3A.cer" subjectName="Contoso Root Certification Authority" />
      ...
      <Result value="80092010">The certificate is revoked.</Result>
     </CertVerifyRevocation>
    </UserData>
   </Event>
   ```

---
## <a name="additional-protection"></a>额外的保护

### <a name="trusted-platform-module-tpm-key-attestation"></a>受信任的平台模块 (TPM) 密钥证明

使用 TPM 证明密钥的用户证书提供更高版本的安全保障，由非可导出性、 反攻击和隔离由 TPM 提供密钥的备份。

有关 Windows 10 中的 TPM 密钥证明的详细信息，请参阅[TPM 密钥证明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)。

## <a name="next-step"></a>下一步

[开始规划 Always On VPN 部署](always-on-vpn-deploy-planning.md):在你计划使用作为 VPN 服务器的计算机上安装远程访问服务器角色之前，请执行以下任务。 正确的规划之后, 可以部署 Always On VPN，并 （可选） 配置的 VPN 连接使用 Azure AD 条件性访问。  

## <a name="related-topics"></a>相关主题
- [NPS 代理服务器负载平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md):远程身份验证拨入用户服务 (RADIUS) 客户端，是如虚拟专用网络 (VPN) 服务器和无线访问点的网络访问服务器，创建连接请求，并将其发送到 RADIUS 服务器，例如 NPS。 在某些情况下，NPS 服务器可能会收到连接请求过多，一次导致性能下降或重载。

- [流量管理器概述](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview):本主题提供概述的 Azure 流量管理器，以便您可以控制用户流量的服务终结点的分发。 流量管理器使用域名系统 (DNS) 将客户端请求定向到最适当的终结点根据流量路由方法和终结点的运行状况。 

- [Windows hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification):本主题提供了先决条件，例如仅限云部署和混合部署。  本主题还列出了有关 Windows hello 企业版产品的常见问题。

- [技术案例研究：启用对 Windows 10 中的业务的远程访问使用 Windows Hello](https://msdn.microsoft.com/library/mt728163.aspx):本技术案例研究介绍 Microsoft 如何实现与 Windows Hello for Business 的远程访问。  Windows hello 企业版是私钥/公钥或基于证书的身份验证方法的组织和消费者而言，超越了密码。 这种形式的身份验证依赖于密钥对凭据，可取代密码且能抵御漏洞、 窃取及钓鱼到。 

- [将 RADIUS 身份验证与 Azure 多重身份验证服务器集成](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius):本主题将指导你完成添加和配置 Azure 多重身份验证服务器 RADIUS 客户端身份验证。 RADIUS 是一种标准协议，可接受身份验证请求并处理这些请求。 Azure 多重身份验证服务器可以充当 RADIUS 服务器。 

- [VPN 安全功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features):本主题提供锁定 VPN、 与 VPN 和流量筛选器的 Windows 信息保护 (WIP) 集成 VPN 安全性准则。 

- [VPN 自动触发的配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile):本主题提供了 VPN 自动触发的配置文件选项，如应用触发器、 基于名称的触发器和 Always On。

- [VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):本主题概述的基于云的条件性访问平台为远程客户端提供的设备符合性选项。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。 

- [TPM 密钥证明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation):本主题提供了用于部署 TPM 密钥证明的受信任的平台模块 (TPM) 和步骤的概述。 此外可以查找故障排除信息和步骤来解决问题。
