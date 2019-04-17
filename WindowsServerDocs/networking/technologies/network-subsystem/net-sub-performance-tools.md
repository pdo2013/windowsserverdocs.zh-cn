---
title: 网络工作负载的性能工具
description: 本主题介绍 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 81c31871b3dfa4644690fe074ae15eaaa680d7f2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tools-for-network-workloads"></a>网络工作负载的性能工具

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解性能工具。

本主题包含有关的客户端，服务器交通工具、TCP/IP 窗口的大小和 Microsoft 服务器性能顾问部分。

##  <a name="bkmk_tuning"></a>客户端，服务器交通工具

客户端，服务器交通 \(ctsTraffic\) 工具到为您提供了创建并验证网络通信的能力。

有关详细信息，以及要下载该工具，请参阅[ctsTraffic（客户端服务器交通）](http://ctstraffic.codeplex.com/)。
  
##  <a name="bkmk_size"></a>TCP/IP 窗口的大小

1 GB 的适配器，如上表所示的设置应提供有关吞吐量很好因为 NTttcp 中将默认 TCP 窗口大小设置为 64 K 通过特定的逻辑处理器选项 \(SO_RCVBUF\) 连接。 这在低延迟的网络提供良好的性能。  

相反，对于高延迟网络或 10 GB 的适配器，NTttcp 默认 TCP 窗口大小值产生小于获得最佳性能。 在这两种情况下，你必须调整 TCP 窗口大小，以使其变得更大带宽延迟产品。  

你可以静态 TCP 窗口的大小较大的值为使用设置**-rb**选项。 此选项禁用 TCP 窗口自动调优，我们建议使用它才用户完全了解结果更改 TCP/IP 行为。 默认情况下，TCP 窗口的大小设置足够值，并且仅在负载或通过高延迟链接调整。  

##  <a name="bkmk_advisor"></a>Microsoft 服务器性能顾问

Microsoft 服务器性能顾问 \(SPA\) 可收集指标以找出、比较和诊断潜在的性能问题，在 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 部署的 IT 管理员的帮助。 

水疗生成完整的诊断报告和图，并提供建议，以帮助你快速分析问题，并开发纠正操作。  
  
 有关详细信息并下载顾问，请参阅[Microsoft 服务器性能顾问](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx)Windows 硬件开发人员中心中。

本指南中的所有主题的链接，请参阅[网络子系统性能优化](net-sub-performance-top.md)。