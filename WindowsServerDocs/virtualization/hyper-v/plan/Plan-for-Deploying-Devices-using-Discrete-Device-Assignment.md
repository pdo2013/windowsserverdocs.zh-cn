---
title: 计划部署使用离散设备分配设备
description: 了解如何在 Windows Server 中的 DDA 工作原理
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: c64c2b75c00f97622278c3e590db46995e108218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840198"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>规划部署的设备使用离散设备分配
>适用于：Microsoft HYPER-V Server 2016，Windows Server 2016 中，Microsoft HYPER-V Server 2019，Windows Server 2019

离散设备分配允许物理 PCIe 硬件可直接从虚拟机中。  本指南介绍了设备可以使用离散设备分配，主机系统要求的虚拟机以及安全隐患的离散设备分配施加限制的类型。

对于离散设备分配的初始版本，我们重点介绍两个设备必须由 Microsoft 正式支持：图形适配器和 NVMe 存储设备。  其他设备可能起作用，硬件供应商能够提供对这些设备的支持的语句。  对于这些设备，请联系支持这些硬件供应商。

如果你已准备好试用离散设备分配，你可以为跳过[部署图形设备使用离散设备分配](../deploy/Deploying-graphics-devices-using-dda.md)或[部署存储设备使用离散设备分配](../deploy/Deploying-storage-devices-using-dda.md)若要开始 ！

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>支持的虚拟机和来宾操作系统
第 1 代或 2 个 Vm 均支持离散设备分配。  此外，来宾支持包括 Windows 10、 Windows Server 2019、 Windows Server 2016、 与 Windows Server 2012r2 [KB 3133690](https://support.microsoft.com/kb/3133690)应用，以及各种分发的[Linux OS。](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>系统要求
除了[Windows Server 系统要求](../../../get-started/System-Requirements--and-Installation.md)并[HYPER-V 的系统要求](../System-requirements-for-Hyper-V-on-Windows.md)，离散设备分配需要的是能够授予服务器类硬件操作系统配置 PCIe 结构 （本机 PCI Express 控件） 的控制。 此外，PCIe 根复杂必须支持"访问控制服务"(ACS)，从而使 HYPER-V 强制所有 I/O MMU 通过 PCIe 流量。

这些功能通常不直接在服务器的 BIOS 中公开，并且通常隐藏在其他设置。  例如，相同的功能所需的 SR-IOV 支持和 BIOS 中，您可能需要设置"启用 SR-IOV。"  请联系你系统的供应商如果不能以标识您的 BIOS 中的正确设置。

为了确保能够离散设备分配硬件硬件，我们的工程师已整合[计算机配置文件脚本](#machine-profile-script)可以测试你的服务器是否正确安装程序以及已启用 HYPER-V 主机上运行设备都能离散设备分配。

## <a name="device-requirements"></a>设备要求
不是每个 PCIe 设备可以用于离散设备分配。  例如，不支持较旧利用旧 (INTx) PCI 中断的设备。 Jake Oshin[博客文章](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/)转到更多详细信息-但是，使用者，运行[计算机配置文件脚本](#machine-profile-script)将显示哪些设备都能用于离散设备分配。

设备制造商可以联系其 Microsoft 代表交流，有关详细信息。

## <a name="device-driver"></a>设备驱动程序
离散设备分配到来宾 VM 传递整个 PCIe 设备，则主机驱动程序不需要在该 VM 中装载设备之前，先安装。  在主机上的唯一要求是，设备的[PCIe 位置路径](#pcie-location-path)可确定。  如果这有助于识别设备，可以选择性地安装设备的驱动程序。  例如，而无需在主机上安装其设备驱动程序的 GPU 可能显示为 Microsoft 基本呈现设备。  如果安装设备驱动程序，则，系统将可能显示其制造商和型号。

在来宾内部装载设备后，现在可以在来宾虚拟机内部照常安装制造商的设备驱动程序。  

## <a name="virtual-machine-limitations"></a>虚拟机限制
由于如何实施离散设备分配的特性，虚拟机的某些功能会受到限制，当设备连接。  以下功能不可用：
- VM 保存/还原
- 实时迁移的 VM
- 动态内存的使用
- 将 VM 添加到高可用性 (HA) 群集

## <a name="security"></a>安全性
离散设备分配到该 VM 将传递整个设备。  这意味着该设备的所有功能都是可从来宾操作系统访问。 一些功能，例如固件更新，可能会产生负面影响系统的稳定性。 在这种情况下，大量警告将提供给管理员，以便从主机设备在卸载时。 我们强烈建议会执行虚拟机的租户受信任仅使用离散设备分配。  

如果管理员想要使用不受信任的租户使用的设备，我们能够创建可以在主机安装的设备缓解驱动程序提供了设备制造商。  请有关它们是否提供设备缓解驱动程序的详细信息，联系设备制造商。

如果你想要绕过没有设备缓解驱动程序的设备的安全检查，必须传递`-Force`参数`Dismount-VMHostAssignableDevice`cmdlet。  了解这样做之后，您已更改该系统的安全配置文件，并且这是仅在原型制作过程建议或受信任的环境。

## <a name="pcie-location-path"></a>PCIe 位置路径
PCIe 位置路径需要卸载并从主机装入设备。  示例位置路径看起来如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。   [计算机配置文件脚本](#machine-profile-script)还将返回 PCIe 设备的位置路径。

### <a name="getting-the-location-path-by-using-device-manager"></a>使用设备管理器获取的位置路径
![设备管理器](../deploy/media/dda-devicemanager.png)
- 打开设备管理器，找到该设备。  
- 右键单击设备，然后选择"属性。
- 导航到详细信息选项卡，并在属性下拉列表中选择"位置路径"。  
- 右键单击与"PCIROOT"开头的项，然后选择"复制"。  您现在拥有该设备的位置路径。

## <a name="mmio-space"></a>MMIO 空间
某些设备，尤其是 Gpu，需要额外的 MMIO 空间，并将其分配到 VM，以便该设备的内存可以访问。 默认情况下，每个 VM 开始对 MMIO 空间不足为 128 MB，512 MB 的高 MMIO 空间分配给它。 但是，设备可能需要更多 MMIO 空间，或可能通过传递多个设备，以便组合的要求超出了这些值。  更改 MMIO 空间非常简单，可以执行在 PowerShell 中使用以下命令：

```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```
最简单的方法来确定是使用分配的空间大小 MMIO[计算机配置文件脚本](#machine-profile-script)。  或者，您可以计算它使用设备管理器。 请参阅 TechNet 博客文章[离散设备分配-Gpu](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)的更多详细信息。

## <a name="machine-profile-script"></a>计算机配置文件脚本
为了简化识别是否正确配置服务器以及哪些设备可用于通过使用离散设备分配传递，其中一位工程师整理出下面的 PowerShell 脚本：[SurveyDDA.ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

在使用前脚本，请确保安装了 HYPER-V 角色，从具有管理员权限的 PowerShell 命令窗口运行脚本。

如果系统未正确配置为支持离散设备分配，该工具将显示有关错误的错误消息。 如果该工具将查找正确配置系统，它将枚举它可以找到 PCIe 总线的所有设备。

它找到的每个设备，该工具会显示是否能够与离散设备分配一起使用。 如果设备被识别为离散设备分配与兼容，该脚本将提供的原因。  当设备被成功识别为兼容时，则将显示设备的位置路径。  此外，如果该设备需要[MMIO 空间](#mmio-space)，它也将会显示。

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>常见问题

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>远程桌面的 RemoteFX vGPU 技术到离散设备分配有何关联？
它们是完全独立的技术。 RemoteFX vGPU 不必为离散设备分配，若要运行安装应用程序。 此外，任何其他角色不是必需的安装。 RemoteFX vGPU 需要 RDVH 角色安装 RemoteFX vGPU 驱动程序必须包含在 VM 中的顺序。 对于离散设备分配，因为你将为虚拟机安装硬件供应商的驱动程序没有其他角色需要存在。  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>我已传递到 VM 的 GPU，但远程桌面或应用程序不识别 GPU
有多种原因而发生此情况，但下面列出了几个常见问题。
- 确保最新 GPU 供应商的驱动程序已安装并且未通过检查设备状态在设备管理器进行报告错误。
- 请确保该设备具有足够[MMIO 空间](#mmio-space)VM 中为其分配。
- 请确保你使用的供应商支持正在使用此配置中的 GPU。 例如，某些供应商阻止其使用者卡工作时向 VM 传递。
- 请确保 VM 内运行的支持和 GPU 和其关联的驱动程序支持应用程序正在运行的应用程序。 某些应用程序具有 Gpu 和环境的白名单。
- 如果在来宾上使用远程桌面会话主机角色或 Windows Multipoint 服务，需要确保特定的组策略条目设置为允许使用 GPU 的默认值。 使用组策略对象应用于在来宾 （或本地组策略编辑器在来宾机器上），导航到以下组策略项：
   - “计算机配置”
   - 管理员模板
   - Windows 组件
   - 远程桌面服务
   - 远程桌面会话主机
   - 远程会话环境
   - 对所有远程桌面服务会话使用硬件默认图形适配器

    将此值设置为 Enabled，则应用该策略后，重新启动 VM。

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>离散设备分配可以充分利用远程桌面 AVC444 编解码器？
是的请访问此博客文章，详细信息：[Windows 10 和 Windows Server 2016 Technical Preview 中的远程桌面协议 (RDP) 10 AVC/H.264 改进。](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>可以使用 PowerShell 获取位置路径？
是的有各种方法来执行此操作。 下面是一个示例：
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>离散设备分配可用来将 USB 设备传递到 VM？
尽管不正式支持，但我们的客户已使用离散设备分配执行此操作通过将整个 USB3 控制器传入到 VM。  因为整个控制器传入的每个 USB 设备插入到该控制器还将可在 VM 中访问。  请注意，只有一些 USB3 控制器可能起作用，USB2 控制器不能用于离散设备分配。
