---
title: 选择网络适配器
description: 本主题是适用于 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2b50f4b286e90a450278243c0294ea0aa7f221bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875848"
---
# <a name="choosing-a-network-adapter"></a>选择网络适配器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解的一些功能可能会影响您的购买选择的网络适配器。

网络密集型应用程序需要高性能网络适配器。 本部分探讨选择网络适配器，以及如何配置不同的网络适配器设置，以实现最佳的网络性能的一些注意事项。

> [!TIP]
>  可以使用 Windows PowerShell 来配置网络适配器设置。 有关详细信息，请参阅[Windows PowerShell 中的网络适配器 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

##  <a name="bkmk_offload"></a> 卸载功能

中央处理单元中的任务卸载\(CPU\)到网络适配器可以降低在服务器上，从而提高总体系统性能的 CPU 使用率。

Microsoft 产品中的网络堆栈可以卸载一个或多个任务如果选择具有适当的网络适配器的网络适配器卸载功能。 下表提供了可在 Windows Server 2016 中的不同卸载功能的简要概述。
  
|卸载类型|描述|
|------------------|-----------------|  
|对于 TCP 校验和计算|网络堆栈可卸载的计算和验证的传输控制协议\(TCP\)上的校验和发送和接收的代码路径。 它还可以将卸载的计算和验证 IPv4 和 IPv6 上的校验和发送和接收的代码路径。|  
|Udp 校验和计算 |网络堆栈可卸载的计算和验证的用户数据报协议\(UDP\)上的校验和发送和接收的代码路径。|
|IPv4 的校验和计算 |计算和验证 IPv4 上的校验和发送和接收的代码路径，则可以将卸载的网络堆栈。 |
|IPv6 的校验和计算 |计算和验证 IPv6 上的校验和发送和接收的代码路径，则可以将卸载的网络堆栈。 | 
|分段的大型 TCP 数据包|TCP/IP 传输层支持大量发送卸载 v2 (LSOv2)。 使用 LSOv2，TCP/IP 传输层可以卸载到网络适配器的大型 TCP 数据包分段。|  
|接收方缩放\(RSS\)|RSS 是网络的一种网络驱动程序技术实现高效分布在多处理器系统中接收处理跨多个 Cpu。 在本主题后面部分提供有关 RSS 的更多详细信息。|  
|接收段合并\(RSC\)|RSC 是对组数据包的能力在一起以最大程度减少标头处理的是所需主机来执行。 最多为 64 KB 的已接收的有效负载可以合并成单个较大数据包进行处理。 在本主题后面部分提供有关 RSC 的更多详细信息。|  
  
###  <a name="bkmk_rss"></a> 接收方缩放

Windows Server 2016、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2008 R2 和 Windows Server 2008 支持接收方伸缩\(RSS\)。 

某些服务器配置为具有共享硬件资源的多个逻辑处理器\(如物理核心\)这都被视为多线程处理同步\(SMT\)对等方。 Intel 超线程技术是一个示例。 RSS 将定向到每个核心最多为 1 个逻辑处理器的网络处理。 例如，在服务器上使用 Intel 超线程、 4 个核心和 8 个逻辑处理器，RSS 使用不超过 4 个逻辑处理器进行网络处理。  

RSS 分布在逻辑处理器之间的传入网络 I/O 数据包，以使属于同一个 TCP 连接的数据包处理的同一逻辑处理器，保留顺序。 

RSS 还加载平衡 UDP 单播和多播的通信，并将路由的相关的流\(这由哈希的源和目标地址\)到相同的逻辑处理器，保留相关到达的请求的顺序。 这有助于提高可伸缩性和性能的服务器比他们符合条件的逻辑处理器的网络适配器的接收密集型方案。 

#### <a name="configuring-rss"></a>配置 RSS

在 Windows Server 2016 中，可以使用 Windows PowerShell cmdlet 和 RSS 配置文件来配置 RSS。 

您可以通过使用定义 RSS 配置文件 **– 配置文件**的参数**Set-netadapterrss** Windows PowerShell cmdlet。

**RSS 配置的 Windows PowerShell 命令**

以下 cmdlet，您可以查看并修改每个网络适配器的 RSS 参数。
  
>[!NOTE]
>有关详细的命令参考，对于每个 cmdlet，包括语法和参数，可以单击以下链接。 此外，可以将传递到 cmdlet 名称**Get-help**有关每个命令的详细信息的 Windows PowerShell 提示符下。  

- [禁用 NetAdapterRss](https://technet.microsoft.com/library/jj130892)。 此命令指定的网络适配器上禁用 RSS。

- [启用 NetAdapterRss](https://technet.microsoft.com/library/jj130859)。 此命令指定的网络适配器上启用 RSS。
  
- [Get-netadapterrss](https://technet.microsoft.com/library/jj130912)。 此命令检索您指定的网络适配器的 RSS 属性。
  
- [Set-netadapterrss](https://technet.microsoft.com/library/jj130863)。 此命令在你指定的网络适配器上设置的 RSS 属性。  

#### <a name="rss-profiles"></a>RSS 配置文件

可以使用 **– 配置文件**Set-netadapterrss cmdlet 来指定哪些逻辑处理器分配给哪个网络适配器参数。 为此参数的可用值有：

- **最接近**。 首选网络适配器的基本 RSS 处理器接近的逻辑处理器编号。 与此配置文件，操作系统可能会重新平衡动态根据负载的逻辑处理器。
  
- **ClosestStatic**。 首选网络适配器的基本 RSS 处理器附近的逻辑处理器编号。 与此配置文件，操作系统不重新平衡动态根据负载的逻辑处理器。
  
- **NUMA**。 逻辑处理器编号通常选择不同的 NUMA 节点上分配负载。 与此配置文件，操作系统可能会重新平衡动态根据负载的逻辑处理器。
  
- **NUMAStatic**。 这是**默认配置文件**。 逻辑处理器编号通常选择不同的 NUMA 节点上分配负载。 与此配置文件，操作系统会重新动态根据负载的逻辑处理器不平衡。

- **保守**。 RSS 使用尽可能少的处理器来维持负载。 此选项可帮助减少中断的数量。

根据方案和工作负荷特征，还可以使用的其他参数**Set-netadapterrss** Windows PowerShell cmdlet 来指定以下内容：

- 对于每个网络适配器，可以为 RSS 使用多少个逻辑处理器。
- 逻辑处理器的范围的起始偏移量。
- 从其网络适配器分配的内存节点。

以下是其他**Set-netadapterrss**可用于配置 RSS 的参数：

>[!NOTE]
>在网络适配器名称下面每个参数的示例语法**以太网**用作示例值 **– 名称**参数**Set-netadapterrss**命令。 当您运行该 cmdlet 时，确保你使用的网络适配器名称适用于你的环境。

- **\* MaxProcessors**:设置要使用的 RSS 处理器的最大数目。 这可确保，应用程序流量绑定到最大处理器数在给定的接口。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\* BaseProcessorGroup**:设置另一个 NUMA 节点的基的处理器组。 这会影响由 RSS 处理器数组。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**:设置另一个 NUMA 节点的最大处理器的组。 这会影响由 RSS 处理器数组。 将此项设置将限制最大处理器组，以便负载平衡对齐 k 组中。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**:设置另一个 NUMA 节点的基的处理器数。 这会影响由 RSS 处理器数组。 这允许在网络适配器分区处理器。 这是第一个逻辑中的处理器范围的 RSS 处理器分配给每个适配器。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**:每个网络适配器可以分配的内存的 NUMA 节点。 这可以是 k 组中或不同的 k 组中。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\* NumberofReceiveQueues**:如果逻辑处理器似乎以接收流量没有得到充分利用\(例如，以查看在任务管理器\)，可以尝试增大 RSS 队列从默认值为 2 到您的网络适配器支持的最大数目. 您的网络适配器可能选项以更改 RSS 队列数目作为驱动程序的一部分。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

有关详细信息，请单击下面的链接以下载[可伸缩网络：消除收到处理瓶颈 — 简介 RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc)中的 Word 格式。
  
#### <a name="understanding-rss-performance"></a>了解 RSS 性能

优化 RSS 需要了解的配置和负载均衡逻辑。 若要验证的 RSS 设置生效，在运行时，可以查看的输出**Get-netadapterrss** Windows PowerShell cmdlet。 下面是此 cmdlet 的输出示例。
  
```

PS C:\Users\Administrator> get-netadapterrss  
Name                           : testnic 2  
InterfaceDescription           : Broadcom BCM5708C NetXtreme II GigE (NDIS VBD Client) #66
Enabled                        : True
NumberOfReceiveQueues          : 2
Profile                        : NUMAStatic
BaseProcessor: [Group:Number]  : 0:0
MaxProcessor: [Group:Number]   : 0:15
MaxProcessors                  : 8
  
IndirectionTable: [Group:Number]:
     0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
…   
(# indirection table entries are a power of 2 and based on # of processors)  
…   
                          0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
```  

除了回显设置参数的输出的关键方面是间接表输出。 间接寻址表显示了用于将传入流量的哈希表存储桶。 在此示例中，n:c 表示法指定 Numa K-用于将传入流量定向的组： CPU 索引对。 我们看到 2 个独特的条目 (0:0，0:4)，分别表示的 k 组 0/卸下 cpu 0 和 k 组 0/cpu 4。

没有为此系统 （k 组 0） 和一个 n 只有一个 k 组 (其中 n < = 128) 间接表条目。 因为接收队列数设置为 2，只有 2 个处理器 (0:0，0:4) 选择-即使最多的处理器设置为 8。 实际上，间接表哈希仅使用超出可用 8 的 2 个 Cpu 的传入流量。

若要充分利用 Cpu，RSS 接收队列数必须等于或大于最大处理器。 在上一示例中，接收队列应设置为 8 或更高版本。

#### <a name="nic-teaming-and-rss"></a>NIC 组合和 RSS

可以使用另一个网络接口卡使用 NIC 组合成组网络适配器上启用 RSS。 在此方案中，只有基础物理网络适配器可以配置为使用 RSS。 用户不能在成组的网络适配器上设置 RSS cmdlet。
  
###  <a name="bkmk_rsc"></a> 接收段合并 (RSC)

接收段合并\(RSC\)处理接收的数据量给定的 IP 标头数，从而有助于提高性能。 它应该用于帮助接收数据的性能缩放通过分组\(合并或\)到更大的单位更小的数据包。

这种方法可能会延迟影响主要示吞吐量增量的优势。 RSC 建议增加收到大量的工作负荷的吞吐量。 请考虑部署支持 RSC 的网络适配器。 

在这些网络适配器，请确保是否已启用了 RSC\(这是默认设置\)，除非有特定的工作负荷\(等低延迟、 低网络吞吐量\)RSC 是断开受益的显示.

#### <a name="understanding-rsc-diagnostics"></a>了解 RSC 诊断

可以使用 Windows PowerShell cmdlet 来诊断 RSC **Get NetAdapterRsc**并**Get NetAdapterStatistics**。

运行 Get NetAdapterRsc cmdlet 时，以下是示例输出。

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

**获取**cmdlet 显示在界面中是否启用了 RSC 以及 TCP 是否启用 RSC 处于操作状态。 失败原因提供了有关失败后，若要启用该接口上的 RSC 的详细信息。

在前面的方案中，IPv4 RSC 在界面中是受支持和操作。 若要了解诊断故障，可以看到合并的字节或导致异常。 这提供了合并问题的指示。

运行 Get NetAdapterStatistics cmdlet 时，以下是示例输出。

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC 和虚拟化

RSC 仅支持在物理主机，主机网络适配器未绑定到 HYPER-V 虚拟交换机时。 当主机绑定到 HYPER-V 虚拟交换机时，RSC 被禁用的操作系统。 此外，虚拟机无法获得 RSC 的优势，因为虚拟网络适配器不支持 RSC。

可以为虚拟机启用 RSC 时单根输入/输出虚拟化\(SR-IOV\)已启用。 在这种情况下，虚函数支持 RSC 功能;因此，虚拟机也享有 RSC 的权益。

##  <a name="bkmk_resources"></a> 网络适配器资源

几个网络适配器中有效地管理其资源，以获得最佳性能。 多个网络适配器，可通过使用手动配置资源**高级网络**适配器的选项卡。 对于此类适配器，可以设置多个参数，包括接收缓冲区的数目的值，并发送缓冲区。

通过使用以下 Windows PowerShell cmdlet 的简化配置网络适配器资源。

- [Get-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [Set-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [Enable-NetAdapter](https://technet.microsoft.com/library/jj130876.aspx)

- [Enable-NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [Enable-NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [Enable-NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [Enable-NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [Enable-NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [Enable-NetAdapterQos](https://technet.microsoft.com/library/jj130866.aspx)

- [Enable-NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [Enable-NetAdapterSriov](https://technet.microsoft.com/library/jj130899.aspx)

有关详细信息，请参阅[Windows PowerShell 中的网络适配器 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

本指南中的所有主题的链接，请参阅[网络子系统性能调整](net-sub-performance-top.md)。