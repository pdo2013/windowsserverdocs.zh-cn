---
title: 虚拟网络对等
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: aab4ec7c69ec5b52eae926cd1065d777415b1124
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446217"
---
# <a name="virtual-network-peering"></a>虚拟网络对等

>适用于：Windows Server

虚拟网络对等互连可以无缝连接两个虚拟网络。 对等互连，出于连接目的后, 的虚拟网络会显示为一个。 

使用虚拟网络对等互连的优点包括：

-   在对等互连的虚拟网络中的虚拟机之间的流量路由通过主干基础结构*专用*仅 IP 地址。 虚拟网络之间的通信不需要公共 Internet 或网关。

-   不同虚拟网络中资源之间的低延迟、 高带宽连接。

-   一个虚拟网络中的资源进行不同的虚拟网络中的资源通信的功能。

-   为虚拟网络时创建的对等互连中的资源无需停机。

## <a name="requirements-and-constraints"></a>要求和约束

虚拟网络对等互连有几个要求和约束：

- 对等互连的虚拟网络必须：

  -   具有非重叠 IP 地址空间

  -   由同一个网络控制器管理

- 一旦建立对等互连虚拟网络与另一个虚拟网络不能添加或删除地址空间中的地址范围。

  >[!TIP]
  >如果你需要添加地址范围：<ol><li>删除对等互连。</li><li>添加地址空间。</li><li>添加对等互连试。</li></ol>

- 由于虚拟网络对等互连是两个虚拟网络之间，对等互连之间存在不派生可传递关系。 例如，如果建立 virtualNetworkA 与 virtualNetworkB 和 virtualNetworkB 与 virtualNetworkC 对等互连，则 virtualNetworkA 不会不获取对等互连与 virtualNetworkC。

  [此处映像]

## <a name="connectivity"></a>连接性

虚拟网络对等互连后，虚拟网络中的资源可直接连接对等互连的虚拟网络中的资源。

-   对等互连的虚拟网络中的虚拟机之间的网络延迟是单个虚拟网络中的延迟相同。

-   网络吞吐量取决于允许的虚拟机的带宽。 没有任何其他对等互连的带宽限制。

-   在对等互连的虚拟网络中的虚拟机之间的流量直接通过主干基础结构，不通过网关或公共 Internet 路由。

-   虚拟网络中的虚拟机可以访问内部负载均衡器对等互连的虚拟网络中。

您可以在必要时阻止访问其他虚拟网络或子网是虚拟网络中应用的访问控制列表 (Acl)。 如果打开对等互连的虚拟网络 （这是默认选项） 之间的完全连接，可以将 Acl 应用到特定子网或虚拟机，以便阻止或拒绝特定访问权限。 若要了解有关 Acl 的详细信息，请参阅[使用访问控制列表 (Acl) 管理数据中心网络流量流到](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

## <a name="service-chaining"></a>链接的服务

可以为下一跃点 IP 地址，以便启用服务链指向对等互连的虚拟网络中的虚拟机的用户定义路由进行配置。 链接的服务可以将流量定向到从一个虚拟网络到虚拟设备，在对等互连的虚拟网络中，通过用户定义的路由。

可以部署中心辐射型网络，中心虚拟网络可以在其中托管基础结构组件，如网络虚拟设备。 所有辐射虚拟网络与中心虚拟网络对等都互连。 流量可以流经中心虚拟网络中的网络虚拟设备。

虚拟网络对等互连使对等互连的虚拟网络中的虚拟机的 IP 地址的用户定义路由中的下一跃点。 若要了解有关用户定义的路由的详细信息，请参阅[虚拟网络上使用网络虚拟设备](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn)。

## <a name="gateways-and-on-premises-connectivity"></a>网关和本地连接

每个虚拟网络，无论是否与另一个虚拟网络对等互连仍可以让自己的网关连接到本地网络。 虚拟网络对等互连时您可以还的网关配置对等互连的虚拟网络中为本地网络的传输点。 在这种情况下，使用远程网关的虚拟网络不能有自己的网关。 虚拟网络只能有一个网关，可以是本地或远程网关 （在对等互连的虚拟网络中）。

## <a name="monitor"></a>监视器

当建立两个虚拟网络对等互连时，必须配置为对等互连中每个虚拟网络对等互连。

你可以监视可以处于以下状态之一的对等互连状态：

-   **启动：** 当你创建的对等互连从第一个虚拟网络到第二个虚拟网络时显示。

-   **连接：** 你已创建的对等互连从第二个虚拟网络到第一个虚拟网络后显示。 第一个虚拟网络对等互连状态从启动更改为已连接。 这两个虚拟网络对等方建立虚拟网络对等互连成功之前必须已连接的状态。

-   **断开连接：** 显示是否一个虚拟网络断开与另一个虚拟网络连接。

[状态的信息图]

## <a name="next-steps"></a>后续步骤
[配置虚拟网络对等互连](sdn-configure-vnet-peering.md):在此过程中，您使用 Windows PowerShell 来查找 HNV 提供程序逻辑网络创建两个虚拟网络，每个都有一个子网。 您还可以配置两个虚拟网络之间对等互连。

