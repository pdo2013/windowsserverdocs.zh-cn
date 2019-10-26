---
title: 在 Windows Server 中规划 GPU 加速
description: 了解 GPU 加速的不同 Hyper-v 技术，包括 DDA 和 RemoteFX vGPU
ms.prod: windows-server-threshold
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: f62357de1ab167d0a6be4eb63b9d6d23bfac7371
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923899"
---
# <a name="plan-for-gpu-acceleration-in-windows-server"></a>在 Windows Server 中规划 GPU 加速

> 适用于： Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

本文介绍 Windows Server 中提供的图形虚拟化功能。

## <a name="when-to-use-gpu-acceleration"></a>何时使用 GPU 加速

根据工作负荷，可能需要考虑 GPU 加速。 下面是选择 GPU 加速之前应考虑的事项：

- **应用和桌面远程处理（VDI/DaaS）工作负载**：如果你要使用 Windows Server 生成应用或桌面远程处理服务，请考虑你希望用户运行的应用的目录。 某些类型的应用（例如 CAD/CAM 应用、模拟应用、游戏和渲染/可视化应用）很大程度上依赖于三维渲染，以提供平稳、快速的交互功能。 大多数客户将 Gpu 视为具有这些类型的应用的合理用户体验的必要条件。
- **远程呈现、编码和可视化工作负荷**：这些面向图形的工作负荷往往非常依赖 GPU 的专用功能，如高效的3d 渲染和帧编码/解码，以便实现成本效益和吞吐量目标。 对于这种类型的工作负荷，单个启用 GPU 的 VM 可能会与多个仅 CPU Vm 的吞吐量相匹配。
- **HPC 和 ML 工作负荷**：对于高度数据并行计算工作负荷（例如高性能计算和机器学习模型定型或推理），gpu 可显著缩短生成时间、推断时间和定型时间。 或者，它们可能比仅 CPU 体系结构的性能级别更具成本效益。 许多 HPC 和机器学习框架都有启用 GPU 加速的选项;考虑这是否会使你的特定工作负荷受益。

## <a name="gpu-virtualization-in-windows-server"></a>Windows Server 中的 GPU 虚拟化

GPU 虚拟化技术在虚拟化环境（通常在虚拟机中）中启用 GPU 加速。 如果你的工作负荷是使用 Hyper-v 虚拟化的，则需要使用图形虚拟化来提供从物理 GPU 到虚拟化应用或服务的 GPU 加速。 但是，如果你的工作负荷直接在物理 Windows Server 主机上运行，则无需使用图形虚拟化;你的应用和服务已经有权访问 Windows Server 中本机支持的 GPU 功能和 Api。

Windows Server 中的 Hyper-v Vm 可使用以下图形虚拟化技术：

- [离散设备分配（DDA）](#discrete-device-assignment-dda)
- [RemoteFX vGPU](#remotefx-vgpu)

除了 VM 工作负载，Windows Server 还支持 Windows 容器中容器化工作负荷的 GPU 加速。 有关详细信息，请参阅[Windows 容器中的 GPU 加速](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/gpu-acceleration)。

## <a name="discrete-device-assignment-dda"></a>离散设备分配（DDA）

离散设备分配（DDA）也称为 GPU 直通，使你能够将一个或多个物理 Gpu 专用于虚拟机。 在 DDA 部署中，虚拟化工作负荷运行在本机驱动程序上，并且通常具有对 GPU 功能的完全访问权限。 DDA 提供最高级别的应用程序兼容性和潜在性能。 DDA 还可以向 Linux Vm 提供 GPU 加速，受支持。

DDA 部署只能加速有限数量的虚拟机，因为每个物理 GPU 最多可为一个 VM 提供加速。 如果要开发的服务的体系结构支持共享的虚拟机，请考虑为每个 VM 托管多个加速的工作负荷。 例如，如果要使用 RDS 构建桌面远程处理服务，则可以利用 Windows Server 的多会话功能在每个 VM 上托管多个用户桌面，从而提高用户规模。 这些用户将分享 GPU 加速的优点。

有关详细信息，请参阅以下主题：

- [规划单独的设备分配部署](plan-for-deploying-devices-using-discrete-device-assignment.md)
- [使用离散设备分配部署图形设备](../deploy/Deploying-graphics-devices-using-dda.md)

## <a name="remotefx-vgpu"></a>RemoteFX vGPU

> [!NOTE]
> Windows Server 2016 完全支持 RemoteFX vGPU，但在 Windows Server 2019 中不受支持。

RemoteFX vGPU 是一种图形虚拟化技术，允许在多个虚拟机之间共享单个物理 GPU。 在 RemoteFX vGPU 部署中，虚拟化的工作负荷运行在 Microsoft 的 RemoteFX 3D 适配器上，用于协调主机和来宾之间的 GPU 处理请求。 RemoteFX vGPU 最适用于不需要专用 GPU 资源的知识工作者和高突发工作负载。 RemoteFX vGPU 只能向 Windows Vm 提供 GPU 加速。

有关详细信息，请参阅以下主题：

- [使用 RemoteFX vGPU 部署图形设备](../deploy/deploy-graphics-devices-using-remotefx-vgpu.md)
- [支持 RemoteFX 3D 视频适配器（vGPU）](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)

## <a name="comparing-dda-and-remotefx-vgpu"></a>比较 DDA 和 RemoteFX vGPU

规划部署时，请考虑以下功能并支持图形虚拟化技术之间的差异：

|                       | RemoteFX vGPU                                                                       | 离散设备分配                                                          |
|-----------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| GPU 资源模型    | 专用或共享                                                                 | 仅专用                                                                      |
| VM 密度            | 高（多个 Vm 的一个或多个 Gpu）                                                 | Low （一个或多个 Gpu 到一个 VM）                                                    |
| 应用兼容性     | DX 11.1、OpenGL 4.4、OpenCL 1.1                                                     | 供应商（DX 12、OpenGL、CUDA）提供的所有 GPU 功能                       |
| AVC444                | 默认情况下启用                                                                  | 通过组策略提供                                                      |
| GPU VRAM              | 最多 1 GB 专用 VRAM                                                           | GPU 支持的 VRAM                                                     |
| 帧速率            | 最高 30 fps                                                                         | 最高 60 fps                                                                         |
| 来宾中的 GPU 驱动程序   | RemoteFX 3D 适配器显示驱动程序 (Microsoft)                                      | GPU 供应商驱动程序（NVIDIA、AMD、Intel）                                              |
| 主机操作系统支持       | WIN ENT LTSB 2016 Finnish 64 Bits                                                                 | Windows Server 2016;Windows Server 2019                                            |
| 来宾操作系统支持      | Windows Server 2012 R2;Windows Server 2016;Windows 7 SP1;Windows 8.1;Windows 10 | Windows Server 2012 R2;Windows Server 2016;Windows Server 2019;Windows 10;Linux |
| 虚拟机监控程序            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                                   |
| GPU 硬件          | 企业 GPU（如 Nvidia Quadro/GRID 或 AMD FirePro）                         | 企业 GPU（如 Nvidia Quadro/GRID 或 AMD FirePro）                         |
| 服务器硬件       | 无特殊要求                                                             | 新式服务器，向操作系统公开 IOMMU（通常是 SR-IOV 兼容硬件）              |
