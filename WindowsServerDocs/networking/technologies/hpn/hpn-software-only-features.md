---
title: 仅软件 (SO) 功能和技术
description: 这些功能作为操作系统的一部分实现的并独立于基础个 NIC。 有时这些功能需要一些调整的最佳操作的 NIC。 其中的示例包括 hyper-v 功能，例如虚拟机服务质量 (vmQoS)、 访问控制列表 (Acl) 和非 HYPER-V 功能，例如 NIC 组合。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 504bc92971e778b468812dc4064fa6f0afff87ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823778"
---
# <a name="software-only-so-features-and-technologies"></a>仅软件 (SO) 功能和技术
仅限软件的功能作为操作系统的一部分实现的并独立于基础个 NIC。 有时这些功能需要一些调整的最佳操作的 NIC。 其中的示例包括 hyper-v 功能，例如虚拟机服务质量 (vmQoS)、 访问控制列表 (Acl) 和非 HYPER-V 功能，例如 NIC 组合。

## <a name="access-control-lists-acls"></a>访问控制列表 (Acl)

有关管理的安全性的 VM 的 HYPER-V 和 SDNv1 功能。 此功能适用于非虚拟化的 HYPER-V 堆栈和 HVNv1 堆栈。 你可以管理的 HYPER-V 切换通过 Acl [Add-vmnetworkadapteracl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps)并[Remove-vmnetworkadapteracl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) PowerShell cmdlet。

## <a name="extended-acls"></a>扩展的 Acl

HYPER-V 虚拟交换机扩展 Acl，可配置的 HYPER-V 虚拟交换机扩展端口 Acl，以提供防火墙保护，并在数据中心中的租户 Vm 实施安全策略。 端口 Acl 的 HYPER-V 虚拟交换机上而不是在 Vm 配置，因为管理员可以管理多租户环境中的所有租户的安全策略。

你可以管理的 HYPER-V 交换机扩展通过 Acl [Add-vmnetworkadapterextendedacl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps)并[Remove-vmnetworkadapterextendedacl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) PowerShell cmdlet。

>[!TIP] 
>此功能适用于 HNVv1 堆栈。 中的 SDN 堆栈的 Acl，请参阅软件定义网络 SDN） 下的 Acl。

有关扩展端口访问控制列表中此库的详细信息，请参阅[使用扩展端口访问控制列表创建安全策略](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists)。

## <a name="nic-teaming"></a>NIC 组合

NIC 组合，也称为 NIC 捆绑是多个 NIC 端口到主机感觉为单个 NIC 端口的实体的聚合。 NIC 组合可防止单个 NIC 端口 （或连接到它的电缆） 的故障。 它还将聚合更快的吞吐量的网络的流量。 有关更多详细信息，请参阅[NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

使用 Windows Server 2016 可以两种方法执行组合：

1.  Windows Server 2012 成组解决方案

2.  Windows Server 2016 交换机嵌入式组合 (SET)


## <a name="rsc-in-the-vswitch"></a>RSC vSwitch 中

接收段合并 (RSC) vSwitch 中是一项功能，将数据包属于相同的流式处理和到达之间网络中断和交付给操作系统之前将它们合并成单个数据包。 Windows Server 2019 中的虚拟交换机具有此功能。 有关此功能的更多详细信息，请参阅[vSwitch 中接收段合并](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch)。

## <a name="software-defined-networking-sdn-acls"></a>软件定义网络 (SDN) 的 Acl

Windows Server 2016 中的 SDN 扩展改进的途径来支持 Acl。 在 Windows Server 2016 SDN v2 堆栈中，而不是 Acl 和扩展 Acl 使用 SDN Acl。 可以使用网络控制器管理 SDN Acl。 

## <a name="sdn-quality-of-service-qos"></a>SDN 的服务质量 (QoS)

Windows Server 2016 中的 SDN 扩展改进方式可在 5 元组的基础上提供带宽控制 （出口的预订、 出口限制的影响和入口限制）。 通常情况下，这些策略将应用在 vNIC 或 vmNIC 级别，但您可以使它们要特别得多。 在 Windows Server 2016 SDN v2 堆栈中，而不是 vmQoS 使用 SDN QoS。 可以使用网络控制器管理 SDN QoS。

## <a name="switch-embedded-teaming-set"></a>切换嵌入组合 (SET)

集是一个可以在 Windows Server 2016 中包括的 HYPER-V 和软件定义网络 (SDN) 堆栈的环境中使用的替代 NIC 组合解决方案。 组将某些 NIC 组合功能集成到 HYPER-V 虚拟交换机。 有关交换机嵌入式组合此库中的信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="virtual-receive-side-scaling-vrss"></a>虚拟接收方缩放 (vRSS)

软件 vRSS 用于分布在多个逻辑处理器 (LPs) VM 的目标为 VM 的传入流量。 软件 vRSS 使的 VM 能够处理更多的网络流量，比单个 LP 能够处理。 有关详细信息，请参阅[虚拟接收方缩放 (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top)。

## <a name="virtual-machine-quality-of-service-vmqos"></a>虚拟机服务质量 (vmQoS)

虚拟机服务质量是允许对每个 VM 产生的流量设置限制的交换机的 HYPER-V 功能。 它还使要保留的外部网络连接上的带宽量，以便一个 VM 不能影响虚拟的带宽的另一个 VM 的 VM。 在 Windows Server 2016 SDN v2 堆栈，SDN QoS 替换 vmQoS。

出口限制的影响和出口预订，可以设置 vmQoS。 创建 HYPER-V 交换机之前，您必须确定出口保留模式下 （相对权重或绝对带宽）。

-  确定与新 VMSwitch PowerShell cmdlet 的 – MinimumBandwidthMode 参数的出口保留模式。

-  设置 Set-vmnetworkadapter PowerShell cmdlet 与 – MaximumBandwidth 参数的出口限制的值。

-  设置为出口保留项与以下参数之一设置 VMNetworkAdapter PowerShell cmdlet 的值：

   -  有关新 VMSwitch cmdlet 的 – MinimumBandwidthMode 参数是否绝对，然后在设置 VMNetworkAdapter cmdlet 上设置 – MinimumBandwidthAbsolute 参数。

   -  有关新 VMSwitch cmdlet 的 – MinimumBandwidthMode 参数是否权重，然后在设置 VMNetworkAdapter cmdlet 上设置 – MinimumBandwidthWeight 参数。

由于使用此功能的算法中的限制，我们建议，最高的权重或绝对带宽不能超过 20 次的最小权重或绝对带宽。 如果需要更多的控制，请考虑使用 SDN 堆栈和 SDN QoS 功能。


---