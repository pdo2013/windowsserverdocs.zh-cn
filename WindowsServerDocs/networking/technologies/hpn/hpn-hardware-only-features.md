---
title: 高性能网络
description: 本主题概述 Windows Server 2016 中的卸载和优化技术，并提供指向有关这些技术的其他指南的链接。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 5cf6a5057151c696bc1c29a1dcf6fc18c776605a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405744"
---
## <a name="hardware-only-ho-features-and-technologies"></a>仅硬件 (HO) 功能和技术

这些硬件加速可提高网络性能和软件的性能，但不它紧密任何软件功能的组成部分。 这些示例包括中断裁决、流控制和接收端 IPv4 校验和卸载。

>[!TIP]
>如果安装的 NIC 支持，SH 和 HO 功能可用。 以下功能说明将介绍如何判断 NIC 是否支持该功能。

### <a name="address-checksum-offload"></a>地址校验和卸载

地址校验和卸载是一项 NIC 功能，该功能可将地址校验和（IP、TCP、UDP）的计算工作方式用于发送和接收的 NIC 硬件。

在接收路径上，校验和将根据需要计算 IP、TCP 和 UDP 标头中的校验和，并向操作系统指示校验和是通过、失败还是未选中。 如果 NIC 断定校验和是有效的，则 OS 接受数据包 unchallenged。 如果 NIC 断言校验和无效或未选中，则 IP/TCP/UDP 堆栈会在内部再次计算校验和。 如果计算出的校验和失败，则丢弃数据包。

在发送路径上，校验和卸载会根据需要计算并将校验和插入到 IP、TCP 或 UDP 标头。

禁用发送路径上的校验和卸载不会对使用大型发送卸载（LSO）功能发送到微型端口驱动程序的数据包禁用校验和计算和插入。  若要禁用所有校验和卸载计算，用户还必须禁用 LSO。

_**管理地址校验和卸载**_

在高级属性中有多个不同的属性：

-   IPv4 校验和卸载

-   TCP 校验和卸载（IPv4）

-   TCP 校验和卸载（IPv6）

-   UDP 校验和卸载（IPv4）

-   UDP 校验和卸载（IPv6）

默认情况下，这些始终处于启用状态。 建议始终启用所有这些卸载。

可以使用 NetAdapterChecksumOffload 和 NetAdapterChecksumOffload cmdlet 来管理校验和卸载。 例如，以下 cmdlet 启用了 TCP （IPv4）和 UDP （IPv4）校验和计算：

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**使用地址校验和卸载的提示**_

不管工作负荷或情况如何，都应始终启用地址校验和卸载。 这一最基本的卸载技术始终会提高你的网络性能。 其他无状态卸载还需要使用校验和卸载，包括接收方缩放（RSS）、接收段合并（RSC）和大型发送卸载（LSO）。

### <a name="interrupt-moderation-im"></a>中断裁决（IM）

IM 在中断操作系统之前缓冲多个接收的数据包。 当 NIC 收到数据包时，它将启动一个计时器。 当缓冲区已满或计时器过期时，NIC 将中断操作系统。 

许多 Nic 不只支持中断仲裁。 大多数 Nic 都支持 IM 的低、中和高的概念。 不同速率表示较短或更长时间的计时器和适当的缓冲区大小调整，以减少延迟（低中断裁决）或减少中断（高中断裁决）。

减少中断与过度延迟数据包传送之间有一定的平衡。 通常，启用中断裁决后，数据包处理的效率会更高。 高性能和低延迟的应用程序可能需要评估禁用或减少中断裁决的影响。

### <a name="jumbo-frames"></a>Jumbo 帧

巨型帧是一项 NIC 和网络功能，它允许应用程序发送比默认的1500字节大得多的帧。 通常，巨型帧的限制大约为9000个字节，但可能更小。

Windows Server 2012 R2 中没有对超长帧支持的更改。

在 Windows Server 2016 中，有一个新的卸载：MTU_for_HNV. 此新卸载适用于巨型帧设置，以确保封装的流量不需要在主机与相邻交换机之间进行分段。 SDN 堆栈的这一新功能使 NIC 自动计算要播发的 MTU 以及要在网络上使用的 MTU。 如果正在使用任何 HNV 卸载，则 MTU 的这些值是不同的。 （在功能兼容性表中，表1，MTU_for_HNV 与 HNVv2 卸载具有相同的交互，因为它直接与 HNVv2 卸载相关。）

### <a name="large-send-offload-lso"></a>大量发送卸载 (LSO)

LSO 允许应用程序将大数据块传递到 NIC，NIC 会将数据分解为可容纳在网络的最大传输单位（MTU）内的数据包。

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

接收段合并（也称为大型接收卸载）是一项 NIC 功能，该功能将数据包作为同一流中的一部分到达网络中断，并将它们合并到单个数据包中，然后将它们传递给操作系统。 RSC 在绑定到 Hyper-v 虚拟交换机的 Nic 上不可用。 有关详细信息，请参阅[接收段合并（RSC）](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch)。