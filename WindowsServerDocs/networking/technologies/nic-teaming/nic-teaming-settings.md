---
title: NIC 组合设置
description: 在本主题中，我们为您提供的 NIC 组属性，如组合概述和负载平衡模式。 我们还会为您提供的备用适配器设置和主要组接口属性的详细信息。 如果 NIC 组中具有至少两个网络适配器，您不需要指定的容错能力的备用适配器。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 57957e88ff4c398be23355534d5cc0ad7f920bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877928"
---
# <a name="nic-teaming-settings"></a>NIC 组合设置
在本主题中，我们为您提供的 NIC 组属性，如组合概述和负载平衡模式。 我们还会为您提供的备用适配器设置和主要组接口属性的详细信息。 如果 NIC 组中具有至少两个网络适配器，您不需要指定的容错能力的备用适配器。


  
![NIC 组属性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>组合模式 
组合模式的选项都**独立于交换机**并**交换机依赖**。 依赖交换机的模式包括**静态组合**并**链接聚合控制协议 (LACP)**。 

>[!TIP]
>为了获得最佳的 NIC 组性能，我们建议使用动态通讯的负载平衡模式。  
  
### <a name="switch-independent"></a>独立于交换机
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
对动态分发独立于交换机模式时，为已修改的动态负载平衡算法基于 TCP 端口地址哈希分布网络流量负载。 动态负载平衡算法将重新分发流优化，以便单独的流传输可以从一个活动的团队成员移动到另一个团队成员带宽利用率。 该算法将考虑在内，重新分发流量可能会导致无序传送的数据包，因此它采用最大程度减少这种可能性的步骤可能发生。  
  
### <a name="switch-dependent"></a>依赖于交换机  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> 所有团队成员都连接到同一个物理交换机或多底盘切换开关依赖组合要求共享在多个机箱之间切换 ID。


- **静态组合。** 静态组合要求您手动配置了交换机和主机以确定哪个链接就会形成团队。 由于这是静态配置的解决方案，没有其他协议，以帮助交换机和主机来识别未正确插入的电缆或可能造成该组失败执行的其他错误。 服务器级交换机通常支持这种模式。

- **链接聚合控制协议 (LACP)。** 与静态组合不同 LACP 组合模式动态找出主机和交换机之间的链接。 此动态连接，只需通过传输和接收 LACP 数据包从对等实体可以自动创建团队的和，从理论上讲，但很少的做法、 扩展和减少的团队中。 所有服务器级交换机都支持 LACP，并且所有需要网络运营商，以管理方式启用 LACP 的交换机端口上。 在配置的 LACP 组合模式时，NIC 组合始终会在运行 LACP 的主动模式下使用短计时器。  目前可修改计时器或 LACP 模式更改任何选项不。


对动态通讯组依赖于交换机模式时，为已修改的动态负载平衡算法基于 TransportPorts 地址哈希分布网络流量负载。  动态负载平衡算法将重新分发流优化团队成员带宽利用率。 单独的流传输可以从一个活动的团队成员移到另一个作为动态通讯组的一部分。 该算法将考虑在内，重新分发流量可能会导致无序传送的数据包，因此它采用最大程度减少这种可能性的步骤可能发生。  
  
作为所有开关的相关配置开关会都确定如何分发的团队成员之间的入站的流量。  交换机应更合理地在团队成员之间分配流量，但它具有完整的独立性，以确定如何执行以上。  


## <a name="load-balancing-modes"></a>负载平衡模式  
负载均衡分发模式的选项都**地址哈希**， **HYPER-V 端口**，并**动态**。  
  
### <a name="address-hash"></a>地址哈希
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
使用 Windows PowerShell 来指定以下哈希函数组件的值。  
  
-   源和目标 TCP 端口以及源和目标 IP 地址。 这是默认的选择时**地址哈希**作为负载平衡模式。  
  
-   源和目标 IP 地址仅。  
  
-   源和目标 MAC 地址仅。  
  
TCP 端口哈希创建最精细分发通信流，从而导致在 NIC 团队成员之间独立移动的流更小。 但是，不能为不是 TCP 的流量使用 TCP 端口哈希或基于 UDP 的或其中的 TCP 和 UDP 端口均为隐藏状态从堆栈，如受 IPsec 保护流量。 在这些情况下，哈希会自动使用 IP 地址哈希，或者，如果该流量不是 IP 流量，则使用 MAC 地址哈希。  
  
### <a name="hyper-v-port"></a>HYPER-V 端口
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
由于相邻交换机始终发现一个端口上的特定 MAC 地址，交换机分配入口负载 （流量从交换机到主机） 上基于目标 MAC (VM MAC) 地址的多个链接。 这是特别有用时使用，虚拟机队列 (Vmq)，因为可以在流量预期到达特定 NIC 上放置一个队列。  
  
但是，如果主机有仅几个 Vm，此模式可能不是足够精细以获得充分均衡的分布。 此模式也始终会限制单个 VM （即，从单一的交换机端口的流量） 到单个接口上可用的带宽。 NIC 组合使用的 HYPER-V 虚拟交换机端口作为标识符而不是因为在某些情况下，可能使用多个交换机端口上的 MAC 地址配置 VM 使用的源 MAC 地址。  
  
### <a name="dynamic"></a>动态
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
在此模式下的出站负载动态均衡根据 flowlets 的概念。 就像人的语音具有单词和句子末尾的自然间隔，TCP 流 （TCP 通信流） 还具有自然会出现一次中断。 两个此类分隔线之间的 TCP 流的部分称为 flowlet。  
  
当动态模式算法检测到，遇到 flowlet 边界-例如足够长的中断发生时在 TCP 流中的算法自动重新平衡流到另一个团队成员在适当。  在某些情况下算法可能也会定期重新均衡它不包含任何 flowlets 的流。 正因为如此，TCP 流和团队成员之间的关联可以更改在任何时间，因为动态负载平衡算法起作用，以平衡的团队成员的工作负荷。  
  
是否使用独立于交换机或依赖于交换机模式之一配置团队，则建议为获得最佳性能，使用动态分配模式。  
  
NIC 组具有两个团队成员、 在独立于交换机模式下，配置和已启用，其中一个 NIC 将活动的活动/独立模式，另一个配置待机状态时，没有此规则的例外。 与此 NIC 组配置中，地址哈希分布提供略有更好的性能比动态通讯组。  


## <a name="standby-adapter-setting"></a>备用适配器设置  
备用适配器的选项都**None （所有适配器处于活动状态）** 或您选择的充当待机适配器 NIC 组中的特定网络适配器。 在 NIC 配置作为备用适配器时，其他所有未选定的团队成员都处于活动状态，并发送到或可用的 NIC 失败之前的适配器处理无网络通信流量。 可用的 NIC 失败后，备用 NIC 将变为活动状态，并且进程网络流量。 所有团队成员获取还原到服务，备用团队成员将返回备用状态。  

如果有两个 NIC 组并选择要将一个 NIC 配置为备用适配器，您丢失带宽聚合许多优势，存在两个活动的 nic。  不需要指定备用适配器，以实现容错功能;容错功能始终是 NIC 组中至少两个网络适配器时存在的。
 
  
## <a name="primary-team-interface-property"></a>主团队接口属性  
若要访问主组接口对话框中，必须单击下图中突出显示的链接。  
  
![主要组接口属性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
单击突出显示的链接，以下后**新的团队界面**对话框随即打开。  
  
![“新建组接口”对话框](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
如果你在使用 Vlan，可以使用此对话框来指定 VLAN 编号。  
  
指示在使用 Vlan，可以指定 NIC 组合的 tNIC 名称。  
  


---