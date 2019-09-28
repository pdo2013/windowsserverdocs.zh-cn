---
title: 使用离散设备分配部署 NVMe 存储设备
description: 了解如何使用 DDA 部署存储设备
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: eb76b25e8ff1428b2c03b37dde1f76562751d3bb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364323"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>使用离散设备分配部署 NVMe 存储设备

>适用于：Microsoft Hyper-V Server 2016，Windows Server 2016

从 Windows Server 2016 开始，可以使用离散设备分配或 DDA 将整个 PCIe 设备传递到 VM。  这样，便可以对设备进行高性能的访问，例如从 VM 内 NVMe 存储或图形卡，同时能够利用设备本机驱动程序。  请访问[使用离散设备分配部署设备的计划](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)，以了解有关设备工作的详细信息、可能的安全含义，等等。使用带有 DDA 的设备有三个步骤：
-   将 VM 配置为 DDA
-   从主机分区卸载设备
-   将设备分配给来宾 VM

可在 Windows PowerShell 控制台上以管理员身份在主机上执行所有命令。

## <a name="configure-the-vm-for-dda"></a>将 VM 配置为 DDA
离散设备分配对 Vm 施加了一些限制，需要执行以下步骤。

1.  通过执行，将 VM 的 "自动停止操作" 配置为 TurnOff

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>从主机分区卸载设备

### <a name="locating-the-devices-location-path"></a>查找设备的位置路径
需要 PCI 位置路径才能从主机卸载和安装设备。  示例位置路径如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。   有关位置路径的更多详细信息，请参阅：[使用离散设备分配计划部署设备](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>禁用设备
使用设备管理器或 PowerShell，确保设备处于 "已禁用" 状态。  

### <a name="dismount-the-device"></a>卸除设备
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>将设备分配给来宾 VM
最后一步是告知 Hyper-v，VM 应具有对设备的访问权限。  除了上述位置路径以外，还需要知道 vm 的名称。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>后续操作
成功将设备装载到 VM 后，现在可以开始该 VM，并像平常一样与设备交互（如果在裸机系统上运行）。  可以通过打开来宾 VM 中的设备管理器来验证此项，并看到硬件现在显示。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>删除设备并将其返回到主机
如果要将设备恢复回其原始状态，需要停止 VM 并发出以下内容：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
然后，你可以在设备管理器中重新启用设备，主机操作系统将能够再次与设备交互。
