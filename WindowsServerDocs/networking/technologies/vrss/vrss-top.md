---
title: 虚拟接收方缩放 (vRSS)
description: 了解虚拟接收方缩放 (vRSS) 在 Windows Server 以及如何配置虚拟网络适配器的虚拟机中的多个逻辑处理器内核负载平衡传入网络流量。 你还可以配置多个物理核心的主机虚拟网络接口卡 (vNIC)。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c1cb11cb8ce69463a31cfa5061290f79d8dda91
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133683"
---
# 虚拟接收方缩放 \(vRSS\)

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，你了解有关虚拟接收方缩放 (vRSS) 以及如何配置虚拟网络适配器，以便跨虚拟机中的多个逻辑处理器内核负载平衡传入网络流量。 你还可以使用 vRSS 来配置多个物理核心的主机虚拟网络接口卡 \(vNIC\)。

此配置允许从分发在虚拟机中的多个虚拟处理器的虚拟网络适配器的负载 \(VM\)，允许将 VM 比它与单个逻辑处理器可以更快地处理更多的网络流量。

>[!TIP]
>你可以在具有多个处理器，单个多核处理器，HYPER-V 主机上 Vm 中使用 vRSS 或多个多核处理器安装并配置为用于虚拟机用途。

vRSS 可与所有其他 HYPER-V 网络技术兼容。 vRSS 是依赖于在 HYPER-V 主机中的虚拟机队列 \(VMQ\) 和 RSS VM 中或在主机 vNIC 上。

默认情况下，Windows Server 支持 vRSS，但你可以通过使用 Windows PowerShell 将其禁用虚拟机中。 有关详细信息，请参阅[管理 vRSS](vrss-manage.md)和[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。



## 操作系统的兼容性

你可以使用 RSS 任何多处理器或多核的计算机上的任何多处理器或多核虚拟机上的运行 Windows Server 2016 的 vRSS。

多处理器或多核运行以下 Microsoft 操作系统的虚拟机还支持 vRSS。

- Windows Server 2016
- Windows 10 专业版或企业版
- Windows Server 2012 R2
- Windows 8.1 专业版或企业版
- 安装 Windows Server 2012 R2 的集成组件的 Windows Server 2012。
- 安装 Windows Server 2012 R2 的集成组件的 Windows 8。

有关作为来宾操作系统中运行 FreeBSD 或 Linux 上的 HYPER-V Vm vRSS 支持的信息，请参阅[有关 Windows 上的 HYPER-V 的受支持的 Linux 和 FreeBSD 虚拟机](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows)。
  
## 硬件要求

以下是 vRSS 的硬件要求。
 
- 物理网络适配器必须支持虚拟机队列 \(VMQ\)。 如果禁用或不支持 VMQ，则 vRSS 会禁用 HYPER-V 主机和主机上配置任何虚拟机。
- 网络适配器必须 10 Gbps 的链接速度或的详细信息。
- HYPER-V 主机必须具有多个处理器或至少一个以核处理器使用 vRSS 配置。
- 虚拟机 \(VMs\) 必须配置为使用两个或多个逻辑处理器。


## 用例方案

下面的两个用例方案描述 vRSS 处理器负载平衡和软件负载平衡常见的用法。

### 处理器负载平衡
  
安，网络管理员，正在设置新的 HYPER-V 主机与支持单个根输入输出虚拟化 \(SR\-IOV\) 两个网络适配器。 他部署 Windows Server 2016 托管虚拟机文件服务器。

安装后的硬件和软件，安配置用于使用 8 个虚拟处理器的 VM 和 4096 MB 内存。 遗憾的是，安没有打开 SR\ IOV，因为其 Vm 依赖于通过他使用 HYPER-V 虚拟交换机管理器创建虚拟交换机的策略实施的选项。 因此，安决定，而不是 SR\ IOV vRSS。

最初，安通过使用 Windows PowerShell 来提供可使用与 vRSS 分配四个虚拟处理器。 使用文件服务器的一周后似乎是非常受欢迎的因此安检查虚拟机的性能。  他发现全面利用的四个虚拟处理器。

因此，安决定将 vRSS 处理器添加到使用虚拟机。  他为虚拟机，自动提供给 vRSS 帮助处理大型网络负载的两个更多的虚拟处理器。 他的精力放导致高效地处理网络流量负载六个处理器的虚拟机文件服务器，更好的性能。


### 软件负载平衡

Sandra，网络管理员，一个单独的高性能 VM 上设置其系统之一以充当软件负载平衡器。 她已分配给此单个虚拟机的所有可用的逻辑处理器。

安装 Windows Server 后, 她使用 vRSS 获取并行处理在 VM 中的多个逻辑处理器上的网络流量。 Windows Server 支持 vRSS，因为 Sandra 无需进行任何配置更改。


## 相关主题

- [规划 vRSS 使用](vrss-plan.md)
- [启用虚拟网络适配器上 vRSS](vrss-enable.md)
- [管理 vRSS](vrss-manage.md)
- [vRSS 常见问题](vrss-faq.md)
- [RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)

---
