---
title: 始终启用 VPN 增强功能
description: Always On VPN 通过过去的 Windows VPN 解决方案有许多优点。 在集成、 安全、 连接、 网络控制和兼容性的关键改进与 Microsoft 的云优先、 移动优先的愿景一致 Always On VPN。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836518"
---
# <a name="always-on-vpn-enhancements"></a>始终启用 VPN 增强功能

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

&#171;[**前面：** 了解有关 Always On VPN 功能](../vpn-map-da.md)<br>
&#187;[**下一步：** 了解有关 Always On VPN 技术](always-on-vpn-technology-overview.md)

Always On VPN 通过过去的 Windows VPN 解决方案有许多优点。 以下重大改进符合 Microsoft 的云优先、 移动优先的愿景的 Always On VPN:

- **平台集成：** Always On VPN 已使用 Windows 操作系统和第三方解决方案来为无数高级的连接方案提供强大的平台改进的集成。

- **安全性：** Always On VPN 具有新的高级安全功能，可限制类型的流量，哪些应用程序可以使用 VPN 连接，并可用来启动连接的身份验证方法。 当连接处于活动状态大多数情况下时，是特别重要的安全连接。 有关更多详细信息，请参阅[VPN 身份验证选项](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication)。

- **VPN 连接：** Always On VPN，无论设备隧道提供自动触发功能。 在 Always On VPN 之前, 能够触发自动连接通过用户或设备身份验证是不可能的。  

- **网络控制：** Always On VPN，管理员可以指定更高粒度级别的路由策略 — 甚至深入到单个应用程序 — 这是需要特殊的远程访问的业务 (LOB) 应用的最佳选择。  Always On VPN 也是完全符合 Internet 协议版本 4 (IPv4) 和版本 6 (IPv6)。 与 DirectAccess，不同于 IPv6 没有任何特定的依赖项。

  >[!Note]
  >在开始之前，请确保在 VPN 服务器上启用 IPv6。 否则为不能建立连接，并显示一条错误消息。
  
- **配置和兼容性：** Always On VPN 可以部署和管理多个方面，它为 Always On VPN 提供了几大优势，其他 VPN 客户端软件。

## <a name="platform-integration"></a>平台集成

Microsoft 已引入或改进了 Always On VPN 中的以下集成功能：

| 重要的改进 | 描述 |
| ---- | ---- |
| **[Windows 信息保护 (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | 使用 WIP 的集成，以确定是否允许通信绕过 VPN 的网络策略强制。 如果用户配置文件处于活动状态，并应用 WIP 策略，将自动触发 Always On VPN 连接。 此外，当您使用 WIP，没有 （除非您想更高级的配置） 单独指定 VPN 配置文件中的 AppTriggerList 和 TrafficFilterList 规则无需因为 WIP 策略和应用程序列表自动才会生效。 |
| **[Windows hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | Always On VPN 本身支持 Windows Hello 企业版 （在基于证书的身份验证模式下） 为这两个登录到计算机和连接到 VPN 提供无缝单一登录体验。 因此，对于 VPN 连接，从而可以与 Windows hello 企业版身份验证使用 Always On 连接需要没有辅助身份验证 （用户凭据）。 |
| **[Microsoft Azure 条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | Always On VPN 客户端可以集成 Azure 条件性访问平台，以强制实施多重身份验证 (MFA)、 设备符合性或两者的组合。 时符合条件性访问策略，Azure Active Directory (Azure AD) 颁发用于对 VPN 网关进行身份验证的生存期较短 （默认为 60 分钟） IP 安全 (IPsec) 身份验证证书。 设备符合性使用 System Center Configuration Manager/Intune 符合性策略，这可能包括设备运行状况证明状态作为连接符合性检查的一部分。 |
| **Azure MFA** | 当针对 Azure MFA 与远程身份验证拨入用户服务 (RADIUS) 服务和网络策略服务器 (NPS) 扩展相结合，VPN 身份验证可以使用强 MFA。 |
| **第三方 VPN 插件** | 使用通用 Windows 平台 (UWP)，第三方 VPN 供应商可以创建单个应用程序的完整范围的 Windows 10 设备。 UWP 提供有保证的核心 API 层设备上，消除的复杂性和通常与编写内核级驱动程序相关的问题。 目前，Windows 10 UWP VPN 插件存在[Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp)， [F5 访问](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0)， [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj)， [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc)， [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)，并[GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); 毫无疑问，其他人将出现在将来。 |
---

## <a name="security"></a>安全性

中安全性的主要改进包括以下几个方面：

| 重要的改进 | 描述 |
| ---- | ---- |
| **流量筛选器** | 通过流量筛选器，可以指定客户端的策略，可确定哪些流量允许进入企业网络。 这样一来，管理员可以应用应用程序或流量限制在 VPN 接口上，限制对特定的源、 目标端口和 IP 地址使用。 提供了两种类型的筛选规则：<ul><li>**基于应用的规则。** 基于应用的防火墙规则基于指定的应用程序的列表，以便允许仅从这些应用程序发出的通信绕过 VPN 接口。</li><li>**基于流量的规则。** 基于流量的防火墙规则根据网络要求，如端口、 地址和协议。 这些规则仅针对这些特定的条件相匹配的流量有权绕过 VPN 接口的使用。<p><p>_**请注意。**_<br>这些规则仅适用于出站流量从设备。 使用的流量的筛选器阻止从企业网络到客户端的入站的流量。 </li></ul> |
| **Per-App VPN** | 每应用 VPN 就像是拥有一个基于应用的流量筛选器，但它更远要将应用程序触发器与基于应用的流量筛选器组合，以便 VPN 连接限制为特定的应用程序而不是 VPN 客户端上的所有应用程序。 该功能会自动启动时启动该应用程序。 |
| **对自定义 IPsec 加密算法的支持** | Always On VPN 支持 RSA 和椭圆曲线加密基于自定义加密算法以满足严格的政府要求或组织的安全策略的使用。 |
| **可扩展身份验证协议 (EAP) 的本机支持** | Always On VPN 以本机方式支持 EAP，可以使用一组不同的 Microsoft 和第三方 EAP 类型作为身份验证工作流的一部分。 EAP 提供了基于以下身份验证类型安全的身份验证：<ul><li>用户名和密码</li><li>智能卡 （物理和虚拟）</li><li>用户证书</li><li>Windows Hello 企业版</li><li>通过 EAP RADIUS 集成 MFA 支持</li></ul>应用程序供应商控制第三方 UWP VPN 插件的身份验证方法，虽然它们具有一系列的可用选项，包括自定义凭据类型和 OTP 的支持。 |
---

## <a name="vpn-connectivity"></a>VPN 连接性

以下是中 Always On VPN 连接的主要改进：

| 重要的改进 | 描述 |
| ---- | ---- |
| **始终可用** | Always On 是一种使活动 VPN 配置文件来自动连接并保持连接基于触发器的 Windows 10 功能 — 即，用户登录、 网络状态更改或设备屏幕活动。 Always On 还已集成到已连接的备用体验以使电池寿命最大化。 |
| **应用程序触发** | 你可以在 Windows 10 中的一组指定的应用程序启动时自动连接配置 VPN 配置文件。 您可以配置桌面和 UWP 应用程序触发的 VPN 连接。 |
| **基于名称的触发** | 使用 Always On VPN，可以定义规则，以便特定域的名称查询触发的 VPN 连接。 Windows 10 现在支持已加入域的和已加入域的计算机的名称基于触发 （以前，仅已加入域的计算机已受支持）。 |
| **受信任的网络检测** | Always On VPN 包括此功能以确保如果用户连接到企业边界内受信任的网络，则不触发 VPN 连接。 可以将此功能与前面所述来提供无缝的"仅连接时需要"用户体验的触发任何的方法组合。 |
| **[设备隧道](../vpn-device-tunnel-config.md)** | Always On VPN 使你能够创建设备或计算机的专用的 VPN 配置文件。 与不同_用户隧道_，它仅在用户登录到设备或计算机之后, 连接_设备隧道_允许 VPN 建立连接之前用户登录。 设备隧道和用户隧道独立操作，使用自己的 VPN 配置文件、 可以一次连接和可以根据需要使用不同的身份验证方法和其他 VPN 配置设置。 |
---

## <a name="networking"></a>网络

以下是一些 Always On VPN 中的网络改进：

| 重要的改进 | 描述 |
| ---- | ---- |
| **对 IPv4 和 IPv6 的双协议栈支持** | Always On VPN 以本机方式支持使用 IPv4 和 IPv6，在双堆栈方法。 它对任何特定的依赖项一种协议优于另一个，这允许与支持未来的 IPv6 网络需要结合使用的最大 IPv4/IPv6 应用程序兼容性。<p>**_请注意。_** 在开始之前，请确保在 VPN 服务器上启用 IPv6。 否则为不能建立连接，并显示一条错误消息。|
| **特定于应用程序的路由策略** | 除了定义的全局 VPN 连接路由策略的 internet 和 intranet 流量分离，则可以添加路由策略以控制使用拆分隧道或强制隧道配置基于每个应用程序。 此选项可提供更精细的控制哪些应用可与哪些资源通过 VPN 隧道进行交互。 |
| **排除路由** | Always On VPN 支持指定专门控制路由行为来定义哪种流量应遍历 VPN 仅和需要通过物理网络接口的排除路由的功能。<p><p>_**说明。**_<br>-排除路由当前可用于客户端，位于同一子网中的流量等 LinkLocal。<br>-排除路由仅在拆分隧道设置中的工作。 |
---

## <a name="configuration-and-compatibility"></a>配置和兼容性 

以下是一些 Always On VPN 中的配置和兼容性改进：

| 重要的改进 | 描述 |
| ---- | ---- |
| **第三方 VPN 网关兼容性** | Always On VPN 客户端不需要使用基于 Microsoft 的 VPN 网关进行操作。 支持 IKEv2 协议的情况下，客户端方便了与第三方 VPN 网关支持此行业标准的隧道类型的互操作性。 此外可以使用 UWP VPN 插件与自定义的隧道类型结合使用而无需牺牲 Always On VPN 平台功能和优势来实现与第三方 VPN 网关互操作性。<p><p>_**请注意。**_<br>请咨询网关或第三方后端设备供应商对配置和使用 Always On VPN 和设备使用 IKEv2 的隧道的兼容性。 |
| **行业标准 IKEv2 VPN 协议支持** | Always On VPN 客户端支持 IKEv2，今天的之一使用最广泛的行业标准隧道协议。 此兼容性可最大化与第三方 VPN 网关互操作性。 |
| **平台支持** | AAlways 上 VPN 支持已加入域的、 非域加入 （工作组） 或加入 Azure AD-设备以便进行这两个企业和自带设备办公 (BYOD) 方案。 此外，Always On VPN 是用于所有 Windows 版本。 |
| **不同的管理和部署机制** | 可以使用许多管理和部署机制来管理 VPN 设置 (称为_VPN 配置文件_)，包括 Windows PowerShell、 System Center Configuration Manager、 Intune （或第三方移动设备管理 [MDM] 工具）和 Windows 配置设计器。 这些选项简化无论您使用的客户端管理工具的 Always On VPN 配置。 |
| **标准化的 VPN 配置文件定义** | Always On VPN 支持使用标准的 XML 配置文件 (ProfileXML) 配置提供了大多数管理和部署工具集使用的标准配置模板格式。 |
---


## <a name="next-steps"></a>后续步骤

- [了解一些高级的 Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [了解有关 Always On VPN 技术的详细信息](always-on-vpn-technology-overview.md)

- [开始规划 Always On VPN 部署](deploy/always-on-vpn-deploy-deployment.md)



---
