---
title: 用于配置的 HYPER-V Vm 的永久性内存设备 Cmdlet
description: 如何为 HYPER-V Vm 配置永久性内存设备
ms.prod: windows-server-threshold
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: fd1b04ce74f0b8d490529d2a7f65091f5847d0f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878178"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>用于配置的 HYPER-V Vm 的永久性内存设备 Cmdlet

>适用于：Windows Server 2019

本文提供有关使用永久内存 （也称为存储类内存或 NVDIMM） 配置的 HYPER-V Vm 的信息与系统管理员和 IT 专业人员。 JDEC 符合 NVDIMM N 永久性内存设备支持 Windows Server 2016 和 Windows 10 中，并提供对延迟非常低的非易失性设备字节级访问权限。 在 Windows Server 2019 支持 VM 的永久内存设备。 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>为 VM 创建的永久性内存设备

使用**[新建 VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)**  cmdlet 为 VM 创建的永久性内存设备。 设备必须在现有的 NTFS DAX 卷上创建。  新的文件扩展名 (.vhdpmem) 用于指定设备为永久性内存设备。 支持仅固定的 VHD 文件格式。

**示例：** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>创建具有永久性内存控制器的 VM



使用**NEW-VM cmdlet**以创建具有指定的内存大小和 VHDX 映像路径的第 2 代 VM。 然后，使用**添加 VMPmemController**将永久性内存控制器添加到 VM。

**示例：** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>附加到 VM 的永久性内存设备

使用**[Add-vmharddiskdrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** 将附加到 VM 的永久性内存设备

**示例：** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

中的 HYPER-V VM 的永久性内存设备显示为永久性内存设备来使用和管理的来宾操作系统。 来宾操作系统可用作将块或 DAX 卷的设备。 时作为 DAX 卷使用的虚拟机内的永久性内存设备，它们可以受益的主机设备 （没有 I/O 虚拟化的代码路径上） 的低延迟字节级地址的能力。 

>[!NOTE] 
>HYPER-V 第 2 代 Vm 仅支持持久的内存。 实时迁移和存储迁移不支持的 Vm 使用永久内存。 生产检查点的 Vm 不包括持久的内存状态。 