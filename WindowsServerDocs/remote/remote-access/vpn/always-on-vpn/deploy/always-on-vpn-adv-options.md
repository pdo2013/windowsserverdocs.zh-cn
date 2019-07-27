---
title: 始终启用 VPN 的高级功能
description: 除了此部署中提供的部署方案之外, 你还可以添加其他高级 VPN 功能, 以提高 VPN 连接的安全性和可用性。
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 07/24/19
ms.author: pashort, v-tea
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: ae3c088122a0100f94b4d9bca41078d901487237
ms.sourcegitcommit: 9f955be34c641b58ae8b3000768caa46ad535d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2019
ms.locfileid: "68590412"
---
# <a name="advanced-features-of-always-on-vpn"></a>Always On VPN 的高级功能

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**了解 Always On VPN 技术](../always-on-vpn-technology-overview.md)
- [**一个**开始规划 Always On 的 VPN 部署](always-on-vpn-deploy-planning.md)

除了提供的部署方案之外, 你还可以添加其他高级 VPN 功能, 以提高 VPN 连接的安全性和可用性。 例如, VPN 服务器可以使用这些功能来帮助确保连接客户端在允许连接之前正常运行。

## <a name="high-availability"></a>高可用性

下面是用于实现高可用性的其他选项。

|Option  |描述  |
|---------|---------|
|服务器复原能力和负载均衡     |在需要高可用性或支持大量请求的环境中, 你可以通过在运行网络策略服务器 (NPS) 的多台服务器之间使用负载均衡来提高远程访问的性能和复原能力, 并启用远程访问服务器群集。<p>相关文档:<ul><li>[NPS 代理服务器负载平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[在群集中部署远程访问](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|地理站点复原     |对于基于 IP 的地理位置, 你可以在 Windows Server 2016 中将全局流量管理器与 DNS 一起使用。 若要实现更可靠的地理负载平衡, 可以使用全局服务器负载均衡解决方案, 如 Microsoft Azure 流量管理器。<p>相关文档:<ul><li>[流量管理器概述](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure 流量管理器](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>高级身份验证

下面是用于身份验证的其他选项。

|Option  |描述  |
|---------|---------|
|Windows Hello 企业版     |在 Windows 10 中, Windows Hello 企业版通过在电脑和移动设备上提供强双重身份验证来替换密码。 此身份验证包含一种新类型的用户凭据, 该凭据绑定到设备并使用生物识别号或个人标识号 (PIN)。<p>Windows 10 VPN 客户端与 Windows Hello 企业版兼容。 用户使用手势登录后, VPN 连接将使用 Windows Hello 企业版证书进行基于证书的身份验证。<p>相关文档:<ul><li>[Windows Hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>技术案例研究:[在 Windows 10 中启用与 Windows Hello 企业版的远程访问](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure 多重身份验证 (MFA)     |Azure MFA 有云和本地版本, 你可以将其与 Windows VPN 身份验证机制集成。<p>有关此机制的工作原理的详细信息, 请参阅将[RADIUS 身份验证与 Azure 多重身份验证服务器集成](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)。         |

## <a name="advanced-vpn-features"></a>高级 VPN 功能

下面是高级功能的其他选项。

|Option  |描述  |
|---------|---------|
|流量筛选     |如果必须强制选择 VPN 客户端可以访问的应用程序, 则可以启用 VPN 流量筛选器。<p>有关详细信息, 请参阅[VPN 安全功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)。         |
|应用触发的 VPN     |可以配置 VPN 配置文件, 以便在启动某些应用程序或类型的应用程序时自动连接。<p>有关此事件和其他触发选项的详细信息, 请参阅[VPN 自动触发的配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)。         |
|VPN 条件性访问   |条件性访问和设备符合性可以要求托管设备满足标准, 然后才能连接到 VPN。 使用 VPN 条件性访问的高级功能之一, 你可以将 VPN 连接限制为仅限客户端身份验证证书包含**1.3.6.1.4.1.311.87**的 "AAD 条件访问" OID 的连接。<p>若要限制 VPN 连接, 必须执行以下操作:<ol><li>在 NPS 服务器上, 打开 "**网络策略服务器**" 管理单元。</li><li>展开 "**策略** > " "**网络策略**"。</li><li>右键单击**虚拟专用网络 (VPN) 连接**网络策略, 然后选择 "**属性**"。</li><li>选择 "**设置**" 选项卡。</li><li>选择 "**特定于供应商**", 然后选择 "**添加**"。</li><li>选择 "**允许的证书-OID** " 选项, 然后选择 "**添加**"。</li><li>将**1.3.6.1.4.1.311.87**的 AAD 条件访问 OID 粘贴为属性值, 然后选择 **"确定"** 两次。</li><li>选择 "**关闭**", 然后选择 "**应用**"。<p>执行这些步骤后, 当 VPN 客户端尝试使用不是生存期较短的云证书的任何证书进行连接时, 连接将失败。</li></ol>有关条件性访问的详细信息, 请参阅[VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)。   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>阻止使用已吊销证书的 VPN 客户端
  
安装更新后, RRAS 服务器可以对使用 IKEv2 和计算机证书进行身份验证的 Vpn 强制执行证书吊销, 如设备隧道的始终启用的 Vpn。 这意味着, 对于此类 Vpn, RRAS 服务器可以拒绝到尝试使用已吊销证书的客户端的 VPN 连接。

**可用性**

下表列出了每个版本的 Windows 的修补程序的大约发布日期。

|操作系统版本 |发布日期或发布日期 * |
|---------|---------|
|Windows Server 版本 1903  |[KB4501375](https://support.microsoft.com/help/4501375/windows-10-update-kb4501375) |
|Windows Server 2019<br />Windows Server 版本 1809  |2019年3季度  |
|Windows Server 版本 1803  |2019年3季度  |
|Windows Server 版本 1709  |2019年3季度  |
|Windows Server 2016, 版本1607  |[KB4503294](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) |
  
\*所有发布日期都列在 "日历季度" 中。 日期为近似值, 可能更改, 恕不另行通知。 发布更新时, 发布的链接将替换发布日期。

**如何配置先决条件** 

1. 在 Windows 更新可用时安装它们。
1. 请确保使用的所有 VPN 客户端和 RRAS 服务器证书都具有 CDP 条目, 并且 RRAS 服务器可以访问相应的 Crl。
1. 在 RRAS 服务器上, 使用**VpnAuthProtocol** PowerShell Cmdlet 配置**RootCertificateNameToAccept**参数。<br /><br />
   下面的示例列出了用于执行此操作的命令。 在此示例中, " **CN = Contoso Root 证书颁发机构**" 代表根证书颁发机构的可分辨名称。 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority,*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**如何为基于 IKEv2 计算机证书的 VPN 连接将 RRAS 服务器配置为强制证书吊销**

1. 在命令提示符窗口中运行以下命令: 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. 重新启动 "**路由和远程访问**" 服务。
  
若要禁用这些 VPN 连接的证书吊销, 请设置**CertAuthFlags = 2**或删除**CertAuthFlags**值, 然后重新启动 "**路由和远程访问**" 服务。 

**如何吊销基于 IKEv2 计算机证书的 VPN 连接的 VPN 客户端证书**
1. 从证书颁发机构吊销 VPN 客户端证书。
1. 从证书颁发机构发布新的 CRL。
1. 在 RRAS 服务器上, 打开 "管理命令提示符" 窗口, 然后运行以下命令:
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**如何验证基于 IKEv2 计算机证书的 VPN 连接的证书吊销是否正常工作**  
>[!Note]  
> 使用此过程之前, 请确保启用 CAPI2 操作事件日志。
1. 按照前面的步骤吊销 VPN 客户端证书。
1. 尝试使用已吊销的证书的客户端连接到 VPN。 RRAS 服务器应该拒绝连接并显示一条消息, 例如 "IKE 身份验证凭据不可接受"。
1. 在 RRAS 服务器上, 打开事件查看器, 然后导航到**应用程序和服务日志/Microsoft/Windows/CAPI2**。 
1. 搜索包含以下信息的事件:
   * 日志名称：**Microsoft CAPI2/操作性 Microsoft-CAPI2/Operational**
   * 事件ID：**41** 
   * 此事件包含以下文本: **subject = "*客户端 fqdn*"** (*客户端 fqdn*表示具有已吊销证书的客户端的完全限定的域名。) 

   事件 **<Result>** 数据的字段应包括**吊销证书**。 例如, 从事件中查看以下摘录:
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
## <a name="additional-protection"></a>额外保护

### <a name="trusted-platform-module-tpm-key-attestation"></a>受信任的平台模块 (TPM) 密钥证明

具有证明密钥的用户证书提供了更高的安全性保障, 并通过非作为后盾、反攻击和 TPM 提供的密钥的隔离进行了备份。

有关 Windows 10 中的 TPM 密钥证明的详细信息, 请参阅[Tpm 密钥证明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)。

## <a name="next-step"></a>下一步

[开始规划 ALWAYS ON VPN 部署](always-on-vpn-deploy-planning.md):在计划用作 VPN 服务器的计算机上安装远程访问服务器角色之前, 请执行以下任务。 适当规划后, 可以部署 Always On VPN, 还可以选择使用 Azure AD 配置 VPN 连接的条件性访问。  

## <a name="related-topics"></a>相关主题
- [NPS 代理服务器负载平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md):远程身份验证拨入用户服务 (RADIUS) 客户端是网络访问服务器, 例如虚拟专用网络 (VPN) 服务器和无线访问点, 创建连接请求并将其发送到 NPS 等 RADIUS 服务器。 在某些情况下, NPS 服务器一次可能会收到太多连接请求, 从而导致性能下降或过载。

- [流量管理器概述](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview):本主题概述 Azure 流量管理器, 该管理器可让你控制服务终结点的用户流量分布。 流量管理器根据流量路由方法和终结点的运行状况, 使用域名系统 (DNS) 将客户端请求定向到最合适的终结点。 

- [Windows Hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification):本主题提供了先决条件, 例如仅限云部署和混合部署。  本主题还列出了有关 Windows Hello 企业版的常见问题。

- [技术案例研究:在 Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)中启用与 Windows Hello 企业版的远程访问:在此技术案例研究中, 你将了解 Microsoft 如何实现使用 Windows Hello 企业版的远程访问。  Windows Hello 企业版是一种专用/公共密钥或基于证书的身份验证方法, 适用于超出密码的组织和使用者。 这种形式的身份验证依赖于密钥对凭据, 这些凭据可替代密码, 并可以抵御泄露、盗窃和网络钓鱼。 

- 将[RADIUS 身份验证与 Azure 多重身份验证服务器集成](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius):本主题将指导你完成使用 Azure 多重身份验证服务器添加和配置 RADIUS 客户端身份验证的步骤。 RADIUS 是一种标准协议，可接受身份验证请求并处理这些请求。 Azure 多重身份验证服务器可充当 RADIUS 服务器。 

- [VPN 安全功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features):本主题为你提供了有关锁定 VPN、Windows 信息保护 (WIP) 与 VPN 集成以及流量筛选器的 VPN 安全指导原则。 

- [VPN 自动触发的配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile):本主题提供 VPN 自动触发的配置文件选项, 如应用触发器、基于名称的触发器和 Always On。

- [VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):本主题概述了基于云的条件性访问平台, 以便为远程客户端提供设备符合性选项。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。 

- [TPM 密钥证明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation):本主题概述了受信任的平台模块 (TPM) 以及部署 TPM 密钥证明的步骤。 你还可以找到疑难解答信息和解决问题的步骤。
