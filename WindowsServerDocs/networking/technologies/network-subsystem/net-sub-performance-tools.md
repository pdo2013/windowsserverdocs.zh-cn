---
title: 用于网络工作负荷的性能工具
description: 本主题是适用于 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: e71c5f34041145907c30b279dc91a94c03c2abed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824928"
---
# <a name="performance-tools-for-network-workloads"></a>用于网络工作负荷的性能工具

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解有关性能工具。

本主题包含有关客户端到服务器的通信工具、 TCP/IP 窗口大小和 Microsoft Server Performance Advisor 部分。

##  <a name="bkmk_tuning"></a> 客户端到服务器的通信工具

客户端到服务器的通信\(ctsTraffic\)工具为您提供创建和验证网络流量的能力。

有关详细信息，以及要下载该工具，请参阅[ctsTraffic （客户端到服务器的通信）](https://github.com/Microsoft/ctsTraffic)。
  
##  <a name="bkmk_size"></a> TCP/IP 窗口大小

对于 1 GB 适配器上, 表中所示的设置应提供较高的吞吐量因为 NTttcp 通过特定的逻辑处理器选项设置的默认 TCP 窗口大小为 64 K \(SO_RCVBUF\)连接。 此低延迟网络上提供良好的性能。  

NTttcp 的默认 TCP 窗口大小值与此相反，对于高延迟网络或 10 GB 的适配器，将产生逊色于最佳性能。 在这两种情况下，您必须调整 TCP 窗口大小，以允许更大的带宽延迟乘积。  

你可以以静态方式将 TCP 窗口大小设置为较大的值使用 **-rb**选项。 此选项将禁用 TCP 窗口自动调节，和我们建议仅当用户完全识别 TCP/IP 行为中的结果更改时，才使用它。 默认情况下，TCP 窗口大小设置为足够大的值，并调整仅在重负载下或在高延迟链路上。  

##  <a name="bkmk_advisor"></a> Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\)帮助 IT 管理员收集测量值以便标识，比较和诊断 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows 中的潜在性能问题Server 2008 R2 或 Windows Server 2008 部署。 

SPA 生成全面的诊断报告和图表，并提供建议来帮助你快速分析问题和制订更正措施。  
  
 有关详细信息及下载顾问，请参阅[Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) Windows 硬件开发人员中心中。

本指南中的所有主题的链接，请参阅[网络子系统性能调整](net-sub-performance-top.md)。