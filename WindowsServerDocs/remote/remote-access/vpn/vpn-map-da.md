---
title: 始终启用 VPN 功能
description: 在本主题中，你了解有关的特性和功能始终启用 VPN。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066851"
---
# 始终启用 VPN 特性和功能

>适用于： Windows Server \(Semi-Annual Channel\)，Windows Server 2016、 Windows 10

& #171; [**以前：** 始终启用 VPN 部署适用于 Windows Server 和 Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
& #187;[**下一步：** 了解始终启用 VPN 增强](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

在本主题中，你了解的特性和功能的始终启用 VPN。  下表中不是详尽列表，但是，包括一些最常见的特性和功能在远程访问解决方案中使用。 

>[!TIP]
>如果你当前使用 DirectAccess，我们建议你调查仔细来确定它解决你在迁移之前的远程访问需求的所有窗体 DirectAccess 到始终启用 VPN 始终启用 VPN 功能。  

| 功能区域 | 始终启用 VPN  |
| ---- | ---- |
| 无缝、 透明连接到公司网络。 | 你可以配置始终启用 VPN 自动触发的具体取决于应用程序启动或命名空间解析请求支持。<p><p>使用定义：<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/DomainNameInformationList/AutoTrigger** |
| 使用专用的基础结构隧道，以便为用户未登录到公司网络连接。 | 你可以通过 VPN 配置文件中使用的设备隧道功能来实现此功能。<p><p>_**注意。**_<br>仅可以在计算机证书身份验证中使用 IKEv2 的已加入域的设备上配置设备隧道。<p><p>使用定义：<br>**VPNv2/ProfileName/DeviceTunnel** |
| 管理外，支持远程连接到客户端位于公司网络上的管理系统的使用。 | 你可以通过设备隧道功能结合配置 VPN 连接的 VPN 配置文件中为动态注册分配给使用内部 DNS 服务 VPN 接口的 IP 地址来实现此功能。<p><p>_**注意。**_<br>如果你打开设备隧道配置文件中的流量筛选器，然后设备隧道拒绝的入站的流量 （从公司网络上向客户端）。<p><p>使用定义：<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| 落返回时，客户端隐藏防火墙或代理服务器。 | 你可以回退到 SSTP （从 IKEv2) 通过使用配置 VPN 配置文件中的自动的隧道协议类型。<p><p>_**注意。**_<br>用户隧道支持 SSTP 和 IKEv2，并且仅与 SSTP 回退不支持的设备隧道支持 IKEv2。<p>使用定义：<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| 结束到边缘访问模式下的支持。 | 始终启用 VPN 提供使用隧道策略需要身份验证和加密，直到到达的 VPN 网关连接到公司资源。 默认情况下，隧道会话终止于充当 IKEv2 网关，提供最终到边缘安全 VPN 网关。 |
| 对计算机证书身份验证的支持。 | IKEv2 协议类型以始终启用 VPN 平台的一部分，专门用于支持使用计算机或计算机的证书进行 VPN 身份验证形式提供。<p><p>_**注意。**_<br>IKEv2 设备隧道唯一支持的协议并没有 SSTP 回退支持选项。 <p>使用定义：<br>**VPNv2/ProfileName/NativeProfile/身份验证/MachineMethod** |
| 使用安全组来限制到特定的客户端的远程访问功能。 | 你可以配置始终启用 VPN 时使用半径，其中包括使用安全组来控制 VPN 访问支持精细的授权。 |
| 支持边缘防火墙或 NAT 设备后面的服务器。 | 始终启用 VPN 使你能够使用 IKEv2 和 SSTP 等完全支持 NAT 设备或 edge 防火墙后的 VPN 网关使用的协议。<p><p>_**注意。**_<br>用户隧道支持 SSTP 和 IKEv2，并且仅与 SSTP 回退不支持的设备隧道支持 IKEv2。 |
| 若要确定 intranet 连接到公司网络连接时的能力。 | 受信任的网络检测能够检测到公司网络连接，并且它基于特定于连接的 DNS 后缀分配给网络接口和网络配置文件的评估。<p><p>使用定义：<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| 使用网络访问保护 (NAP) 的合规性。 | 始终启用 VPN 客户端可与 Azure MFA、 设备合规性或两者的组合强制执行的条件访问。 当符合条件访问策略时，Azure AD 颁发的短期 （默认情况下，60 分钟） IPsec 身份验证证书，然后可以使用客户端对 VPN 网关进行身份验证。 设备合规性利用 System Center Configuration Manager/Intune 合规性策略，可以包含设备运行状况证明状态。 此时，Azure VPN 条件访问提供到现有的 NAP 解决方案，最接近的替换，但没有任何形式的修正服务或隔离网络功能。 有关详细信息，请参阅[VPN 和条件访问](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access)。<p>使用定义：<br>**VPNv2/ProfileName/DeviceCompliance** |
| 定义在用户登录前访问哪些管理服务器的功能。 | 你可以通过使用设备隧道功能 （适用于版本 1709 – 仅 IKEv2） VPN 配置文件结合流量筛选器来控制可以通过访问企业网络上的哪些管理系统中实现此功能始终启用 VPN设备隧道。<p><p>_**注意。**_<br>如果你打开设备隧道配置文件中的流量筛选器，然后设备隧道拒绝的入站的流量 （从公司网络上向客户端）。<p>使用定义：<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## 其他功能

本部分中的每个项目是使用情况或为其始终启用 VPN 已改进功能的常用的远程访问功能 — 通过功能扩展或消除了以前的限制。


| 功能区域 | 始终启用 VPN  |
| ---- | ---- |
| 企业版 Sku 要求与已加入域的设备。 | 始终启用 VPN 支持已加入域的、 已加入域的 （工作组） 或 Azure AD – 设备以允许企业版和 BYOD 方案。 始终启用 VPN 是可在所有 Windows 版本中，并且适用于通过支持 UWP VPN 插件的第三方的平台功能。<p><p>_**注意。**_<br>仅可以在运行 Windows 10 企业版或教育版版本 1709年或更高版本的已加入域的设备上配置设备隧道。 没有支持的设备隧道的第三方控件。 |
| 为 IPv4 和 IPv6 支持。 | 通过始终启用 VPN，用户可以访问公司网络上的 IPv4 和 IPv6 资源。 始终启用 VPN 客户端使用双堆栈方法不会专门依赖于 IPv6 或 VPN 网关需要提供 NAT64 或 DNS64 翻译服务。 |
| 支持的双因素或 OTP 身份验证。 |始终启用 VPN 平台在本机支持 EAP，允许使用的各种 Microsoft 和第三方 EAP 类型的身份验证工作流的一部分。 始终启用 VPN 专门用于支持智能卡 （物理和虚拟） 和 Windows Hello 企业证书来满足的双因素身份验证要求。 此外，始终启用 VPN 支持 MFA （不受本地支持，仅支持第三方插件） 通过 OTP 通过 EAP 半径集成。<p><p>使用定义：<br>**VPNv2/ProfileName/NativeProfile/身份验证** |
| 多个域和林的支持。 | 始终启用 VPN 平台无需依赖有在 Active Directory 域服务 (AD DS) 林或域拓扑 （或相关功能的/架构级别） 上，因为它不需要 VPN 客户端需要域加入到函数。 组策略不因此的依赖项，因为你不使用它在客户端配置期间定义 VPN 配置文件设置。 在 Active Directory 授权集成是必需的你可以通过 EAP 身份验证和授权流程的一部分的半径实现它。 |
| 这两个拆分的支持和强制隧道 internet/intranet 流量分离。 | 你可以配置始终启用 VPN 支持这两个强制隧道 （默认运行模式） 和本机拆分隧道。 始终启用 VPN 提供了其他的特定于应用程序的路由策略的粒度。<p><p>_**注意。**_<br>由用户隧道仅支持。<p><p>使用定义：<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/应用/RoutingPolicyType** |
| 多个协议的支持。 | 始终启用 VPN 可以配置为支持 SSTP 本机是否需要从 IKEv2 回退的安全套接字层。<p><p>_**注意。**_<br>用户隧道支持 SSTP 和 IKEv2，并且仅与 SSTP 回退不支持的设备隧道支持 IKEv2。  |
| 提供公司连接状态的连接助手。 | 始终启用 VPN 使用本机网络连接助手完全集成，并提供从查看所有网络接口的连接状态。 对于出现的 Windows 10 创意者更新 （版本 1703年）、 VPN 连接状态和用户隧道的 VPN 连接控制现已通过网络浮出控件 （适用于 Windows 内置 VPN 客户端），以及。 |
| 使用短名称、 完全限定的域名 (FQDN) 和 DNS 后缀的公司资源的名称解析。 | 始终启用 VPN 本身可以定义为 VPN 连接和 IP 地址分配过程，包括公司资源的短名称、 Fqdn 或整个 DNS 命名空间的名称解析的一部分的一个或多个 DNS 后缀。 始终启用 VPN 还支持使用的名称解析策略表，提供特定于命名空间的分辨率的精度。<p><p>_**注意。**_<br>避免使用全局后缀，因为它们会干扰 shortname 分辨率时使用名称解析策略表。<p><p>使用定义：<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/DomainNameInformationList** |
---



## 后续步骤

- [了解有关始终启用 VPN 增强功能的详细信息](always-on-vpn/always-on-vpn-enhancements.md)

- [了解的一些高级始终启用 VPN 功能](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [了解有关始终启用 VPN 技术的详细信息](always-on-vpn/always-on-vpn-technology-overview.md)

- [规划始终启用 VPN 部署开始菜单](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
