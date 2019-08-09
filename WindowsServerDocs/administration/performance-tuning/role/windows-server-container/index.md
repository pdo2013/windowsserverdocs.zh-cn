---
title: Windows Server 容器的性能优化
description: 针对 Windows Server 16 上的容器的性能优化建议
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: akino
ms.date: 10/16/2017
ms.openlocfilehash: 072d71a7825907ada7d4bc02eb5390722692e81d
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863424"
---
# <a name="performance-tuning-windows-server-containers"></a>Windows Server 容器的性能优化

## <a name="introduction"></a>简介
Windows Server 2016 是支持内置于 OS 的容器技术的第一个 Windows 版本。 Server 2016 中提供了两种类型的容器：Windows Server 容器和 Hyper-V 容器。 每个容器类型均支持 Windows Server 2016 的 Server Core 或 Nano Server SKU。 

这些配置具有不同的性能影响，我们将在下方详细介绍这些影响，帮助你了解适合你方案的配置。 此外，我们还会详细介绍影响性能的配置，并且会说明针对每个选项的权衡。

### <a name="windows-server-container-and-hyper-v-containers"></a>Windows Server 容器和 Hyper-V 容器

虽然 Windows Server 容器和 Hyper-V 容器提供了许多相同的可移植性和一致性优势，但在隔离保证和性能特性方面，它们却存在差异。

**Windows Server 容器** 通过进程和命名空间隔离技术来提供应用程序隔离。 Windows Server 容器与容器主机和该主机上运行的所有容器共享内核。

**Hyper-V 容器** 通过在高度优化的虚拟机中运行每个容器，从而在由 Windows Server 容器提供的隔离上扩展。 在此配置中，容器主机的内核不与 Hyper-V 容器共享。

Hyper-V 容器提供的额外隔离在很大程度上是通过容器和容器主机之间的虚拟机监控程序隔离层来实现的。 与 Windows Server 容器不同，这会影响容器密度，因为系统文件和二进制文件的共享可能会减少，从而导致总体存储和内存占用变大。 此外，一些网络、存储 io 和 CPU 路径中可能会存在预期的额外开销。

### <a name="nano-server-and-server-core"></a>Nano Server 和 Server Core

Windows Server 容器和 Hyper-V 容器提供了针对 Server Core 的支持，以及针对 Windows Server 2016 中提供的新安装选项的支持：[Nano Server](https://technet.microsoft.com/windows-server-docs/compute/nano-server/getting-started-with-nano-server)。 

Nano Server 是针对私有云和数据中心进行优化的远程管理的服务器操作系统。 类似于 Windows Server 的“服务器核心”模式，但显著变小了，无本地登录功能，且仅支持 64 位应用程序、工具和代理。 它所需的磁盘空间更少，并且启动速度更快。

## <a name="container-start-up-time"></a>容器启动时间
在容器提供最大优势的诸多方案中，容器启动时间是一项关键指标。 正因为如此，了解如何最大限度地优化容器启动时间至关重要。 以下是一些需要了解的优化权衡，可以提高启动时间。

### <a name="first-logon"></a>首次登录

Microsoft 为 Nano Server 和 Server Core 均提供了基础映像。 通过消除与首次登录 (OOBE) 相关的启动时间开销，已对 Server Core 附带的基础映像进行了优化。 但 Nano Server 基础映像却并非如此。 但是，可以通过将至少一层提交到容器映像，从基于 Nano Server 的映像消除此费用。 从此映像启动的后续容器将不会产生首次登录费用。
### <a name="scratch-space-location"></a>暂存空间位置

默认情况下，在运行的容器的生命周期内，容器将使用容器主机的系统驱动器介质上的临时暂存空间进行存储。 这充当容器的系统驱动器，因此，在容器操作中完成的许多写入和读取均遵循此路径。 对于系统驱动器存在于旋转磁盘磁性介质 (HDD) 上但提供有更快的存储介质（更快的 HDD 或 SSD）的主机系统，可以将容器暂存空间移动到不同驱动器。 此操作可以通过使用 dockerd –g 命令实现。 此命令是全局的，将影响系统上运行的所有容器。

### <a name="nested-hyper-v-containers"></a>嵌套的 Hyper-V 容器
Windows Server 2016 的 Hyper-V 引入了嵌套的虚拟机监控程序支持。 也就是说，现在可以在虚拟机中运行虚拟机。 这开启了许多有用的方案，但同时还扩大了虚拟机监控程序会产生的一些性能影响，因为物理主机上正在运行两个级别的虚拟机监控程序。

对于容器而言，在虚拟机中运行 Hyper-V 容器时，这会产生影响。 由于 Hyper-V 容器通过其自身与容器主机之间的虚拟机监控程序层来提供隔离，因此，当容器主机是基于 Hyper-V 的虚拟机时，将产生有关容器启动时间、存储 io、网络 io 和吞吐量以及 CPU 的性能开销。

## <a name="storage"></a>存储
### <a name="mounted-data-volumes"></a>已装载的数据卷

容器能够将容器主机系统驱动器用于容器暂存空间。 但是，容器暂存空间具有与容器相等的生存期。 也就是说，在容器停止后，暂存空间和所有关联的数据都将会消失。

但是，在很多情况下，需要独立于容器生存期保留数据。 在这些情况下，我们支持将数据卷从容器主机装载到容器。 对于 Windows Server 容器而言，与已装载的数据卷（接近本机性能）相关联的 IO 路径开销可以忽略不计。 但是，在将数据卷装载到 Hyper-V 容器时，此路径中的某些 IO 性能会降低。 此外，当在虚拟机中运行 Hyper-V 容器时，此影响会被放大。

### <a name="scratch-space"></a>暂存空间

Windows Server 容器和 Hyper-V 容器默认为容器暂存空间提供 20 GB 动态 VHD。 对于这两种容器类型，容器 OS 都会占用此空间的一部分，并且对于每个启动的容器而言都是如此。 因此，请务必记住，每个启动的容器都有一些存储影响，并且根据工作负荷，可以至多写入 20 GB 的备用存储介质。 在设计服务器存储配置时应记住这一点。
（我们是否可以配置暂存大小）

## <a name="networking"></a>网络
Windows Server 容器和 Hyper-V 容器提供了各种网络模式，从而最大限度上满足不同网络配置的需求。 每个选项都呈现了其自己的性能特征。

### <a name="windows-network-address-translation-winnat"></a>Windows 网络地址转换 (WinNAT)

每个容器都将收到来自内部专用 IP 前缀（例如 172.16.0.0/12）的一个 IP 地址。 支持从容器主机到容器终结点的端口转发/映射。 首次运行 dockerd 时，Docker 将默认创建一个 NAT 网络。

在这三种模式中，NAT 配置是最昂贵的网络 IO 路径，但需要进行的配置量最少。 

Windows Server 容器使用主机 vNIC 连接到虚拟交换机。 Hyper-V 容器使用合成 VM NIC（不公开到实用工具 VM）连接到虚拟交换机。 当容器与外部网络通信时，数据包将通过应用了地址转换的 WinNAT 进行路由，从而产生一些开销。

### <a name="transparent"></a>透明

每个容器终结点都是直接连接到物理网络的。 可以使用外部 DHCP 服务器静态或动态分配来自物理网络的 IP 地址。

就网络 IO 路径而言，透明模式是最便宜的，并且外部数据包是直接传递到容器虚拟 NIC 的，从而实现直接访问外部网络。

### <a name="l2-bridge"></a>L2 桥接
每个容器终结点都将与容器主机位于同一 IP 子网内。 必须从与容器主机相同的前缀中静态分配 IP 地址。 由于第 2 层地址转换，主机上的所有容器终结点都将具有相同的 MAC 地址。

L2 桥接模式能够比 WinNAT 模式提供更好的性能，因为它提供了对外部网络的直接访问，但由于它还引入了 MAC 地址转换，因此，相较于透明模式，它提供的性能要差一些。




