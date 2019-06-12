---
title: 软件和硬件 (SH) 集成功能和技术
description: 这些功能有软件和硬件组件。 该软件联系紧密绑定到所需的功能，若要运行的硬件功能。 其中的示例包括 VMMQ、 VMQ，发送端 IPv4 校验和卸载和 RSS。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 98857a5a6d665728c1aab2a6a2df64997d4166b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446222"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>软件和硬件 (SH) 集成功能和技术

这些功能有软件和硬件组件。 该软件联系紧密绑定到所需的功能，若要运行的硬件功能。 其中的示例包括 VMMQ、 VMQ，发送端 IPv4 校验和卸载和 RSS。

>[!TIP]
>SH 和主机功能都可用，如果已安装的 NIC 支持它。 下面的功能说明将介绍如何判断 NIC 是否支持该功能。

## <a name="converged-nic"></a>聚合的 NIC 

聚合的 NIC 是一种技术，允许虚拟 Nic 中的 HYPER-V 主机到主机进程的 RDMA 服务进行公开。 Windows Server 2016 无法再用于 RDMA 需要独立的 Nic。 聚合 NIC 功能允许主分区 (Vnic) 到主分区公开 RDMA 和公平合理且易于管理的方式共享之间的 RDMA 通信和 VM 和其他 TCP/UDP 流量的 Nic 的带宽中的虚拟 Nic。

![使用 SDN 的聚合的 NIC](../../media/Converged-NIC/conv-nic-sdn.png)

你可以管理通过 VMM 或 Windows PowerShell 的聚合的 NIC 操作。 PowerShell cmdlet 是相同的 cmdlet 用于 RDMA （见下文）。

若要使用的聚合的 NIC 功能：

1.  请确保要将 DCB 为主机设置。

2.  确保要启用 RDMA 的 NIC 上或在集团队的情况下 Nic 绑定到 HYPER-V 交换机。 

3.  请确保主机中指定用于 RDMA Vnic 上启用 RDMA。 

有关 RDMA 和 SET 的更多详细信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="data-center-bridging-dcb"></a>数据中心桥接 (DCB) 

DCB 是一套电气和电子工程师协会 (IEEE) 标准，使数据中心的聚合构造。 DCB 提供的协作从相邻的交换机的主机中的硬件基于队列的带宽管理。 有关存储的所有流量，数据网络、 群集进程间通信 (IPC) 和管理都共享同一个以太网网络基础结构。 在 Windows Server 2016 中，DCB 可分别应用于任何 NIC，并于 Nic 绑定到的 HYPER-V 交换机。

对于 DCB，Windows Server 使用基于优先级的流控制 (PFC)，在 IEEE 802.1Qbb 中标准化。 PFC 创建通过阻止流量类中的溢出 （几乎） 无损的网络结构。 Windows Server 还使用增强传输选择 (ETS)，在 IEEE 802.1Qaz 中标准化。 ETS 使保留部分的最多八个的类的流量的带宽分成。 每个流量类具有其自己的传输队列，并通过使用 PFC，可以启动和停止在类中的传输。

有关详细信息，请参阅[数据中心桥接 (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top)。

## <a name="hyper-v-network-virtualization"></a>Hyper-V 网络虚拟化

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 (HNVv1)**       |                     在 Windows Server 2012 中引入的 HYPER-V 网络虚拟化 (HNV) 可以实现客户网络共享的物理网络基础结构之上的虚拟化。 需在物理网络 fabric 上的最小更改，HNV 使服务提供商可以部署和任何位置跨三个云迁移的租户工作负荷的灵活性： 服务提供商云、 私有云或 Microsoft Azure 公有云。                     |
| **v2 NVGRE (HNVv2 NVGRE)** | 在 Windows Server 2016 和 System Center Virtual Machine Manager 中，Microsoft 提供了端到端网络虚拟化解决方案，包括 RAS 网关、 软件负载平衡、 网络控制器和的详细信息。 有关详细信息，请参阅[Windows Server 2016 中的 HYPER-V 网络虚拟化概述](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server)。 |
| **v2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        在 Windows Server 2016 中，为 SDN 扩展中，通过网络控制器管理的一部分。                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>IPsec 任务卸载 (IPsecTO) 

IPsec 任务卸载是一种使操作系统在 NIC 上的处理器用于 IPsec 加密工作的 NIC 功能。

>[!IMPORTANT] 
>IPsec 任务卸载是一项传统技术，大多数网络适配器，不支持，其中文件确实存在，它默认处于禁用状态。

## <a name="private-virtual-local-area-network-pvlan"></a>专用虚拟局域网 (PVLAN)。 

Pvlan 允许仅在同一虚拟化服务器上的虚拟机之间的通信。 专用虚拟网络未绑定到物理网络适配器。 与虚拟化服务器上的所有外部网络流量，以及管理操作系统和外部网络之间的网络流量隔离的专用虚拟网络。 当你需要创建隔离的网络环境（例如隔离的测试域）时，此类型的网络很有用。 HYPER-V 和 SDN 堆栈支持仅限 PVLAN 隔离端口模式。

有关 PVLAN 隔离的详细信息，请参阅[System Center:Virtual Machine Manager 工程博客](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/)。

## <a name="remote-direct-memory-access-rdma"></a>远程直接内存访问 (RDMA) 

RDMA 是一种可提供最大程度减少 CPU 使用情况的高吞吐量、 低延迟通信的网络技术。 RDMA 支持零复制网络使网络适配器，可以直接向或从应用程序内存传输数据。 支持 RDMA 的则意味着是能够为 RDMA 客户端公开 RDMA NIC （物理或虚拟）。 但是，启用了 RDMA 的意味着支持 RDMA 的 NIC 公开在堆栈中向上 RDMA 接口。

有关 RDMA 的更多详细信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS 是该 NIC 功能可承受程度分离不同的流并将其交付给不同的处理器进行处理。 RSS 进行并行化处理，使主机可以扩展到非常高的数据速率的网络。 

有关更多详细信息，请参阅[接收方缩放 (RSS)](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling)。

## <a name="single-root-input-output-virtualization-sr-iov"></a>单根输入-输出虚拟化 (SR-IOV) 

SR-IOV 允许虚拟机流量直接从转为 NIC VM 而不通过 HYPER-V 主机。 SR-IOV 是一个令人难以置信的 vm 的性能改进，但不具备主机来管理该管道的功能。 仅在主机上使用 SR-IOV 时的工作负荷是运行良好，受信任，并通常的唯一 VM。

流量使用 SR-IOV 绕过 HYPER-V 交换机，这意味着任何策略，例如，Acl，或不会应用带宽管理。 SR-IOV 流量也不能传递通过任何网络虚拟化功能，因此无法应用 NV GRE 或 VxLAN 封装。 仅适用于受信任的工作负荷，在特定情况下使用 SR-IOV。 此外，不能使用的主机策略、 带宽管理和虚拟化技术。

将来，这两种技术将允许 SR-IOV:泛型流表 (GFT) 和硬件 QoS 卸载 （的 nic 的带宽管理 –） 后在我们的生态系统中的 Nic 支持它们。 这两种技术的组合会使 SR-IOV 适用于所有 Vm，将允许策略、 虚拟化和带宽管理规则要应用并可能导致很好的观念转发中通常 SR-IOV 的应用程序。

有关更多详细信息，请参阅[概述的单根 I/O 虚拟化 (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-)。

## <a name="tcp-chimney-offload"></a>TCP 烟囱卸载

TCP 烟囱卸载，也称为 TCP 引擎卸载 (TOE)，是一种技术，允许宿主卸载所有 TCP 处理到 nic。 因为 Windows Server TCP 堆栈方法通常更高效比 TOE 引擎，TCP 烟囱卸载，建议不要使用。

>[!IMPORTANT]
>TCP 烟囱卸载是不推荐使用的技术。 我们建议您不要使用 TCP 烟囱卸载如 Microsoft 可能会停止在将来支持。

## <a name="virtual-local-area-network-vlan"></a>虚拟局域网 (VLAN) 

VLAN 是要启用的 LAN 划分到多个 Vlan，每个使用其自己的地址空间的以太网框架标头的扩展。 在 Windows Server 2016 中，Vlan 上的 HYPER-V 交换机或通过 NIC 组合团队设置团队接口的端口设置。 有关详细信息，请参阅[NIC 组合和虚拟局域网 (Vlan)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans)。

## <a name="virtual-machine-queue-vmq"></a>虚拟机队列 (VMQ) 

Vmq 是 NIC 功能，为每个 VM 分配一个队列。 你有已启用; HYPER-V您还必须启用 VMQ。 在 Windows Server 2016 Vmq 使用 NIC 交换机 vPorts 使用单个队列分配给 vPort 提供相同的功能。 有关详细信息，请参阅[虚拟接收方缩放 (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top)并[NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## <a name="virtual-machine-multi-queue-vmmq"></a>虚拟机多队列 (VMMQ) 

VMMQ 是允许的 vm 分布在多个队列，每个不同的物理处理器处理流量的 NIC 功能。 然后，因为它将在 vRSS，允许用于向 VM 传送大量网络带宽，系统会流量传递到 VM 中的多个 LPs。

---