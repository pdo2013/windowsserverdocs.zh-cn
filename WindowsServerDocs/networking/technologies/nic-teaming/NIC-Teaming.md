---
title: NIC 组合
description: 在本主题中，我们将概述 Windows Server 2016 中的网络接口卡（NIC）组合。 NIC 组合允许在一个或多个基于软件的虚拟网络适配器之间分组到32物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: 2356de674bfc6e57c9444136b1244934464a2d02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396500"
---
# <a name="nic-teaming"></a>NIC 组合

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，我们将概述 Windows Server 2016 中的网络接口卡（NIC）组合。 NIC 组合允许在一个或多个基于软件的虚拟网络适配器之间分组到32物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。  
  
> [!IMPORTANT]
> 你必须在同一物理主机计算机上安装 NIC 组成员网络适配器。 

> [!TIP]  
> 仅包含一个网络适配器的 NIC 组无法提供负载平衡和故障转移。 但是，对于一个网络适配器，在使用虚拟局域网（Vlan）时，可以使用 NIC 组合来分隔网络流量。  
  
将网络适配器配置为 NIC 组时，它们将连接到 NIC 组合解决方案公共核心，然后将一个或多个虚拟适配器（也称为团队 Nic [tNICs] 或团队接口）提供给操作系统。 

由于 Windows Server 2016 支持每个团队最多32个团队界面，因此可通过多种算法在 Nic 之间分布出站流量（负载）。  下图描绘了具有多个 tNICs 的 NIC 组。  
  
![具有多个 tNICs 的 NIC 团队](../../media/NIC-Teaming/nict_overview.jpg)  
  
此外，还可以将组合 Nic 连接到相同的开关或不同的交换机。 如果将 Nic 连接到不同的交换机，则这两个交换机必须位于同一子网中。  
  
## <a name="availability"></a>可用性  
所有版本的 Windows Server 2016 中都提供了 NIC 组合。 你可以使用各种工具来管理运行客户端操作系统的计算机的 NIC 组合，例如：• Windows PowerShell cmdlet •远程桌面•远程服务器管理工具  
  
## <a name="supported-and-unsupported-nics"></a>支持和不支持的 Nic   
你可以使用在 Windows Server 2016 的 NIC 组中已通过 Windows 硬件认证和徽标测试（WHQL 测试）的任何以太网 NIC。  
  
不能将以下 Nic 放入 NIC 组：
  
-   Hyper-v 虚拟网络适配器，作为主机分区中作为 Nic 公开的 hyper-v 虚拟交换机端口。  
  
    > [!IMPORTANT]  
    > 不要将 Hyper-v 虚拟 Nic 置于团队的主机分区（Vnic）中。 在任何配置中不支持主机分区内部的 Vnic 组合。 如果发生网络故障，尝试团队 Vnic 可能会导致完全的通信丢失。  
  
-   内核调试网络适配器（KDNIC）。  
  
-   用于网络启动的 Nic。  
  
-   使用除以太网之外的其他技术的 Nic，如 WWAN、WLAN/Wi-fi、蓝牙和未占用，包括 Internet 协议 over 无线网络（IPoIB） Nic。  
  
## <a name="compatibility"></a>兼容性  
NIC 组合与 Windows Server 2016 中的所有网络技术兼容，但有以下例外。  
  
-   **单根 i/o 虚拟化（sr-iov）** 。 对于 SR-IOV，数据直接传递到 NIC，而不通过网络堆栈（在虚拟化的情况下为主机操作系统）传递数据。 因此，NIC 组无法检查或将数据重定向到组中的另一个路径。  
  
-   **本机主机服务质量（QoS）** 。 在本机系统或主机系统上设置 QoS 策略时，如果这些策略调用最小带宽限制，则 NIC 组的总体吞吐量小于无需使用带宽策略。  
  
-   **TCP 烟囱**。 NIC 组合不支持 TCP 烟囱，因为 TCP 烟囱会将整个网络堆栈直接卸载到 NIC。  
  
-   **802.1 x Authentication**。 不应将 802.1 X Authentication 用于 NIC 组合，因为某些交换机不允许在同一端口上配置 802.1 X Authentication 和 NIC 组合。  
  
若要了解如何在 Hyper-v 主机上运行的虚拟机（Vm）中使用 NIC 组合，请参阅[在主计算机或 VM 上创建新的 NIC 组](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)。
  
## <a name="virtual-machine-queues-vmqs"></a>虚拟机队列（Vmq）  

Vmq 是一项 NIC 功能，用于为每个 VM 分配队列。  只要启用了 Hyper-v，还必须启用 VMQ。 在 Windows Server 2016 中，Vmq 将 NIC 交换机 vPorts 与分配给 vPort 的单个队列一起使用，以提供相同的功能。 

根据交换机配置模式和负载分配算法，NIC 组合提供了团队中的任何适配器（最小队列模式）或在所有团队中可用的队列总数的最小可用队列数成员（队列总数模式）。  

如果团队处于独立于交换机的组合模式，并且你将负载分配设置为 Hyper-v 端口模式或动态模式，则报告的队列数是团队成员（队列总计模式）中所有可用队列的总和。 否则，报告的队列数是团队的任何成员（最小队列模式）支持的最小队列数。

原因是：  
  
-   当独立于交换机的团队处于 Hyper-v 端口模式或动态模式时，Hyper-v 交换机端口（VM）的入站流量始终到达同一组成员。 主机可以预测/控制哪些成员接收特定 VM 的流量，以便 NIC 组合可以更周密地了解要在特定团队成员上分配哪些 VMQ 队列。 NIC 组合，使用 Hyper-v 交换机，可在恰好一个团队成员上为 VM 设置 VMQ，并了解入站流量是否达到了该队列。  
  
-   当团队处于任何交换机相关模式（静态组合或 LACP 组合）时，团队连接到的交换机控制入站流量分布。 主机的 NIC 组合软件无法预测哪个团队成员会获得 VM 的入站流量，并且它可能是交换机跨所有团队成员分发 VM 的流量。 作为 NIC 组合软件的结果，使用 Hyper-v 交换机，每个团队成员（而不只是一个团队成员）为 VM 提供一个队列。  
  
-   当团队处于与交换机无关的模式并使用地址哈希负载平衡时，入站流量始终出现在一个 NIC （主要团队成员）中-所有这一切都是在一个团队成员上。 由于其他团队成员不处理入站流量，因此，它们使用与主成员相同的队列进行编程，因此，如果主成员出现故障，则可以使用任何其他团队成员来选择入站流量，并且队列已准备就绪。  

- 大多数 Nic 都有队列用于接收方缩放（RSS）或 VMQ，但不是同时使用。 某些 VMQ 设置看起来像是 RSS 队列的设置，但这些设置是 RSS 和 VMQ 使用的通用队列的设置，具体取决于目前正在使用的功能。 每个 NIC 的高级属性中都有 * RssBaseProcNumber 和\*MaxRssProcessors 的值。 下面是几个提供更好系统性能的 VMQ 设置。  
  
-   理想情况下，每个 NIC 的 * RssBaseProcNumber 设置为一个大于或等于2（2）的偶数。 第一个物理处理器 Core 0 （逻辑处理器0和1）通常执行大部分系统处理，因此网络处理应远离此物理处理器。 有些计算机体系结构每个物理处理器没有两个逻辑处理器，因此对于这类计算机，基本处理器应大于或等于1。 如果有疑问，假定主机使用的是每个物理处理器体系结构2个逻辑处理器。  
  
-   如果团队处于队列中的模式，则团队成员的处理器应为非重叠。 例如，在带有2个 10Gbps Nic 组的4核主机（8个逻辑处理器）中，你可以将第一个主机设置为使用第2个基本处理器，并使用4个内核;第二个设置为使用基本处理器6并使用2个内核。  
  
-   如果团队处于最小队列模式，则团队成员使用的处理器集必须相同。  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Hyper-V 网络虚拟化 (HNV)  
NIC 组合与 Hyper-v 网络虚拟化（HNV）完全兼容。  HNV 管理系统为 NIC 组合驱动程序提供信息，使 NIC 组合能够以一种优化 HNV 流量的方式分发负载。  
  
## <a name="live-migration"></a>实时迁移  
Vm 中的 NIC 组合不会影响实时迁移。 与在 VM 中配置 NIC 组合实时迁移相同的规则。  


## <a name="virtual-local-area-networks-vlans"></a>虚拟局域网（Vlan）
使用 NIC 组合时，创建多个团队界面允许主机同时连接到不同的 Vlan。 使用以下准则配置环境：
  
- 启用 NIC 组合之前，请将连接到组合主机的物理交换机端口配置为使用干线（混杂）模式。 物理交换机应将所有流量传递到主机，以便在不修改流量的情况下进行筛选。  

- 不要使用 NIC 的 "高级属性" 设置来配置 Nic 上的 VLAN 筛选器。 让 NIC 组合软件或 Hyper-v 虚拟交换机（如果存在）执行 VLAN 筛选。  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>在 VM 中使用 NIC 组合的 Vlan  
当团队连接到 Hyper-v 虚拟交换机时，所有 VLAN 隔离都必须在 Hyper-v 虚拟交换机中完成，而不是在 NIC 组合中完成。  

使用以下准则计划在配置有 NIC 的 VM 中使用 Vlan：
  
-   在 VM 中支持多个 Vlan 的首选方法是使用 Hyper-v 虚拟交换机上的多个端口配置 VM，并将每个端口与 VLAN 相关联。 请勿将这些端口组合到 VM 中，因为这样做会导致网络通信问题。  

-   如果 VM 有多个 SR-IOV 虚拟功能（VFs），请确保它们位于同一 VLAN 上，然后在 VM 中对它们进行分组。 可以轻松地将不同的 VFs 配置到不同的 Vlan 上，这样做会导致网络通信问题。  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>管理网络接口和 Vlan 
如果来宾操作系统中必须公开多个 VLAN，请考虑重命名以太网接口，以明确分配给接口的 VLAN。 例如，如果将**以太网**接口与 vlan 12 和带有 vlan 48 的**以太网 2**接口相关联，请将接口以太网重命名为**EthernetVLAN12** ，将另一个连接到**EthernetVLAN48**。 

通过使用 Windows PowerShell 命令**get-netadapter**或执行以下过程，重命名接口：
  
1.  在服务器管理器中，在要重命名的网络适配器的 "**属性**" 中，单击网络适配器名称右侧的链接。 
  
2.  右键单击要重命名的网络适配器，然后选择 "**重命名**"。  
  
3.  键入网络适配器的新名称，然后按 ENTER。  


## <a name="virtual-machines-vms"></a>虚拟机（Vm）

如果要在虚拟机中使用 NIC 组合，则必须仅将 VM 中的虚拟网络适配器连接到外部 Hyper-v 虚拟交换机。 这样一来，即使连接到一个虚拟交换机的某个物理网络适配器发生故障或断开连接，VM 也可以维持网络连接。 如果连接到内部或专用 Hyper-v 虚拟交换机的虚拟网络适配器在组中时无法连接到该交换机，并且 VM 的网络连接失败。  
  
Windows Server 2016 中的 NIC 组合支持 Vm 中包含两个成员的团队。 您可以创建更大的团队，但不支持更大的团队。 每个团队成员都必须连接到不同的外部 Hyper-v 虚拟交换机，并且必须将 VM 的网络接口配置为允许组合。

  
如果在 VM 中配置 NIC 组，则必须选择 "_独立交换机_"和 "_地址哈希_的**负载平衡" 模式**。   
  
  
## <a name="sr-iov-capable-network-adapters"></a>支持 SR-IOV 的网络适配器  
Hyper-v 主机或 Hyper-v 主机下的 NIC 组无法保护 SR-IOV 流量，因为它不通过 Hyper-v 交换机。  利用 VM NIC 组合选项，你可以配置两个外部 Hyper-v 虚拟交换机，每个交换机都连接到其自己的支持 SR-IOV 的 NIC。  
  
![NIC 与支持 SR-IOV 的网络适配器成组](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
每个 VM 可以有一个或两个 SR-IOV Nic 中的虚拟功能（VF），并且在 NIC 断开连接时，从主 VF 到备份适配器（VF）的故障转移。 此外，VM 还可以有一个 NIC 的 VF，而非 VF vmNIC 连接到另一个虚拟交换机。 如果与 VF 关联的 NIC 断开连接，则流量可以故障转移到其他交换机，而不会失去连接。  
  
由于 VM 中 Nic 之间的故障转移可能导致流量与其他 vmNIC 的 MAC 地址一起发送，因此，使用 NIC 组合与 VM 关联的每个 Hyper-v 虚拟交换机端口都必须设置为允许组合。 


## <a name="related-topics"></a>相关主题

- [NIC 组合 MAC 地址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md)：当你使用 "交换机独立" 模式配置 NIC 组，并使用 "地址哈希" 或 "动态负载" 分发时，团队将在出站流量上使用主要 NIC 组成员的媒体访问控制（MAC）地址。 主 NIC 组成员是操作系统从一组初始团队成员中选择的网络适配器。

- [NIC 组合设置](nic-teaming-settings.md)：在本主题中，我们将为你概述 NIC 组属性，例如组合和负载平衡模式。 此外，我们还会向你介绍备用适配器设置和主团队接口属性的详细信息。 如果 NIC 组中至少有两个网络适配器，则无需指定备用适配器来实现容错。
  
- [在主计算机或 VM 上创建新的 NIC 组](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)：在本主题中，你将在主计算机或运行 Windows Server 2016 的 Hyper-v 虚拟机（VM）中创建新的 NIC 组。

- [NIC 组合故障排除](Troubleshooting-NIC-Teaming.md)：在本主题中，我们将讨论使用 Windows PowerShell 对 NIC 组合（如硬件、物理交换机证券和禁用或启用网络适配器）进行故障排除的方法。 
 
