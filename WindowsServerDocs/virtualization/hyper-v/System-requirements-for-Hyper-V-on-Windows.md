---
title: Windows Server 上的 Hyper-v 的系统要求
description: 列出 Windows Server 中 Hyper-v 的硬件和固件要求
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: fabaa1933fef836bb6ce3fc01badf337b832d072
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365443"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-v 的系统要求

>适用于：Windows Server 2016，Microsoft Hyper-V Server 2016，Windows Server 2019，Microsoft Hyper-V 服务器2019

Hyper-v 具有特定硬件要求，某些 Hyper-v 功能具有其他要求。 使用本文中的详细信息可以确定系统必须满足的要求，以便你可以使用你计划的方式来使用 Hyper-v。 然后，查看[Windows Server 目录](https://www.windowsservercatalog.com/)。 请记住，Hyper-v 要求超过 Windows Server 2016 的一般最低要求，因为虚拟化环境需要更多的计算资源。

如果已在使用 Hyper-v，则很可能会使用现有的硬件。 一般硬件要求在 Windows Server 2012 R2 中没有明显的变化。  但你需要使用较新的硬件来使用受防护的虚拟机或离散设备分配。 这些功能依赖于特定硬件支持，如下所述。 另一方面，硬件的主要区别在于，现在需要二级地址转换（SLAT），而不是建议使用。

有关 Hyper-v 支持的最大配置（例如正在运行的虚拟机的数目）的详细信息，请参阅[规划 Windows Server 2016 中的 hyper-v 可伸缩性](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。 [Windows Server 上的 Hyper-v 支持的 windows 来宾操作系统](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)中介绍了可在虚拟机中运行的操作系统的列表。

## <a name="general-requirements"></a>常规要求

无论你要使用哪些 Hyper-v 功能，你都需要：

- 带有二级地址转换（SLAT）的64位处理器。 若要安装 Hyper-v 虚拟化组件（如 Windows 虚拟机监控程序），处理器必须具有 SLAT。 但是，无需安装 Hyper-v 管理工具（例如虚拟机连接（VMConnect））、Hyper-v 管理器和 Windows PowerShell 的 Hyper-v cmdlet。 若要查明处理器是否有 SLAT，请参阅下面的 "如何检查 Hyper-v 要求"。

- VM 监视器模式扩展

- *至少*4 GB RAM 的内存计划。 更多内存更好。 对于要同时运行的主机和所有虚拟机，你将需要足够的内存。

- 虚拟化支持在 BIOS 或 UEFI 中启用：

  - 硬件协助的虚拟化。 此功能在包含虚拟化选项的处理器（特别是具有 Intel 虚拟化技术（Intel VT）或 AMD 虚拟化（AMD）技术的处理器）中提供。

  - 硬件强制实施的数据执行保护 (DEP) 必须可用且已启用。 对于 Intel 系统，这是 XD 位（执行禁用位）。 对于 AMD 系统，这是 NX 位（无执行位）。

## <a name="how-to-check-for-hyper-v-requirements"></a>如何检查 Hyper-v 要求

打开 Windows PowerShell 或命令提示符并键入：

```cmd
Systeminfo.exe
```

滚动到 "Hyper-v 要求" 部分以查看报表。

## <a name="requirements-for-specific-features"></a>特定功能的要求

下面是对离散设备分配和受防护的虚拟机的要求。 有关这些功能的说明，请参阅[Windows Server 上的 hyper-v 中的新增](What-s-new-in-Hyper-V-on-Windows.md)功能。

### <a name="discrete-device-assignment"></a>离散设备分配

**主机**要求与 HYPER-V 中 sr-iov 功能的现有要求类似。

- 处理器必须具有 Intel 的扩展页表（EPT）或 AMD 的嵌套页表（NPT）。

- 芯片组必须具有：

  - 中断重映射-采用中断重映射功能（VT-d2）或任何版本的 AMD i/o 内存管理单元（i/o MMU）的 Intel 的 VT。

  - DMA 重新映射-包含排队失效或任何 AMD i/o MMU 的 Intel VT-d。

  - PCI Express 根端口上的访问控制服务（ACS）。

- 固件表必须向 Windows 虚拟机监控程序公开 i/o MMU。 请注意，此功能可能在 UEFI 或 BIOS 中处于关闭状态。 有关说明，请参阅硬件文档或与硬件制造商联系。

**设备**需要 GPU 或非易失性内存 Express （NVMe）。 对于 GPU，只有某些设备支持离散设备分配。 若要验证，请参阅硬件文档或与硬件制造商联系。 有关此功能的详细信息，包括如何使用它和注意事项，请参阅虚拟化博客中的发布 "[离散设备分配-说明和背景](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)"。

### <a name="shielded-virtual-machines"></a>受防护的虚拟机

这些虚拟机依赖于基于虚拟化的安全性，可从 Windows Server 2016 开始使用。

**主机**要求如下：

- UEFI 2.3.1 c-支持安全、标准启动

  下面的两个选项对于一般基于虚拟化的安全是可选的，但如果你想要提供这些功能提供的保护，主机需要此选项：

- TPM v2.0-保护平台安全资产
- IOMMU （Intel VT）-以便虚拟机监控程序可以提供直接内存访问（DMA）保护

**虚拟机**要求如下：

- 第 2 代
- Windows Server 2012 或更高版本作为来宾操作系统

