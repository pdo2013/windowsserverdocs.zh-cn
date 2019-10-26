---
title: 使用离散设备分配部署图形设备
description: 了解如何使用 DDA 在 Windows Server 中部署图形设备
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.date: 08/21/2019
ms.openlocfilehash: 5466cecf9f11a53dc6e205f36d50d7b27b310ea1
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923880"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>使用离散设备分配部署图形设备

> 适用于： Microsoft Hyper-V Server 2016、Windows Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019  

从 Windows Server 2016 开始，可以使用离散设备分配或 DDA 将整个 PCIe 设备传递到 VM。  这样，便可以对设备进行高性能的访问，例如从 VM 内[NVMe 存储](./Deploying-storage-devices-using-dda.md)或图形卡，同时能够利用设备本机驱动程序。  请访问[使用离散设备分配部署设备的计划](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)，以了解有关设备工作的详细信息、可能的安全含义，等等。

使用具有离散设备分配的设备有三个步骤：
-   配置虚拟机以进行离散设备分配
-   从主机分区卸载设备
-   将设备分配给来宾 VM

可在 Windows PowerShell 控制台上以管理员身份在主机上执行所有命令。

## <a name="configure-the-vm-for-dda"></a>将 VM 配置为 DDA
离散设备分配对 Vm 施加了一些限制，需要执行以下步骤。

1.  通过执行，将 VM 的 "自动停止操作" 配置为 TurnOff

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>图形设备需要一些额外的 VM 准备工作

如果以特定方式配置了 VM，某些硬件的性能会更好。  有关是否需要为硬件执行以下配置的详细信息，请与硬件供应商联系。 有关详细信息，请参阅[使用离散设备分配部署设备](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)和此[博客文章。](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266)

1. 对 CPU 启用写入合并
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. 配置32位 MMIO 空间
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. 配置大于32位 MMIO 空间
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   > [!TIP] 
   > 以上 MMIO space 值是要设置为使用单个 GPU 进行试验的合理值。  如果启动 VM 后，设备会报告与资源不足相关的错误，则可能需要修改这些值。 若要了解如何精确计算 MMIO 要求，请参阅[使用离散设备分配部署设备的计划](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

## <a name="dismount-the-device-from-the-host-partition"></a>从主机分区卸载设备
### <a name="optional---install-the-partitioning-driver"></a>可选-安装分区驱动程序
"离散设备分配" 提供硬件 venders 为其设备提供安全缓解驱动程序的能力。  请注意，此驱动程序与将在来宾 VM 中安装的设备驱动程序不同。  它由硬件供应商自行决定是否提供此驱动程序，但是，如果它们提供此驱动程序，请在从主机分区卸载设备之前安装它。  请与硬件供应商联系，以了解有关是否具有缓解驱动程序的详细信息
> 如果未提供分区驱动程序，则在卸载过程中，必须使用 `-force` 选项来绕过安全警告。 有关[使用离散设备分配部署设备的计划](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)，请阅读有关如何执行此操作的详细信息。

### <a name="locating-the-devices-location-path"></a>查找设备的位置路径
需要 PCI 位置路径才能从主机卸载和安装设备。  示例位置路径如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。  可在此处找到有关位置路径的更多详细信息：[计划使用离散设备分配部署设备](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>禁用设备
使用设备管理器或 PowerShell，确保设备处于 "已禁用" 状态。  

### <a name="dismount-the-device"></a>卸除设备
根据供应商是否提供缓解驱动程序，你需要使用 "-force" 选项。
- 如果安装了缓解驱动程序
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- 如果未安装缓解驱动程序
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>将设备分配给来宾 VM
最后一步是告知 Hyper-v，VM 应具有对设备的访问权限。  除了上述位置路径以外，还需要知道 vm 的名称。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>后续操作
成功将设备装载到 VM 后，现在可以开始该 VM，并像平常一样与设备交互（如果在裸机系统上运行）。  这意味着你现在可以在 VM 中安装硬件供应商的驱动程序，并且应用程序将能够看到该硬件存在。  可以通过打开来宾 VM 中的设备管理器来验证此项，并看到硬件现在显示。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>删除设备并将其返回到主机
如果要将设备恢复回其原始状态，需要停止 VM 并发出以下内容：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
然后，你可以在设备管理器中重新启用设备，主机操作系统将能够再次与设备交互。

## <a name="example"></a>示例

### <a name="mounting-a-gpu-to-a-vm"></a>将 GPU 装载到 VM
在此示例中，我们使用 PowerShell 配置名为 "ddatest1" 的 VM，以获取制造商 NVIDIA 提供的第一个 GPU，并将其分配给 VM。  
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

## <a name="troubleshooting"></a>“疑难解答”

如果已将 GPU 传递到 VM，但远程桌面或应用程序未识别 GPU，请检查是否存在以下常见问题：

- 请确保已安装最新版本的 GPU 供应商支持的驱动程序，并且该驱动程序不会通过检查中的设备状态设备管理器来报告错误。
- 确保设备在 VM 内分配了足够的 MMIO 空间。 若要了解详细信息，请参阅[MMIO Space](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md#mmio-space)。
- 请确保正在使用供应商支持在此配置中使用的 GPU。 例如，某些供应商在传递到 VM 后，阻止其使用者卡工作。
- 请确保正在运行的应用程序支持在 VM 内运行，并且该应用程序支持 GPU 及其相关的驱动程序。 某些应用程序具有 Gpu 和环境的允许列表。
- 如果你使用的是来宾上的远程桌面会话主机角色或 Windows Multipoint 服务，则需要确保将特定组策略条目设置为允许使用默认 GPU。 使用应用于来宾的组策略对象（或来宾上的本地组策略编辑器），导航到以下组策略项： **Windows 组件** > 的**计算机配置** > **管理员模板** > **远程桌面服务** > **远程桌面会话主机** > **远程会话环境** > **对所有远程桌面服务会话使用硬件默认图形适配器**。 将此值设置为 "已启用"，然后在应用策略后重新启动 VM。
