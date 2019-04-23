---
title: 数据中心桥接 (DCB)
description: 有关 Windows Server 2016 中的数据中心桥接的介绍性信息，可以使用本主题。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb488192a873743db27d9c1d09c5912bc810bb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834178"
---
# <a name="data-center-bridging-dcb"></a>数据中心桥接\(DCB\)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

您可以使用本主题介绍性信息有关的数据中心桥接\(DCB\)。

DCB 是一套电气和电子工程师协会\(IEEE\)标准，使数据中心，其中存储、 数据网络、 群集间的聚合构造\-进程通信\(IPC\)，并管理流量全部共享同一个以太网网络基础结构。

>[!NOTE]
>除了本主题，以下 DCB 文档也可用
>
>- [在 Windows Server 2016 或 Windows 10 安装 DCB](dcb-install.md)
>- [管理数据中心桥接 (DCB)](dcb-manage.md)

DCB 提供硬件\-基于带宽分配到特定类型的网络流量，并增强了优先级使用以太网传输可靠性\-基于流控制。

硬件\-基于的带宽分配就至关重要，如果流量跳过操作系统并卸载到融合的网络适配器，它有可能支持 Internet 小型计算机系统接口\(iSCSI\)，远程直接内存访问\(RDMA\)通过以太网或通过以太网光纤通道\(FCoE\)。

优先级\-基于的流控制就至关重要，如果上层协议，例如光纤通道假定无损基础传输。

## <a name="dcb-protocols-and-management-options"></a>DCB 协议和管理选项

DCB 包括以下一组协议。 

- 增强的传输服务\(ETS\) – IEEE 802.1Qaz，基础之上构建的 802.1p 和 802.1Q 标准
- 优先级流控制\(PFS\)，IEEE 802.1Qbb 
- DCB 交换协议\(DCBX\)，IEEE 802.1AB，作为在标准 802.1Qaz 扩展。

DCBX 协议允许您配置一个开关，然后可以自动配置一个最终设备，如运行 Windows Server 2016 的计算机上的 DCB。

如果想要从交换机管理 DCB，不需要安装 DCB 作为一项功能在 Windows Server 2016 中，但是这种方法包括一些限制。

因为 DCBX 仅可以通知 ETS 类大小和 PFC 支持的主机网络适配器，但是，Windows Server 主机通常需要安装 DCB，以便将流量映射到 ETS 类。

Windows 应用程序通常不旨在参与 DCBX 交换。 正因为如此，主机必须配置单独来自网络交换机，但具有相同的设置。

如果您选择从交换机管理 DCB，您可以使用 Windows PowerShell 命令，Windows Server 2016 中查看的配置。

##  <a name="important-dcb-functionality"></a>重要的 DCB 功能

以下是总结 DCB 所提供功能的列表。

1. 提供 DCB 之间的互操作性\-功能的网络适配器和 DCB\-支持交换机。

2. 提供无损以太网传输的计算机通过开启优先级运行 Windows Server 2016 和及其邻居交换机之间\-基于网络适配器上的流控制。

3. 提供的流量控制分配带宽的功能\(TC\)按百分比，其中，TC 可能包含的一个或多个类的流量的 802.1p 流量类通过区分\(优先级\)指示器。

4. 允许服务器管理员或网络管理员根据知名的协议、知名的 TCP/UDP 端口或该应用程序使用的 NetworkDirect 端口为特定流量类型或优先级分配应用程序。

5. 通过 Windows Server 2016 Windows Management Instrumentation 提供 DCB 管理\(WMI\)和 Windows PowerShell。 有关详细信息，请参阅明[DCB 的 Windows PowerShell 命令](#bkmk_wps)后面本主题中，除了以下主题。
    - [系统提供 DCB 组件](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [数据中心桥接的 NDIS QoS 要求](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. 通过提供 DCB 管理 Windows Server 2016 组策略。

7. 支持 Windows Server 2016 服务质量的共存\(QoS\)解决方案。

>[!NOTE]
>之前使用任何 RDMA over Converged Ethernet \(RoCE\)版本的 RDMA，必须启用 DCB。 而无需进行 Internet 范围区域 RDMA 协议\(iWARP\)网络，测试已确定的所有以太网\-基于 RDMA 技术更好地利用 DCB。 因此，应考虑 iWARP RDMA 部署中使用 DCB。 有关详细信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

##  <a name="practical-applications-of-dcb"></a>DCB 的实际应用程序

许多组织都有大型光纤通道\(FC\)存储区域网络\(SAN\)存储服务的安装。 FC SAN 需要在服务器上配备特殊的网络适配器，并且需要在网络中配备 FC 交换机。 这些组织通常也使用以太网网络适配器和交换机。

在大多数情况下，FC 硬件是贵很多，来部署比以太网硬件，这会导致巨大的资本开支。 此外，单独以太网和 FC SAN 网络适配器和交换机的硬件的要求需要额外的空间、 电源和冷却能力可以在数据中心内，这会导致额外和不间断的运营开支。

从成本角度看，它是有好处的很多组织要合并其以太网与自己的 FC 技术\-基于硬件解决方案来提供存储和网络服务的数据。

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>使用 DCB 的以太网\-基于聚合的存储和数据网络的结构

对于已经拥有 FC SAN 但希望摆脱对 FC 技术额外投资的组织，DCB，可生成基于以太网聚合的存储和数据网络构造。 DCB 融合架构构造可以减少未来的总拥有成本\(TCO\)并简化管理。

托管方人员已采用或计划采用 iSCSI 作为自己存储解决方案，DCB 可提供硬件\-辅助的带宽预留，为 iSCSI 流量，从而确保性能隔离。 与其他专有解决方案不同，DCB 是标准\-基于的因此相对较容易部署和管理在异类网络环境中。

Windows Server 2016\-基于的 DCB 实施缓解了许多可以会出现的问题时聚合构造解决方案提供的多个原始设备制造商\(Oem\)。

由多个 Oem 提供的专有解决方案的实现可能不与彼此互操作，可能很难管理，并且一般都有不同的软件维护日程安排。 

与此相反，Windows Server 2016 DCB 是标准\-基于，也可以部署和管理在异类网络中的 DCB。

## <a name="bkmk_wps"></a>DCB 的 Windows PowerShell 命令

有用于 Windows Server 2016 和 Windows Server 2012 R2 DCB Windows PowerShell 命令。 对于 Windows Server 2016 中的 Windows Server 2012 R2，您可以使用的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 的 DCB 的新 Windows PowerShell 命令

Windows Server 2016 的以下主题提供 Windows PowerShell cmdlet 说明和语法的所有数据中心桥接\(DCB\)服务质量\(QoS\)\-特定的 cmdlet。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>有关 DCB 的 Windows Server 2012 R2 的 Windows PowerShell 命令

Windows Server 2012 R2 的以下主题提供 Windows PowerShell cmdlet 说明和语法的所有数据中心桥接\(DCB\)服务质量\(QoS\)\-特定的 cmdlet。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [数据中心桥接 (DCB) 服务质量 (QoS) Cmdlet 在 Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
