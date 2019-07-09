---
title: RDS - 哪种图形虚拟化技术适合你？
description: 规划信息以帮你为 RDS 部署选择正确的图形虚拟化选项。
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
ms.openlocfilehash: af5d5ce89561c89d8468627e20dfdb6f35eca5ef
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66447110"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>哪种图形虚拟化技术适合你？

在远程桌面服务中启用图形渲染时，你有一系列选项可用。 在规划虚拟化环境时，以下注意事项将促使你选择哪种图形渲染技术：

![图形渲染注意事项 - 比较用户规模和 GPU 要求，以确定适合环境的最佳 GPU 技术](media/rds-gpu.png)

在 Windows Server 2016 中，有两种与 Hyper-V 一起提供的图形虚拟化技术，让你可以利用 GPU 硬件：

- [离散设备分配 (DDA)](#discrete-device-assignment) - 使用一个或多个专用于 VM 的 GPU 实现最高性能，同时在 VM 内提供本机 GPU 驱动程序支持。 密度较低，因为它受到服务器上可用的物理 GPU 数量的限制。 
- [远程 FX vGPU](#remotefx-vgpu) - 适用于知识工作者和高突发 GPU 场景，其中多个 VM 通过半虚拟化利用一个或多个 GPU。 此解决方案提供更高的每服务器用户密度。

下图显示了 Windows Server 2016 中的图形虚拟化选项。

![带 RDS 的 Windows Server 2016 中的图形虚拟化选项 - 显示了可用的三种技术以及它们在规模和性能方面的差异](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>离散设备分配
离散设备分配 (DDA) 是一个提供最佳性能的硬件传递解决方案，假定 VM 可使用本机驱动程序完全访问 GPU。 VM 用户可以访问其设备的所有功能和本机驱动程序。 这意味着在 VM 镜像中运行设备的特性和功能与在裸机上运行相同设备的一样。

有关 DDA 的详细信息，请查看[部署离散设备分配的计划](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
RemoteFX vGPU 是一种图形虚拟化技术，允许将 GPU 的处理能力拆分到各种来宾操作系统中，以实现知识工作者场景（见上面的第一张图）。 Windows Server 2016 的改进允许进一步增强 GPU 突发场景，例如设计器应用程序和数据可视化效果。 其他功能的改进包括：

- 支持第 2 代来宾 VM、Windows Server 2016 来宾 VM 和 Windows 客户端 Hyper-V 主机。
  >[!NOTE] 
  > Windows Server 2016 来宾 VM 不支持远程桌面会话主机；每个 Windows Server 2016 来宾 VM 只能托管 1 个会话。

- 改进了应用程序兼容性和稳定性。
- VM 连接增强会话模式，允许通过 VM 连接将 USB 和剪贴板重定向到为 RemoteFX vGPU 启用的 VM。

有关详细信息，请查看[为远程桌面服务设置和配置 RemoteFX vGPU](rds-remotefx-vgpu.md)。

## <a name="which-should-you-use"></a>你应使用哪一种？

关于哪种虚拟化技术可能取决于环境中的硬件规范或应用程序要求的关键注意事项。 下面是有关 DDA 和 RemoteFX vGPU 功能的简短表：

| 功能               | RemoteFX vGPU                                                                       | 离散设备分配                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| 设备 GPU 分配 | 半虚拟化（多个 VM 对一个或多个 GPU）                                     | 1 个或多个 GPU 对 1 个 VM                                                  |
| 比例                 | 最佳比例/1 个 GPU 对多个 VM                                                      | 较低比例/1 个或多个 GPU 对 1 个 VM                                     |
| 应用兼容性     | DX 11.1、OpenGL 4.4、OpenCL 1.1                                                     | 供应商（DX 12、OpenGL、CUDA）提供的所有 GPU 功能          |
| AVC444                | 默认启用（Windows 10 和 Windows Server 2016）                             | 可通过组策略使用（Windows 10 和 Windows Server 2016）    |
| GPU VRAM              | 最多 1 GB 专用 VRAM                                                           | GPU 支持的 VRAM                                        |
| 帧速率            | 最高 30 fps                                                                         | 最高 60 fps                                                            |
| 来宾中的 GPU 驱动程序   | RemoteFX 3D 适配器显示驱动程序 (Microsoft)                                      | GPU 供应商驱动程序（Nvidia、AMD、Intel）                                 |
| 来宾操作系统支持      |  Windows Server 2012 R2  Windows Server 2016  Windows 7 SP1  Windows 8.1 Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| 虚拟机监控程序            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| 主机操作系统高可用性  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| GPU 硬件          | 企业 GPU（如 Nvidia Quadro/GRID 或 AMD FirePro）                         | 企业 GPU（如 Nvidia Quadro/GRID 或 AMD FirePro）            |
| 服务器硬件       | 无特殊要求                                                             | 新式服务器，向操作系统公开 IOMMU（通常是 SR-IOV 兼容硬件） |

一般的经验法则是使用 DDA 以获得最佳的应用程序兼容性，因为虚拟机可以直接访问 GPU。 如果你的应用程序或工作负载没有严格的 GPU 要求，并且你希望为更广泛的用户群提供服务，则 RemoteFX vGPU 可能最适合你。