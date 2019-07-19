---
title: Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机
description: 列出每个版本中包含的 Linux integration services 和功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 593068f4fc2015c7f8f94bfe49c5a11c23cb6599
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314983"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机

>适用于：Windows Server 2019, Windows Server 2016, Hyper-v Server 2016, Windows Server 2012 R2, Hyper-v server 2012 R2, Windows Server 2012, Hyper-v Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Hyper-v 支持适用于 Linux 和 FreeBSD 虚拟机的模拟和 Hyper-v 特定设备。 与模拟设备一起运行时, 无需安装任何其他软件。 但模拟设备不提供高性能, 并且无法利用 Hyper-v 技术提供的丰富的虚拟机管理基础结构。 为了充分利用 Hyper-v 提供的所有权益, 最好使用适用于 Linux 和 FreeBSD 的 Hyper-v 特定设备。 运行特定于 Hyper-v 的设备所需的驱动程序集合称为 Linux Integration Services (.LIS) 或 FreeBSD Integration Services (BIS)。

已将 .LIS 添加到 Linux 内核, 并更新了新版本。 但基于较旧内核的 Linux 分发可能没有最新的增强功能或修补程序。 Microsoft 提供下载, 其中包含基于这些旧内核的某些 Linux 安装的可安装的 .LIS 驱动程序。 由于分发供应商包括 Linux Integration Services 的版本, 因此最好安装适用于你的安装的最新可下载版本 (如果适用)。

对于其他 Linux 分发, IIS 会定期将其集成到操作系统内核和应用程序中, 因此不需要单独下载或安装。

对于较旧的 FreeBSD 版本 (10.0 之前的版本), Microsoft 提供了包含可安装的 BIS 驱动程序和 FreeBSD 虚拟机的相应守护程序的端口。 对于更高版本的 FreeBSD 版本, BIS 内置于 FreeBSD 操作系统中, 无需单独下载或安装, 因为 FreeBSD 10.0 需要下载 KVP 端口。

> [!TIP]
> - 从评估中心下载[Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019) 。

此内容的目标是提供信息, 有助于简化 Hyper-v 上的 Linux 或 FreeBSD 部署。 具体的详细信息包括:

* 需要下载和安装 .LIS 或 BIS 驱动程序的 Linux 分发版或 FreeBSD 版本。

* Linux 分发版或包含内置 .LIS 或 BIS 驱动程序的 FreeBSD 版本。

* 功能分发映射, 用于指示主要 Linux 分发版或 FreeBSD 版本中的功能。

* 每个分发或发布的已知问题和解决方法。

* 每个 .LIS 或 BIS 功能的功能说明。

**想要对特性和功能提出建议？** 是否有一些更好的做法？ 你可以使用[Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support)网站为 hyper-v 上的 Linux 和 FreeBSD 虚拟机建议新特性和功能, 以及查看其他人的看法。

## <a name="in-this-section"></a>本节内容

* [Hyper-v 上支持的 CentOS 和 Red Hat Enterprise Linux 虚拟机](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 Oracle Linux 虚拟机](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 SUSE 虚拟机](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 Ubuntu 虚拟机](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 FreeBSD 虚拟机](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上运行 Linux 的最佳实践](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [在 Hyper-v 上运行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
