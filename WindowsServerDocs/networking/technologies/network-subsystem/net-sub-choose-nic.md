---
title: 选择网络适配器
description: 本主题是 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19318bfa9807d209bd9a195b668c1787bd3aff25
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871916"
---
# <a name="choosing-a-network-adapter"></a>选择网络适配器

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来了解可能会影响您的购买选项的网络适配器的某些功能。

需要高性能的网络适配器的网络密集型应用程序。 本部分探讨选择网络适配器的一些注意事项，以及如何配置不同的网络适配器设置以实现最佳网络性能。

> [!TIP]
>  你可以使用 Windows PowerShell 配置网络适配器设置。 有关详细信息，请参阅[Windows PowerShell 中的网络适配器 cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

##  <a name="bkmk_offload"></a>卸载功能

从中央处理器\(cpu\)将任务卸载到网络适配器可降低服务器上的 cpu 使用率，从而提高系统的整体性能。

如果你选择具有适当卸载功能的网络适配器，Microsoft 产品中的网络堆栈可以将一个或多个任务卸载到网络适配器。 下表提供了 Windows Server 2016 中提供的不同卸载功能的简要概述。
  
|卸载类型|描述|
|------------------|-----------------|  
|TCP 的校验和计算|网络堆栈可以在发送和接收代码路径上卸载传输控制\(协议\) TCP 校验和的计算和验证。 它还可以在发送和接收代码路径上卸载 IPv4 和 IPv6 校验和的计算和验证。|  
|UDP 的校验和计算 |网络堆栈可以在发送和接收代码路径上卸载用户数据\(报\)协议 UDP 校验和的计算和验证。|
|IPv4 的校验和计算 |网络堆栈可以在发送和接收代码路径上卸载 IPv4 校验和的计算和验证。 |
|IPv6 的校验和计算 |网络堆栈可以在发送和接收代码路径上卸载 IPv6 校验和的计算和验证。 | 
|大 TCP 数据包的细分|TCP/IP 传输层支持大型发送卸载 v2 （LSOv2）。 通过 LSOv2，TCP/IP 传输层可以将大型 TCP 数据包的分段卸载到网络适配器。|  
|接收方缩放\(RSS\)|RSS 是一种网络驱动程序技术，可在多处理器系统中跨多个 Cpu 高效地分发网络接收处理。 有关 RSS 的详细信息将在本主题的后面部分提供。|  
|接收段合并\(RSC\)|RSC 可以将数据包组合在一起，以最大程度地减少主机执行所需的标头处理。 最多可将接收到的有效负载的最大大小 64 KB 合并为一个较大的数据包进行处理。 本主题后面提供了有关 RSC 的更多详细信息。|  
  
###  <a name="bkmk_rss"></a>接收方缩放

Windows server 2016、windows server 2012、windows server 2012 R2、windows server 2008 R2 和 windows server 2008 支持接收方缩放\(RSS。\) 

某些服务器配置了多个逻辑处理器，它们共享了\(物理内核\)等硬件资源，并将这些资源视为同时进行多\(线程\) SMT 对等互连。 Intel 超线程技术是一个示例。 RSS 将网络处理定向到每个核心最多一个逻辑处理器。 例如，在具有 Intel 超线程、4核和8个逻辑处理器的服务器上，RSS 将不超过4个逻辑处理器用于网络处理。  

RSS 在逻辑处理器之间分发传入的网络 i/o 数据包，以便在相同的逻辑处理器上处理属于同一 TCP 连接的数据包，从而保留排序。 

RSS 还会对 UDP 单播和多播流量进行负载均衡，并通过\(将源地址和目标地址\)哈希到同一个逻辑处理器来路由相关的流，从而保留相关到达的顺序。 这有助于提高具有较少网络适配器（而不是符合合格逻辑处理器）的服务器的接收密集型方案的可伸缩性和性能。 

#### <a name="configuring-rss"></a>配置 RSS

在 Windows Server 2016 中，可以通过使用 Windows PowerShell cmdlet 和 RSS 配置文件来配置 RSS。 

可以使用**Get-netadapterrss** Windows PowerShell cmdlet 的 **– PROFILE**参数定义 RSS 配置文件。

**用于 RSS 配置的 Windows PowerShell 命令**

以下 cmdlet 可用于查看和修改每个网络适配器的 RSS 参数。
  
>[!NOTE]
>若要详细了解每个 cmdlet （包括语法和参数），可以单击以下链接。 此外，还可以在 Windows PowerShell 提示符下将 cmdlet 名称传递到**get-help** ，以获取有关每个命令的详细信息。  

- [Get-netadapterrss](https://technet.microsoft.com/library/jj130892)。 此命令将在指定的网络适配器上禁用 RSS。

- [Get-netadapterrss](https://technet.microsoft.com/library/jj130859)。 此命令在指定的网络适配器上启用 RSS。
  
- [Get-netadapterrss](https://technet.microsoft.com/library/jj130912)。 此命令检索指定的网络适配器的 RSS 属性。
  
- [Get-netadapterrss](https://technet.microsoft.com/library/jj130863)。 此命令在指定的网络适配器上设置 RSS 属性。  

#### <a name="rss-profiles"></a>RSS 配置文件

你可以使用 Get-netadapterrss cmdlet 的 **– Profile**参数来指定向哪个网络适配器分配哪些逻辑处理器。 此参数的可用值为：

- **最接近**。 首选网络适配器基本 RSS 处理器附近的逻辑处理器编号。 在此配置文件中，操作系统可能会根据负载动态重新平衡逻辑处理器。
  
- **ClosestStatic**。 首选网络适配器基本 RSS 处理器附近的逻辑处理器数。 在此配置文件中，操作系统不会根据负载动态重新平衡逻辑处理器。
  
- **NUMA**。 通常在不同的 NUMA 节点上选择逻辑处理器编号，以分散负载。 在此配置文件中，操作系统可能会根据负载动态重新平衡逻辑处理器。
  
- **NUMAStatic**。 这是**默认配置文件**。 通常在不同的 NUMA 节点上选择逻辑处理器编号，以分散负载。 对于此配置文件，操作系统将不会根据负载动态重新平衡逻辑处理器。

- **保守**。 RSS 使用尽可能少的处理器来维持负载。 此选项可帮助减少中断的数量。

根据方案和工作负荷特征，还可以使用 Get-netadapterrss Windows PowerShell cmdlet 的其他参数来指定以下各**项**：

- 在每个网络适配器基础上，可以将多少个逻辑处理器用于 RSS。
- 逻辑处理器范围的起始偏移量。
- 网络适配器从其分配内存的节点。

下面是可用于配置 RSS 的其他**get-netadapterrss**参数：

>[!NOTE]
>在下面每个参数的示例语法中，网络适配器名称 "**以太网**" 将用作**get-netadapterrss**命令的 **– name**参数的示例值。 当你运行 cmdlet 时，请确保你使用的网络适配器名称适用于你的环境。

- **MaxProcessors\*** ：设置要使用的 RSS 处理器的最大数量。 这可确保将应用程序流量绑定到给定接口上的最大处理器数。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **BaseProcessorGroup\*** ：设置 NUMA 节点的基本处理器组。 这会影响 RSS 使用的处理器数组。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **MaxProcessorGroup\*** ：设置 NUMA 节点的最大处理器组。 这会影响 RSS 使用的处理器数组。 设置此选项会限制最大处理器组，以便在 k 组内对齐负载平衡。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **BaseProcessorNumber\*** ：设置 NUMA 节点的基础处理器编号。 这会影响 RSS 使用的处理器数组。 这允许跨网络适配器分区处理器。 这是分配给每个适配器的 RSS 处理器范围内的第一个逻辑处理器。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **NumaNode\*** ：每个网络适配器可从其分配内存的 NUMA 节点。 这可以在 k 组内，也可以位于不同的 k 组中。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **NumberofReceiveQueues\*** ：例如，如果在 "任务管理器\(\)" 中查看了逻辑处理器的接收流量，则可以尝试将 RSS 队列的数量从默认值2增加到网络适配器所支持的最大值. 网络适配器可能有选项可用于更改作为驱动程序的一部分的 RSS 队列的数量。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

有关详细信息，请单击以下链接下载[可缩放的网络：消除接收处理瓶颈-以 Word 格式](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc)引入 RSS。
  
#### <a name="understanding-rss-performance"></a>了解 RSS 性能

优化 RSS 需要了解配置和负载平衡逻辑。 若要验证 RSS 设置是否生效，可以在运行**Get-netadapterrss** Windows PowerShell cmdlet 时查看输出。 下面是此 cmdlet 的示例输出。
  
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

除了已设置的回显参数外，输出的主要方面是间接表输出。 间接寻址表显示用于分发传入流量的哈希表存储桶。 在此示例中，n:c 表示法指定 Numa K 组： CPU 索引对，用于指示传入流量。 我们看到的是两个唯一条目（0:0 和0:4），分别表示 k 组 0/cpu0 和 k 组 0/cpu 4。

此系统只有一个 k 组（k 组0）和 n （其中 n < = 128）间接表条目。 由于接收队列数设置为2，因此仅选择2个处理器（0:0，0:4），即使将最大处理器设置为8。 实际上，间接寻址表将对传入流量进行哈希处理，以便仅使用8个可用的2个 Cpu。

若要充分利用 Cpu，RSS 接收队列的数量必须等于或大于最大处理器数。 在前面的示例中，接收队列应该设置为8或更大。

#### <a name="nic-teaming-and-rss"></a>NIC 组合和 RSS

可以在与使用 NIC 组合的另一个网络接口卡成组的网络适配器上启用 RSS。 在此方案中，只能将基础物理网络适配器配置为使用 RSS。 用户无法在成组的网络适配器上设置 RSS cmdlet。
  
###  <a name="bkmk_rsc"></a>接收段合并（RSC）

接收段合并\(RSC\)通过减少针对给定数量接收的数据处理的 IP 标头来帮助提高性能。 它应该用于通过将较小的数据包分组\(或合并\)为较大的单位，来帮助扩展接收的数据的性能。

此方法可能会影响延迟，其中的优点主要体现在吞吐量提升。 建议使用 RSC 来增加收到的大量工作负荷的吞吐量。 请考虑部署支持 RSC 的网络适配器。 

在这些网络适配器上， \(确保 rsc 为默认设置\)，除非你有特定的工作负荷\(（例如，低延迟、低吞吐量网络\) ，显示 RSC 的权益已关闭）.

#### <a name="understanding-rsc-diagnostics"></a>了解 RSC 诊断

可以使用 Windows PowerShell cmdlet **NetAdapterRsc**和**NetAdapterStatistics**诊断 RSC。

下面是运行 NetAdapterRsc cmdlet 时的示例输出。

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

**Get** cmdlet 显示是否在接口中启用了 RSC 以及 TCP 是否允许 rsc 处于操作状态。 失败原因提供有关在该接口上启用 RSC 失败的详细信息。

在前面的方案中，IPv4 RSC 在接口中受支持并且可操作。 若要了解诊断失败，可以看到合并的字节或异常。 这将提供合并问题的指示。

下面是运行 NetAdapterStatistics cmdlet 时的示例输出。

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC 和虚拟化

仅当主机网络适配器未绑定到 Hyper-v 虚拟交换机时，才能在物理主机中支持 RSC。 当主机绑定到 Hyper-v 虚拟交换机时，操作系统将禁用 RSC。 此外，虚拟机不会获得 RSC 的优势，因为虚拟网络适配器不支持 RSC。

当启用单一根输入/输出虚拟化\(sr-iov\)时，可以为虚拟机启用 RSC。 在这种情况下，虚拟函数支持 RSC 功能;因此，虚拟机也会获得 RSC 的好处。

##  <a name="bkmk_resources"></a>网络适配器资源

几个网络适配器主动管理其资源，以获得最佳性能。 多个网络适配器允许使用适配器的 "**高级网络**" 选项卡手动配置资源。 对于此类适配器，可以设置多个参数的值，包括接收缓冲区数和发送缓冲区数。

使用以下 Windows PowerShell cmdlet 可简化配置网络适配器资源的配置。

- [NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [Get-netadapter](https://technet.microsoft.com/library/jj130876.aspx)

- [NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [Get-netadapterqos](https://technet.microsoft.com/library/jj130866.aspx)

- [NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [Get-netadaptersriov](https://technet.microsoft.com/library/jj130899.aspx)

有关详细信息，请参阅[Windows PowerShell 中的网络适配器 cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

有关本指南中的所有主题的链接，请参阅[网络子系统性能优化](net-sub-performance-top.md)。