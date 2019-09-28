---
title: 远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)
description: 本主题提供有关使用 Windows Server 2016 中的 Hyper-v 配置远程直接内存访问（RDMA）接口的信息，以及有关交换机嵌入组合（SET）的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b39cac842f115a1828c666eec52f17f80971510c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365693"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>远程直接内存访问 \(RDMA @ no__t; 交换机嵌入组合 \(SET @ no__t-3

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题提供有关使用 Windows Server 2016 中的 Hyper-v 配置远程直接内存访问 \(RDMA @ no__t 接口的信息，以及有关交换机嵌入组合的信息 \(SET @ no__t。  

> [!NOTE]
> 除了本主题之外，还提供了以下交换机嵌入的组合内容。 
> - TechNet 库下载：[Windows Server 2016 NIC 和交换机嵌入式组合用户指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>通过 Hyper-v 配置 RDMA 接口  

在 Windows Server 2012 R2 中，使用与提供 RDMA 服务的网络适配器相同的计算机上的 RDMA 和 Hyper-v 不能绑定到 Hyper-v 虚拟交换机。 这会增加需要安装在 Hyper-v 主机上的物理网络适配器的数量。

>[!TIP]
>在 Windows server 2016 以前的 Windows Server 版本中，不能在绑定到 NIC 组或 Hyper-v 虚拟交换机的网络适配器上配置 RDMA。 在 Windows Server 2016 中，可以在绑定到已设置或未设置的 Hyper-v 虚拟交换机的网络适配器上启用 RDMA。

在 Windows Server 2016 中，使用或不使用 RDMA 时，可以使用较少的网络适配器。

下图演示了 Windows Server 2012 R2 和 Windows Server 2016 之间的软件体系结构更改。

![体系结构更改](../media/RDMA-and-SET/rdma_over.jpg)

以下各节提供了有关如何使用 Windows PowerShell 命令启用数据中心桥接（DCB）的说明，如何创建具有 RDMA 虚拟 NIC 的 Hyper-v 虚拟交换机 \(vNIC @ no__t），并使用 SET 和 RDMA Vnic 创建 Hyper-v 虚拟交换机。

### <a name="enable-data-center-bridging-dcb"></a>启用数据中心桥接 \(DCB @ no__t-1

在使用任何 rdma over 聚合以太\(网\) RoCE 版本的 rdma 之前，必须启用 DCB。  尽管 Internet 广域 RDMA 协议不需要 \(iWARP @ no__t 网络，测试已确定所有基于以太网的 RDMA 技术更适用于 DCB。 因此，即使对于 iWARP RDMA 部署，也应考虑使用 DCB。

以下 Windows PowerShell 示例命令演示了如何为 SMB Direct 启用和配置 DCB。

打开 DCB

    Install-WindowsFeature Data-Center-Bridging

为 SMB Direct 设置策略：

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

为 SMB 启用流控制：

    Enable-NetQosFlowControl  -Priority 3

确保为其他流量关闭流控制：

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

将策略应用到目标适配器：

    Enable-NetAdapterQos  -Name "SLOT 2"

为 SMB 直接指定最小带宽的 30%：

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

如果系统中已安装了内核调试器，则必须通过运行以下命令将调试器配置为允许设置 QoS。

重写调试器-默认情况下，调试器会阻止 NetQos：
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>使用 RDMA vNIC 创建 Hyper-v 虚拟交换机

如果你的部署不需要设置，则可以使用以下 Windows PowerShell 命令创建具有 RDMA vNIC 的 Hyper-v 虚拟交换机。

> [!NOTE]
> 将集团队与支持 RDMA 的物理 Nic 结合使用，可提供更多 RDMA 资源供 Vnic 使用。

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

添加 host Vnic 并使其支持 RDMA：

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

验证 RDMA 功能：

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>使用 SET 和 RDMA Vnic 创建 Hyper-v 虚拟交换机

若要在 hyper-v 主机虚拟网络 @no__t 适配器上利用 RDMA 功能，请在支持 RDMA 组合的 Hyper-v 虚拟交换机上使用 0vNICs @ no__t-1，可以使用以下示例 Windows PowerShell 命令。

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

添加主机 Vnic：

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

许多交换机不会传递无标记 VLAN 流量的流量类信息，因此请确保 RDMA 的主机适配器位于 Vlan 上。 此示例将两个 SMB_ * 主机虚拟适配器分配到 VLAN 42。
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

在主机 Vnic 上启用 RDMA：

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

验证 RDMA 功能;确保功能不为零：

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>交换机嵌入式组合（SET）  

本部分概述了 Windows Server 2016 中的交换机嵌入组合（SET），其中包含以下各节。

- [设置概述](#bkmk_over)

- [设置可用性](#bkmk_avail)

- [用于集的支持和不支持的 Nic](#bkmk_nics)

- [设置与 Windows Server 网络技术的兼容性](#bkmk_compat)

- [设置模式和设置](#bkmk_modes)

- [集和虚拟机队列（Vmq）](#bkmk_vmq)

- [设置和 Hyper-v 网络虚拟化（HNV）](#bkmk_hnv)

- [设置和实时迁移](#bkmk_live)

- [MAC 地址在传输的数据包上使用](#bkmk_mac)

- [管理集团队](#bkmk_manage)

## <a name="bkmk_over"></a>设置概述

SET 是一种备用 NIC 组合解决方案，可在包括 Hyper-v 和 Windows Server 2016 中的软件定义网络 \(SDN @ no__t 堆栈的环境中使用。 将一些 NIC 组合功能集成到 Hyper-v 虚拟交换机。

集允许在一个或多个基于软件的虚拟网络适配器之间分组到8个物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。

设置成员网络适配器必须全部安装在要放置在团队中的同一物理 Hyper-v 主机中。

> [!NOTE]
> 仅在 Windows Server 2016 中的 Hyper-v 虚拟交换机中支持使用集。 你无法在 Windows Server 2012 R2 中部署集。

可以将组合 Nic 连接到同一个物理交换机，也可以连接到不同的物理交换机。 如果将 Nic 连接到不同的交换机，则这两个交换机必须位于同一子网中。

下图描绘了集体系结构。

![设置体系结构](../media/RDMA-and-SET/set_architecture.jpg)

由于已将集集成到 Hyper-v 虚拟交换机，因此不能使用虚拟机（VM）中的设置。 但是，可以在 Vm 中使用 NIC 组合。

有关详细信息，请参阅[虚拟机（vm）中的 NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms)。

此外，集体系结构不会公开团队界面。 相反，你必须配置 Hyper-v 虚拟交换机端口。

## <a name="bkmk_avail"></a>设置可用性

集适用于所有版本的 Windows Server 2016，其中包括 Hyper-v 和 SDN 堆栈。 此外，你可以使用 Windows PowerShell 命令和远程桌面连接来管理从运行支持这些工具的客户端操作系统的远程计算机进行设置。

## <a name="bkmk_nics"></a>用于集的支持的 Nic

你可以使用在 Windows Server 2016 的集团队中通过 Windows 硬件限定和徽标 \(WHQL @ no__t 测试的任何以太网 NIC。 设置要求所有属于集团队成员的网络适配器必须完全相同 \( i、相同型号、相同型号、相同的固件和驱动程序 @ no__t-1。 在一个组中的一到八个网络适配器之间设置支持。
  
## <a name="bkmk_compat"></a>设置与 Windows Server 网络技术的兼容性

集与 Windows Server 2016 中的以下网络技术兼容。

- 数据中心桥接 \(DCB @ no__t-1
  
- Hyper-v 网络虚拟化-Windows Server 2016 中同时支持 NV 和 VxLAN。  
- 接收方校验和卸载 \(IPv4、IPv6、TCP @ no__t-1-如果任何一个集成员支持这些设置，则支持这些设置。

- 远程直接内存访问 \(RDMA @ no__t-1

- 单个根 i/o 虚拟化 \(SR @ no__t-1

- 传输端校验和卸载 \(IPv4、IPv6、TCP @ no__t-1-如果所有组成员都支持这些设置，则支持这些设置。

- 虚拟机队列 \(VMQ @ no__t-1

- 虚拟接收方缩放 \(RSS @ no__t-1

集与 Windows Server 2016 中的以下网络技术不兼容。

- 802.1 x authentication。 802.1 x 可扩展身份验证协议 \(EAP @ no__t-1 数据包在集方案中由超级 @ no__t-Hqb-2v-fyv 虚拟交换机自动删除。
 
- IPsec 任务卸载 \(IPsecTO @ no__t-1。 这是一种传统的技术，它不受大多数网络适配器支持，并在其中存在，默认情况下处于禁用状态。

- 在主机或本机操作系统中使用 QoS \(pacer @ no__t-1。 这些 QoS 方案不是超级 @ no__t-0V 方案，因此这些技术不会相交。 此外，QoS 仍可用，但默认情况下未启用-必须有意启用 QoS。

- 接收方合并 \(RSC @ no__t-1。 已由超级 @ no__t-0V 虚拟交换机自动禁用 RSC。

- 接收方缩放 \(RSS @ no__t-1。 由于 Hyper-v 使用 VMQ 和 VMMQ 的队列，因此，在创建虚拟交换机时，RSS 始终处于禁用状态。

- TCP 烟囱卸载。 默认情况下，此技术处于禁用状态。

- 虚拟机 QoS \(VM @ no__t-1。 VM QoS 可用，但默认情况下处于禁用状态。 如果在设置环境中配置 VM QoS，则 QoS 设置将导致不可预知的结果。

## <a name="bkmk_modes"></a>设置模式和设置

不同于 NIC 组合，在创建集团队时，无法配置团队名称。 此外，NIC 组合支持使用备用适配器，但在集中不支持。 在部署集时，所有网络适配器都处于活动状态，并且没有处于备用模式。

NIC 组合和设置的另一个主要区别在于 NIC 组合提供了三种不同的组合模式，而集仅支持**独立于交换机**的模式。 对于交换机独立模式，设置团队成员所连接的交换机或交换机不会意识到集团队是否存在，也不会确定如何将网络流量分布到设置团队成员-而是设置团队分发入站网络跨集团队成员的流量。

创建新的集团队时，必须配置以下团队属性。

- 成员适配器

- 负载均衡模式

### <a name="member-adapters"></a>成员适配器

创建集组时，必须指定最多八个相同的网络适配器，这些适配器将绑定到 Hyper-v 虚拟交换机作为设置团队成员适配器。

### <a name="load-balancing-mode"></a>负载均衡模式

用于设置团队负载平衡分发模式的选项为**Hyper-v 端口**和**动态**。

**Hyper-v 端口**

Vm 连接到 Hyper-v 虚拟交换机上的端口。 当对集团队使用 Hyper-v 端口模式时，将使用 Hyper-v 虚拟交换机端口和关联的 MAC 地址来划分集团队成员之间的网络流量。

> [!NOTE]
> 当你结合使用 "设置" 和 "数据包直通" 时，将需要使用组合模式 "**独立**" 和 "负载平衡 **" 模式。**

由于相邻交换机始终在给定端口上看到特定 MAC 地址，因此交换机会将入口负载（从交换机到主机的流量）分发到 MAC 地址所在的端口。 当使用虚拟机队列（Vmq）时，此方法特别有用，因为队列可以放在流量预期到达的特定 NIC 上。

但是，如果主机只有几个 Vm，则此模式可能不够精细，无法实现合理的分发。 此模式还始终将单个 VM （即来自单个交换机端口的流量）限制为单个接口上可用的带宽。

**动态**

此负载平衡模式提供以下优点。

- 基于 TCP 端口和 IP 地址的哈希分布出站负载。  动态模式还将实时地重新平衡负载，以便给定的出站流可以在设置团队成员之间来回移动。

- 入站负载按与 Hyper-v 端口模式相同的方式进行分布。

此模式下的出站负载根据 flowlets 的概念进行动态平衡。 正如人类语音在单词和句子的结尾处自然中断一样，TCP 流（TCP 通信流）也自然发生了中断。 两个此类中断之间 TCP 流的部分称为 flowlet。

当动态模式算法检测到已遇到 flowlet 边界时（例如，当 TCP 流中发生了足够的长度中断时，算法会自动将流间重新平衡到其他团队成员（如果适用）。  在某些不常见的情况下，该算法还可能会定期重新平衡不包含任何 flowlets 的流。 因此，TCP 流和团队成员之间的相关性可能会随时更改，因为动态平衡算法可用于平衡团队成员的工作负荷。

## <a name="bkmk_vmq"></a>集和虚拟机队列（Vmq）

VMQ 并将工作设置得很好，并且应在每次使用 Hyper-v 并设置时启用 VMQ。

> [!NOTE]
> 设置 "始终显示所有集团队成员的可用队列总数"。 在 NIC 组合中，这称为 "队列总数" 模式。

大多数网络适配器的队列都可用于接收方缩放 \(RSS @ no__t 或 VMQ，但不能同时用于这两种情况。
  
某些 VMQ 设置似乎是 RSS 队列的设置，但实际上是基于当前正在使用的功能的通用队列上的设置。 每个 NIC 的高级属性中都有 `*RssBaseProcNumber` 和 `*MaxRssProcessors` 的值。

下面是几个提供更好系统性能的 VMQ 设置。

- 理想情况下，每个 NIC 都应将 @no__t 设置为一个大于或等于2（2）的偶数。 这是因为第一个物理处理器（核心 0 @no__t 0logical 处理器0和 1 @ no__t）通常执行大部分系统处理，因此，网络处理应 steered 远离此物理处理器。 

>[!NOTE]
>某些计算机体系结构每个物理处理器没有两个逻辑处理器，因此，对于这类计算机，基本处理器应大于或等于1。 如果有疑问，假定主机使用的是每个物理处理器体系结构2个逻辑处理器。

- 团队成员的处理器应为可行的、不重叠的范围。 例如，在4核主机 \(8 个逻辑处理器 @ no__t-1 （包含2个 10Gbps Nic 的团队）中，你可以将第一个主机设置为使用第2个基处理器，并使用4个内核;第二个设置为使用基本处理器6并使用2个内核。

## <a name="bkmk_hnv"></a>设置和 Hyper-v 网络虚拟化 \(HNV @ no__t-2

设置与 Windows Server 2016 中的 Hyper-v 网络虚拟化完全兼容。 HNV 管理系统向集驱动程序提供信息，该信息允许设置以一种针对 HNV 流量进行优化的方式分发网络流量负载。
  
## <a name="bkmk_live"></a>设置和实时迁移

Windows Server 2016 支持实时迁移。

## <a name="bkmk_mac"></a>MAC 地址在传输的数据包上使用

使用动态负载分布配置集团队时，来自单个源 \(such 作为单个 VM @ no__t 的数据包将同时分布在多个团队成员之间。 

若要防止交换机混淆并阻止 MAC 稳定告警，请将源 MAC 地址替换为在关联团队成员以外的团队成员上传输的帧上的不同 MAC 地址。 因此，每个团队成员使用不同的 MAC 地址，并阻止 MAC 地址冲突，除非发生故障。

当在主 NIC 上检测到故障时，集组合软件会开始使用被选为临时关联团队 @no__t 成员的团队成员上 VM 的 MAC 地址，该地址将作为 VM 的接口 @ no_ 立即显示给交换机： @_t-1。

此更改仅适用于要在 VM 的关联团队成员上发送的流量，并将 VM 自己的 MAC 地址作为源 MAC 地址。 其他流量将继续与发生故障之前使用的任何源 MAC 地址一起发送。

以下列表描述了基于团队配置方式的组分组 MAC 地址替换行为：

- 在具有 Hyper-v 端口分发的交换机独立模式下

    - 每个 vmSwitch 端口都关联给团队成员
  
    - 在关联端口的团队成员上发送每个数据包  
  
    - 未完成源 MAC 替换  
  
- 采用动态分布的交换机独立模式
  
    - 每个 vmSwitch 端口都关联给团队成员  
  
    - 所有 ARP/NS 数据包都发送到关联端口的团队成员。  
  
    - 作为关联团队成员的团队成员发送的数据包没有源 MAC 地址替换  
  
    - 在关联团队成员以外的团队成员上发送的数据包将进行源 MAC 地址替换  
  
## <a name="bkmk_manage"></a>管理集团队

建议使用 System Center Virtual Machine Manager \(VMM @ no__t 来管理集团队，但也可以使用 Windows PowerShell 来管理集。 以下各节提供了可用于管理集的 Windows PowerShell 命令。

有关如何使用 VMM 创建组集的信息，请参阅 System Center VMM 库主题[创建逻辑交换机](https://docs.microsoft.com/system-center/vmm/network-switch)中的 "设置逻辑交换机" 一节。
  
### <a name="create-a-set-team"></a>创建集团队

你必须在创建 Hyper-v 虚拟交换机时，使用**新的 VMSwitch** Windows PowerShell 命令创建一个集团队。

创建 Hyper-v 虚拟交换机时，你必须在命令语法中包含新的**EnableEmbeddedTeaming**参数。 在下面的示例中，将创建一个名为**TeamedvSwitch**且具有嵌入组合和两个初始团队成员的 hyper-v 交换机。
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
当**NetAdapterName**的参数是 nic 数组而不是单个 nic 时，Windows PowerShell 将采用**EnableEmbeddedTeaming**参数。 因此，你可以通过以下方式修改上一个命令。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

如果要创建具有单个团队成员的可设置支持的交换机，以便以后可以添加团队成员，则必须使用 EnableEmbeddedTeaming 参数。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>添加或删除集团队成员

**VMSwitchTeam**命令包括**NetAdapterName**选项。 若要更改集团队中的团队成员，请在**NetAdapterName**选项后输入所需的团队成员列表。 如果**TeamedvSwitch**最初是通过 nic 1 和 nic 2 创建的，则下面的示例命令将删除设置团队成员 "nic 2" 并添加新的设置团队成员 "nic 3"。
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>删除集团队

仅可通过删除包含集组的 Hyper-v 虚拟交换机来删除集组。  有关如何删除 Hyper-v 虚拟交换机的信息，请使用删除主题[-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) 。 下面的示例删除名为**SETvSwitch**的虚拟交换机。

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>更改集团队的负载分配算法

**VMSwitchTeam** Cmdlet 具有**LoadBalancingAlgorithm**选项。 此选项采用以下两个可能的值之一：**HyperVPort**或**Dynamic**。 若要设置或更改交换机嵌入的团队的负载分配算法，请使用此选项。 

在下面的示例中，名为**TeamedvSwitch**的 VMSwitchTeam 使用**动态**负载平衡算法。  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>将虚拟接口层面到物理团队成员

集允许您在虚拟接口 \( i、Hyper-v 虚拟交换机端口 @ no__t 和团队中的一个物理 Nic 之间创建关联。 

例如，如果为 SMB @ no__t-0Direct 创建了两个主机 Vnic，如[使用 SET 和 RDMA 创建 Hyper-v 虚拟交换机 vnic](#bkmk_set-rdma)部分中所述，可以确保这两个 vnic 使用不同的团队成员。 

添加到该部分中的脚本后，可以使用以下 Windows PowerShell 命令。

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

有关详细信息，请详细了解《 [Windows Server 2016 NIC 和交换机嵌入式组合用户指南》](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)部分的4.2.5。
