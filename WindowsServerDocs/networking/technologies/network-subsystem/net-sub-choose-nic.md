---
title: 选择网络适配器
description: 本主题介绍 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b3b9d206273dfd0e9115ebc27cf28aa960bfb0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-network-adapter"></a>选择网络适配器

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解的某些功能可能会影响你的购买选择的网络适配器。

网络密集型应用程序需要高性能的网络适配器。 此部分中介绍了一些注意事项选择网络适配器，以及如何配置不同的网络适配器设置，以实现最佳的网络性能。

> [!TIP]
>  你可以通过使用 Windows PowerShell 配置网络适配器设置。 有关详细信息，请参阅[Windows PowerShell 中的网络适配器 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

##  <a name="bkmk_offload"></a>卸载功能

卸载从中央处理的任务到该网络适配器 \(CPU\) 可以减少在服务器上，可改进整体系统性能的 CPU 使用率。

Microsoft 产品中的网络堆栈可以卸载一个或多个任务到的网络适配器，如果你选择具有相应的网络适配器卸载功能。 下表提供适用于 Windows Server 2016 的不同卸载功能的简短概述。
  
|卸载类型|描述|
|------------------|-----------------|  
|校验和 TCP 计算|网络堆栈可以卸载计算和验证传输控件协议 \(TCP\) 校验和在发送和接收的代码路径。 它还可以卸载计算和 IPv4 的验证和 IPv6 校验和在发送和接收的代码路径。|  
|校验和计算 udp |网络堆栈可以卸载计算和用户数据报协议 \(UDP\) 校验和上验证发送和接收的代码路径。|
|Ipv4 校验和计算 |网络堆栈可以卸载计算和 IPv4 校验和在发送和接收的代码路径的验证。 |
|对于 IPv6 校验和计算 |网络堆栈可以卸载计算和 IPv6 校验和在发送和接收的代码路径的验证。 | 
|大 TCP 数据包 segmentation|TCP/IP 传输层支持大发送卸载 v2 (LSOv2)。 与 LSOv2，TCP/IP 传输层可以卸载到该网络适配器的大型 TCP 数据包分段。|  
|接收缩放 \(RSS\) 一侧|RSS 是使网络高效 distribution 的网络驱动程序技术接收处理多个 Cpu 跨多处理器的系统中。 在本主题后面提供有关 RSS 的更多详细信息。|  
|接收线段合并 \(RSC\)|RSC 是聚拢以最小化处理程序的标题组数据包的功能都需要执行的主机。 可以将最多 64 KB 接收负载的合并到单个变得更大数据包进行处理。 在本主题后面提供有关 RSC 的更多详细信息。|  
  
###  <a name="bkmk_rss"></a>收到一侧缩放

Windows Server 2016、Windows Server 2012、Windows Server 2012 R2、Windows Server 2008 R2 和 Windows Server 2008 支持收到一侧缩放 \(RSS\)。 

某些服务器具有多个共享的硬件资源的逻辑处理器配置 \（如物理 core\)，这视为同时多线程 \(SMT\) 同级。 Intel 超线程技术是一个示例。 RSS 将定向到某个的逻辑处理器核心每到的网络处理。 例如，Intel Hyper 线程、4 个核心，和 8 逻辑处理器服务器，在 RSS 用于最多 4 逻辑处理器网络处理。  

RSS 分发之间逻辑处理器传入网络 I/O 数据包，以便数据包属于相同的 TCP 连接的同一保留排序的逻辑处理器上处理。 

RSS 还加载余额 UDP 单址广播和多址广播的通信，以及它路由相关的版本流 \（这由哈希源代码和目的地的 addresses\）相同的逻辑处理器保留相关到达顺序。 这有助于提高可扩展性和接收密集型场景服务器具有较少的网络适配器不符合条件的逻辑处理器的性能。 

#### <a name="configuring-rss"></a>配置 RSS

Windows Server 2016，可以使用 Windows PowerShell cmdlet 和 RSS 配置文件来配置 RSS。 

你可以通过使用定义 RSS 配置文件**– 个人资料**参数**组 NetAdapterRss** Windows PowerShell cmdlet。

**对于 RSS 配置的 Windows PowerShell 命令**

以下 cmdlet 允许你查看和修改 RSS 参数每个网络适配器。
  
>[!NOTE]
>有关每个 cmdlet、语法和参数，包括详细的命令参考，你可以单击以下链接。 此外，可以将传递到 cmdlet 名称**获取帮助**在每个命令的详细信息的 Windows PowerShell 提示。  

- [禁用 NetAdapterRss](https://technet.microsoft.com/library/jj130892)。 此命令禁用指定你的网络适配器上的 RSS。

- [启用 NetAdapterRss](https://technet.microsoft.com/library/jj130859)。 此命令，在你所指定的网络适配器 RSS。
  
- [获取 NetAdapterRss](https://technet.microsoft.com/library/jj130912)。 此命令检索你指定的网络适配器 RSS 属性。
  
- [设置 NetAdapterRss](https://technet.microsoft.com/library/jj130863)。 在你所指定的网络适配器，此命令设置 RSS 属性。  

#### <a name="rss-profiles"></a>RSS 配置文件

你可以使用**– 个人资料**参数 Set-NetAdapterRss cmdlet 指定的逻辑处理器分配到的网络适配器。 为此参数可用的值为：

- **最近**。 附近的网络适配器基 RSS 处理器的逻辑处理器编号是首选项。 对于该配置文件，可能重新操作系统平衡逻辑动态基于加载的处理器。
  
- **ClosestStatic**。 附近的网络适配器基 RSS 处理器的逻辑处理器编号是首选项。 对于该配置文件，操作系统不重新平衡逻辑动态基于加载的处理器。
  
- **纽**。 逻辑处理器编号通常上选择不同的纽节点以分散负载。 对于该配置文件，可能重新操作系统平衡逻辑动态基于加载的处理器。
  
- **NUMAStatic**。 这是**默认个人资料**。 逻辑处理器编号通常上选择不同的纽节点以分散负载。 对于该配置文件，操作系统将重新逻辑处理器根据加载动态不平衡。

- **保守**。 RSS 使用少处理器尽可能保持加载。 该选项有助于减少中断次数。

根据我们项 scenario 和的工作负载特征，你还可以使用的其他参数**组 NetAdapterRss** Windows PowerShell cmdlet 指定以下：

- 针对每个网络适配器，可以为 RSS 使用多少逻辑处理器。
- 范围内的逻辑处理器起始偏移。
- 从该网络适配器分配内存节点。

以下是其他**组 NetAdapterRss**参数，可用于配置 RSS:

>[!NOTE]
>在每个参数网络适配器名称下面的示例语法**以太网**用作示例值**– 名称**参数**组 NetAdapterRss**命令。 Cmdlet 运行时，请确保你使用的网络适配器名称是适合您的环境。

- **\ * MaxProcessors**：设置 RSS 处理器用于最大数目。 这将确保，应用程序交通上绑定到最多的处理器给定的界面。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\ * BaseProcessorGroup**：设置基本处理器组的纽节点。 这会影响由 RSS 处理器深刻。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\ * MaxProcessorGroup**：使最大处理器组中的纽节点。 这会影响由 RSS 处理器深刻。 此设置会限制最大的处理器组，以便负载平衡对齐 k 组中。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\ * BaseProcessorNumber**：设置基本处理器数纽节点。 这会影响由 RSS 处理器深刻。 这允许所有网络适配器分区处理器。 这是第一个逻辑处理器的范围内的 RSS 处理器分配到每个适配器。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\ * NumaNode**：每个网络适配器可以分配内存的纽节点。 这可以 k 组中，或从不同 k 组。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\ * NumberofReceiveQueues**：如果似乎无法接收交通充分利用你的逻辑处理器 \（例如，以查看的任务 Manager\），你可以尝试增加 RSS 队列从 2 默认支持的网络适配器的最大到数。 你的网络适配器可能具有选项更改为驱动程序的一部分的 RSS 队列数。 示例语法：

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

详细信息，请单击以下链接下载[可缩放网络：消除接收处理瓶颈 — 引入 RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) Word 格式。
  
#### <a name="understanding-rss-performance"></a>了解 RSS 性能

调优 RSS 需要了解配置和负载平衡逻辑。 若要验证 RSS 设置生效运行时，你可以查看输出**获取 NetAdapterRss** Windows PowerShell cmdlet。 下面是此 cmdlet 示例输出。
  
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

除了回显已设置的参数，主要方面的输出是间接表输出。 间接表显示了用于分配接收的通信希表时段。 在此示例中，将指定 n:c 表示法，纽 K-用于引导接收的通信的组：CPU 索引配对。 我们看到完全 2 个唯一条目 (0:0 和 0:4)，这表示卸下 cpu k 组 0 0/和 k 组 0/cpu 4，分别。

没有只有一个 k 组此系统（k 组 0）和一个 n (其中 n < = 128) 间接表条目。 因为接收队列大量设置为仅 2 个月 2 日，处理器 (0:0，0:4) 选择-即使最大的处理器设置为 8 也是如此。 实际上，间接表哈希传入仅使用退出可用 8 2 Cpu 的交通。

为了充分利用 Cpu，必须等于或大于大化处理器的 RSS 收到队列数。 在之前的示例中，收到队列应设置为 8 或更高版本。

#### <a name="nic-teaming-and-rss"></a>NIC 组合和 RSS

可以与 NIC 组合使用其他网络接口卡搭配使用的网络适配器上启用 RSS。 在此情况下，为配置仅基础物理网络适配器使用 RSS。 用户无法在成组的网络适配器设置 RSS cmdlet。
  
###  <a name="bkmk_rsc"></a>接收线段合并 (RSC)

减少 IP 标题给定接收的数据量用于处理大量收到有助于合并段 \(RSC\) 的性能。 应用于帮助缩放接收的数据的性能分组 \(or coalescing\) 较小的数据包插入变得更大的设备。

与主要吞吐量提升中看到的权益，此方法可能会影响延迟。 建议 RSC 增加吞吐量收到的大量工作负载。 请考虑部署支持 RSC 的网络适配器。 

在这些网络适配器，请确保 RSC 处于打开状态 \（这是默认 setting\），除非你具有特定的工作负载 \（例如低延迟) 低吞吐量 networking\ 从 RSC 正在关闭该显示好处。

#### <a name="understanding-rsc-diagnostics"></a>了解 RSC 诊断

你可以通过使用 Windows PowerShell cmdlet 诊断 RSC**获取 NetAdapterRsc**和**获取 NetAdapterStatistics**。

以下是运行 Get-NetAdapterRsc cmdlet 时的示例输出。

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

**获取**cmdlet 显示在界面是否启用了 RSC 和 TCP 是否可 RSC 可操作的状态。 失败的原因提供有关失败启用 RSC 界面上的详细信息。

在以前的情况下，IPv4 RSC 在界面是受支持和运营。 若要了解诊断失败，人可以看到合并的字节或引起异常。 这将提供合并问题的指示。

以下是运行 Get-NetAdapterStatistics cmdlet 时的示例输出。

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC 和虚拟化

RSC 仅支持的物理主机时的主网络适配器未绑定到 Hyper-V 虚拟交换机用来。 RSC 由操作系统处于禁用状态，当主机绑定到 Hyper-V 虚拟交换机用来。 此外，虚拟机无法获取 RSC 的好处，因为虚拟网络适配器不支持 RSC。

启用后单根输入/输出虚拟化 \(SR-IOV\) 可以虚拟机启用 RSC。 在此情况下，虚拟功能支持 RSC 功能;因此，虚拟机还收到 RSC 的好处。

##  <a name="bkmk_resources"></a>网络适配器的资源

几个网络适配器积极管理他们的资源，以实现最佳性能。 多个网络适配器允许你使用手动配置资源**高级网络**适配器的选项卡。 对此类适配器，您可以设置的大量参数，包括接收缓冲区数值，并发送缓冲区。

使用下面的 Windows PowerShell cmdlet 简化配置网络适配器的资源。

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

本指南中的所有主题的链接，请参阅[网络子系统性能优化](net-sub-performance-top.md)。