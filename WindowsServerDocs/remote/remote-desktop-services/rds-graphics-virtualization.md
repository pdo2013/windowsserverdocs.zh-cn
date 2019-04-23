---
title: RDS-图形虚拟化技术最适合您？
description: 若要帮助您选择恰当的图形的 RDS 部署虚拟化选项的规划信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/16/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: 7cf7fdf3510fcaaa955bd0031fb3564fe4372472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875798"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>图形虚拟化技术最适合你？

启用远程桌面服务中的图形呈现时具有一系列选项。 在规划虚拟化环境时，以下注意事项驱动器的图形呈现您选择的技术：

![图形呈现注意事项-进行比较的用户规模和 GPU 要求，以确定您的环境的最佳 GPU 技术](media/rds-gpu.png)

在 Windows Server 2016 中，有两个图形虚拟化技术可让您充分利用 GPU 硬件的 hyper-v:

- [离散设备分配 (DDA)](#discrete-device-assignment) -对于使用一个或多个 Gpu 的最高性能专用于提供本机 GPU 驱动程序支持在 vm 的 VM。 密度较低，因为它受到服务器上可用的物理 Gpu 数。 
- [远程 FX vGPU](#remotefx-vgpu) -对于知识工作者和高迸发 GPU 方案，其中多个 Vm 利用通过半虚拟化的一个或多个 Gpu。 此解决方案提供了每个服务器的更高用户密度。

下图显示图形中 Windows Server 2016 虚拟化选项。

![图形与 RDS-Windows Server 2016 中的虚拟化选项显示可用的三个技术以及规模和性能之间的区别](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>离散设备分配
离散设备分配 (DDA) 是一种硬件传递解决方案，可获得最佳性能，考虑到 VM 具有到 GPU 使用本机驱动程序的完全访问权限。 VM 用户可以访问其设备的所有功能，作为良好设备的本机驱动程序。 这意味着的特性和在裸机上运行同一设备的 VM 镜像中运行该设备的功能。

有关 DDA 的详细信息，请查看[规划部署离散设备分配](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
RemoteFX vGPU 是一种图形虚拟化技术，允许处理 GPU 的强大功能拆分到不同来宾操作系统启用知识工作者方案 （请参阅上面的第一个图形）。 Windows Server 2016 中的改进允许进一步增强功能的 GPU 迸发方案，例如设计器的应用程序和数据可视化效果。 其他功能的改进包括：

-   支持第 2 代来宾虚拟机、 Windows Server 2016 来宾虚拟机和 Windows 客户端 HYPER-V 主机。
   >[!NOTE] 
   > Windows Server 2016 来宾 VM; 上不支持远程桌面会话主机只有一个会话可以承载每个 Windows Server 2016 来宾 VM。

-   改进的应用程序兼容性和稳定性。
-   虚拟机连接增强会话模式，允许通过 VM 连接到启用 RemoteFX vgpu 的 VM 的 USB 和剪贴板重定向。

有关详细信息，请查看[设置的安装和配置 RemoteFX vGPU 远程桌面服务](rds-remotefx-vgpu.md)。

## <a name="which-should-you-use"></a>您将使用？

的虚拟化技术可能依赖于硬件规范或您的环境中的应用程序要求的关键注意事项。 下面是有关 DDA 和 RemoteFX vGPU 功能的简短表：

| 功能               | RemoteFX vGPU                                                                       | 离散设备分配                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| 设备 GPU 分配 | 半虚拟化 （多个 Vm 到一个或多个 Gpu)                                     | 到 1 个虚拟机的一个或多个 GPU                                                  |
| 比例                 | 最佳方式横向 / 1 对多个 Vm 的 GPU                                                      | 较低的规模 / 到 1 个虚拟机的一个或多个 Gpu                                     |
| 应用程序兼容性     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | 供应商 (DX 12，OpenGL，CUDA) 提供的所有 GPU 功能          |
| AVC444                | （Windows 10 和 Windows Server 2016） 的默认情况下启用                             | 可通过组策略 （Windows 10 和 Windows Server 2016）    |
| GPU VRAM              | 最多 1GB 专用 vram 能够                                                           | 最多由 GPU 支持的 vram 能够                                        |
| 帧速率            | 最多 30 fps                                                                         | 最多 60 fps                                                            |
| 在来宾中的 GPU 驱动程序   | RemoteFX 3D 适配器显示驱动程序 (Microsoft)                                      | GPU 供应商驱动程序 (Nvidia，AMD、 Intel)                                 |
| 来宾 OS 支持      |  Windows Server 2012 R2  Windows Server 2016  Windows 7 SP1  Windows 8.1 Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| 虚拟机监控程序            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| 主机 OS 可用性  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| GPU 的硬件          | 企业 Gpu （如 Nvidia Quadro/网格线或 AMD FirePro）                         | 企业 Gpu （如 Nvidia Quadro/网格线或 AMD FirePro）            |
| 服务器硬件       | 没有特殊的要求                                                             | 新式服务器公开 IOMMU 到 OS （通常是 SR-IOV 符合硬件） |

一般的经验法则是使用 DDA 最佳应用程序兼容性，因为虚拟机将具有对 GPU 的直接访问。 如果你的应用程序或工作负荷不是 GPU 要求严格的并且希望服务器更广的用户的基础，RemoteFX vGPU 可能更适合您。