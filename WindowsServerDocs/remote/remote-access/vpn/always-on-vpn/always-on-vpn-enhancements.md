---
title: 始终启用 VPN 增强功能
description: Always On VPN 对过去的 Windows VPN 解决方案有许多好处。 集成、安全性、连接性、网络控制和兼容性方面的重要改进 Always On VPN 与 Microsoft 的云优先、移动优先的远景规划相一致。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 65bf9f732e28e5974a3880c7d07ab7262de18144
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871378"
---
# <a name="always-on-vpn-enhancements"></a>始终启用 VPN 增强功能

>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10

- [**以前**了解 Always On VPN 功能](../vpn-map-da.md)
- [**一个**了解 Always On VPN 技术](always-on-vpn-technology-overview.md)

Always On VPN 对过去的 Windows VPN 解决方案有许多好处。 以下重要改进与 Microsoft 的云优先、移动优先愿景 Always On 的 VPN 一致：

- **平台集成：** Always On VPN 改进了与 Windows 操作系统和第三方解决方案的集成，为无数种高级连接方案提供了可靠的平台。

- **安全**Always On VPN 提供了新的高级安全功能来限制流量类型、哪些应用程序可以使用 VPN 连接，以及可用于启动连接的身份验证方法。 在大多数情况下，连接处于活动状态时，确保连接安全尤其重要。 有关更多详细信息，请参阅[VPN 身份验证选项](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-authentication)。

- **VPN 连接：** 无需设备隧道即可 Always On VPN 提供自动触发器功能。 在 Always On VPN 之前，无法通过用户或设备身份验证触发自动连接。  

- **网络控制：** Always On VPN 允许管理员以更精细的级别指定路由策略，甚至可指定单个应用程序，这非常适合需要特殊远程访问的业务线（LOB）应用程序。  Always On VPN 也完全兼容 Internet 协议版本4（IPv4）和版本6（IPv6）。 与 DirectAccess 不同，IPv6 没有特定的依赖关系。

  >[!NOTE]
  >在开始之前，请确保在 VPN 服务器上启用 IPv6。 否则，无法建立连接，并显示一条错误消息。

- **配置和兼容性：** 可以通过多种方式部署和管理 Always On VPN，这为 Always On VPN 提供了多项与其他 VPN 客户端软件的好处。

## <a name="platform-integration"></a>平台集成

Microsoft 在 Always On VPN 中引入或改进了以下集成功能：


| 密钥改进   |  描述  |
|----------------|---|
| **[Windows 信息保护（WIP）](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | 与 WIP 的集成允许网络策略实施来确定是否允许流量通过 VPN。 如果用户配置文件处于活动状态并且应用了 WIP 策略，则会自动触发 Always On VPN 以进行连接。 此外，在使用 WIP 时，无需在 VPN 配置文件中单独指定 AppTriggerList 和 TrafficFilterList 规则（除非你需要更高级的配置），因为 WIP 策略和应用程序列表会自动生效。 |
|**[Windows Hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** |Always On VPN 本机支持 Windows Hello 企业版（在基于证书的身份验证模式下），以便为登录到计算机和连接到 VPN 提供无缝单一登录体验。 因此，VPN 连接不需要辅助身份验证（用户凭据），因此可以将 Always On 连接与 Windows Hello 企业版身份验证配合使用。 |
| **[Microsoft Azure 条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)**  |Always On VPN 客户端可以与 Azure 条件访问平台集成，以强制执行多重身份验证（MFA）、设备符合性或二者的组合。 当符合条件性访问策略时，Azure Active Directory （Azure AD）会发出一个短期（默认为60分钟） IP 安全（IPsec）身份验证证书，然后使用该证书对 VPN 网关进行身份验证。 设备符合性使用 System Center Configuration Manager/Intune 相容性策略，这些策略可能包括设备运行状况证明状态作为连接符合性检查的一部分。|
|  **Azure MFA** |与 Azure MFA 的远程身份验证拨入用户服务（RADIUS）服务和网络策略服务器（NPS）扩展结合使用时，VPN 身份验证可以使用强 MFA。 | **第三方 VPN 插件**  | 使用通用 Windows 平台（UWP），第三方 VPN 提供程序可以为所有 Windows 10 设备创建单个应用程序。 UWP 跨设备提供有保障的核心 API 层，消除了通常与写入内核级驱动程序相关的问题和问题的复杂性。 目前，Windows 10 个 UWP VPN 插件适用于[脉冲安全](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp)、 [F5 访问](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0)、 [Check Point 胶囊 Vpn](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj)、 [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc)、 [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)和[GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3);毫无疑问，以后会出现其他情况。 |

## <a name="security"></a>安全性

主要的安全性改进在以下方面：

| 密钥改进 | 描述  |
|---|---|
| **流量筛选器** | 通过流量筛选器，你可以指定客户端策略来确定允许哪些流量进入企业网络。 通过这种方式，管理员可以在 VPN 接口上应用应用或流量限制，从而限制其对特定源、目标端口和 IP 地址的使用。 提供两种类型的筛选规则：<ul><li>**基于应用的规则。** 基于应用的防火墙规则基于指定应用程序的列表，以便仅允许来自这些应用的流量通过 VPN 接口。</li><li>**基于流量的规则。** 基于流量的防火墙规则基于网络要求，例如端口、地址和协议。 仅将这些规则用于符合这些特定条件的流量，才能通过 VPN 接口。<p><p>***注意：***<br>这些规则仅适用于来自设备的出站流量。 使用流量筛选器会阻止来自企业网络的入站流量发送到客户端。 </li></ul> |**每应用 VPN**|基于应用的 VPN 与使用基于应用的流量筛选器类似，但将应用程序触发器与基于应用的流量筛选器组合在一起，以便将 VPN 连接限制为特定的应用程序，而不是 VPN 客户端上的所有应用程序。 此功能在应用程序启动时自动启动。|
|  **支持自定义 IPsec 加密算法**   |  Always On VPN 支持使用基于 RSA 和椭圆曲线加密的自定义加密算法来满足严格的政府或组织安全策略。|
| **本机可扩展身份验证协议（EAP）支持** |Always On VPN 本身支持 EAP，这允许使用一组不同的 Microsoft 和第三方 EAP 类型作为身份验证工作流的一部分。 EAP 提供基于以下身份验证类型的安全身份验证：<ul><li>用户名和密码</li><li>智能卡（物理和虚拟）</li><li>用户证书</li><li>Windows Hello 企业版</li><li>通过 EAP RADIUS 集成的方式支持 MFA</li></ul>应用程序供应商控制第三方 UWP VPN 插件身份验证方法，虽然它们具有可用选项的数组，包括自定义凭据类型和 OTP 支持。|

## <a name="vpn-connectivity"></a>VPN 连接性

下面是 Always On VPN 连接的主要改进：

|  密钥改进  | 描述 |
|---|---|
|                    **Always On**                    |                                                                                          Always On 是一种 Windows 10 功能，它使活动的 VPN 配置文件能够自动连接，并根据触发器（即用户登录、网络状态更改或设备屏幕活动）保持连接状态。 Always On 也集成到连接的备用体验中，以最大限度地延长电池寿命。                                                                                           |
|             **触发应用程序**              |                                                                                                                                          你可以将 Windows 10 中的 VPN 配置文件配置为在启动指定的一组应用程序时自动连接。 你可以配置桌面和 UWP 应用程序来触发 VPN 连接。                                                                                                                                          |
|              **基于名称的触发**              |                                                                                                            通过 Always On VPN，可以定义规则，以便特定域名查询触发 VPN 连接。 Windows 10 现在支持已加入域的计算机和未加入域的计算机的基于名称的触发（以前仅支持未加入域的计算机）。                                                                                                            |
|            **受信任的网络检测**            |                                                                                    Always On VPN 包含此功能，以确保在用户连接到企业边界内的受信任网络时不触发 VPN 连接。 可以将此功能与前面提到的任何触发方法结合使用，以提供无缝的 "仅在需要时连接" 用户体验。                                                                                     |
| **[设备隧道](../vpn-device-tunnel-config.md)** | Always On VPN 使你能够为设备或计算机创建专用 VPN 配置文件。 与仅在用户登录到设备或计算机之后进行连接的*用户隧道*不同，通过*设备隧道*，VPN 可以在用户登录之前建立连接。 设备隧道和用户隧道独立操作其 VPN 配置文件，可以同时连接，并且可以根据需要使用不同的身份验证方法和其他 VPN 配置设置。 |

## <a name="networking"></a>网络

下面是 Always On VPN 中的一些网络改进：


|              密钥改进              |                                                                                                                                                                                                                描述                                                                                                                                                                                                                |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **对 IPv4 和 IPv6 的双堆栈支持**  | Always On VPN 本机支持在双堆栈方法中同时使用 IPv4 和 IPv6。 它在一个协议上没有特定的依赖关系，这允许将最大 IPv4/IPv6 应用程序兼容性与支持用于未来的 IPv6 网络需求结合起来。<p>***注意：*** 在开始之前，请确保在 VPN 服务器上启用 IPv6。 否则，无法建立连接，并显示一条错误消息。 |
| **特定于应用程序的路由策略** |                            除了为 internet 和 intranet 通信分离定义全局 VPN 连接路由策略外，还可以添加路由策略来控制拆分隧道的使用或强制每个应用程序的隧道配置。 此选项使你可以更精细地控制允许哪些应用与通过 VPN 隧道的资源进行交互。                             |
|           **排除路由**            |                 Always On VPN 支持指定排除路由，这些路由专门控制路由行为，以定义哪些流量应仅遍历 VPN，而不是通过物理网络接口。<p><p>***注意：***<br>-排除路由当前适用于与客户端位于同一子网中的流量，例如，LinkLocal。<br>-排除路由仅适用于拆分隧道设置。                  |

## <a name="configuration-and-compatibility"></a>配置和兼容性 

下面是 Always On VPN 中的一些配置和兼容性改进：


|                 密钥改进                  |                                                                                                                                                                                                                                                                                                                       描述                                                                                                                                                                                                                                                                                                                       |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    **第三方 VPN 网关兼容性**     | Always On VPN 客户端不需要使用基于 Microsoft 的 VPN 网关来运行。 通过支持 IKEv2 协议，客户端可帮助与支持此行业标准隧道类型的第三方 VPN 网关进行互操作。 你还可以通过使用包含自定义隧道类型的 UWP VPN 插件来实现与第三方 VPN 网关的互操作性，而无需牺牲 Always On VPN 平台功能和优势。<p><p>***注意：***<br>向网关或第三方后端设备供应商咨询使用 IKEv2 Always On VPN 和设备隧道的配置和兼容性。 |
| **行业标准 IKEv2 VPN 协议支持** |                                                                                                                                                                                                                              Always On VPN 客户端支持 IKEv2，这是目前最广泛使用的行业标准隧道协议之一。 此兼容性最大化了与第三方 VPN 网关的互操作性。                                                                                                                                                                                                                               |
|               **平台支持**               |                                                                                                                                                                                                           VPN 上的 AAlways 支持已加入域（工作组）的已加入域的设备，或 Azure AD 联接的设备，允许两个企业和自带设备（BYOD）方案。 此外，Always On VPN 在所有 Windows 版本中都可用。                                                                                                                                                                                                           |
| **多样化的管理和部署机制** |                                                                                                                                 你可以使用许多管理和部署机制来管理 VPN 设置（称为*vpn 配置文件*），包括 windows PowerShell、System Center Configuration Manager、Intune 或第三方移动设备管理（MDM）工具和 Windows配置设计器。 无论使用何种客户端管理工具，这些选项都可以简化 Always On VPN 的配置。                                                                                                                                 |
|     **标准化 VPN 配置文件定义**      |                                                                                                                                                                                                                                  Always On VPN 支持使用标准 XML 配置文件（ProfileXML）的配置，并提供了大多数管理和部署工具集使用的标准配置模板格式。                                                                                                                                                                                                                                   |

## <a name="next-steps"></a>后续步骤

- [了解一些高级 Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [了解有关 Always On VPN 技术的详细信息](always-on-vpn-technology-overview.md)

- [开始规划 Always On 的 VPN 部署](deploy/always-on-vpn-deploy-deployment.md)