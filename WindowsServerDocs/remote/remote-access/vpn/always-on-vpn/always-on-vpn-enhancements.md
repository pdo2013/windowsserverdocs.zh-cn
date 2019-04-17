---
title: 始终启用 VPN 增强功能
description: 始终启用 VPN 相比过去的 Windows VPN 解决方案中有许多优势。 在集成、 安全、 连接、 网络控件和兼容性的重要改进与 Microsoft 的云优先、 移动优先构想对齐始终启用 VPN。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067038"
---
# 始终启用 VPN 增强功能

>适用于: Windows Server (半年频道)、Windows Server 2016、Windows 10

& #171;[**上一步：** 了解始终启用 VPN 功能](../vpn-map-da.md)<br>
& #187;[**下一步：** 了解始终启用 VPN 技术](always-on-vpn-technology-overview.md)

始终启用 VPN 相比过去的 Windows VPN 解决方案中有许多优势。 下面的重要改进将始终启用 VPN 符合 Microsoft 的云优先、 移动优先构想：

- **平台集成：** 始终启用 VPN 具有与 Windows 操作系统和第三方解决方案，以提供无数高级的连接方案可靠平台改进的集成。

- **安全：** 始终启用 VPN 具有新的高级安全功能，以限制的通信，应用程序可以使用 VPN 连接，类型，你可以使用的身份验证方法以启动连接。 当连接处于活动状态大多数情况下时，它是尤其重要，来保护连接。 有关更多详细信息，请参阅[VPN 身份验证选项](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication)。

- **VPN 连接：** 始终启用 VPN，无论设备隧道提供了自动触发功能。 始终启用 VPN 之前, 未可能进行触发自动连接到用户或设备身份验证的能力。  

- **网络控件：** 始终启用 VPN 允许管理员指定更精细的级别上的路由策略-即使到单独的应用程序，这非常适合需要特殊的远程访问的业务线 (LOB) 应用。  始终启用 VPN 也是完全兼容，这两个 Internet 协议版本 4 (IPv4) 和版本 6 (IPv6)。 与 DirectAccess，不同于 IPv6 没有任何特定的依赖项。

  >[!Note]
  >在开始之前，请确保在 VPN 服务器上启用 IPv6。 否则为不能建立的连接，并显示一条错误消息。
  
- **配置和兼容性：** 始终启用 VPN 可以进行部署和管理几种方法，这样可以使始终启用 VPN 通过其他 VPN 客户端的软件的多项优势。

## 平台集成

Microsoft 已引入或改进始终启用 VPN 中的以下集成功能：

| 密钥改进 | 说明 |
| ---- | ---- |
| **[Windows 信息保护 (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | 与 WIP 集成允许网络策略实施，以确定是否允许流量通过 VPN。 如果用户配置文件处于活动状态，并且应用 WIP 策略，始终启用 VPN 自动触发连接。 此外，当你使用 WIP，没有分别在 VPN 配置文件中指定 AppTriggerList 和 TrafficFilterList 规则，（除非你希望更高级的配置） 需要因为 WIP 策略和应用程序列表将自动生效。 |
| **[Windows Hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | 始终启用 VPN 本机支持 Windows Hello 企业版 （在基于证书的身份验证模式下） 为两个登录到计算机和连接到 VPN，提供无缝单一登录体验。 因此，没有辅助身份验证 （用户凭据） 需要用于 VPN 连接，使其可以使用始终启用连接 Windows Hello 企业版身份验证。 |
| **[Microsoft Azure 的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | 始终启用 VPN 客户端可与 Azure 条件访问平台强制执行多重身份验证 (MFA)、 设备合规性或两者的组合。 当符合条件访问策略时，Azure Active Directory (Azure AD) 颁发用于对 VPN 网关进行身份验证的短期 （默认情况下，60 分钟） IP 安全性 (IPsec) 身份验证证书。 设备合规性使用 System Center Configuration Manager/Intune 合规性策略，可以包含设备运行状况证明状态作为连接合规性检查的一部分。 |
| **Azure MFA** | 当为 Azure MFA 与远程身份验证拨入用户服务 (RADIUS) 服务和网络策略服务器 (NPS) 扩展相结合，VPN 身份验证可以使用强 MFA。 |
| **第三方 VPN 插件** | 使用通用 Windows 平台 (UWP)，第三方 VPN 提供商可以创建单个应用程序的完整范围的 Windows 10 设备。 UWP 提供有保证的核心 API 层跨设备，从而消除的复杂性和问题通常与编写内核级驱动程序相关联。 目前，Windows 10 UWP VPN 插件存在[Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp)、 [F5 Access](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0)、[检查点胶囊形 VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj)、 [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc)、 [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)和[GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3);毫无疑问，其他人将会出现在将来。 |
---

## 安全性

在以下方面是安全的主要改进：

| 密钥改进 | 说明 |
| ---- | ---- |
| **流量筛选器** | 通过流量筛选器，你可以指定确定哪些流量允许进入公司网络的客户端的策略。 这样一来，管理员可以应用的应用或流量限制在 VPN 接口上，将其使用特定源、 目标端口和 IP 地址范围限制。 两种类型的筛选规则有：<ul><li>**基于应用的规则。** 基于应用的防火墙规则基于指定的应用程序的列表，以便只有来自这些应用的流量才允许通过 VPN 接口。</li><li>**基于流量的规则。** 基于流量的防火墙规则基于网络要求，如端口、 地址和协议。 使用这些规则仅用于匹配这些特定条件的流量才允许通过 VPN 接口。<p><p>_**注意。**_<br>这些规则仅适用于出站流量从该设备。 使用流量筛选器块从企业网络到客户端的入站的流量。 </li></ul> |
| **每应用 VPN** | 每应用 VPN 就好像基于应用的流量筛选器中，但它进入越远结合和基于应用的流量筛选器的应用程序触发器，以便 VPN 连接限制为特定的应用程序，而不是在 VPN 客户端上的所有应用程序。 在应用启动时，该功能自动启动。 |
| **自定义 IPsec 加密算法的支持** | 始终启用 VPN 支持使用 RSA 和椭圆曲线加密 – 基于自定义加密算法以满足严格的政府或组织的安全策略。 |
| **可扩展身份验证协议 (EAP) 的本机支持** | 始终启用 VPN 本机支持 EAP，允许你使用不同的一组 Microsoft 和第三方 EAP 类型的身份验证工作流的一部分。 EAP 提供安全身份验证，具体取决于以下身份验证类型：<ul><li>用户名和密码</li><li>智能卡 （物理和虚拟）</li><li>用户证书</li><li>Windows Hello 企业版</li><li>通过 EAP 半径集成的 MFA 支持</li></ul>应用程序供应商控制第三方 UWP VPN 插件的身份验证方法，尽管它们的可用选项，包括自定义凭据类型和 OTP 支持数组。 |
---

## VPN 连接

以下是始终启用 VPN 连接的主要改进：

| 密钥改进 | 说明 |
| ---- | ---- |
| **始终启用** | 始终启用是可以使活动的 VPN 配置文件，来自动连接并保持连接触发器上基于 Windows 10 功能，即，在用户登录、 网络状态更改或活动的设备屏幕。 始终启用也集成到连接待机的体验，若要最大化电池使用时间。 |
| **应用程序触发** | 你可以在一组特定的应用程序启动时自动连接的 Windows 10 中配置 VPN 配置文件。 你可以配置桌面版和 UWP 应用程序，以触发 VPN 连接。 |
| **基于名称的触发** | 与始终启用 VPN，你可以定义规则，以便特定域名查询触发 VPN 连接。 Windows 10 现在支持基于名称的触发的加入域和已加入域的计算机 （在此之前，仅加入域的计算机已支持）。 |
| **受信任的网络检测** | 始终启用 VPN 包括此功能，以确保用户已连接到公司边界内信任的网络不会触发 VPN 连接。 可以将此功能与之前所述来提供无缝"只能连接时需要"用户体验的触发任何的方法组合。 |
| **[设备隧道](../vpn-device-tunnel-config.md)** | 始终启用 VPN 使你能够创建设备或计算机的专用的 VPN 配置文件。 与_用户隧道_，这仅连接到设备或计算机用户登录后，_设备隧道_允许 VPN 建立连接之前在用户登录。 设备隧道和用户隧道独立运行其 VPN 配置文件可以在同一时间进行连接，可以根据需要使用不同的身份验证方法和其他 VPN 配置设置。 |
---

## 网络

以下是一些始终启用 VPN 网络改进：

| 密钥改进 | 说明 |
| ---- | ---- |
| **双堆栈支持为 IPv4 和 IPv6** | 始终启用 VPN 本机支持双堆栈方法中的使用 IPv4 和 IPv6。 它具有没有特定的依赖项上一个协议通过其他，以允许结合支持将来的 IPv6 网络需要的最大 IPv4/IPv6 应用程序兼容性。<p>**_注意。_** 在开始之前，请确保在 VPN 服务器上启用 IPv6。 否则为不能建立的连接，并显示一条错误消息。|
| **特定于应用程序的路由策略** | 除了定义全局 VPN 连接路由策略 internet 和 intranet 流量分离，很可能添加路由策略来控制拆分隧道使用或强制隧道配置基于每个应用程序。 此选项将为你提供更精确地控制哪些应用允许通过 VPN 隧道哪些资源与交互。 |
| **排除路由** | 始终启用 VPN 支持指定明确地控制来定义哪些流量应遍历 VPN 仅并不会通过物理网络接口的路由行为的排除路由的功能。<p><p>_**备注。**_<br>排除路由目前适用于与客户端，位于同一子网内的流量，LinkLocal。<br>-在拆分隧道设置仅适用排除路由。 |
---

## 配置和兼容性 

以下是一些始终启用 VPN 的配置和兼容性改进：

| 密钥改进 | 说明 |
| ---- | ---- |
| **第三方 VPN 网关兼容性** | 始终启用 VPN 客户端不需要使用基于 Microsoft 的 VPN 网关操作。 通过 IKEv2 协议的支持，客户端促进互操作性第三方 VPN 网关的支持此行业标准隧道类型。 你还可以通过使用 UWP VPN 插件结合自定义隧道类型，而不会影响始终启用 VPN 平台功能与优点实现与第三方 VPN 网关互操作性。<p><p>_**注意。**_<br>请咨询你网关或第三方后端装置的供应商上配置和兼容性始终启用 VPN 和使用 IKEv2 设备隧道。 |
| **行业标准 IKEv2 VPN 协议支持** | 始终启用 VPN 客户端支持 IKEv2、 当今之一使用最广泛的行业标准隧道协议。 这种兼容性最大化与第三方 VPN 网关互操作性。 |
| **平台支持** | 已加入域的 AAlways 上 VPN 支持、 已加入域的 （工作组） 或 Azure AD – 设备以允许在两个企业，并且自带设备办公 (BYOD) 方案。 此外，始终启用 VPN 是所有 Windows 版本中可用。 |
| **不同的管理和部署机制** | 你可以使用许多管理和部署机制来管理 VPN 设置 （称为_VPN 配置文件_），包括 Windows PowerShell，System Center Configuration Manager、 Intune （或第三方移动设备管理 [MDM] 工具） 和 Windows配置设计器。 这些选项可简化始终启用 VPN 无论你使用的客户端管理工具的配置。 |
| **标准化的 VPN 配置文件定义** | 始终启用 VPN 支持配置使用标准 XML 配置文件 (ProfileXML)，提供了一种标准配置模板格式，大部分管理和部署工具集使用。 |
---


## 后续步骤

- [了解的一些高级始终启用 VPN 功能](deploy/always-on-vpn-adv-options.md)

- [了解有关始终启用 VPN 技术的详细信息](always-on-vpn-technology-overview.md)

- [规划始终启用 VPN 部署开始菜单](deploy/always-on-vpn-deploy-deployment.md)



---
