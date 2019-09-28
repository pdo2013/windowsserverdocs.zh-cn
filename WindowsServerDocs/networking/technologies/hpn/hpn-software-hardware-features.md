---
title: 软件和硬件 (SH) 集成功能和技术
description: 这些功能包括软件和硬件组件。 该软件与它紧密功能所需的硬件功能相结合。 其中包括 VMMQ、VMQ、发送端 IPv4 校验和卸载以及 RSS。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: f032717b9f4dca65454d8251083b73ff2d57dba7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355321"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>软件和硬件 (SH) 集成功能和技术

这些功能包括软件和硬件组件。 该软件与它紧密功能所需的硬件功能相结合。 其中包括 VMMQ、VMQ、发送端 IPv4 校验和卸载以及 RSS。

>[!TIP]
>如果安装的 NIC 支持，SH 和 HO 功能可用。 以下功能说明将介绍如何判断 NIC 是否支持该功能。

## <a name="converged-nic"></a>汇聚 NIC 

聚合 NIC 是一种技术，它允许 Hyper-v 主机中的虚拟 Nic 公开 RDMA 服务以托管进程。 Windows Server 2016 不再需要针对 RDMA 的单独 Nic。 利用汇聚 NIC 功能，主机分区（Vnic）中的虚拟 Nic 可向主机分区公开 RDMA，并以合理且可管理的方式在 RDMA 流量与 VM 与其他 TCP/UDP 流量之间共享 Nic 的带宽。

![包含 SDN 的汇聚 NIC](../../media/Converged-NIC/conv-nic-sdn.png)

可以通过 VMM 或 Windows PowerShell 管理汇聚 NIC 操作。 PowerShell cmdlet 是用于 RDMA 的相同 cmdlet （请参阅下文）。

使用收敛 NIC 功能：

1.  请确保为 DCB 设置该主机。

2.  请确保在 NIC 上启用 RDMA，或在设置组的情况下，将 Nic 绑定到 Hyper-v 交换机。 

3.  确保为主机中的 RDMA 指定的 Vnic 启用 RDMA。 

有关 RDMA 和 SET 的详细信息，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="data-center-bridging-dcb"></a>数据中心桥接 (DCB) 

DCB 是一套电气和电子工程师协会（IEEE）标准，可在数据中心内实现聚合结构。 DCB 在与相邻交换机之间的协作的主机中提供基于硬件队列的带宽管理。 用于存储、数据网络、群集进程间通信（IPC）和管理的所有流量共享同一以太网网络基础结构。 在 Windows Server 2016 中，DCB 可单独应用于任何 NIC，并可应用于绑定到 Hyper-v 交换机的 Nic。

对于 DCB，Windows Server 使用 IEEE 802.1 Q b b 中标准化的基于优先级的流控制（PFC）。 PFC 通过防止在通信类中溢出来创建（将近）无损网络构造。 Windows Server 还使用 IEEE 802.1 Qaz 中标准化的增强型传输选择（ETS）。 ETS 允许将带宽划分为多达八类流量的保留部分。 每个交通类都有自己的传输队列，通过使用 PFC，可以在类中启动和停止传输。

有关详细信息，请参阅[数据中心桥接（DCB）](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top)。

## <a name="hyper-v-network-virtualization"></a>Hyper-V 网络虚拟化

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 （HNVv1）**       |                     Hyper-v 网络虚拟化（HNV）是在 Windows Server 2012 中引入的，用于在共享的物理网络基础结构的基础上实现客户网络虚拟化。 由于物理网络结构上只需进行少量的更改，HNV 为服务提供商提供灵活的灵活性，可在三个云中的任何位置部署和迁移租户工作负荷：服务提供商云、私有云或 Microsoft Azure 公有云。                     |
| **v2 NVGRE （HNVv2 NVGRE）** | 在 Windows Server 2016 和 System Center Virtual Machine Manager 中，Microsoft 提供了端到端网络虚拟化解决方案，其中包括 RAS 网关、软件负载平衡、网络控制器等。 有关详细信息，请参阅[Windows Server 2016 中的 Hyper-v 网络虚拟化概述](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server)。 |
| **v2 VxLAN （HNVv2 VxLAN）** |                                                                                                                                                                                        在 Windows Server 2016 中，是 SDN 扩展的一部分，可通过网络控制器进行管理。                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>IPsec 任务卸载（IPsecTO） 

IPsec 任务卸载是一项 NIC 功能，使操作系统可以使用 NIC 上的处理器进行 IPsec 加密工作。

>[!IMPORTANT] 
>IPsec 任务卸载是一种传统的技术，它不受大多数网络适配器支持，并在其中存在，默认情况下处于禁用状态。

## <a name="private-virtual-local-area-network-pvlan"></a>专用虚拟局域网（PVLAN）。 

Pvlan 仅允许在同一虚拟化服务器上的虚拟机之间进行通信。 专用虚拟网络未绑定到物理网络适配器。 专用虚拟网络与虚拟化服务器上的所有外部网络流量隔离，以及管理操作系统和外部网络之间的任何网络流量。 当你需要创建隔离的网络环境（例如隔离的测试域）时，此类型的网络很有用。 Hyper-v 和 SDN 堆栈仅支持 PVLAN 隔离端口模式。

有关 PVLAN 隔离的详细信息， [请参阅 System Center：Virtual Machine Manager 工程博客](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/)。

## <a name="remote-direct-memory-access-rdma"></a>远程直接内存访问 (RDMA) 

RDMA 是一种网络技术，可提供高吞吐量、低延迟的通信，可最大程度地降低 CPU 使用率。 RDMA 通过允许网络适配器将数据直接传输到应用程序内存或从应用程序内存中传输数据，支持零复制网络。 支持 RDMA 意味着 NIC （物理或虚拟）能够向 RDMA 客户端公开 RDMA。 另一方面，已启用 RDMA 意味着支持 RDMA 的 NIC 在堆栈上公开 RDMA 接口。

有关 RDMA 的详细信息，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS 是一项 NIC 功能，它分隔开来不同的流集，并将它们传递给不同的处理器进行处理。 RSS 并行网络处理，使主机可以扩展为非常高的数据速率。 

有关更多详细信息，请参阅[接收方缩放（RSS）](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling)。

## <a name="single-root-input-output-virtualization-sr-iov"></a>单个根输入-输出虚拟化（SR-IOV） 

SR-IOV 允许 VM 流量直接从 NIC 移至 VM，而无需通过 Hyper-v 主机。 SR-IOV 是 VM 性能的惊人改善，但缺乏主机管理该管道的能力。 仅当工作负荷的行为良好、受信任且通常是主机中唯一的 VM 时，才使用 SR-IOV。

使用 SR-IOV 的流量将绕过 Hyper-v 交换机，这意味着将不会应用任何策略（如 Acl）或带宽管理。 不能通过任何网络虚拟化功能传递 SR-IOV 流量，因此不能应用 NV 或 VxLAN 封装。 仅在特定情况下，为受信任的工作负荷使用 SR-IOV。 此外，不能使用主机策略、带宽管理和虚拟化技术。

将来，两种技术会允许 SR-IOV：一般流表（GFT）和硬件 QoS 卸载（NIC 中的带宽管理）–生态系统中的 Nic 支持它们。 这两种技术的组合会使 SR-IOV 对所有 Vm 都有用，这将允许应用策略、虚拟化和带宽管理规则，并可能在 SR-IOV 的一般应用程序中带来极大的进步。

有关更多详细信息，请参阅[单根 I/o 虚拟化（sr-iov）概述](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-)。

## <a name="tcp-chimney-offload"></a>TCP 烟囱卸载

TCP 烟囱卸载（也称为 TCP 引擎卸载（TOE））是一种技术，允许主机将所有 TCP 处理卸载到 NIC。 由于 Windows Server TCP stack 几乎总是比 TOE 引擎更有效，因此不建议使用 TCP 烟囱卸载。

>[!IMPORTANT]
>TCP 烟囱卸载是一种不推荐使用的技术。 建议你不要使用 TCP 烟囱卸载，因为 Microsoft 可能会在将来停止支持。

## <a name="virtual-local-area-network-vlan"></a>虚拟局域网（VLAN） 

VLAN 是对以太网框架标头的扩展，用于启用将 LAN 分区为多个 Vlan，每个 Vlan 使用其自己的地址空间。 在 Windows Server 2016 中，Vlan 是在 Hyper-v 交换机的端口上设置的，或者是通过设置 NIC 组合团队上的团队界面设置的。 有关详细信息，请参阅[NIC 组合和虚拟局域网（vlan）](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans)。

## <a name="virtual-machine-queue-vmq"></a>虚拟机队列 (VMQ) 

Vmq 是一项 NIC 功能，用于为每个 VM 分配队列。 只要启用了 Hyper-v，还必须启用 VMQ。 在 Windows Server 2016 中，Vmq 将 NIC 交换机 vPorts 与分配给 vPort 的单个队列一起使用，以提供相同的功能。 有关详细信息，请参阅[虚拟接收方缩放（vRSS）](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top)和[NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## <a name="virtual-machine-multi-queue-vmmq"></a>虚拟机多队列（VMMQ） 

VMMQ 是一项 NIC 功能，该功能允许 VM 的流量分布到多个队列中，每个队列由不同的物理处理器处理。 然后，会将流量传递到 VM 中的多个 LPs，就像在 vRSS 中那样，这允许向 VM 提供大量的网络带宽。

---