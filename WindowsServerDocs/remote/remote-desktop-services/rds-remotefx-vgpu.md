---
title: RDS - RemoteFX vGPU 的安装和配置
description: 用于配置 RemoteFX vGPU 图形虚拟化的规划信息。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/23/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0263fa6b-2185-4cc3-99ef-3588e2f4ada5
author: lizap
manager: scottman
ms.openlocfilehash: a216b84383e6edb3e0537189af5938eb5d62ce42
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387273"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>为远程桌面服务安装和配置 RemoteFX vGPU


RemoteFX 的 vGPU 功能使多个虚拟机可以共用一个物理图形适配器。 虚拟机能够将图形信息的绘制从处理器卸载到专用图形适配器。 这将减少 CPU 负载，提高在 VDI 虚拟机中运行的图形密集型工作负荷的可伸缩性。 

## <a name="remotefx-vgpu-requirements"></a>RemoteFX vGPU 要求

主机系统要求： 

- Windows Sever 2016 或 Windows 10
- 采用 WDDM 1.2 兼容驱动程序的 DX 11.0 兼容 GPU 
- 启用 Windows Server RD 虚拟化主机角色（启用 Hyper-V 角色） 
- 具有支持 SLAT（二级地址转换）的 CPU 的服务器 

来宾 VM 要求：

- 运行 Windows 企业版客户端（Windows 7 Service Pack 1、Windows 8.1、Windows 10）或 Windows Server（Windows Server 2012 R2 或 Windows Server 2016）的来宾 VM。 有关其他操作系统支持，请参阅[远程桌面服务支持的配置](rds-supported-config.md)。

来宾 VM 的其他注意事项：

- OpenGL 和 OpenCL 功能仅适用于 Windows 10 或 Windows Server 2016。  
- DirectX 11.0 仅适用于 Windows 8 或更高版本的来宾 VM。 
- 远程桌面会话主机只有在作为[个人会话桌面](rds-personal-session-desktops.md)运行时才受 RemoteFX vGPU 支持。

对于来宾 VM，请务必查看 [VDI 部署 - 受支持的来宾操作系统](rds-supported-config.md#vdi-deployment--supported-guest-oss)。

## <a name="install-remotefx-vgpu"></a>安装 RemoteFX vGPU

使用以下步骤在 Windows Server 2016 和 Windows 10 主机上安装并配置 RemoteFX：

1. 安装操作系统。
2. 安装图形卡供应商站点提供的最新 Windows 10/Windows Server 2016 GPU 驱动程序。
3. 在 Windows 10/Windows Server 2016 主机上安装 RemoteFX vGPU：
   1. 在 Windows 10 主机上，在控制面板中启用 Hyper-V 功能（转到“控制面板”/“程序和功能”/“打开或关闭 Windows 功能”）：

      ![用于启用 Hyper-V 功能的“Windows 功能”窗口](media/rds-hyperv-settings.png)

   2. 在 Windows Server 2016 主机上，安装远程桌面虚拟化主机 (RDVH) 角色。
   

4. 现在，创建并配置来宾 VM：
   1. 使用 Windows 10 企业版或 Windows Server 2016 创建 VM。
   2. 添加 RemoteFX 3D 图形适配器。 有关如何使用 Hyper-V 管理器或 PowerShell cmdlet 执行此操作的信息，请参阅[配置 RemoteFX vGPU 3D 适配器](#configure-the-remotefx-vgpu-3d-adapter)。 

当有多个 GPU 可用时，RemoteFX vGPU 将使用所有 GPU。 但是，在某些情况下，你可能希望限制 RemoteFX 使用哪些 GPU。 在 Hyper-V 环境中，可以通过专门选择 RemoteFX *不*应该使用哪些 GPU 来进行控制。 请使用以下步骤： 

   1. 导航到 Hyp​​er-V 管理器中的 Hyper-V 设置。
   2. 单击 Hyper-V 设置中的“物理 GPU”  。
   3. 选择不想使用的 GPU，然后清除“将此 GPU 用于 RemoteFX”  。


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>配置 RemoteFX vGPU 3D 适配器
可以使用 Hyper-V 管理器 UI 或 PowerShell cmdlet 来配置 RemoteFX vGPU 3D 图形适配器。 

#### <a name="through-hyper-v-manager"></a>通过 Hyper-V 管理器：

1. 确保系统安装了 Hyper-V 并配置了 VM。  
2. 如果 VM 正在运行，请将其停止。 
3. 在 Hyper-V 管理器中，导航到“VM 设置”，然后单击“添加硬件”   。
4. 选择“RemoteFX 3D 图形适配器”，单击“添加”   。 
5. 设置最大监视器数、最大监视器分辨率和专用视频内存，或保留默认值。

   > [!NOTE]
   > - 为这些选项中的任何一个设置较高的值都会对缩放产生影响，因此应当只设置绝对必要的值。
   >
   > - 如果需要使用 1GB 的专用 VRAM，请使用 64 位来宾 VM 而不是 32位 (x86)，以获得最佳效果。
6. 单击“确定”完成配置  。

#### <a name="with-powershell-cmdlets"></a>使用 PowerShell cmdlet：

运行以下 cmdlet 以添加、查看和配置适配器： 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

有关详细信息，请参阅 [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter)。

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

有关详细信息，请参阅 [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

有关详细信息，请参阅 [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter)。

运行以下 cmdlet 以查看物理 GPU：

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

有关详细信息，请参阅 [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter)。

## <a name="monitor-performance"></a>监视性能

VDI 系统的性能和规模由多种因素决定，例如 GPU 的总内存、系统内存和内存速度、CPU 内核数和 CPU 时钟频率、存储速度以及 NUMA 实现。

RDS 中的远程 vGPU 支持包括以下性能计数器，可以在性能监视器 (perfmon.exe) 中查看这些计数器，以收集有关帧速率吞吐量的信息。

- RemoteFX 图形 - 远程桌面协议图形压缩计数器。 例如，若要查看呈现给 RDP 以进行压缩的帧数，请查看“每秒输入帧数”计数器  。
- RemoteFX 网络 - 远程桌面协议网络流量计数器。 例如“往返时间(RTT)”  。
- RemoteFX 根 GPU 管理 - 测量 VRAM 可用内存和保留内存。
- RemoteFX 软件 - 提供用于捕获率、GPU 响应时间等项目的计数器。

对于更低级别的性能监视，尤其是故障排除，可以使用以下附加性能计数器：

- RemoteFX Synth3D VSC VM 设备 
- RemoteFX Synth3D VSC VM 传输通道 
- RemoteFX Synth3D VSP 
- RemoteFX Synth3D VSP VM 设备 
- RemoteFX Synth3D VSP VM 通道
