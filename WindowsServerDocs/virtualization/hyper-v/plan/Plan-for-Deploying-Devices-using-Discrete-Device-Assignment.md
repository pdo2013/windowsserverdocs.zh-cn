---
title: 使用离散设备分配计划部署设备
description: 了解如何在 Windows Server 中使用 DDA
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 08/21/2019
ms.openlocfilehash: 114dd87b86bfffd1070229af57ae65deea2c2db0
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923863"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>使用离散设备分配计划部署设备
>适用于： Microsoft Hyper-V Server 2016、Windows Server 2016、Microsoft Hyper-V Server 2019、Windows Server 2019

离散设备分配允许从虚拟机中直接访问物理 PCIe 硬件。  本指南将讨论可使用离散设备分配、主机系统要求、对虚拟机施加的限制以及离散设备分配的安全影响的设备类型。

对于离散设备分配的初始版本，我们将重点放在 Microsoft 正式支持的两个设备类：图形适配器和 NVMe 存储设备。  其他设备可能工作，硬件供应商能够为这些设备提供支持声明。  对于这些其他设备，请与这些硬件供应商联系以获得支持。

若要了解有关 GPU 虚拟化的其他方法，请参阅[在 Windows Server 中规划 GPU 加速](plan-for-gpu-acceleration-in-windows-server.md)。 如果你已准备好尝试使用离散设备分配，则可以跳转到[使用离散设备分配部署图形设备](../deploy/Deploying-graphics-devices-using-dda.md)，或[使用离散设备分配部署存储设备](../deploy/Deploying-storage-devices-using-dda.md)开始使用。

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>支持的虚拟机和来宾操作系统
第1代或第2代 Vm 支持离散设备分配。  此外，支持的来宾包括 Windows 10、Windows Server 2019、Windows Server 2016、应用了[KB 3133690](https://support.microsoft.com/kb/3133690)的 windows server 2012R2 和[Linux 操作系统](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)的各种分发。

## <a name="system-requirements"></a>系统要求
除了适用于[Windows Server 的系统要求](../../../get-started/System-Requirements--and-Installation.md)以及 Hyper-v 的[系统要求](../System-requirements-for-Hyper-V-on-Windows.md)外，离散设备分配要求服务器类硬件，该硬件能够授予操作系统控制配置 PCIe 的权限fabric （本机 PCI Express 控件）。 此外，PCIe 根复杂必须支持 "访问控制服务" （ACS），这使 Hyper-v 可以通过 i/o MMU 强制所有 PCIe 流量。

通常不会在服务器的 BIOS 中直接公开这些功能，这些功能通常隐藏在其他设置后面。  例如，SR-IOV 支持需要相同的功能，在 BIOS 中，你可能需要设置 "Enable SR-IOV"。  如果无法识别 BIOS 中的正确设置，请联系你的系统供应商。

为了帮助确保硬件硬件能够分配不同的设备，我们的工程师将[计算机配置文件脚本](#machine-profile-script)组合在一起，你可以在启用 hyper-v 的主机上运行该脚本，以测试你的服务器是否安装正确以及哪些设备能够离散设备分配。

## <a name="device-requirements"></a>设备要求
并非每个 PCIe 设备都可用于离散设备分配。  例如，不支持使用旧版（INTx） PCI 中断的旧版设备。 Jake Oshin 的[博客文章](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/)更详细-但是，对于使用者，运行[计算机配置文件脚本](#machine-profile-script)将显示哪些设备能够用于离散设备分配。

有关更多详细信息，设备制造商可以联系 Microsoft 代表。

## <a name="device-driver"></a>设备驱动程序
由于离散设备分配将整个 PCIe 设备传递到来宾 VM，因此在 VM 内装入设备之前无需安装主机驱动程序。  主机上的唯一要求是可以确定设备的[PCIe 位置路径](#pcie-location-path)。  如果这有助于识别设备，则可以选择安装设备的驱动程序。  例如，主机上未安装设备驱动程序的 GPU 可能会显示为 Microsoft 基本渲染设备。  如果安装了设备驱动程序，则可能会显示其制造商和型号。

在来宾内装入设备后，现在可以在来宾虚拟机内像平时一样安装制造商的设备驱动程序。  

## <a name="virtual-machine-limitations"></a>虚拟机限制
由于实现了离散设备分配的性质，因此在附加设备时，虚拟机的某些功能会受到限制。  以下功能不可用：
- VM 保存/还原
- VM 的实时迁移
- 使用动态内存
- 将 VM 添加到高可用性（HA）群集

## <a name="security"></a>安全
离散设备分配将整个设备传递到 VM 中。  这意味着可以从来宾操作系统访问该设备的所有功能。 某些功能（如固件更新）可能会对系统的稳定性产生不利影响。 因此，在从主机中卸载设备时，会向管理员显示许多警告。 我们强烈建议仅在 Vm 的租户受信任的情况下使用离散设备分配。  

如果管理员想要将设备与不受信任的租户一起使用，我们提供了设备制造商，可以创建可在主机上安装的设备缓解驱动程序。  有关是否提供设备缓解驱动程序的详细信息，请与设备制造商联系。

如果要跳过对没有设备缓解驱动程序的设备进行安全检查，则必须将 `-Force` 参数传递到 `Dismount-VMHostAssignableDevice` cmdlet。  请注意，通过这样做，你已更改了该系统的安全配置文件，这只是建议在原型设计环境或受信任环境中使用。

## <a name="pcie-location-path"></a>PCIe 位置路径
需要通过 PCIe 位置路径从主机中卸载并装载设备。  示例位置路径如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。   [计算机配置文件脚本](#machine-profile-script)还会返回 PCIe 设备的位置路径。

### <a name="getting-the-location-path-by-using-device-manager"></a>使用设备管理器获取位置路径
![“设备管理器”](../deploy/media/dda-devicemanager.png)
- 打开设备管理器并找到设备。  
- 右键单击该设备，然后选择 "属性"。
- 导航到 "详细信息" 选项卡，然后选择属性下拉的 "位置路径"。  
- 右键单击以 "PCIROOT" 开头的条目，然后选择 "复制"。  你现在具有该设备的位置路径。

## <a name="mmio-space"></a>MMIO 空间
某些设备，尤其是 Gpu，需要将额外的 MMIO 空间分配给 VM，以使该设备的内存可供访问。 默认情况下，每个 VM 启动时均为128MB，并为其分配的高 mmio 空间为512MB。 但是，设备可能需要更多的 MMIO 空间，或可能会传递多个设备，因此组合的要求超出了这些值。  更改 MMIO 空间非常简单，可以使用以下命令在 PowerShell 中执行此操作：

```PowerShell
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```

确定要分配的 MMIO 空间的最简单方法是使用[计算机配置文件脚本](#machine-profile-script)。 若要下载并运行计算机配置文件脚本，请在 PowerShell 控制台中运行以下命令：

```PowerShell
curl -o SurveyDDA.ps1 https://raw.githubusercontent.com/MicrosoftDocs/Virtualization-Documentation/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1
.\SurveyDDA.ps1
```

对于可以分配的设备，该脚本将显示给定设备的 MMIO 要求，如以下示例所示：

```PowerShell
NVIDIA GRID K520
Express Endpoint -- more secure.
    ...
    And it requires at least: 176 MB of MMIO gap space
...
```

低 MMIO 空间仅用于32位的操作系统和使用32位地址的设备。 在大多数情况下，设置 VM 的高 MMIO 空间就足够了，因为32位配置不太常见。

> [!IMPORTANT]
> 向 VM 分配 MMIO 空间时，用户需要确保将 MMIO 空间设置为所需的所有所需的 MMIO 空间的总和，并在存在需要几 MB 的 MMIO 空间的其他虚拟设备时，另外添加一个缓冲区。 使用上面所述的默认 MMIO 值作为低和高 MMIO （128 MB 和 512 MB）的缓冲区。

如果用户要分配单个 K520 GPU （如上面的示例所示），则必须将 VM 的 MMIO 空间设置为计算机配置文件脚本输出的值，并将缓冲区设置为-176 MB + 512 MB。 如果用户要分配三个 K520 Gpu，则必须将 MMIO 空间设置为 176 MB 加上一个缓冲区，或 528 MB + 512 MB。

若要深入了解 MMIO 空间，请参阅 TechCommunity 博客上的[离散设备分配-gpu](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266) 。

## <a name="machine-profile-script"></a>计算机配置文件脚本
为了简化确定服务器的配置是否正确以及哪些设备可通过使用离散设备分配进行传递，我们的一位工程师将以下 PowerShell 脚本组合在一起： [SurveyDDA。](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

使用脚本之前，请确保安装了 Hyper-v 角色，并从具有管理员权限的 PowerShell 命令窗口中运行该脚本。

如果系统未正确配置为支持离散设备分配，则该工具将显示错误消息，显示错误消息。 如果该工具发现系统配置正确，则它将枚举它可以在 PCIe 总线上找到的所有设备。

对于它找到的每个设备，该工具都将显示它是否能够与离散设备分配一起使用。 如果将设备标识为与离散设备分配兼容，则该脚本将提供原因。  如果设备成功标识为兼容，则会显示设备的位置路径。  此外，如果该设备需要[MMIO 空间](#mmio-space)，则也会显示它。

![SurveyDDA](./images/hyper-v-surveydda-ps1.png)
