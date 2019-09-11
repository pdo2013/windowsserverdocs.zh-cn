---
title: 虚拟接收方缩放 (vRSS)
description: 了解 Windows Server 中的虚拟接收方缩放（vRSS），以及如何配置虚拟网络适配器，以便对 VM 中的多个逻辑处理器核心的传入网络流量进行负载均衡。 你还可以为主机虚拟网络接口卡（vNIC）配置数个物理核心。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ae017d7d78adea565942a952aaea3da1669f39a9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871804"
---
# <a name="virtual-receive-side-scaling-vrss"></a>虚拟接收方缩放\(vRSS\)

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，你将了解有关虚拟接收方缩放（vRSS）的信息，以及如何配置虚拟网络适配器，以便对 VM 中的多个逻辑处理器核心的传入网络流量进行负载均衡。 你还可以使用 vRSS 为主机虚拟网络接口卡\(vNIC\)配置多个物理内核。

此配置允许将虚拟网络适配器的负载分布在虚拟机\(VM\)中的多个虚拟处理器上，从而使 VM 能够更快地处理更多网络流量，而不能使用单个逻辑处理器。

>[!TIP]
>可以在具有多个处理器的 hyper-v\-主机、单个多核处理器或安装了多个多核处理器并为 VM 用途配置多个的虚拟机上使用 vRSS。

vRSS 与所有其他 hyper-v\-网络技术兼容。 vRSS 依赖于 hyper-v \(\-主机中\)的虚拟机队列 VMQ 和 VM 中的 RSS 或主机上的 vNIC。

默认情况下，Windows Server 启用 vRSS，但你可以通过使用 Windows PowerShell 在虚拟机中禁用它。 有关详细信息，请参阅[为 RSS 和 VRSS](vrss-wps.md)[管理 VRSS](vrss-manage.md)和 Windows PowerShell 命令。



## <a name="operating-system-compatibility"></a>操作系统兼容性

可以在运行 Windows Server 2016 的任何多处理器或多核 VM 上的任何多处理器或多核计算机或 vRSS 上使用 RSS。

运行以下 Microsoft 操作系统的多处理器或多核 Vm 还支持 vRSS。

- Windows Server 2016
- Windows 10 专业版或企业版
- Windows Server 2012 R2
- Windows 8.1 Pro 或 Enterprise
- Windows Server 2012 安装了 Windows Server 2012 R2 集成组件。
- 安装了 Windows Server 2012 R2 集成组件的 windows 8。

有关对运行 FreeBSD 或 Linux 作为 Hyper-v 上的来宾操作系统的 Vm 的 vRSS 支持的信息，请参阅[Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows)。
  
## <a name="hardware-requirements"></a>硬件要求

下面是 vRSS 的硬件要求。
 
- 物理网络适配器必须支持虚拟机队列\(VMQ\)。 如果 VMQ 已禁用或不受支持，则会为 hyper-v\-主机和在主机上配置的任何 vm 禁用 vRSS。
- 网络适配器的链接速度必须为 10 Gbps 或更高。
- Hyper-v\-主机必须配置有多个处理器或至少一个多\-核处理器才能使用 vRSS。
- 虚拟机\(vm\)必须配置为使用两个或多个逻辑处理器。


## <a name="use-case-scenarios"></a>用例方案

以下两个用例方案描述了 vRSS 的常见用法，以实现处理器负载平衡和软件负载平衡。

### <a name="processor-load-balancing"></a>处理器负载平衡
  
Anthony 是一个网络管理员，它是使用两个支持单一根输入-输出虚拟化\(\)sr-iov\-的网络适配器设置新的 hyper-v 主机。 他部署 Windows Server 2016 以托管 VM 文件服务器。

安装硬件和软件后，Anthony 会将 VM 配置为使用8个虚拟处理器和 4096 MB 内存。 遗憾的是，Anthony 没有打开 sr-iov\-的选项，因为他的 vm 依赖于通过 hyper-v\-虚拟交换机管理器创建的虚拟交换机执行的策略。 因此，Anthony 决定使用 vRSS 而不是 sr-iov\-。

最初，Anthony 使用 Windows PowerShell 分配四个虚拟处理器，使其可用于 vRSS。 一周后使用文件服务器非常受欢迎，因此 Anthony 检查 VM 的性能。  他发现四个虚拟处理器的全部利用率。

因此，Anthony 决定向 VM 添加要供 vRSS 使用的处理器。  他向 VM 分配了两个更多虚拟处理器，可自动将其提供给 vRSS 来帮助处理大型网络负载。 他的工作会提高 VM 文件服务器的性能，并且六个处理器有效处理网络流量负载。


### <a name="software-load-balancing"></a>软件负载平衡

Sandra 是网络管理员，它是在其一个系统上设置单个高性能 VM，用作软件负载平衡器。 她向此单个 VM 分配了所有可用的逻辑处理器。

安装 Windows Server 后，她使用 vRSS 获取 VM 中多个逻辑处理器上的并行网络流量处理。 由于 Windows Server 启用 vRSS，Sandra 无需进行任何配置更改。


## <a name="related-topics"></a>相关主题

- [规划 vRSS 的使用](vrss-plan.md)
- [在虚拟网络适配器上启用 vRSS](vrss-enable.md)
- [管理 vRSS](vrss-manage.md)
- [vRSS 常见问题](vrss-faq.md)
- [用于 RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)

---
