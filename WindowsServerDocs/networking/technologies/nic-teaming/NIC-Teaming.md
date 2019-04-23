---
title: NIC 组合
description: 在本主题中，我们提供您的网络接口卡 (NIC) 合作概述 Windows Server 2016 中。 NIC 组合允许你将介于 1 和 32 之间分组到一个或多个基于软件的虚拟网络适配器的物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: 367de10e8c77490ff27be81ddc05239f931ad1f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860468"
---
# <a name="nic-teaming"></a>NIC 组合

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，我们提供您的网络接口卡 (NIC) 合作概述 Windows Server 2016 中。 NIC 组合允许你将介于 1 和 32 之间分组到一个或多个基于软件的虚拟网络适配器的物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。  
  
>[!IMPORTANT]
>必须在同一台物理主机计算机中安装的 NIC 组成员的网络适配器。 

> [!TIP]  
> NIC 组包含只有一个网络适配器不能提供负载平衡和故障转移。 但是，使用一个网络适配器，你可以使用 NIC 组合的网络流量进行隔离还使用虚拟局域网 (Vlan) 时。  
  
当将网络适配器配置到 NIC 组中时，它们连接到 NIC 成组解决方案的共同核心，然后提供一个或多个虚拟适配器 （也称为团队 Nic [tNICs] 或团队接口） 到操作系统。 

由于 Windows Server 2016 支持每个团队的最多 32 个组接口，有各种分发 Nic 之间的出站流量 （加载） 的算法。  下图描绘了使用多个 tNICs NIC 组。  
  
![使用多个 tNICs NIC 组](../../media/NIC-Teaming/nict_overview.jpg)  
  
此外，你可以连接到同一交换机或不同的交换机的成组的 Nic。 如果将 Nic 连接到不同的交换机，这两个开关必须位于同一子网。  
  
## <a name="availability"></a>可用性  
NIC 组合是在 Windows Server 2016 的所有版本中可用。 可以使用各种工具来管理 NIC 组合的计算机运行客户端操作系统，如: • Windows PowerShell cmdlet • 远程桌面 • 远程服务器管理工具  
  
## <a name="supported-and-unsupported-nics"></a>支持和不支持的 Nic   
可以使用任何已在 Windows Server 2016 中的 NIC 组中通过 Windows 硬件认证和徽标测试 （WHQL 测试） 的以太网 NIC。  
  
不可以在 NIC 组中放置以下 Nic:
  
-   公开为 Nic 主机分区中的 HYPER-V 虚拟交换机端口的 HYPER-V 虚拟网络适配器。  
  
    > [!IMPORTANT]  
    > 不要将放置在团队中的主分区 (Vnic) 中公开的 HYPER-V 虚拟 Nic。 任何配置中不支持成组的主分区内的 Vnic。 如果发生网络故障，团队 Vnic 的尝试可能会导致完全丧失通信。  
  
-   内核调试网络适配器 (KDNIC)。  
  
-   用于网络启动的 Nic。  
  
-   使用以太网，例如 WWAN、 WLAN/Wi-fi、 蓝牙和 Infiniband，包括 Internet 协议通过 Infiniband (IPoIB) Nic 以外的技术的 Nic。  
  
## <a name="compatibility"></a>兼容性  
NIC 组合是与 Windows Server 2016 中有以下例外的所有网络的技术兼容。  
  
-   **单根 I/O 虚拟化 (SR-IOV)**。 有关 SR-IOV，数据被直接传递到 NIC 而不通过网络堆栈 （主机操作系统中，在虚拟化的情况下）。 因此，不可能 NIC 组合，若要检查或将数据重定向到团队中的另一个路径。  
  
-   **本机主机服务质量 (QoS)**。 当您设置本机或主机系统上的 QoS 策略和这些策略调用最小带宽限制，NIC 组的总体吞吐量小于相比它而无需在位置的带宽策略。  
  
-   **TCP 烟囱**。 不支持 TCP 烟囱使用 NIC 组合，因为 TCP 烟囱卸载整个网络堆栈直接到 nic。  
  
-   **802.1x 身份验证**。 因为某些开关不允许 802.1 X 身份验证和 NIC 组合在同一端口上的配置，则不应的 NIC 组合使用 802.1x 身份验证。  
  
若要了解如何在 HYPER-V 主机运行的虚拟机 (Vm) 中使用 NIC 组合，请参阅[主机计算机或 VM 上创建新的 NIC 组](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)。
  
## <a name="virtual-machine-queues-vmqs"></a>虚拟机队列 (Vmq)  

Vmq 是 NIC 功能，为每个 VM 分配一个队列。  你有已启用; HYPER-V您还必须启用 VMQ。 在 Windows Server 2016 Vmq 使用 NIC 交换机 vPorts 使用单个队列分配给 vPort 提供相同的功能。 

具体取决于交换机配置模式和负载分布算法，NIC 组合提供了最少数量的提供和支持团队 （最小队列模式） 中的任何适配器的队列或队列可用的总数目跨所有团队成员 （之和的队列模式）。  

如果团队在独立于交换机的组合模式，而将负载分配设置为 HYPER-V 端口模式或动态模式下，报告的队列数是可从团队成员 （之和的队列模式） 的所有队列的总和。 否则，报告的队列数目为最少数量的队列支持团队 （最小队列模式） 的任何成员。

原因是：  
  
-   当独立于交换机的团队都在 HYPER-V 端口或动态模式下的 HYPER-V 交换机端口 (VM) 的入站的流量将到达同一个团队成员。 主机可以预测/控制哪个成员为特定 vm 接收流量，因此可以更缜密有关哪些 VMQ 队列上特定团队成员要分配给 NIC 组合。 NIC 组合，使用 HYPER-V 交换机，准确地说是一个团队成员上为 VM 设置 VMQ 并且知道的入站的流量到达该队列。  
  
-   当团队处于任何切换从属模式 （静态组合或 LACP 组合），团队已连接到的交换机控制入站的流量分布。 主机的 NIC 组合软件无法预测哪个团队成员将获取 vm 的入站的流量，并且它可能是，交换机将流量分配的 vm 在所有团队成员之间。 如的 NIC 组合软件，使用 HYPER-V 交换机中，结果为每个团队成员上的 VM 程序队列，而不仅仅是一个团队成员。  
  
-   当团队中独立于交换机的模式和使用地址哈希负载平衡时，入站的流量始终显示上一个 NIC （主要组成员）-它只是一个团队成员上的所有。 其他团队成员不处理入站流量，因为它们获取通过编程使用与主成员的同一队列中，以便当主成员出现故障，可以使用任何其他团队成员要选取的入站流量，并且队列均已就位。  

- 大多数 Nic 具有队列的接收方缩放 (RSS) 或 VMQ，而不是在同一时间。 某些 VMQ 设置似乎是 RSS 队列设置，但为 RSS 和 VMQ 的使用，具体取决于哪种功能目前正在使用的泛型队列上设置。 每个 NIC，在其高级属性中，对于具有值 * RssBaseProcNumber 和\*MaxRssProcessors。 以下是更好的系统性能的几个 VMQ 设置。  
  
-   理想情况下，每个 NIC 都应有 * RssBaseProcNumber 将设置为偶数，大于或等于两 （2)。 第一个物理处理器，内核 0 （0 和 1 逻辑处理器），通常执行大部分系统处理，因此网络处理应控制离开此物理处理器。 某些计算机体系结构不具有两个逻辑处理器，每个物理处理器，因此，对于此类计算机，基处理器应大于或等于 1。 如果不确定假定您的主机每个物理处理器体系结构使用 2 个逻辑处理器。  
  
-   如果团队为之和的队列模式应为团队成员的处理器不重叠。 例如，在具有一组 2 10Gbps Nic 的 4 核主机 （8 个逻辑处理器），可以设置第一个以使用 2 基处理器并使用 4 个核心;第二个将设置为使用基本处理器 6，使用 2 个内核。  
  
-   如果团队在最小队列模式下的团队成员使用的处理器集必须相同。  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Hyper-V 网络虚拟化 (HNV)  
NIC 组合是使用 HYPER-V 网络虚拟化 (HNV) 完全兼容。  HNV 管理系统提供的 NIC 组合的驱动程序允许 NIC 组合的方式来优化 HNV 流量负载分配到的信息。  
  
## <a name="live-migration"></a>实时迁移  
中的 Vm 的 NIC 组合不会影响实时迁移。 指示在 VM 中配置 NIC 组合，相同的规则存在实时迁移。  


## <a name="virtual-local-area-networks-vlans"></a>虚拟局域网 (Vlan)
当你使用 NIC 组合时，创建多个团队接口使宿主可以同时连接到不同的 Vlan。 配置环境使用以下准则：
  
- 启用 NIC 组合之前，配置连接到要使用 trunk (promiscuous) 模式的组合主机的物理交换机端口。 物理交换机应将所有流量都传递到主机，而无需修改流量筛选。  

- 不要使用高级属性设置的 NIC 的 Nic 上配置 VLAN 筛选器。 允许进行筛选，VLAN 的 NIC 组合软件或 HYPER-V 虚拟交换机 （如果存在）。  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>使用 Vlan 中 VM 的 NIC 组合  
当团队连接到 HYPER-V 虚拟交换机时，必须完成所有 VLAN 隔离的 HYPER-V 虚拟交换机而不是 NIC 组合中。  

计划在与 NIC 团队使用以下准则配置的 VM 中使用 Vlan:
  
-   在 VM 中支持多个 Vlan 的首选的方法是使用多个端口的 HYPER-V 虚拟交换机上配置 VM 并将每个端口与 VLAN 关联。 永远不会团队在 VM 中的这些端口，因为这样会导致网络通信问题。  

-   如果 VM 有多个 SR-IOV 虚拟函数 (VFs)，请确保它们在 VM 中的组合它们之前位于同一个 VLAN。 它是可以轻松地配置不同的 VFs 位于不同的 Vlan，这样会导致网络通信问题。  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>管理网络接口和 Vlan 
如果你必须拥有到来宾操作系统公开多个 VLAN，请考虑重命名要阐明 VLAN 分配给的接口的以太网接口。 例如，如果您将相关联**以太网**接口与 VLAN 12 并**以太网 2**具有 VLAN 48 接口中，重命名接口以太网到**EthernetVLAN12**和到其他**EthernetVLAN48**。 

使用 Windows PowerShell 命令重命名接口**重命名 NetAdapter**或通过执行以下过程：
  
1.  在服务器管理器中，在**属性**要重命名的网络适配器，请单击网络适配器名称右侧的链接。 
  
2.  右键单击你想要重命名和选择的网络适配器**重命名**。  
  
3.  键入新名称的网络适配器，然后按 ENTER。  


## <a name="virtual-machines-vms"></a>虚拟机 (Vm)

如果你想要在 VM 中使用 NIC 组合，你必须连接到外部 HYPER-V 虚拟交换机仅在 VM 中的虚拟网络适配器。 执行此操作允许 VM 可承受网络连接，即使在所处环境中的一个物理网络适配器连接到一个虚拟交换机出现故障或断开连接时。 连接到内部或专用的 HYPER-V 虚拟交换机的虚拟网络适配器不能在它们是在组中，并且网络连接失败时的 vm 连接到的交换机。  
  
Windows Server 2016 中的 NIC 组合支持在 Vm 中的团队具有两个成员。 您可以创建更大的团队，但不支持更大的团队。 每个团队成员必须连接到不同外部 HYPER-V 虚拟交换机，并必须将虚拟机的网络接口配置为允许进行组合。

  
如果要在 VM 中配置 NIC 组，则必须选择**组合模式**的_独立于交换机_和一个**负载均衡模式**的_地址哈希_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>支持 SR-IOV 网络适配器  
NIC 组中或在 HYPER-V 主机不能保护 SR-IOV 流量，因为它不需要通过 HYPER-V 交换机。  使用 VM 的 NIC 组合选项，你可以配置两个外部 HYPER-V 虚拟交换机、 每个连接到其自己的支持 SR-IOV 的 nic。  
  
![NIC 组合与-支持 SR-IOV 网络适配器](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
每个 VM 可以从一个或两个 SR-IOV Nic，并当 NIC 断开连接，从主 VF 故障转移到备份适配器 (VF) 时的虚拟函数 (VF)。 或者，VM 可能拥有 VF 从一个 NIC 和非 VF vmNIC 连接到另一个虚拟交换机。 如果断开与取景器关联的 NIC，则可将流量故障转移到其他交换机而不会丢失连接。  
  
由于在 VM 中 Nic 之间的故障转移可能会导致流量与其他 vmNIC 的 MAC 地址一起发送，则必须设置与使用 NIC 组合的 VM 相关联的每个 HYPER-V 虚拟交换机端口以允许进行组合。 


## <a name="related-topics"></a>相关主题

- [NIC 组合 MAC 地址的使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md):在 NIC 组配置为切换独立模式和地址哈希或动态负载分发时，该团队使用的媒体访问控制 (MAC) 地址的出站流量上的主 NIC 组成员。 主 NIC 团队成员是由操作系统来自团队成员的初始集选择的网络适配器。

- [NIC 组合设置](nic-teaming-settings.md):在本主题中，我们为您提供的 NIC 组属性，如组合概述和负载平衡模式。 我们还会为您提供的备用适配器设置和主要组接口属性的详细信息。 如果 NIC 组中具有至少两个网络适配器，您不需要指定的容错能力的备用适配器。
  
- [在主计算机或 VM 上创建新的 NIC 组](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md):本主题中的主计算机上或在运行 Windows Server 2016 的 HYPER-V 虚拟机 (VM) 中创建新的 NIC 组。

- [NIC 组合疑难解答](Troubleshooting-NIC-Teaming.md):在本主题中，我们将讨论如何解决 NIC 组合，如硬件、 物理交换机证券和使用 Windows PowerShell 的禁用或启用网络适配器。 
 
