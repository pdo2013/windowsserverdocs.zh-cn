---
title: 使用 RemoteFX vGPU 部署图形设备
description: 了解如何在 Windows Server 中部署和配置 RemoteFX vGPU
ms.prod: windows-server-threshold
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7111899557279d825191948e737d83d7467ce786
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923909"
---
# <a name="deploy-graphics-devices-using-remotefx-vgpu"></a>使用 RemoteFX vGPU 部署图形设备

> 适用于： Windows Server 2016、Microsoft Hyper-V Server 2016

使用 RemoteFX 的 vGPU 功能可以让多个虚拟机共享物理 GPU。 呈现和计算资源在虚拟机之间动态共享，这使得 RemoteFX vGPU 适用于不需要专用 GPU 资源的高突发负载。 例如，在 VDI 服务中，可以使用 RemoteFX vGPU 将应用呈现成本卸载到 GPU，同时降低 CPU 负载并提高服务可伸缩性。

## <a name="remotefx-vgpu-requirements"></a>RemoteFX vGPU 要求

主机系统要求：

- WIN ENT LTSB 2016 Finnish 64 Bits
- 与 WDDM 1.2 兼容的驱动程序的 DirectX 11.0 兼容 GPU
- 支持二级地址转换（SLAT）的 CPU

来宾 VM 要求：

- 支持的来宾操作系统。 有关详细信息，请参阅[RemoteFX 3D 视频适配器（vGPU）支持](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)。

来宾 VM 的其他注意事项：

- OpenGL 和 OpenCL 功能仅适用于运行 Windows 10 或 Windows Server 2016 的来宾。  
- DirectX 11.0 仅适用于运行 Windows 8 或更高版本的来宾。

## <a name="enable-remotefx-vgpu"></a>启用 RemoteFX vGPU

在 Windows Server 2016 主机上配置 RemoteFX vGPU：

1. 安装适用于 Windows Server 2016 的 GPU 供应商建议的图形驱动程序。
2. 创建运行 RemoteFX vGPU 支持的来宾操作系统的虚拟机。 若要了解详细信息，请参阅[RemoteFX 3D 视频适配器（vGPU）支持](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)。
3. 将 RemoteFX 3D 图形适配器添加到 VM。 若要了解详细信息，请参阅[配置 RemoteFX VGPU 3d 适配器](#configure-the-remotefx-vgpu-3d-adapter)。

默认情况下，RemoteFX vGPU 将使用所有可用且受支持的 Gpu。 若要限制 RemoteFX vGPU 使用的 Gpu，请遵循以下步骤：

1. 导航到 Hyp​​er-V 管理器中的 Hyper-V 设置。
2. 选择 "Hyper-v 设置" 中的**物理 gpu** 。
3. 选择不想使用的 GPU，然后清除“将此 GPU 用于 RemoteFX”。

### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>配置 RemoteFX vGPU 3D 适配器

可以使用 Hyper-V 管理器 UI 或 PowerShell cmdlet 来配置 RemoteFX vGPU 3D 图形适配器。

#### <a name="configure-remotefx-vgpu-with-hyper-v-manager"></a>通过 Hyper-v 管理器配置 RemoteFX vGPU

1. 如果 VM 当前正在运行，请停止它。
2. 打开 "Hyper-v 管理器"，导航到 " **VM 设置**"，然后选择 "**添加硬件**"。
3. 选择 " **RemoteFX 3D 图形适配器**"，然后选择 "**添加**"。
4. 设置最大监视器数、最大监视器分辨率和专用视频内存，或保留默认值。

   > [!NOTE]
   > - 为这些选项中的任何一个设置较高的值会影响服务规模，因此，只需设置所需的值。
   > - 如果需要使用 1 GB 专用 VRAM，请使用64位来宾 VM，而不是32位（x86）以获得最佳结果。

5. 选择 **"确定"** 完成配置。

#### <a name="configure-remotefx-vgpu-with-powershell-cmdlets"></a>通过 PowerShell cmdlet 配置 RemoteFX vGPU

使用以下 PowerShell cmdlet 来添加、查看和配置适配器：

- [VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/add-vmremotefx3dvideoadapter?view=win10-ps)
- [VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefx3dvideoadapter?view=win10-ps)
- [VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/set-vmremotefx3dvideoadapter?view=win10-ps)
- [VMRemoteFXPhysicalVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefxphysicalvideoadapter?view=win10-ps)

## <a name="monitor-performance"></a>监视性能

支持 RemoteFX vGPU 的服务的性能和规模由各种因素决定，如系统上的 Gpu 数、总的 GPU 内存、系统内存和内存速度、CPU 内核数和 CPU 时钟频率、存储速度和 NUMA部署.

### <a name="host-system-memory"></a>主机系统内存

对于使用 vGPU 启用的每个 VM，RemoteFX 在来宾操作系统和主机服务器中都使用系统内存。 虚拟机监控程序保证来宾操作系统的系统内存可用性。 在主机上，每个启用了 vGPU 的虚拟桌面都需要将其系统内存要求播发到虚拟机监控程序。 当启用了 vGPU 的虚拟机启动时，虚拟机监控程序会在主机中保留额外的系统内存。

启用 RemoteFX 的服务器的内存要求是动态的，因为启用了 RemoteFX 的服务器上使用的内存量取决于与启用了 vGPU 的虚拟桌面关联的监视器数量和最大分辨率这些监视器。

### <a name="host-gpu-video-memory"></a>主机 GPU 视频内存

每个启用了 vGPU 的虚拟桌面都使用主机服务器上的 GPU 硬件视频内存来呈现桌面。 此外，编解码器使用视频内存来压缩呈现的屏幕。 呈现和压缩所需的内存量直接基于预配到虚拟机的监视器数量。 保留的视频内存量因系统屏幕分辨率和监视器数量而异。 某些用户需要较高的屏幕分辨率来执行特定任务，但如果所有其他设置保持不变，则分辨率设置的伸缩性将更高。

### <a name="host-cpu"></a>主机 CPU

虚拟机监控程序计划 CPU 上的主机和 Vm。 启用 RemoteFX 的主机上的开销会增加，因为系统会对每个启用了 vGPU 的虚拟机运行附加的进程（rdvgm）。 此过程使用图形设备驱动程序在 GPU 上运行命令。 编解码器还使用 CPU 压缩需要发送回客户端的屏幕数据。

更多虚拟处理器意味着更好的用户体验。 建议为每个启用了 vGPU 的虚拟机分配至少两个虚拟 Cpu。 我们还建议为启用了 vGPU 的虚拟桌面使用 x64 体系结构，因为与 x86 虚拟机相比，x64 虚拟机上的性能更好。

### <a name="gpu-processing-power"></a>GPU 处理能力

每个启用了 vGPU 的虚拟桌面都有一个在主机服务器上运行的相应 DirectX 进程。 此过程会将其从 RemoteFX 虚拟桌面接收的所有图形命令重播到物理 GPU。 这就像在同一物理 GPU 上同时运行多个 DirectX 应用程序。

通常，图形设备和驱动程序会经过优化，以便一次只在桌面上运行几个应用程序，而 RemoteFX 会拉伸 Gpu 以进一步进行。 vGPUs 附带了性能计数器，用于测量 RemoteFX 请求的 GPU 响应，并有助于确保 Gpu 不会延伸到太远。

当 GPU 资源不足时，读取和写入操作需要很长时间才能完成。 管理员可以使用性能计数器来了解何时调整资源并防止用户停机。

详细了解在[远程桌面诊断图形性能问题](https://docs.microsoft.com/azure/virtual-desktop/remotefx-graphics-performance-counters)时用于监视 RemoteFX vGPU 行为的性能计数器。
