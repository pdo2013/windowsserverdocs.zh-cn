---
title: 动态主机配置协议 (DHCP)
description: 本主题提供的动态主机配置协议 (DHCP) 在 Windows Server 2016 中的简要概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08b07e902486ae633b30949270e15f8bf94afaaf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857488"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>动态主机配置协议 (DHCP)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

有关 Windows Server 2016 中 DHCP 的简要概述，可以使用本主题。

>[!NOTE]
>除了本主题提供了以下 DHCP 文档。
>
>- [什么是中 DHCP 的新增功能](What-s-New-in-DHCP.md)
>- [使用 Windows PowerShell 将 DHCP 部署](dhcp-deploy-wps.md)

动态主机配置协议 (DHCP) 是使用其 IP 地址和其他相关的配置信息如子网掩码和默认网关将自动提供 Internet 协议 (IP) 主机的客户端/服务器协议。 Rfc 2131 和 2132年定义 DHCP 作为 Internet 工程任务组 (IETF) 标准基于上 Bootstrap 协议 (BOOTP)，一种协议与 DHCP 共享许多实现细节。 DHCP 允许主机以从 DHCP 服务器获取所需的 TCP/IP 配置信息。

Windows Server 2016 包括 DHCP 服务器，这是你可以部署到租用的 IP 地址在网络和其他信息的 DHCP 客户端的可选网络服务器角色。 所有基于 Windows 的客户端操作系统包含 DHCP 客户端的 TCP/IP，并默认启用 DHCP 客户端。

## <a name="why-use-dhcp"></a>为什么要使用 DHCP？

基于 IP 的网络上每个设备必须具有唯一的单播 IP 地址来访问网络及其资源。 未安装 DHCP，对于新计算机或从一个子网移到另一种的计算机的 IP 地址必须手动配置;必须手动回收从网络中删除的计算机的 IP 地址。

使用 DHCP，这整个过程中自动执行并集中管理。 DHCP 服务器维护的 IP 地址池，并将地址租借给任何已启用 DHCP 的客户端在网络上启动时。 IP 地址是动态的 （租用） 而不是静态 （永久分配），因为不能再正在使用的地址会自动返回到重新分配的池。

网络管理员建立维护的 TCP/IP 配置信息并提供给启用 DHCP 的客户端租约产品/服务的窗体中的地址配置的 DHCP 服务器。 DHCP 服务器将配置信息存储在包含的数据库：

- 在网络上的所有客户端的有效 TCP/IP 配置参数。

- 有效的 IP 地址保留在一个池，以便分配给客户端，以及排除地址。

- 与特定的 DHCP 客户端关联的保留的 IP 地址。 这允许一致分配给单个 DHCP 客户端的单个 IP 地址。

- 租约持续时间或需要提供租约续订之前，则可以使用为其 IP 地址的时间长度。

已启用 DHCP 的客户端，在接受提供的租约时接收：

- 连接到的子网是有效 IP 地址。  
  
- 请求的 DHCP 选项，这些都是其他参数的 DHCP 服务器配置为将分配给客户端。 DHCP 选项的一些示例包括路由器 （默认网关）、 DNS 服务器和 DNS 域名。

## <a name="benefits-of-dhcp"></a>DHCP 的优点

DHCP 提供以下优势。

- **可靠的 IP 地址配置**。 DHCP 最大程度减少配置错误引起的手动的 IP 地址配置，如输入错误，或解决冲突引起的同时在多台计算机的 IP 地址的分配。

- **减少网络管理**。 DHCP 包括以下功能，可减少网络管理：

    - 集中式、 自动的 TCP/IP 配置。

    - 定义从一个中心位置的 TCP/IP 配置的功能。

    - 能够通过 DHCP 选项分配一系列完整的其他 TCP/IP 配置值。

    - 例如，用于移动到不同的位置的无线网络的便携式设备必须频繁更新的客户端的 IP 地址更改高效处理。

    - 通过使用 DHCP 中继代理，而不需要将为每个子网上的 DHCP 服务器的初始 DHCP 消息的转发。

