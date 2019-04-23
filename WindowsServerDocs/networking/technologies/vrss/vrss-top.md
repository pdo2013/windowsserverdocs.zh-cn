---
title: 虚拟接收方缩放 (vRSS)
description: 在 Windows Server 以及如何配置流量进行负载平衡传入网络到虚拟机中的多个逻辑处理器内核的虚拟网络适配器中了解有关虚拟接收方缩放 (vRSS) 的信息。 此外可以配置多个物理核心，主机的虚拟网络接口卡 (vNIC)。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875228"
---
# <a name="virtual-receive-side-scaling-vrss"></a>虚拟接收方缩放\(vRSS\)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，您了解有关虚拟接收方缩放 (vRSS) 以及如何配置虚拟网络适配器，以传入网络流量负载平衡到多个逻辑处理器内核的 VM 中。 VRSS 还可用来配置主机的多个物理核心虚拟网络接口卡\(vNIC\)。

此配置让分散在虚拟机中的多个虚拟处理器上的虚拟网络适配器的负载\(VM\)，使 VM 比使用单个更快地处理更多的网络流量逻辑处理器。

>[!TIP]
>可以在超上的 Vm 中使用 vRSS\-V 主机具有多个四核处理器，多个处理器，一个或多个核心的多个处理器为安装和配置 VM 的使用。

vRSS 适用于所有其他超\-V 网络技术。 vRSS 是依赖于虚拟机队列\(VMQ\)在超\-V 主机和 RSS 在 VM 中或在主机 vNIC 上。

默认情况下，Windows Server 启用 vRSS，但可以通过使用 Windows PowerShell 禁用它在 VM 中。 有关详细信息，请参阅[管理 vRSS](vrss-manage.md)并[Windows PowerShell 命令适用于 RSS 和 vRSS](vrss-wps.md)。



## <a name="operating-system-compatibility"></a>操作系统兼容性

可以在任何多处理器或多核计算机中-或 vRSS 任何多处理器或多核 VM 上的正在运行 Windows Server 2016 上使用 RSS。

多处理器或多核运行以下 Microsoft 操作系统的 Vm 还支持 vRSS。

- Windows Server 2016
- Windows 10 Pro 或 Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro 或 Enterprise
- Windows Server 2012 安装的 Windows Server 2012 R2 的集成组件。
- 安装了 Windows Server 2012 R2 的集成组件的 Windows 8。

VRSS 支持的 FreeBSD 或 Linux 作为来宾操作系统运行在 HYPER-V 上的 Vm 的信息，请参阅[Windows 上的 HYPER-V 的支持的 Linux 和 FreeBSD 虚拟机](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows)。
  
## <a name="hardware-requirements"></a>硬件要求

以下是 vRSS 的硬件要求。
 
- 物理网络适配器必须支持虚拟机队列\(VMQ\)。 如果 VMQ 已禁用或不受支持，则对于 Hyper 禁用 vRSS\-V 主机和主机上配置的任何 Vm。
- 网络适配器必须具有链接速度为 10 Gbps 或更多。
- 超\-V 主机必须配置与多个处理器或至少一个多\-核心处理器使用 vRSS。
- 虚拟机\(Vm\)必须配置为使用两个或多个逻辑处理器。


## <a name="use-case-scenarios"></a>用例场景

下面的两个用例场景描述为处理器负载平衡和软件负载平衡 vRSS 的常见用法。

### <a name="processor-load-balancing"></a>处理器负载平衡
  
Anthony，网络管理员，设置具有两个网络适配器支持单根输入-输出虚拟化的新的 HYPER-V 主机\(SR\-IOV\)。 他将部署 Windows Server 2016 以托管文件服务器的 VM。

在安装硬件和软件，Anthony 配置 VM 以使用八个虚拟处理器和 4096 MB 内存。 遗憾的是，Anthony 不具有选项来打开 SR\-IOV 因为其 Vm 依赖于通过虚拟交换机使用超他创建的策略实施\-V 虚拟交换机管理器。 正因为如此，Anthony 决定而不是 SR 使用 vRSS\-IOV。

最初，Anthony 将通过使用 Windows PowerShell 可配合 vRSS 使用来分配四个虚拟处理器。 一周后的使用文件服务器似乎是广受欢迎，因此 Anthony 检查 VM 的性能。  他发现充分利用所有四个虚拟处理器。

正因为如此，Anthony 决定 vRSS 通过将处理器添加到 VM，以便使用。  他将两个虚拟处理器分配给 VM，会自动提供给 vRSS 以帮助处理较大的网络负载。 他工作导致有效地处理网络流量负载的六个处理器使用的文件服务器 VM，更好的性能。


### <a name="software-load-balancing"></a>软件负载平衡

Sandra，网络管理员，一个单独的高性能 VM 上设置其系统中的一个充当软件负载均衡器。 她已为所有可用逻辑处理器分配到此单个 VM。

在安装后 Windows Server，她将使用 vRSS 获得并行处理 VM 中的多个逻辑处理器上的网络流量。 由于 Windows Server 允许 vRSS，因此 Sandra 无需进行任何配置更改。


## <a name="related-topics"></a>相关主题

- [规划 vRSS 使用](vrss-plan.md)
- [启用虚拟网络适配器上 vRSS](vrss-enable.md)
- [管理 vRSS](vrss-manage.md)
- [vRSS Frequently Asked Questions](vrss-faq.md)
- [Windows PowerShell 命令适用于 RSS 和 vRSS](vrss-wps.md)

---
