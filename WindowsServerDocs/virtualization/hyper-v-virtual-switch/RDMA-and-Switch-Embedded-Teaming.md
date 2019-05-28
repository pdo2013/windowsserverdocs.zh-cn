---
title: 远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)
description: 本主题提供信息有关交换机嵌入式组合 (SET) 除了使用 Windows Server 2016 中的 HYPER-V 配置远程直接内存访问 (RDMA) 接口上的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 485da451eb092336ec93eddfadc6ffa0e677452b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222750"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>远程直接内存访问\(RDMA\)和交换机嵌入式组合\(设置\)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题提供有关配置远程直接内存访问的信息\(RDMA\)与 Windows Server 2016 中的 HYPER-V 此外到有关交换机嵌入式组合信息交互\(设置\)。  

> [!NOTE]
> 除了本主题，以下交换机嵌入式组合内容可用。 
> - TechNet 库下载：[Windows Server 2016 NIC 和交换机嵌入式组合用户指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>使用 HYPER-V 配置 RDMA 接口  

在 Windows Server 2012 R2 中，提供 RDMA 服务可未绑定到 HYPER-V 虚拟交换机的网络适配器在同一台计算机上使用 RDMA 和 HYPER-V。 这会增加所需安装在 HYPER-V 主机的物理网络适配器的数量。

>[!TIP]
>Windows Server 2016 之前的 Windows Server 版本，不能绑定到 NIC 组或 HYPER-V 虚拟交换机的网络适配器上配置 RDMA。 在 Windows Server 2016 中，可以绑定到带有或不带集的 HYPER-V 虚拟交换机的网络适配器上启用 RDMA。

在 Windows Server 2016 中，可以使用 RDMA，带或不带集时使用的网络适配器。

下图说明了 Windows Server 2012 R2 和 Windows Server 2016 之间的软件体系结构更改。

![体系结构更改](../media/RDMA-and-SET/rdma_over.jpg)

以下部分介绍如何使用 Windows PowerShell 命令来启用数据中心桥接 (DCB)、 使用 RDMA 虚拟 NIC 创建的 HYPER-V 虚拟交换机\(vNIC\)，并使用 SET 创建 HYPER-V 虚拟交换机和 RDMA Vnic。

### <a name="enable-data-center-bridging-dcb"></a>启用数据中心桥接\(DCB\)

之前使用任何 RDMA over Converged Ethernet \(RoCE\)版本的 RDMA，必须启用 DCB。  而无需进行 Internet 范围区域 RDMA 协议\(iWARP\)网络，测试已确定所有基于以太网的 RDMA 技术更好地与 DCB。 因此，应考虑使用 DCB，即使对于 iWARP RDMA 部署。

以下 Windows PowerShell 的示例命令演示了如何启用和配置的 SMB 直通的 DCB。

开启 DCB

    Install-WindowsFeature Data-Center-Bridging

为 SMB Direct 设置策略：

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

为 SMB 启用流控制：

    Enable-NetQosFlowControl  -Priority 3

请确保流控制处于关闭状态的其他流量：

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

将策略应用于目标适配器：

    Enable-NetAdapterQos  -Name "SLOT 2"

提供 SMB 直通的 30%的最小带宽：

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

如果必须安装在系统内核调试程序，则必须配置调试器以允许 QoS，通过运行以下命令来设置。

重写调试器-默认情况下调试器阻止 NetQos:
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>使用 RDMA vNIC 创建 HYPER-V 虚拟交换机

如果不需要为你的部署组，则可以使用以下 Windows PowerShell 命令以使用 RDMA vNIC 创建 HYPER-V 虚拟交换机。

> [!NOTE]
> 使用支持 RDMA 物理 Nic 组团队提供 Vnic 以使用更多 RDMA 的资源。

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

将主机 vNICs 添加并使其 RDMA 支持：

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

验证 RDMA 功能：

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>使用 SET 和 RDMA Vnic 创建 HYPER-V 虚拟交换机

若要使用的 RDMA 功能的 HYPER-V 上托管虚拟网络适配器\(Vnic\)上的 HYPER-V 虚拟交换机支持 RDMA 组合，可以使用这些示例 Windows PowerShell 命令。

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

将主机 vNICs 添加：

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

许多开关不会在未标记的 VLAN 流量传递流量类信息，因此请务必 RDMA 的主机适配器的 Vlan 上。 此示例将两个 SMB_ * 主机虚拟适配器为 VLAN 42。
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

在主机 Vnic 上启用 RDMA:

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

验证 RDMA 功能;请确保功能为非零值：

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>切换嵌入组合 (SET)  

本部分提供了概述的交换机嵌入式组合 (SET) 在 Windows Server 2016 中，并包含以下各节。

- [组概述](#bkmk_over)

- [设置可用性](#bkmk_avail)

- [支持和不支持的 Nic 组](#bkmk_nics)

- [设置与 Windows Server 网络技术的兼容性](#bkmk_compat)

- [设置模式和设置](#bkmk_modes)

- [组和虚拟机队列 (Vmq)](#bkmk_vmq)

- [组和 HYPER-V 网络虚拟化 (HNV)](#bkmk_hnv)

- [设置和实时迁移](#bkmk_live)

- [传输数据包的 MAC 地址使用](#bkmk_mac)

- [管理组团队](#bkmk_manage)

## <a name="bkmk_over"></a>组概述

集是一个可以在包含 HYPER-V 和软件定义网络环境中使用的替代 NIC 组合解决方案\(SDN\) Windows Server 2016 中的堆栈。 组将某些 NIC 组合功能集成到 HYPER-V 虚拟交换机。

组可以 1 至 8 物理以太网网络适配器之间到一个或多个基于软件的虚拟网络适配器组。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。

组成员的网络适配器必须全部安装在同一物理 HYPER-V 主机来放置在一个组。

> [!NOTE]
> 在 Windows Server 2016 中的 HYPER-V 虚拟交换机仅支持使用组。 不能部署 Windows Server 2012 R2 中的组。

可以连接成组的 Nic 到同一个物理交换机或不同的物理交换机。 如果将 Nic 连接到不同的交换机，这两个开关必须位于同一子网。

下图描绘了集体系结构。

![集体系结构](../media/RDMA-and-SET/set_architecture.jpg)

因为集集成到 HYPER-V 虚拟交换机中，不能在虚拟机 (VM) 内部使用集。 但是可以使用在 Vm 的 NIC 组合。

有关详细信息，请参阅[虚拟机 (Vm) 中的 NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms)。

此外，组体系结构不会显示团队的接口。 相反，您必须配置的 HYPER-V 虚拟交换机端口。

## <a name="bkmk_avail"></a>设置可用性

集是包括 HYPER-V 和 SDN 堆栈的所有版本的 Windows Server 2016 中可用。 此外，您可以使用 Windows PowerShell 命令和远程桌面连接从远程计算机运行的工具支持的客户端操作系统的管理组。

## <a name="bkmk_nics"></a>受支持的 Nic 组

可以使用任何已通过 Windows 硬件认证和徽标的以太网 NIC \(WHQL\)测试在 Windows Server 2016 中的设置组中。 组要求将团队成员的所有网络适配器都必须完全相同\(即，同一制造商、 同一个模型，同一个固件和驱动程序\)。 在团队中的一个和 8 个网络适配器之间支持组。
  
## <a name="bkmk_compat"></a>设置与 Windows Server 网络技术的兼容性

集是与 Windows Server 2016 中的以下网络技术兼容。

- 数据中心桥接\(DCB\)
  
- HYPER-V 网络虚拟化-NV GRE 和 VxLAN 同时支持 Windows Server 2016 中。  
- 接收方校验和卸载\(IPv4、 IPv6、 TCP\) -支持这些客户端如果集团队成员的任何支持它们。

- 远程直接内存访问\(RDMA\)

- 单根 I/O 虚拟化\(SR-IOV\)

- 传输端校验和卸载\(IPv4、 IPv6、 TCP\) -支持这些客户端如果集团队成员的所有支持它们。

- 虚拟机队列\(VMQ\)

- 虚拟接收方缩放\(RSS\)

集不是与 Windows Server 2016 中的以下网络技术兼容。

- 802.1x 身份验证。 802.1x 可扩展身份验证协议\(EAP\)数据包将被自动删除由超\-V 集方案中的虚拟交换机。
 
- IPsec 任务卸载\(IPsecTO\)。 这是一项传统技术不支持的大多数网络适配器，并且其中文件确实存在，它默认处于禁用状态。

- 使用 QoS \(pacer.exe\)在主机或本机操作系统中。 这些 QoS 方案并不超\-V 方案，因此不相交的技术。 此外，QoS 是可用，但默认情况下未启用-有意必须启用 QoS。

- 接收端合并\(RSC\)。 通过超自动禁用 RSC\-V 虚拟交换机。

- 接收方缩放\(RSS\)。 HYPER-V 使用 VMQ 和 VMMQ 队列，因为你创建虚拟交换机时，始终禁用 RSS。

- TCP 烟囱卸载。 默认情况下禁用了此技术。

- 虚拟机 QoS \(VM QoS\)。 VM QoS 是可用，但默认情况下禁用。 如果在设置环境中配置 VM QoS，QoS 设置将导致不可预知的结果。

## <a name="bkmk_modes"></a>设置模式和设置

与 NIC 组合，创建一个组团队时不能配置团队名称。 此外，使用备用适配器但不支持 NIC 组合，它不支持在组中。 在部署组时，所有网络适配器都处于活动状态，都没有处于备用模式。

NIC 组合和组的另一个主要区别在于，NIC 组合提供了三种不同的组合模式的选择而集仅支持**独立于交换机**模式。 交换机独立模式下，开关或开关设置团队成员连接到将团队存在无法识别并不用于确定如何将网络流量设置团队成员分配-而是将团队分配入站的网络在集团队成员之间的流量。

在创建新的组团队时，必须配置以下团队属性。

- 成员适配器

- 负载均衡模式

### <a name="member-adapters"></a>成员适配器

在创建组团队时，必须指定作为集团队成员适配器绑定到 HYPER-V 虚拟交换机的最多八个相同的网络适配器。

### <a name="load-balancing-mode"></a>负载平衡模式

集的选项团队负载平衡分发模式下都**HYPER-V 端口**并**动态**。

**Hyper-V Port**

Vm 连接到 HYPER-V 虚拟交换机上的端口。 针对组团队使用的 HYPER-V 端口模式，HYPER-V 虚拟交换机端口和关联的 MAC 地址用于划分集团队成员之间的网络流量。

> [!NOTE]
> 当与 Packet Direct，组合模式结合使用组**独立于交换机**和负载平衡模式**HYPER-V 端口**所需。

由于相邻交换机始终发现给定端口上的特定 MAC 地址，切换到 MAC 地址的端口分配入口负载 （流量从交换机到主机）。 这是特别有用时使用，虚拟机队列 (Vmq)，因为可以在流量预期到达特定 NIC 上放置一个队列。

但是，如果主机有仅几个 Vm，此模式可能不是足够精细以获得充分均衡的分布。 此模式也始终会限制单个 VM （即，从单一的交换机端口的流量） 到单个接口上可用的带宽。

**Dynamic**

此负载平衡模式下可提供以下优势。

- 出站负载分发基于哈希的 TCP 端口和 IP 地址。  动态模式还重新平衡在真实时间中的负载，以便在给定的出站流可以设置团队成员之间来回移动。

- 入站的负载分发中的 HYPER-V 端口模式与相同的方式。

在此模式下的出站负载动态均衡根据 flowlets 的概念。 就像人的语音具有单词和句子末尾的自然间隔，TCP 流 （TCP 通信流） 还具有自然会出现一次中断。 两个此类分隔线之间的 TCP 流的部分称为 flowlet。

当动态模式算法检测到，flowlet 边界遇到了-例如足够长的中断发生时在 TCP 流中的算法会自动重新平衡到另一个团队成员在适当的流。  在某些不常见的情况下，该算法可能也会定期范围重新平衡不包含任何 flowlets 的流。 正因为如此，TCP 流和团队成员之间的关联可以更改在任何时间，因为动态负载平衡算法起作用，以平衡的团队成员的工作负荷。

## <a name="bkmk_vmq"></a>组和虚拟机队列 (Vmq)

VMQ 和集很好地协同工作，每当你使用 HYPER-V 和设置时，应启用 VMQ。

> [!NOTE]
> 设置始终显示跨所有组可用的队列的总数团队成员。 在 NIC 组合，这称为总和的队列模式。

大多数网络适配器都具有可用于任一接收方缩放队列\(RSS\)或 VMQ，但不是能同时在同一时间。
  
某些 VMQ 设置似乎是 RSS 队列设置，但实际上是 RSS 和 VMQ 的使用，具体取决于哪种功能目前正在使用的泛型队列上的设置。 每个 NIC，在其高级属性中，对于具有值`*RssBaseProcNumber`和`*MaxRssProcessors`。

以下是更好的系统性能的几个 VMQ 设置。

- 理想情况下每个 NIC 都应有`*RssBaseProcNumber`设置为偶数，大于或等于两 （2)。 这是因为第一个物理处理器核心 0 \(0 和 1 个逻辑处理器\)，通常执行大部分系统处理，因此应离开此物理处理器 steered 网络处理。 

>[!NOTE]
>某些计算机体系结构没有为每个物理处理器的两个逻辑处理器，因此对于此类计算机基处理器应大于或等于 1。 如果有疑问，假定您的主机每个物理处理器体系结构使用 2 个逻辑处理器。

- 应为团队成员的处理器，就很实用的、 非重叠。 例如，在 4 核主机\(8 个逻辑处理器\)10Gbps 的 2 个的团队，你也设置为使用基本的 2 处理器并使用 4 个核心的第一个; 第二个将被设置为使用基本处理器 6，使用 2 个内核。

## <a name="bkmk_hnv"></a>组和 HYPER-V 网络虚拟化\(HNV\)

集是与 Windows Server 2016 中的 HYPER-V 网络虚拟化完全兼容。 HNV 管理系统提供信息集驱动程序的允许集将网络流量负载进行了优化的 HNV 流量的方式。
  
## <a name="bkmk_live"></a>设置和实时迁移

在 Windows Server 2016 中支持实时迁移。

## <a name="bkmk_mac"></a>传输数据包的 MAC 地址使用

配置后，将团队的动态负载分发、 从单个源数据包\(如单个 VM\)同时分布在多个团队成员。 

若要阻止开关会混淆，并且若要防止 MAC 摇摆警报设置可替换在关联的团队成员以外的团队成员传输的帧上不同的 MAC 地址的源 MAC 地址。 因此，每个团队成员使用不同的 MAC 地址，而 MAC 地址冲突会阻止，直到发生故障。

组合软件的集的主 NIC 上检测到故障时，启动上选择要用作临时关联的团队成员的团队成员使用 VM 的 MAC 地址\(，即现在将出现到交换机的 vm接口\)。

此更改仅适用于已将要发送使用 VM 自己的 MAC 地址的 VM 的关联的团队成员作为其源 MAC 地址的流量。 其他流量可继续与任何源故障发生之前就会使用 MAC 地址一起发送。

以下是描述组合基于团队的配置方式的 MAC 地址替换行为的组的列表：

- 在独立于交换机模式下的 HYPER-V 端口分发

    - 每个 vmSwitch 端口关联到团队成员
  
    - 每个数据包发送端口关联到的团队成员对  
  
    - 不完成任何源 MAC 替换  
  
- 在独立于交换机模式下使用动态分配
  
    - 每个 vmSwitch 端口关联到团队成员  
  
    - 发送端口关联到的团队成员对所有 ARP/NS 数据包  
  
    - 是关联的团队成员的团队成员对发送的数据包将没有源 MAC 地址替换完成  
  
    - 上的团队成员不关联的团队成员发送的数据包将已完成的源 MAC 地址替换  
  
## <a name="bkmk_manage"></a>管理组团队

建议你使用 System Center Virtual Machine Manager \(VMM\)管理集团队，但是您还可用于 Windows PowerShell 管理组。 以下部分提供 Windows PowerShell 命令可用于管理组。

有关如何创建一组的信息的团队使用 VMM，请参阅 System Center VMM 库主题中的部分"设置逻辑交换机"[创建逻辑交换机](https://docs.microsoft.com/system-center/vmm/network-switch)。
  
### <a name="create-a-set-team"></a>创建一个组的团队

必须使用创建的 HYPER-V 虚拟交换机时创建集 team **New-vmswitch** Windows PowerShell 命令。

当你创建的 HYPER-V 虚拟交换机时，必须包括的新**EnableEmbeddedTeaming**命令语法中的参数。 在以下示例中，HYPER-V 交换机的名称为**TeamedvSwitch**与嵌入式组合和两个初始团队创建的成员。
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
**EnableEmbeddedTeaming**参数假定 Windows powershell 时的参数**NetAdapterName**是一个数组而不是单个 NIC 的 Nic 结果，则可以按以下方式修改前一命令。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

如果你想要创建集能够切换与单个团队成员，以便可以在更高版本时，添加团队成员则必须使用 EnableEmbeddedTeaming 参数。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>添加或删除设置团队成员

**集 VMSwitchTeam**命令包括**NetAdapterName**选项。 若要更改团队成员在组的组中，输入后的团队成员所需的列表**NetAdapterName**选项。 如果**TeamedvSwitch**最初创建时的 NIC 1 和 NIC 2，则下面的示例命令中删除组团队成员"NIC 2"，并添加新设置团队成员"NIC 3"。
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>删除组团队

可以仅通过删除包含将团队的 HYPER-V 虚拟交换机中删除组团队。  使用主题[Remove-vmswitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch)有关如何删除 HYPER-V 虚拟交换机的信息。 以下示例删除名为的虚拟交换机**SETvSwitch**。

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>更改集团队的负载分布算法

**集 VMSwitchTeam** cmdlet 具有**LoadBalancingAlgorithm**选项。 此选项采用两个可能值之一：**HyperVPort**或**动态**。 若要设置或更改开关嵌入团队的负载分布算法，使用此选项。 

在以下示例中，名为 VMSwitchTeam **TeamedvSwitch**使用**动态**负载平衡算法。  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>关联到物理团队成员的虚拟接口

设置允许你创建的虚拟接口之间的关联\(即 HYPER-V 虚拟交换机端口\)，另一个团队中的物理 Nic。 

例如，如果创建两个 Vnic 为托管 SMB\-直通，如部分中所示[使用 SET 和 RDMA Vnic 创建 HYPER-V 虚拟交换机](#bkmk_set-rdma)，可以确保两个 Vnic 使用不同的团队成员。 

将添加到该部分中的脚本，可以使用以下 Windows PowerShell 命令。

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

本主题中的部分 4.2.5 更加详细地检查[Windows Server 2016 NIC 和交换机嵌入式组合用户指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)。
