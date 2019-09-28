---
title: NIC 组合设置
description: 在本主题中，我们将为你概述 NIC 组属性，例如组合和负载平衡模式。 此外，我们还会向你介绍备用适配器设置和主团队接口属性的详细信息。 如果 NIC 组中至少有两个网络适配器，则无需指定备用适配器来实现容错。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ab9a8e309c8031108d58c73d82357e913d5ce398
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396480"
---
# <a name="nic-teaming-settings"></a>NIC 组合设置
在本主题中，我们将为你概述 NIC 组属性，例如组合和负载平衡模式。 此外，我们还会向你介绍备用适配器设置和主团队接口属性的详细信息。 如果 NIC 组中至少有两个网络适配器，则无需指定备用适配器来实现容错。


  
![NIC 团队属性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>分组模式 
分组模式选项是**独立交换**和**交换机依赖**的选项。 交换机相关模式包括**静态组合**和**链接聚合控制协议（LACP）** 。 

>[!TIP]
>为了获得最佳的 NIC 团队性能，我们建议你使用动态分发的负载平衡模式。  
  
### <a name="switch-independent"></a>独立于交换机
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
使用带有动态分布的交换机独立模式时，会根据动态负载平衡算法修改的 TCP 端口地址哈希来分发网络流量负载。 动态负载平衡算法会重新分配流以优化团队成员带宽利用率，以便单个流传输可以从一个活动的团队成员移到另一个活动的团队成员。 此算法将考虑重新分布流量可能导致不按序传递数据包的小可能性，因此，需要执行一些步骤来最大程度地降低数据包的顺序。  
  
### <a name="switch-dependent"></a>依赖于交换机  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> 交换机依赖组合要求所有团队成员均连接到相同的物理交换机或多底盘交换机，它们在多个机箱中共享交换机 ID。


- **静态组合。** 静态组合要求你手动配置交换机和主机，以确定构成团队的链接。 由于这是一个静态配置的解决方案，因此没有其他协议可帮助交换机和主机标识错误插入的电缆或可能导致团队无法执行的其他错误。 服务器级交换机通常支持这种模式。

- **链接聚合控制协议（LACP）。** 与静态组合不同，LACP 组合模式动态标识在主机和交换机之间连接的链接。 这种动态连接使团队能够自动创建，但在理论上，只是通过传输或接收来自对等实体的 LACP 数据包来扩展和减少团队。 所有服务器类交换机都支持 LACP，所有这些交换机都需要网络运营商在交换机端口上以管理方式启用 LACP。 配置 LACP 的分组模式时，NIC 组合始终在具有短计时器的 LACP 活动模式下运行。  目前不能使用任何选项来修改计时器或更改 LACP 模式。


当你结合使用交换机依赖模式和动态分布时，会根据动态负载平衡算法修改的 TransportPorts 地址哈希来分配网络流量负载。  动态负载平衡算法会重新分发流，以优化团队成员带宽利用率。 单个流传输可以作为动态分发的一部分从一个活动的团队成员移到另一个。 此算法将考虑重新分布流量可能导致不按序传递数据包的小可能性，因此，需要执行一些步骤来最大程度地降低数据包的顺序。  
  
与所有交换机相关配置一样，开关决定了如何在团队成员之间分发入站流量。  此开关应能合理地在团队成员之间分配流量，但它具有完全独立性来确定其工作方式。  


## <a name="load-balancing-modes"></a>负载均衡模式  
负载均衡分布模式的选项包括：**地址哈希**、 **Hyper-v 端口**和**动态**。  
  
### <a name="address-hash"></a>地址哈希
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
使用 Windows PowerShell 指定下列哈希函数组件的值。  
  
-   源和目标 TCP 端口以及源和目标 IP 地址。 当你选择 "**地址哈希**" 作为负载平衡模式时，这是默认设置。  
  
-   仅源和目标 IP 地址。  
  
-   仅限源和目标 MAC 地址。  
  
TCP 端口哈希可创建流量流的最精细分布，从而可在 NIC 团队成员之间独立移动的更小流。 但是，不能对不是 TCP 或 UDP 的通信使用 TCP 端口哈希，也不能将 TCP 和 UDP 端口与堆栈隐藏在一起（例如，使用 IPsec 保护的流量）。 在这些情况下，哈希会自动使用 IP 地址哈希，如果流量不是 IP 流量，则使用 MAC 地址哈希。  
  
### <a name="hyper-v-port"></a>Hyper-v 端口
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
由于相邻交换机始终在一个端口上看到特定 MAC 地址，因此交换机会根据目标 MAC （VM MAC）地址在多个链接上分发入口负载（从交换机到主机的流量）。 当使用虚拟机队列（Vmq）时，此方法特别有用，因为队列可以放在流量预期到达的特定 NIC 上。  
  
但是，如果主机只有几个 Vm，则此模式可能不够精细，无法实现合理的分发。 此模式还始终将单个 VM （即来自单个交换机端口的流量）限制为单个接口上可用的带宽。 NIC 组合使用 Hyper-v 虚拟交换机端口作为标识符，而不是使用源 MAC 地址，因为在某些情况下，可能会在一个交换机端口上配置一个 VM 的多个 MAC 地址。  
  
### <a name="dynamic"></a>动态
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
此模式下的出站负载根据 flowlets 的概念进行动态平衡。 正如人类语音在单词和句子的结尾处自然中断一样，TCP 流（TCP 通信流）也自然发生了中断。 两个此类中断之间 TCP 流的部分称为 flowlet。  
  
当动态模式算法检测到已遇到 flowlet 边界时（例如 TCP 流中发生了足够长的时间段）时，算法会自动将流间重新平衡到其他团队成员（如果适用）。  在某些情况下，该算法还可能会定期重新平衡不包含任何 flowlets 的流。 因此，TCP 流和团队成员之间的相关性可能会随时更改，因为动态平衡算法可用于平衡团队成员的工作负荷。  
  
无论是使用独立交换机还是使用交换机相关模式来配置团队，建议使用动态分布模式以获得最佳性能。  
  
如果 NIC 组仅有两个团队成员，在独立于交换机的模式下配置，并且启用了活动/备用模式，并且有一个 NIC 处于活动状态，另一个 NIC 配置为备用，则此规则例外。 通过此 NIC 团队配置，地址哈希分发可提供比动态分发更好的性能。  


## <a name="standby-adapter-setting"></a>备用适配器设置  
备用适配器的选项为 "**无" （所有适配器处于活动状态）** ，或选择作为备用适配器的 NIC 组中的特定网络适配器。 将 NIC 配置为备用适配器时，所有其他未选择的团队成员都处于活动状态，并且适配器不会发送或处理任何网络流量，直到活动的 NIC 出现故障。 活动 NIC 出现故障后，备用 NIC 将变为活动状态并处理网络流量。 所有团队成员都还原到服务时，备用团队成员返回到备用状态。  

如果你有两个 NIC 组，并选择将一个 NIC 配置为备用适配器，则会丢失两个活动 Nic 存在的带宽聚合优势。  不需要指定备用适配器即可实现容错;如果 NIC 组中至少有两个网络适配器，则容错始终存在。
 
  
## <a name="primary-team-interface-property"></a>主要团队界面属性  
若要访问 "主团队界面" 对话框，必须单击下图中突出显示的链接。  
  
![主要团队界面属性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
单击突出显示的链接后，将打开 "**新建团队界面**" 对话框。  
  
![“新建组接口”对话框](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
如果你使用的是 Vlan，则可以使用此对话框来指定 VLAN 编号。  
  
无论是否使用 Vlan，都可以为 NIC 组指定 tNIC 名称。  
  


---