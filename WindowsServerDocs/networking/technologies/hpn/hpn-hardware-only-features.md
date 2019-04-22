---
title: 高性能网络
description: 本主题提供的卸载和 Windows Server 2016 中的优化技术的概述，并包括有关这些技术的附加指导的链接。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 09e18a41452baa0add8e055ceb22d2f5c2ad886e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815828"
---
## <a name="hardware-only-ho-features-and-technologies"></a>仅硬件 (HO) 功能和技术

这些硬件加速功能提高配合该软件中的网络性能，但不联系紧密的任何软件功能的一部分。 其中的示例包括中断裁决、 流控制和接收方 IPv4 校验和卸载。

>[!TIP]
>SH 和主机功能都可用，如果已安装的 NIC 支持它。 下面的功能说明将介绍如何判断 NIC 是否支持该功能。

### <a name="address-checksum-offload"></a>地址校验和卸载

地址校验和卸载是一种 NIC 功能，将计算地址的校验和 （IP、 TCP、 UDP） 与 NIC 硬件卸载发送和接收。

接收路径上校验和卸载计算 （根据需要） 的 IP、 TCP 和 UDP 标头中的校验和，并向操作系统指示，校验和传递、 失败或未选中。 如果 NIC 断言的校验和有效，操作系统将接受不经质询地将数据包。 如果 NIC 断言校验和是无效或未选中，IP/TCP/UDP 堆栈在内部将重新计算校验和。 如果计算的校验和失败，获取丢弃该数据包。

发送路径上校验和卸载计算并插入适当的 IP、 TCP 或 UDP 标头的校验和。

禁用发送路径上的校验和卸载不会禁用校验和计算和插入数据包发送到使用大量发送卸载 (LSO) 功能的微型端口驱动程序。  若要禁用校验和卸载的所有计算，用户还必须都禁用 LSO。

_**管理地址校验和卸载**_

在高级属性中有多个非重复属性：

-   IPv4 校验和卸载

-   TCP 校验和卸载 (IPv4)

-   TCP 校验和卸载 (IPv6)

-   UDP 校验和卸载 (IPv4)

-   UDP 校验和卸载 (IPv6)

默认情况下，这些都始终启用。 我们建议始终启用所有这些卸载。

可以使用启用 NetAdapterChecksumOffload 和禁用 NetAdapterChecksumOffload cmdlet 管理校验和卸载。 例如，以下 cmdlet 可启用 TCP (IPv4) 和 UDP (IPv4) 校验和计算：

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**有关使用地址校验和卸载提示**_

无论何种工作负荷或情况下始终应启用地址校验和卸载。 大多数基本的所有卸载此技术始终提升网络性能。 校验和卸载，还需要为其他无状态卸载工作包括接收方缩放 (RSS)、 接收段合并 (RSC) 和大量发送卸载 (LSO)。

### <a name="interrupt-moderation-im"></a>中断裁决 (IM)

IM 中断操作系统之前进行缓冲多个接收的数据包。 当 NIC 收到数据包时，它将启动一个计时器。 当缓冲区已满，或计时器过期，具体取决于第一个，NIC 会中断操作系统。 

数目的 Nic，支持多个只需打开/关闭中断裁决。 大多数 Nic 支持 IM 低、 中和速率很高的概念。 不同的费率表示更短或更长时间的计时器和适当的缓冲区大小调整，以减少延迟 （低中断裁决） 或减少中断 （高中断裁决）。

没有被摧毁减少中断和过度延迟的数据包发送之间的平衡。 通常情况下，数据包处理与已启用的中断裁决会更有效。 高性能或低延迟的应用程序可能需要评估的禁用或减少中断裁决的影响。

### <a name="jumbo-frames"></a>Jumbo 帧

Jumbo 帧是允许应用程序发送为比默认值大得多 1500 个字节的帧的 NIC 和网络功能。 通常 jumbo 帧的限制为大约 9000 个字节，但可能更小。

没有到 jumbo 帧支持 Windows Server 2012 R2 中的更改。

在 Windows Server 2016 中新增了一个卸载：MTU_for_HNV. 此新卸载适用于 Jumbo 帧设置，以确保封装的流量不需要在主机与相邻的交换机之间的分段。 SDN 堆栈的这一新功能已自动计算哪些 MTU 播发和在网络上使用哪些 MTU 的 NIC。 如果任何 HNV 卸载正在使用不同的 MTU 的这些值。 （在功能兼容性表，表 1 MTU_for_HNV 必须相同交互如卸载具有，因为它直接与 HNVv2 HNVv2 减轻负载。）

### <a name="large-send-offload-lso"></a>大量发送卸载 (LSO)

LSO 允许应用程序将大数据块到 NIC 和 NIC 分隔线数据传递到适合内最大传输单元 (MTU) 的网络数据包。

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

接收段合并，也称为大接收卸载，是采用相同流到达之间网络中断并交付给操作系统之前将它们合并成单个数据包的一部分的数据包的 NIC 功能。 RSC 不绑定到 HYPER-V 虚拟交换机的 Nic 上可用。 有关详细信息，请参阅[接收段合并 (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch)。