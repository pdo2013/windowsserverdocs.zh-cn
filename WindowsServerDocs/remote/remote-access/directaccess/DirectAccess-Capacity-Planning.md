---
title: DirectAccess 容量规划
description: 你可以使用本主题来了解 Windows Server 2012 DirectAccess 服务器性能，以帮助你在 Windows Server 2016 中进行 DirectAccess 的容量规划。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 456e5971-3aa7-4a24-bc5d-0c21fec7687e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9969cade328b19f16dbdbad432b96dabb5518007
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394844"
---
# <a name="directaccess-capacity-planning"></a>DirectAccess 容量规划

>适用于：Windows Server（半年频道）、Windows Server 2016

本文档是一份有关 Windows Server 2012 DirectAccess 服务器性能的报告。 测试的目的是为了确定使用高端和低端计算机硬件时的吞吐量。 高端和低端 CPU 性能取决于网络流量吞吐量和所用客户端的类型。 典型的 DirectAccess 部署（同时作为这些测试的基础）包括占数量 1/3 (30%) 的 IPHTTPS 客户端，以及占数量 2/3 (70%) 的 Teredo 客户端。 在某种程度上，Teredo 客户端的性能优于 IPHTTPS 客户端，原因是 Windows Server 2012 使用了接收方缩放 (RSS)，从而可以利用所有的 CPU 核心。 在这些测试中，由于启用了 RSS，因此可禁用超线程。 此外，Windows Server 2012 中的 TCP/IP 支持 UDP 流量，使 Teredo 客户端能够在 CPU 之间平衡负载。  
  
数据是从低端（4 核，4 G）服务器中，以及预期在高端（8 核，8 G）服务器上较常用的硬件中收集的。  下面是在低端硬件上新的 Windows 8 任务管理器的屏幕截图，其中包含750个客户端（562 Teredo、188 IPHTTPS），运行 ~ 77 Mb/秒。这是为了模拟不提供智能卡凭据的用户。  
  
这些测试结果表明，在 Windows 8 中，Teredo 的性能优于 IPHTTPS，但与 Windows 7 相比，Teredo 和 IPHTTPS 的带宽使用量都已提高。  
  
![测试结果](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning1.gif)  
  
## <a name="high-end-hardware-test-environment"></a>高端硬件测试环境  
下图显示了高端硬件性能测试环境的结果。 本文档详细说明了所有测试结果和分析。  
  
||||  
|-|-|-|  
|配置-硬件|低端硬件（4GB RAM，4 核）|高端硬件（8 GB，8 核）|  
|双隧道<br /><br />-PKI<br /><br />-包括 DNS64/NAT64|750 个并发连接占用 50% 的 CPU 和 50% 的内存，Corpnet NIC 吞吐量为 75 Mbps。 扩展目标为 1000 个用户，占用 50% 的 CPU。|1500 个并发连接占用 50% 的 CPU 和 50% 的内存，Corpnet NIC 吞吐量为 150 Mbps。|  
## <a name="test-environment"></a>测试环境

**性能工作台拓扑**  
  
![测试环境](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning2.gif)  
  
性能测试环境为 5 台计算机构成的试验台。 对于低端硬件测试，使用了一台 4 核 4G DirectAccess 服务器；对于高端硬件测试，使用了一台 8 核 16G DirectAccess 服务器。 对于低端和高端测试环境，使用了以下硬件：一台后端服务器（发送器）和两台客户端计算机（接收器）。  接收器在两台客户端计算机之间分割。 否则，接收器将受 CPU 的约束，并限制客户端数量和带宽。 接收端有一个模拟器用于模拟数百个客户端（模拟 HTTPS 或 Teredo 客户端）。 同时配置了 IPsec 和 DOSp。 已在 DirectAccess 服务器上启用 RSS。 RSS 队列大小设置为 8。  如果不配置 RSS，将有一个处理器维持在高利用率水平，而其他核心处于利用不足的状态。 另请注意，DirectAccess 服务器是关闭了超线程的 4 核计算机。  之所以关闭超线程，是因为 RSS 只能在物理核心上工作，并且使用超线程会生成有偏差的结果。 （这意味着，并非所有核心都承受了均衡的负载）。  
  
## <a name="testing-results-for-low-end-hardware"></a>低端硬件的测试结果：

测试是使用 1000 个和 750 个客户端执行的。  在所有方案中，都将 70% 的流量分割到了 Teredo，将 30% 的流量分割到了 IPHTTPS。  所有测试都为每个客户端使用 2 个 IPsec 隧道，通过 Nat64 传递 TCP 流量。  在所有测试中，内存利用率很低，CPU 利用率可接受。  
  
**单个测试结果：**  
  
以下部分描述了各项测试。 每个部分的标题突出显示了每项测试的关键要素，其后是结果的摘要描述，再后是显示详细结果数据的图表。  
  
@no__t 0Low-结束 Perf：750客户端，70/30 split，84.17 Mb/秒的吞吐量： **  
  
下列三项测试与低端硬件相关。  在以下测试运行中，有 750 个吞吐量为 84.17 Mbps 的客户端，流量分别分割到了 562 个 Teredo 和 188 个 IPHTTPS。 Teredo MTU 设置为 1472，已启用 Teredo 分流。 完成三项测试后，平均 CPU 利用率为 46.42%，平均内存利用率（以 4GB 总可用内存的提交字节百分比表示）为 25.95%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**方案**|**CPUAvg （来自计数器）**|**Mbit/秒（Corp 端）**|**Mbit/s （internet 端）**|**活动 QMSA**|**活动 MMSA**|**内存使用率（4 g 系统）**|  
|0Low-结束 HW。 @no__t562 个 Teredo 客户端。188 IPHTTPS 客户端。 **|47.7472542|84.3|119.13|1502.05|1502.1|26.27%|  
|0Low-结束 HW。 @no__t562 个 Teredo 客户端。188 IPHTTPS 客户端。 **|46.3889778|84.146|118.73|1501.25|1501.2|25.90%|  
|0Low-结束 HW。 @no__t562 个 Teredo 客户端。188 IPHTTPS 客户端。 **|45.113082|84.0494|118.43|1546.14|1546.1|25.68%|  
  
**1000客户端，70/30 split，78 Mb/秒的吞吐量：**  
  
下列三项测试表明了低端硬件的性能。 在以下测试运行中，有 1000 个平均吞吐量大约为 78.64 Mbps 的客户端，流量分别分割到了 700 个 Teredo 和 300 个 IPHTTPS。  Teredo MTU 设置为 1472，已启用 Teredo 分流。  平均 CPU 利用率为大约 50.7%，平均内存利用率（以 4GB 总可用内存的提交字节百分比表示）为大约 28.7%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**方案**|**CPUAvg （来自计数器）**|**Mbit/秒（Corp 端）**|**Mbit/s （internet 端）**|**活动 QMSA**|**活动 MMSA**|**内存使用率（4 g 系统）**|  
|0Low-结束 HW。 @no__t700 个 Teredo 客户端。300 IPHTTPS 客户端。 **|51.28406247|78.6432|113.19|2002.42|1502.1|25.59%|  
|0Low-结束 HW。 @no__t700 个 Teredo 客户端。300 IPHTTPS 客户端。 **|51.06993128|78.6402|113.22|2001.4|1501.2|30.87%|  
|0Low-结束 HW。 @no__t700 个 Teredo 客户端。300 IPHTTPS 客户端。 **|49.75235617|78.6387|113.2|2002.6|1546.1|30.66%|  
  
**1000客户端，70/30 split，109 Mb/秒的吞吐量：**  
  
在以下三项测试运行中，有 1000 个平均吞吐量大约为 109.2 Mbps 的客户端，流量分别分割到了 700 个 Teredo 和 300 个 IPHTTPS。 Teredo MTU 设置为 1472，已启用 Teredo 分流。 完成三项测试后，平均 CPU 利用率为大约 59.06%，平均内存利用率（以 4GB 总可用内存的提交字节百分比表示）为大约 27.34%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**方案**|**CPUAvg （来自计数器）**|**Mbit/秒（Corp 端）**|**Mbit/s （internet 端）**|**活动 QMSA**|**活动 MMSA**|**内存使用率（4 g 系统）**|  
|0Low-结束 HW。 @no__t700 个 Teredo 客户端。300 IPHTTPS 客户端。 **|59.81640675|108.305|153.14|2001.64|2001.6|24.38%|  
|0Low-结束 HW。 @no__t700 个 Teredo 客户端。300 IPHTTPS 客户端。 **|59.46473798|110.969|157.53|2005.22|2005.2|28.72%|  
|0Low-结束 HW。 @no__t700 个 Teredo 客户端。300 IPHTTPS 客户端。 **|57.89089768|108.305|153.14|1999.53|2018.3|24.38%|  
  
## <a name="testing-results-for-high-end-hardware"></a>高端硬件的测试结果：  
测试是使用 1500 个客户端执行的。 将 70% 的流量分割到了 Teredo，将 30% 的流量分割到了 IPHTTPS。 所有测试都为每个客户端使用 2 个 IPsec 隧道，通过 Nat64 传递 TCP 流量。 在所有测试中，内存利用率很低，CPU 利用率可接受。  
  
**单个测试结果：**  
  
以下部分描述了各项测试。 每个部分的标题突出显示了每项测试的关键要素，其后是结果的摘要描述，再后是包含详细结果数据的图表。  
  
**1500客户端，70/30 split，153.2 Mb/秒的吞吐量**  
  
下列五项测试与高端硬件相关。 在以下测试运行中，有 1500 个平均吞吐量为 153.2 Mbps 的客户端，流量分别分割到了 1050 个 Teredo 和 450 个 IPHTTPS。 完成五项测试后，平均 CPU 利用率为 50.68%，平均内存利用率（以 8GB 总可用内存的提交字节百分比表示）为 22.25%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**方案**|**CPUAvg （来自计数器）**|**Mbit/秒（Corp 端）**|**Mbit/s （internet 端）**|**活动 QMSA**|**活动 MMSA**|**内存使用率（4 g 系统）**|  
|0High-结束 HW。 @no__t1050 个 Teredo 客户端。450 IPHTTPS 客户端。 **|51.712437|157.029|216.29|3000.31|3046|21.58%|  
|0High-结束 HW。 @no__t1050 个 Teredo 客户端。450 IPHTTPS 客户端。 **|48.86020205|151.012|206.53|3002.86|3045.3|21.15%|  
|0High-结束 HW。 @no__t1050 个 Teredo 客户端。450 IPHTTPS 客户端。 **|52.23979519|155.511|213.45|3001.15|3002.9|22.90%|  
|0High-结束 HW。 @no__t1050 个 Teredo 客户端。450 IPHTTPS 客户端。 **|51.26269767|155.09|212.92|3000.74|3002.4|22.91%|  
|0High-结束 HW。 @no__t1050 个 Teredo 客户端。450 IPHTTPS 客户端。 **|50.15751307|154.772|211.92|3000.9|3002.1|22.93%|  
|0High-结束 HW。 @no__t1050 个 Teredo 客户端。450 IPHTTPS 客户端。 **|49.83665607|145.994|201.92|3000.51|3006|22.03%|  
  
![高端硬件测试结果](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning3.gif)  
  


