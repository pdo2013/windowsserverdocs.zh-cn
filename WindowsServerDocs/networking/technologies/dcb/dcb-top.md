---
title: 数据中心桥 (DCB)
description: 你可以使用本主题介绍有关 Windows Server 2016 中的数据中心桥的信息。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d465e855adc387d7136919ac11fbab56c792c34
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="data-center-bridging-dcb"></a>桥 \(DCB\) 数据中心

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题介绍有关的数据中心桥 \(DCB\)。

DCB 是一套电气和电子工程师 \(IEEE\) 标准启用聚合数据中心中，其中存储、数据联网、群集 Inter\ 进程通信 \(IPC\) 和管理交通所有共享的以太网网络基础结构的结构。

>[!NOTE]
>本主题中，除了以下 DCB 文档可能会提供
>
>- [Windows Server 2016 或 Windows 10 中安装 DCB](dcb-install.md)
>- [管理数据中心桥 (DCB)](dcb-manage.md)

DCB 提供了 hardware\ 基于带宽分配，到特定类型的网络通信，并增强了使用的流量 priority\ 基于控制以太网传输可靠性。

如果跳过操作系统路况，以及被卸载到已合并好的网络适配器，这可能支持 Internet 小型计算机系统接口 \(iSCSI\)、远程直接内存访问 \(RDMA\) 通过以太网或光纤通道以太网 \(FCoE\) 通过，Hardware\ 基于带宽分配是非常重要。

基于 Priority\ 流控制至关重要如果上层协议，如光纤通道假定无损的基础传输。

## <a name="dcb-protocols-and-management-options"></a>DCB 协议和管理选项

DCB 包含套以下协议。 

- 增强的传输服务 \(ETS\) – IEEE 802.1Qaz，版本 802.1 P 和 802.1 q 标准的
- 优先级流控制 \(PFS\)，IEEE 802.1Qbb 
- DCB Exchange 协议 \(DCBX\)，IEEE 802.1AB，作为中标准 802.1Qaz 扩展。

DCBX 协议允许你配置 DCB 交换机，然后自动配置最终的设备，如运行 Windows Server 2016 的计算机上。

如果你想要管理与开关 DCB，你无需安装 DCB 作为一项功能在 Windows Server 2016，但是此方法包含一些限制。

因为 DCBX 可以仅通知 ETS 类大小和 PFC 启用，但是，Windows Server 的主网络适配器主机通常需要以便交通映射到 ETS 类是否安装了 DCB。

Windows 应用程序通常不旨在参与 DCBX 交易所。 出于此原因，主机必须配置单独从网络交换机的成员，但使用相同的设置。

如果您选择一个交换机用来从管理 DCB，你可以通过使用 Windows PowerShell 命令，在 Windows Server 2016 中查看配置。

##  <a name="important-dcb-functionality"></a>重要 DCB 功能

下面是概述由 DCB 功能的列表。

1. 提供之间 DCB\ 支持的网络适配器和支持 DCB\ 开关互操作性。

2. 提供了一台运行 Windows Server 2016 和其邻居开关打开在该网络适配器的 priority\ 基于流控制计算机之间无损以太网传输协议。

3. 提供了分配到交通控制 \(TC\) 带宽，通过百分比，TC 可能包含一个或多个类别的交通通过 802.1 p 交通类 \(priority\) 指示器区分的能力。

4. 使服务器管理员或网络管理员，要给其指定为特定交通类或优先级基于普遍协议、已知 TCP/UDP 端口或 NetworkDirect 端口该应用使用的应用。

5. 提供了通过 Windows Server 2016 Windows 管理检测 \(WMI\) 和 Windows PowerShell DCB 管理。 有关详细信息，请参阅部分[DCB 的 Windows PowerShell 命令](#bkmk_wps)本主题中，除了以下主题后面。
    - [系统提供 DCB 组件](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [有关数据中心桥 NDIS 服务质量要求](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. 提供 DCB 管理通过 Windows Server 2016 组策略。

7. 支持 Windows Server 2016 服务质量 \(QoS\) 解决方案的并存。

>[!NOTE]
>使用之前任何 RDMA RDMA 汇聚以太网 \(RoCE\) 版本上，你必须启用 DCB。 不需要 Internet 宽区域 RDMA 协议 \(iWARP\) 网络，而测试已确定所有 Ethernet\ 基于 RDMA 技术 DCB 与更好地都工作。 出于此原因，你应该考虑使用 DCB iWARP RDMA 部署。 有关详细信息，请参阅[远程直接内存访问 (RDMA) 和切换 Embedded 组队（集）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

##  <a name="practical-applications-of-dcb"></a>DCB 的实际应用

许多组织都有大型纤维信道 \(FC\) 存储区域网络 \(SAN\) 安装存储服务。 光纤通道 SAN 需要特定的网络适配器上的服务器和网络中的光纤通道开关。 这些组织通常还使用以太网网络适配器和开关。

在大多数情况下，光纤通道硬件是占用比以太网硬件，这将导致大量资金支出部署。 此外，对于单独以太网和光纤通道 SAN 网络适配器和切换硬件要求需要额外的空间、电源和冷却，这会导致额外、持续运营支出数据中心中的能力。

从成本的角度，它是适用于许多组织合并其光纤通道技术来存储和数据网络服务提供其 Ethernet\ 基于硬件解决方案。

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>使用 DCB 适用于将已合并好结构 Ethernet\ 基于存储和数据联网

对于已具有较大的光纤通道圣，但想要离开光纤通道技术方面的其他投资迁移的组织，DCB 允许你生成的以太网基于汇聚结构存储和数据联网。 集中 DCB 结构可以减少将来的总体拥有成本 \(TCO\) 和简化管理。

对于谁已经采用，或谁打算采用的宿主，为它们的存储解决方法，DCB iSCSI 可以提供 iSCSI 交通，以确保性能隔离 hardware\ 协助带宽预订。 以及与其他专有解决方案不同 DCB standards\ 基于-，因此相对轻松部署和管理在不同网络环境中。

Windows Server 2016 \-based 实现 DCB 缓解了很多时，会出现汇聚结构解决方案所提供的多个原始设备制造商 \(OEMs\) 的问题。

实现的多个 Oem 提供的解决方案专有可能未与另一互操作，可能很难管理和通常会有不同的软件的维护计划。 

相反，Windows Server 2016 DCB 基于 standards\，可以将其部署和管理 DCB 在不同的网络。

## <a name="bkmk_wps"></a>Windows PowerShellDCB 的命令

有 DCB Windows PowerShell 适用于 Windows Server 2016 和 Windows Server 2012 R2 的命令。 你可以对 Windows Server 2016 中的 Windows Server 2012 R2 使用的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016Windows PowerShellDCB 的命令

Windows Server 2016 的以下主题提供 Windows PowerShell cmdlet 说明和语法所有数据中心桥 \(DCB\) 服务质量 \ (QoS\) \-specific cmdlet。 列出了这些 cmdlet 基于 cmdlet 开头的动词按字母顺序。

- [DcbQoS 模块](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 对于 DCB R2 Windows PowerShell 命令

Windows Server 2012 R2 的以下主题提供 Windows PowerShell cmdlet 说明和语法所有数据中心桥 \(DCB\) 服务质量 \ (QoS\) \-specific cmdlet。 列出了这些 cmdlet 基于 cmdlet 开头的动词按字母顺序。

- [数据中心中的 Windows PowerShell 服务 (QoS) Cmdlet 过渡 (DCB) 质量](https://technet.microsoft.com/library/hh967440.aspx)
