---
title: Windows 上的 HYPER-V 的受支持的 Linux 和 FreeBSD 虚拟机
description: 列出的 Linux 集成服务和每个版本中包含的功能
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
ms.openlocfilehash: 9df495bdc67b06a675fec050fb4c2960337ce8ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832898"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Windows 上的 HYPER-V 的受支持的 Linux 和 FreeBSD 虚拟机

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

HYPER-V 支持的 Linux 和 FreeBSD 虚拟机的模拟和 Hyper V 特定设备。 运行模拟设备，不则需要安装任何其他软件。 但是模拟的设备无法提供高性能，并且无法利用 HYPER-V 技术提供了丰富的虚拟机管理基础结构。 若要充分利用 HYPER-V 提供的所有权益，它是适用于 Linux 和 FreeBSD 使用 Hyper V 特定设备。 运行 Hyper V 特定设备所需的驱动程序的集合称为 Linux Integration Services (LIS) 或 FreeBSD Integration Services (BIS)。

LIS 已添加到 Linux 内核和新版本中进行了更新。 但是，Linux 分发版基于较旧的内核可能没有最新的增强功能或修补程序。 Microsoft 提供了包含对某些基于这些较旧的内核上的 Linux 安装的可安装 LIS 驱动程序的下载。 分发供应商包括版本的 Linux 集成服务，因为它是安装最新的可下载的 LIS 中版本，如果适用，请为您的安装。

对于其他 Linux 分发版 LIS 更改定期集成到操作系统内核和应用程序中，因此没有单独的下载或安装是必需的。

对于较旧的 FreeBSD 版本中 （在之前 10.0)，Microsoft 提供包含可安装 BIS 驱动程序和 FreeBSD 虚拟机的相应守护程序的端口。 对于更高版本的 FreeBSD 发行版本，BIS 内置于 FreeBSD 操作系统，并且没有单独的下载或安装需要 KVP 端口下载所需的 FreeBSD 10.0 除外。

> [!TIP]
> - 下载[Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)从评估中心。
> - 下载[Microsoft HYPER-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016)从评估中心。

此内容的目标是提供可帮助促进 Linux 或 FreeBSD HYPER-V 上的部署的信息。 特定的详细信息包括：

* Linux 分发版或需要下载和安装 LIS 或 BIS 驱动程序的 FreeBSD 版本。

* Linux 分发版或包含的内置 LIS 或 BIS 驱动程序的 FreeBSD 版本。

* 指示主要 Linux 发行版本或 FreeBSD 版本中的功能的功能分发映射。

* 已知的问题和解决方法的每个分发或发布。

* 每个 LIS 或 BIS 功能的功能说明。

**想要提出有关特性和功能建议吗？** 有我们可以更好地做的事情吗？ 可以使用[Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support)站点适用于 Linux 和 FreeBSD 虚拟机上的 HYPER-V，建议新特性和功能以及查看其他人的看法。

## <a name="in-this-section"></a>本节内容

* [支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [受支持的 Oracle Linux 虚拟机上的 HYPER-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [受支持的 SUSE 虚拟机上的 HYPER-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [受支持的 Ubuntu 虚拟机上的 HYPER-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [受支持的 FreeBSD 虚拟机上的 HYPER-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [HYPER-V 上运行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [HYPER-V 上运行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
