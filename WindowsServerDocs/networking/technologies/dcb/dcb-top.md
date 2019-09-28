---
title: 数据中心桥接 (DCB)
description: 您可以使用本主题获取有关 Windows Server 2016 中的数据中心桥接的介绍性信息。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c4948789be642f7a190a3cb51d7ee05c4a9822b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405737"
---
# <a name="data-center-bridging-dcb"></a>数据中心桥\(接 DCB\)

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题获取有关数据中心桥接\(DCB\)的介绍性信息。

DCB 是一套电气和电子工程师\(协会 IEEE\)标准，可在数据中心实现聚合构造，其中存储、数据网络、群集间\-进程通信\(IPC\)和管理流量全部共享同一个以太网网络基础结构。

>[!NOTE]
>除本主题外，还提供以下 DCB 文档
>
>- [在 Windows Server 2016 或 Windows 10 中安装 DCB](dcb-install.md)
>- [管理数据中心桥接（DCB）](dcb-manage.md)

DCB 提供针对\-特定类型的网络流量的基于硬件的带宽分配，并通过使用基于优先级\-的流控制增强以太网传输可靠性。

如果\-流量跳过操作系统并卸载到汇聚网络适配器（可能支持 Internet 小型计算机系统接口\(iSCSI\)），基于硬件的带宽分配是必不可少的。通过以太网远程\(直接\)内存访问 RDMA，或通过以太网\(FCoE\)光纤通道。

如果\-上层协议（例如光纤通道）假定无损基础传输，则基于优先级的流控制是至关重要的。

## <a name="dcb-protocols-and-management-options"></a>DCB 协议和管理选项

DCB 包含以下一组协议。 

- 增强了传输\(服务\) ETS – IEEE 802.1 qaz，它基于 802.1 p 和 802.1 q 标准构建
- 优先级流控制\(PFS\)，IEEE 802.1 q b b 
- DCB Exchange Protocol \(DCBX\)，IEEE 802.1 ab，如 802.1 qaz standard 中的扩展。

DCBX 协议允许你在交换机上配置 DCB，然后可以自动配置终端设备，如运行 Windows Server 2016 的计算机。

如果希望从交换机管理 DCB，则无需在 Windows Server 2016 中将 DCB 作为功能安装，但此方法包含一些限制。

由于 DCBX 只能通知主机网络适配器 ETS 类大小和 PFC 启用，因此 Windows Server 主机通常要求安装 DCB，以便将流量映射到 ETS 类。

Windows 应用程序通常不能参与 DCBX 交换。 因此，必须将主机与网络交换机分开配置，但设置相同。

如果选择从交换机管理 DCB，则仍可使用 Windows PowerShell 命令查看 Windows Server 2016 中的配置。

##  <a name="important-dcb-functionality"></a>重要的 DCB 功能

以下是总结 DCB 所提供功能的列表。

1. 提供支持 DCB\-的网络适配器和支持 DCB\-的交换机之间的互操作性。

2. 通过在网络适配器上打开基于优先级\-的流控制，在运行 Windows Server 2016 的计算机及其邻居交换机之间提供无损以太网传输。

3. 提供按百分比向流量\(控制 tc\)分配带宽的功能，其中，tc 可能包含一个或多个由 802.1 p 流量类\(优先级\)区分的通信类提前.

4. 允许服务器管理员或网络管理员根据知名的协议、知名的 TCP/UDP 端口或该应用程序使用的 NetworkDirect 端口为特定流量类型或优先级分配应用程序。

5. 通过 windows Server 2016 Windows Management Instrumentation \(WMI\)和 windows PowerShell 提供 DCB 管理。 有关详细信息，请参阅本主题后面的[用于 DCB 的 Windows PowerShell 命令](#bkmk_wps)部分，以及以下主题。
    - [系统提供的 DCB 组件](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [数据中心桥接的 NDIS QoS 要求](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. 通过 Windows Server 2016 组策略提供 DCB 管理。

7. 支持 Windows Server 2016 Service \(QoS\)解决方案的共存。

>[!NOTE]
>在使用任何 rdma over 聚合以太\(网\) RoCE 版本的 rdma 之前，必须启用 DCB。 尽管 Internet 广域 rdma 协议\(iWARP\)网络不需要，但测试已确定所有基于以太网\-的 rdma 技术更适用于 DCB。 因此，应考虑使用 DCB 进行 iWARP RDMA 部署。 有关详细信息，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

##  <a name="practical-applications-of-dcb"></a>DCB 的实际应用

许多组织都有用于存储\(服务的大型光纤\(通道\) FC\)存储区域网络 SAN 安装。 FC SAN 需要在服务器上配备特殊的网络适配器，并且需要在网络中配备 FC 交换机。 这些组织通常也使用以太网网络适配器和交换机。

在大多数情况下，部署 FC 硬件比以太网硬件更昂贵，这会产生大量的资本支出。 此外，对单独的以太网和 FC SAN 网络适配器和交换机硬件的要求需要在数据中心提供额外的空间、电源和冷却能力，从而导致额外的运营支出。

从成本角度来看，许多组织都可以结合使用其 FC 技术和基于以太网\-的硬件解决方案来提供存储和数据网络服务，这一点非常有利。

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>将 DCB 用于存储和\-数据网络的基于以太网的聚合结构

对于已经有大型 FC SAN 但希望迁移到光纤 FC 技术额外投资之外的组织，DCB 允许你为存储和数据网络构建基于以太网的聚合结构。 DCB 聚合结构可降低未来的总拥有\(\)成本，并简化管理。

对于已采用或计划采用 iscsi 作为其存储解决方案的托管商来说，DCB 可为 iscsi 流量提供硬件\-辅助带宽预留，以确保性能隔离。 与其他专有解决方案不同，DCB 基于标准\-，因此在异类网络环境中部署和管理相对较容易。

基于 Windows Server 2016\-的 DCB 实现缓解了在多个原始设备制造商\(oem\)提供聚合构造解决方案时可能发生的许多问题。

由多个 Oem 提供的专有解决方案的实现可能无法相互互操作，并且可能难以管理，并且通常会有不同的软件维护计划。 

与此相反，Windows Server 2016 DCB 是\-基于标准的，你可以在异类网络中部署和管理 DCB。

## <a name="bkmk_wps"></a>适用于 DCB 的 Windows PowerShell 命令

对于 Windows Server 2016 和 Windows Server 2012 R2，有 DCB 的 Windows PowerShell 命令。 你可以使用 windows Server 2016 中 Windows Server 2012 R2 的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>适用于 DCB 的 windows Server 2016 Windows PowerShell 命令

以下适用于 windows Server 2016 的主题提供 windows PowerShell cmdlet 说明和语法，适用于所有\(数据\)中心桥接\(DCB\)Service QoS\-特定 cmdlet 的质量。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [DcbQoS 模块](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>适用于 DCB 的 windows Server 2012 R2 Windows PowerShell 命令

适用于 windows Server 2012 R2 的以下主题提供了 windows PowerShell cmdlet 说明和语法，适用于\(所有\)数据中心桥\(接\)DCB Service QoS\-特定的 cmdlet。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [Windows PowerShell 中的数据中心桥接（DCB）服务质量（QoS） Cmdlet](https://technet.microsoft.com/library/hh967440.aspx)
