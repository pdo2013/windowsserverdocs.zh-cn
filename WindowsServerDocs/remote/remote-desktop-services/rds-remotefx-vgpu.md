---
title: RDS 的 RemoteFX vGPU 安装和配置
description: 若要配置 RemoteFX vGPU 图形虚拟化的规划信息。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3e7da1a70826dc720a96ceb3fe5d04868943f163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876838"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>为远程桌面服务设置和配置 RemoteFX vGPU


RemoteFX vGPU 功能使多个虚拟机共享的物理图形适配器。 虚拟机将无法卸载从处理器到专用的图形适配器的图形信息的呈现。 这将降低 CPU 负载并提高图形密集型工作负荷在 VDI 虚拟机中运行的可伸缩性。 

## <a name="remotefx-vgpu-requirements"></a>RemoteFX vGPU 要求

主机系统的要求： 

- Windows Server 2016 或 Windows 10
- DX 11.0 兼容 GPU 使用 WDDM 1.2 兼容驱动程序 
- 已启用 Windows Server RD 虚拟化主机角色 （启用 HYPER-V 角色） 
- 服务器的 CPU 支持 SLAT （第二级地址转换） 

来宾 VM 的要求：

- 来宾 VM 运行的 Windows 企业版客户端 (使用 Service Pack 1、 Windows 8.1、 Windows 10 的 Windows 7) 或 Windows Server （Windows Server 2012 R2 或 Windows Server 2016）。 对于其他 OS 支持，请参阅[远程桌面服务的支持配置](rds-supported-config.md)。

来宾 Vm 的其他注意事项：

- OpenGL 和 OpenCL 功能才可在 Windows 10 或 Windows Server 2016 中。  
- DirectX 11.0 才适用于 Windows 8 或更高版本的来宾虚拟机。 
- 如果它正在作为 RemoteFX vGPU 仅支持远程桌面会话主机[个人会话桌面](rds-personal-session-desktops.md)。

对于来宾虚拟机，请确保查看[VDI 部署的受支持的来宾 Os](rds-supported-config.md#vdi-deployment--supported-guest-oss)。

## <a name="install-remotefx-vgpu"></a>安装 RemoteFX vGPU

使用以下步骤来安装和适用于 Windows Server 2016 和 Windows 10 主机上配置 RemoteFX:

1. 安装操作系统。
2. 从图形卡供应商站点安装最新 Windows 10/windows Server 2016 GPU 驱动程序可用。
3. 在 Windows 10/Windows Server 2016 主机上安装 RemoteFX vGPU:
   1. Windows 10 主机上启用控制面板 （转到控制面板/程序和功能/关闭 Windows 功能打开或关闭） 中的 HYPER-V 功能：

      ![若要启用的 HYPER-V 功能的 Windows 功能窗口](media/rds-hyperv-settings.png)

   2. 在 Windows Server 2016 主机上安装远程桌面虚拟化主机 (RDVH) 角色。
   

4. 现在，创建并配置来宾 VM:
   1. 创建具有 Windows 10 企业版或 Windows Server 2016 的 VM。
   2. 添加的 RemoteFX 3D 图形适配器。 请参阅[配置 RemoteFX vGPU 3D 适配器](#configure-the-remotefx-vgpu-3d-adapter)有关如何使用 Hyper-v 管理器或 PowerShell cmdlet 执行该操作的信息。 

RemoteFX vGPU 有多个可用时将使用所有 Gpu。 但是，在某些情况下，你可能想要限制的 Gpu 使用 RemoteFX。 在 HYPER-V 环境中，您对此进行控制专门选择的 Gpu 应*不*由 RemoteFX。 请使用以下步骤： 

   1. 导航到在 Hyper-v 管理器中的 HYPER-V 设置。
   2. 单击**物理 Gpu**的 HYPER-V 设置中。
   3. 选择不想要使用，，然后清除 GPU**与 RemoteFX 一起使用此 GPU**。


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>配置 RemoteFX vGPU 3D 适配器
可以使用 HYPER-V 管理器 UI 或 PowerShell cmdlet 来配置 RemoteFX vGPU 3D 图形适配器。 

#### <a name="through-hyper-v-manager"></a>通过 HYPER-V 管理器：

1. 请确保含 HYPER-V 设置系统和配置 VM。  
2. 停止 VM，如果它正在运行。 
3. 在 Hyper-v 管理器导航到**VM 设置**，然后单击**添加硬件**。
4. 选择**RemoteFX 3D 图形适配器**，然后单击**添加**。 
5. 设置监视器、 最大监视器分辨率和专用视频内存的最大数目，或保留默认值。

   > [!NOTE]
   > - 设置这些选项的任何更高的值会影响要进行缩放，因此您应该只设置什么是绝对必要。
   >
   > - 当您需要使用 1 GB 的专用 vram 能够时，为获得最佳结果而不是 32 位 (x86) 使用 64 位来宾 VM。
6. 单击**确定**以完成配置。

#### <a name="with-powershell-cmdlets"></a>使用 PowerShell cmdlet:

运行以下 cmdlet 以添加、 查看并配置适配器： 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

有关详细信息，请参阅[Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter)。

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

有关详细信息，请参阅[Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

有关详细信息，请参阅[Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter)。

运行以下 cmdlet，以查看物理 Gpu:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

有关详细信息，请参阅[Get-vmremotefxphysicalvideoadapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter)。

## <a name="monitor-performance"></a>监视性能

性能和规模的 VDI 系统由许多因素，如 GPU 的总内存的系统内存和内存速度、 CPU 核心数和 CPU 时钟频率、 存储速度和 NUMA 实现数量决定。

在 RDS 的远程 vGPU 支持包括以下性能计数器，可以查看性能监视器 (perfmon.exe) 来收集有关帧速率吞吐量信息中。

- RemoteFX 图形-远程桌面协议图形压缩的计数器。 例如，如果你想要查看显示通过 rdp 连接进行压缩的帧数，看看**输入 Frames/Second**计数器。
- RemoteFX 网络的远程桌面协议的网络流量的计数器。 例如，**往返时间 (RTT)**。
- RemoteFX 根 GPU 管理-vram 能够可用且已保留的度量值。
- RemoteFX 软件-提供的捕获速度、 GPU 响应时间和其他人的计数器。

对于更低级别性能监视，特别是对于故障排除，可以使用以下的其他性能计数器：

- RemoteFX Synth3D VSC VM 设备 
- RemoteFX Synth3D VSC VM 传输通道 
- RemoteFX Synth3D VSP 
- RemoteFX Synth3D VSP VM 设备 
- RemoteFX Synth3D VSP VM 通道
