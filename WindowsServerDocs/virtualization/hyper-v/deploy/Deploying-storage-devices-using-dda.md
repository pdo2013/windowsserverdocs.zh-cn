---
title: 部署使用离散设备分配 NVMe 存储设备
description: 了解如何使用 DDA 部署存储设备
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: d6fe54789d37386d5dc782ef8a2ca26b47adc69e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841358"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>部署使用离散设备分配 NVMe 存储设备

>适用于：Microsoft 的 HYPER-V Server 2016 中，Windows Server 2016

从 Windows Server 2016 开始，你可以使用离散设备分配或 DDA，若要将整个 PCIe 设备传递到 VM。  这将同时能够利用设备的本机驱动程序允许设备 NVMe 存储或从 VM 中的图形卡等高性能访问。  请访问[规划部署的设备使用离散设备分配](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)有关哪些设备工作的更多详细信息，什么是可能的安全隐患，等等。有三个步骤通过 DDA 使用设备：
-   将 VM 配置为 DDA
-   卸除主分区的设备
-   将设备分配给来宾 VM

以管理员身份，可以在 Windows PowerShell 控制台上的主机上执行所有命令。

## <a name="configure-the-vm-for-dda"></a>将 VM 配置为 DDA
离散设备分配都有一些限制到 Vm 并不需要执行以下步骤。

1.  通过执行配置"自动停止操作"的 VM 到 TurnOff

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>卸除主分区的设备

### <a name="locating-the-devices-location-path"></a>定位设备的位置路径
PCI 位置路径需要卸载并从主机装入设备。  示例位置路径看起来如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。   更多详细信息所在的位置路径可以在此处找到：[规划部署的设备使用离散设备分配](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>禁用设备
使用设备管理器或 PowerShell，请确保该设备为"disabled"。  

### <a name="dismount-the-device"></a>卸载设备
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>将设备分配给来宾 VM
最后一步是告知 HYPER-V VM 应该有权访问设备。  除了上面找到的位置路径，将需要知道的 vm 的名称。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>什么是下一步
现在，已成功在 VM 中装载设备后，你可以启动该虚拟机，并像通常那样就像在裸机系统上运行，与设备交互。  可以通过在来宾 VM 中打开设备管理器并查看硬件现在显示对此进行验证。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>删除设备并将其返回到主机
如果你想要返回他恢复到其原始状态的设备，你需要停止 VM，并发出以下：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
您可以重新启用设备管理器中的设备和主机操作系统将能够与设备交互。
