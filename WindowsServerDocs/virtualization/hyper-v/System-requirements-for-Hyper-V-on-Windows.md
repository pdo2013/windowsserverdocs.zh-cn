---
title: Windows Server 上的 HYPER-V 的系统要求
description: 列出了 Windows Server 中的 HYPER-V 的硬件和固件要求
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 97fb1b9003705ba8ad26c2b3e71eda34e88642ee
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812616"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Windows Server 上的 HYPER-V 的系统要求

>适用于：Windows Server 2016 中，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

HYPER-V 都有特定的硬件要求，并且某些 HYPER-V 功能有其他要求。 使用本文中详细信息来决定哪些系统必须满足，因此，可以使用 HYPER-V 到计划的方式的要求。 然后，查看[Windows Server 目录](https://www.windowsservercatalog.com/)。 请记住有关 HYPER-V 要求超过 Windows Server 2016 的常规最低要求，因为虚拟化环境需要更多计算资源。

如果你已在使用 HYPER-V，则可能使用你现有的硬件。 常规硬件要求未从 Windows Server 2012 R2 显著更改。  但是，你将需要使用受防护的虚拟机或离散设备分配到较新的硬件。 这些功能依赖于特定硬件支持，如下所述。 除此之外，在硬件中的主要区别是该第二级地址转换 (SLAT) 现在要求而不是建议。

有关支持的最大配置 HYPER-V，如正在运行的虚拟机的数量的详细信息请参阅[规划 Windows Server 2016 中的 HYPER-V 可伸缩性](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。 中介绍了可以在你的虚拟机中运行的操作系统的列表[支持 Windows 来宾操作系统为 Windows Server 上的 HYPER-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)。

## <a name="general-requirements"></a>常规要求

无论你想要使用的 HYPER-V 功能，你将需要：

- 具有二级地址转换 (SLAT) 的 64 位处理器。 若要安装的 HYPER-V 虚拟化组件，如 Windows 虚拟机监控程序，处理器必须具有 SLAT。 但是，它具有不需要安装用于 Windows PowerShell 的 HYPER-V 虚拟机连接 (VMConnect)、 HYPER-V 管理器等的 HYPER-V cmdlet 的管理工具。 请参阅下面的"如何查看 HYPER-V 要求"中，要找出您的处理器具有 SLAT。

- 虚拟机监视器模式扩展

- 足够的内存-规划*至少*4 GB 的 RAM。 更多的内存是更好。 主机和所有你想要在同一时间运行的虚拟机，你将需要足够的内存。

- 在 BIOS 或 UEFI 中启用虚拟化支持：

  - 硬件协助的虚拟化。 这是包含处理器的虚拟化选项-特别是具有 Intel 虚拟化技术 (Intel VT) 或 AMD 虚拟化 (AMD-V) 技术的处理器中提供。

  - 硬件强制实施的数据执行保护 (DEP) 必须可用且已启用。 对于 Intel 系统，这是 XD 位 （执行禁用位）。 对于 AMD 系统，这是 NX 位 （无执行位）。

## <a name="how-to-check-for-hyper-v-requirements"></a>如何检查有关 HYPER-V 要求

打开 Windows PowerShell 或命令提示符并键入：

```cmd
Systeminfo.exe
```

滚动到 HYPER-V 要求部分来查看报告。

## <a name="requirements-for-specific-features"></a>特定功能的要求

以下是离散设备分配和受防护的虚拟机的要求。 有关这些功能的说明，请参阅[什么是 Windows Server 上的 HYPER-V 中的新增功能](What-s-new-in-Hyper-V-on-Windows.md)。

### <a name="discrete-device-assignment"></a>离散设备分配

**主机**要求就类似于在 HYPER-V 中的 SR-IOV 功能的现有要求。

- 处理器必须具有任一 Intel 扩展页表 (EPT) 或 AMD 的嵌套页表 (NPT)。

- 芯片组必须具有：

  - 中断重新映射-Intel VT d 的中断重新映射功能 (VT d2) 或 AMD I/O 内存管理单元 (MMU I/O) 的任何版本。

  - DMA 重新映射-与排队失效或任何 AMD I/O MMU Intel VT d。

  - PCI Express 上的访问控制服务 (ACS) 的根的端口。

- 固件表必须公开到 Windows 虚拟机监控程序 I/O MMU。 请注意可能在 BIOS 或 UEFI 中禁用此功能。 有关说明，请参阅硬件文档或者联系硬件制造商。

**设备**需要 GPU 或非易失性内存 express (NVMe)。 对于 GPU，某些设备仅支持离散设备分配。 若要验证，请参阅硬件文档或者联系硬件制造商。 有关此功能的详细信息，包括如何使用它以及注意事项，请参阅文章"[离散设备分配-说明和背景](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)"虚拟化博客中。

### <a name="shielded-virtual-machines"></a>受防护的虚拟机

这些虚拟机依赖于基于虚拟化的安全性，并可从 Windows Server 2016 开始。

**主机**要求是：

- UEFI 2.3.1c 的支持安全，以启动

  以下两个一般情况下，都是可选的基于虚拟化的安全性，但如果您希望使用这些功能提供了保护所需的主机：

- TPM 2.0 版的可保护平台安全资产
- IOMMU (Intel VT-D)-因此，虚拟机监控程序可以提供直接内存访问 (DMA) 保护

**虚拟机**要求是：

- 第 2 代
- Windows Server 2012 或更高版本作为来宾操作系统

