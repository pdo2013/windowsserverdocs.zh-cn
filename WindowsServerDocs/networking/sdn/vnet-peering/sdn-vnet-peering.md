---
title: 虚拟网络对等
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: ccdcbb953939345ef5e9a45dff87fc7af62eb7bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355487"
---
# <a name="virtual-network-peering"></a>虚拟网络对等

>适用于：Windows Server

利用虚拟网络对等互连，无缝连接两个虚拟网络。 对等互连后，出于连接目的，虚拟网络显示为一。 

使用虚拟网络对等互连的优点包括：

-   对等互连虚拟网络中虚拟机之间的流量仅通过*专用*IP 地址通过主干基础结构路由。 虚拟网络之间的通信不需要公共 Internet 或网关。

-   不同虚拟网络中的资源之间的低延迟、高带宽连接。

-   一个虚拟网络中的资源与另一个虚拟网络中的资源进行通信的能力。

-   创建对等互连时，任一虚拟网络中的资源不会造成停机。

## <a name="requirements-and-constraints"></a>要求和约束

虚拟网络对等互连有几个要求和限制：

- 对等互连虚拟网络必须：

  -   具有不重叠的 IP 地址空间

  -   由同一网络控制器管理

- 将虚拟网络与另一个虚拟网络对等互连后，不能在地址空间中添加或删除地址范围。

  >[!TIP]
  >如果需要添加地址范围：<ol><li>删除对等互连。</li><li>添加地址空间。</li><li>再次添加对等互连。</li></ol>

- 由于虚拟网络对等互连在两个虚拟网络之间进行，因此不存在跨对等互连的派生的可传递关系。 例如，如果对等 virtualNetworkA 与 virtualNetworkB 和 virtualNetworkB 结合 virtualNetworkC，则 virtualNetworkA 不会获得对等互连 with virtualNetworkC。

  [此处为 image]

## <a name="connectivity"></a>连接性

对等互连虚拟网络后，任一虚拟网络中的资源可以直接与对等互连虚拟网络中的资源进行连接。

-   对等互连虚拟网络中虚拟机之间的网络延迟与单个虚拟网络中的延迟相同。

-   网络吞吐量取决于虚拟机允许的带宽。 对等互连中的带宽没有任何其他限制。

-   对等互连虚拟网络中虚拟机之间的流量直接通过主干基础结构路由，而不通过网关或公共 Internet 路由。

-   虚拟网络中的虚拟机可访问对等互连虚拟网络中的内部负载均衡器。

如果需要，可以在虚拟网络中应用访问控制列表（Acl），阻止访问其他虚拟网络或子网。 如果打开对等互连虚拟网络之间的完全连接（这是默认选项），则可以将 Acl 应用到特定子网或虚拟机，以便阻止或拒绝特定访问。 若要了解有关 Acl 的详细信息，请参阅[使用访问控制列表（acl）管理数据中心网络流量流](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

## <a name="service-chaining"></a>服务链

你可以将用户定义的路由配置为指向对等互连虚拟网络中的虚拟机作为下一个跃点 IP 地址，以启用服务链接。 使用服务链，可以通过用户定义的路由将流量从一个虚拟网络定向到对等互连虚拟网络中的虚拟设备。

你可以部署中心辐射型网络，其中，中心虚拟网络可托管基础结构组件，如网络虚拟设备。 所有辐射虚拟网络与中心虚拟网络对等。 流量可以流经中心虚拟网络中的网络虚拟设备。

虚拟网络对等互连使用户定义的路由中的下一跃点成为对等互连虚拟网络中虚拟机的 IP 地址。 若要详细了解用户定义的路由，请参阅[在虚拟网络上使用网络虚拟设备](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn)。

## <a name="gateways-and-on-premises-connectivity"></a>网关和本地连接

每个虚拟网络无论是否与另一个虚拟网络对等互连，都可以有自己的网关连接到本地网络。 如果对等互连虚拟网络，还可以在对等互连虚拟网络中将网关配置为本地网络的传输点。 在这种情况下，使用远程网关的虚拟网络不能有自己的网关。 一个虚拟网络只能有一个网关，该网关可以是本地网关或远程网关（在对等互连虚拟网络中）。

## <a name="monitor"></a>监视器

对两个虚拟网络进行对等互连时，必须为对等互连中的每个虚拟网络配置对等互连。

你可以监视对等互连连接的状态，这可能处于以下状态之一：

-   **起始**创建从第一个虚拟网络到第二个虚拟网络的对等互连时显示的。

-   **联机**在创建从第二个虚拟网络到第一个虚拟网络的对等互连后显示。 第一个虚拟网络的对等互连状态从 "已启动" 更改为 "已连接"。 在成功建立虚拟网络对等互连之前，这两个虚拟网络对等机的状态必须为 "已连接"。

-   **式**当一个虚拟网络与另一个虚拟网络断开连接时显示。

[状态的信息图]

## <a name="next-steps"></a>后续步骤
[配置虚拟网络对等互连](sdn-configure-vnet-peering.md)：在此过程中，你将使用 Windows PowerShell 查找 HNV 提供程序逻辑网络，以创建两个虚拟网络，每个虚拟网络具有一个子网。 你还可以在两个虚拟网络之间配置对等互连。

