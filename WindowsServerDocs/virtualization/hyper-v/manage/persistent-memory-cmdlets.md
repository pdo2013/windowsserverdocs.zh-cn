---
title: 用于为 Hyper-v Vm 配置永久性内存设备的 cmdlet
description: 如何为 Hyper-v Vm 配置永久性内存设备
ms.prod: windows-server
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: ecae1fe96bc5088fa840c6e2e24a75bb72a9e8f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392540"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>用于为 Hyper-v Vm 配置永久性内存设备的 cmdlet

>适用于：Windows Server 2019

本文为系统管理员和 IT 专业人员提供有关配置具有永久性内存（亦即存储类内存或 NVDIMM）的 Hyper-v Vm 的信息。 Windows Server 2016 和 Windows 10 支持 JDEC 兼容的 NVDIMM-N 永久性内存设备，并提供对非常低延迟非易失性设备的字节级别访问。 Windows Server 2019 支持 VM 永久性内存设备。 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>为 VM 创建永久性内存设备

使用 **[新的 VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** CMDLET 为 VM 创建永久性内存设备。 设备必须在现有 NTFS DAX 卷上创建。  新的文件扩展名（. vhdpmem）用于指定设备是永久性内存设备。 仅支持固定 VHD 文件格式。

示例：`New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>使用持久性内存控制器创建 VM



使用**新的 VM cmdlet**创建具有指定内存大小和 VHDX 映像路径的第2代 VM。 然后，使用**VMPmemController**将永久性内存控制器添加到 VM。

**示例：** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>将永久性内存设备附加到 VM

使用 **[add-vmharddiskdrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** 将永久性内存设备附加到 VM

示例：`Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Hyper-v VM 中的持久性内存设备显示为永久性内存设备，由来宾操作系统使用和管理。 来宾操作系统可以将设备用作块或 DAX 卷。 当 VM 中的永久性内存设备用作 DAX 卷时，它们将受益于主机设备的低延迟字节级别地址（在代码路径上没有 i/o 虚拟化）。 

>[!NOTE] 
>永久性内存仅支持 Hyper-v Gen2 Vm。 对于具有永久性内存的 Vm，不支持实时迁移和存储迁移。 Vm 的生产检查点不包括永久性内存状态。 