---
title: 部署使用离散设备分配的图形设备
description: 了解如何使用 DDA 部署 Windows Server 中的图形设备
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 6c528535fd34f57957a37992843933d4cd9f8824
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447868"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>部署使用离散设备分配的图形设备

>适用于：Microsoft HYPER-V Server 2016，Windows Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019  

从 Windows Server 2016 开始，你可以使用离散设备分配或 DDA，若要将整个 PCIe 设备传递到 VM。  这将允许对等设备的高性能访问[NVMe 存储](./Deploying-storage-devices-using-dda.md)或从 VM 同时能够利用设备的本机驱动程序中的图形卡。  请访问[规划部署的设备使用离散设备分配](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)有关哪些设备工作的更多详细信息，什么是可能的安全隐患，等等。

有三个步骤使用离散设备分配设备：
-   将 VM 配置为离散设备分配
-   卸除主分区的设备
-   将设备分配给来宾 VM

以管理员身份，可以在 Windows PowerShell 控制台上的主机上执行所有命令。

## <a name="configure-the-vm-for-dda"></a>将 VM 配置为 DDA
离散设备分配都有一些限制到 Vm 并不需要执行以下步骤。

1.  通过执行配置"自动停止操作"的 VM 到 TurnOff

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>一些额外的虚拟机准备工作是所必需的图形设备

如果以某种方式中的 VM 配置，某些硬件更好地执行。  有关需要以下配置为你的硬件的详细信息，请联系硬件供应商联系。 可以在上找到其他详细信息[规划部署的设备使用离散设备分配](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)和此[博客文章。](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)

1. 启用写组合在 CPU 上
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. 配置的 32 位 MMIO 空间
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. 配置大于 32 位 MMIO 空间
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   请注意，上面的 MMIO 空间值是合理的值来对单个 GPU 进行试验。  如果在启动 VM 后, 设备报告为没有足够的资源相关的错误，可能需要修改这些值。  此外，如果您要分配多个 Gpu，需要增加这些值。

## <a name="dismount-the-device-from-the-host-partition"></a>卸除主分区的设备
### <a name="optional---install-the-partitioning-driver"></a>可选-安装分区驱动程序
离散设备分配提供硬件供应商协作以与设备提供安全缓解驱动程序的功能。  请注意此驱动程序不是将在来宾 VM 中安装的设备驱动程序相同。  它具有由硬件供应商的决定，以便提供此驱动程序，但是，如果它们提供它，请安装它之前为从主分区中卸除该设备。  请联系硬件供应商的详细信息为如果它们具有缓解驱动程序
> 如果不提供任何分区驱动程序，则在卸载期间必须使用`-force`选项以跳过安全警告。 请阅读更多有关执行此操作上的安全隐患[规划部署的设备使用离散设备分配](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="locating-the-devices-location-path"></a>定位设备的位置路径
PCI 位置路径需要卸载并从主机装入设备。  示例位置路径看起来如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。  更多详细信息所在的位置路径可以在此处找到：[规划部署的设备使用离散设备分配](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>禁用设备
使用设备管理器或 PowerShell，请确保该设备为"disabled"。  

### <a name="dismount-the-device"></a>卸载设备
具体取决于供应商提供了缓解驱动程序，如果您将需要使用"-force"选项或不。
- 如果已安装缓解驱动程序
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- 如果未安装缓解驱动程序
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>将设备分配给来宾 VM
最后一步是告知 HYPER-V VM 应该有权访问设备。  除了上面找到的位置路径，将需要知道的 vm 的名称。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>什么是下一步
现在，已成功在 VM 中装载设备后，你可以启动该虚拟机，并像通常那样就像在裸机系统上运行，与设备交互。  这意味着您现在可以在 VM 中安装的硬件供应商的驱动程序和应用程序将能够看到存在该硬件。  可以通过在来宾 VM 中打开设备管理器并查看硬件现在显示对此进行验证。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>删除设备并将其返回到主机
如果你想要返回他恢复到其原始状态的设备，你需要停止 VM，并发出以下：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
您可以重新启用设备管理器中的设备和主机操作系统将能够与设备交互。

## <a name="examples"></a>示例

### <a name="mounting-a-gpu-to-a-vm"></a>装载到 VM 的 GPU
在此示例中我们使用 PowerShell 配置名为"ddatest1"按制造商 NVIDIA 的第一个 GPU 并将其分配到 VM 的 VM。  
```
#Configure the VM for a Discrete Device Assignment
$vm =   "ddatest1"
#Set automatic stop action to TurnOff
Set-VM -Name $vm -AutomaticStopAction TurnOff
#Enable Write-Combining on the CPU
Set-VM -GuestControlledCacheTypes $true -VMName $vm
#Configure 32 bit MMIO space
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
#Configure Greater than 32 bit MMIO space
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm

#Find the Location Path and disable the Device
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
#Disable the PNP Device
Disable-PnpDevice  -InstanceId $gpudevs[0].InstanceId

#Dismount the Device from the Host
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath

#Assign the device to the guest VM.
Add-VMAssignableDevice -LocationPath $locationPath -VMName $vm
```
