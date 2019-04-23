---
ms.assetid: ''
title: 在 Windows Server 2019 HPN 功能的深入了解预览版
description: 了解有关 Windows Server 2019 中新的高性能网络功能。
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3d3e974472f28c30d093fbd1094ef3693d984d19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886268"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>动态 vRSS 和 VMMQ

>适用于：Windows Server 2019

在过去，虚拟机队列和虚拟机多队列启用到各个 Vm 多更高的吞吐量为首先达到的 10GbE 标记的网络吞吐量和更高版本。 遗憾的是，规划、 确定基准、 优化和监视所需的成功成为一项较大的任务;通常多个 IT 管理员应花费。 

Windows Server 2019 通过动态分配和优化处理所需的网络工作负荷提高了这些优化。 Windows Server 2019 可确保最高效率，并为 IT 管理员删除配置负担。

有关详细信息，请参阅：

-   [公告博客](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [验证指南面向 IT 专业人员](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>在 vSwitch 中接收段合并 (RSC)

>适用于：Windows Server 2019 和 Windows 10，版本 1809

接收段合并 (RSC) vSwitch 中是将多个 TCP 段合并到数据遍历 vSwitch 之前更大段的增强功能。 大段可以提高网络性能的虚拟工作负载。

以前，这是由 NIC 实现卸载 遗憾的是，这已被禁用适配器连接到虚拟交换机的时刻。 在 Windows Server 2019 和 Windows 上 vSwitch RSC 10 2018 年 10 月更新中删除此限制。

默认情况下，RSC vSwitch 中的是外部虚拟交换机上启用的。

有关详细信息，请参阅：

-  [公告博客](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [验证指南面向 IT 专业人员](https://aka.ms/RSC-Validation)
