---
title: 是否应在 Hyper-v 中创建第1代或第2代虚拟机？
description: 提供有关支持的引导方法和其他功能差异的注意事项，以帮助你选择哪一代能满足你的需求。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: bd0b50534096bc06edb41390ef2c4ec3554d8406
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364076"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>是否应在 Hyper-v 中创建第1代或第2代虚拟机？

>适用于：Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

> [!NOTE]
> 如果你计划将 Windows 虚拟机（VM）从本地上传到 Microsoft Azure，则使用 VHD 文件格式的第1代和第2代 Vm，并支持固定大小的磁盘。 请参阅[azure 上的第2代 vm](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) ，了解有关 Azure 支持的第2代功能的详细信息。 有关上传 Windows VHD 或 VHDX 的详细信息，请参阅[准备要上传到 Azure 的 WINDOWS vhd 或 vhdx](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。

创建第1代或第2代虚拟机的选择取决于要安装的来宾操作系统以及要用于部署虚拟机的启动方法。 建议创建第2代虚拟机，以便充分利用安全启动等功能，除非满足以下任一陈述：  

- 要从其启动的 VHD 不是[UEFI 兼容](https://technet.microsoft.com/library/hh824898.aspx)的。  
- 第2代不支持要在虚拟机上运行的操作系统。  
- 第2代不支持您要使用的启动方法。  

有关第2代虚拟机可用功能的详细信息，请参阅[hyper-v 功能按代和来宾提供兼容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。

创建虚拟机后，无法对其进行更改。 因此，我们建议你在选择代之前查看此处的注意事项，并选择要使用的操作系统、启动方法和功能。  

## <a name="which-guest-operating-systems-are-supported"></a>支持哪些来宾操作系统？

第1代虚拟机支持大多数来宾操作系统。 第2代虚拟机支持大多数64位版本的 Windows 和更高版本的 Linux 和 FreeBSD 操作系统。 使用以下部分来了解哪一代虚拟机支持你想要安装的来宾操作系统。  

- [Windows 来宾操作系统支持](#windows-guest-operating-system-support)  

- [CentOS 和 Red Hat Enterprise Linux 来宾操作系统支持](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Debian 来宾操作系统支持](#debian-guest-operating-system-support)  

- [FreeBSD 来宾操作系统支持](#freebsd-guest-operating-system-support)  

- [Oracle Linux 来宾操作系统支持](#oracle-linux-guest-operating-system-support)  

- [SUSE 来宾操作系统支持](#suse-guest-operating-system-support)  

- [Ubuntu 来宾操作系统支持](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Windows 来宾操作系统支持

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 Windows 64 位版本。  

|64位版本的 Windows|第 1 代|第 2 代|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 Windows 32 位版本。

|32位版本的 Windows|第 1 代|第 2 代|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>CentOS 和 Red Hat Enterprise Linux 来宾操作系统支持

下表显示了哪些版本的 Red Hat Enterprise Linux 可以用作第1代和第2代虚拟机的来宾操作系统 \(RHEL @ no__t 和 CentOS。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|RHEL/CentOS 7、windows 系列|&#10004;|&#10004;|  
|RHEL/CentOS 1.x 系列|&#10004;|&#10004;<br />**注意：** 仅在 Windows Server 2016 及更高版本上受支持。|  
|RHEL/CentOS 5. x 系列|&#10004;| &#10006;|  

有关详细信息，请参阅[CentOS 和 Red Hat Enterprise Linux hyper-v 上的虚拟机](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="debian-guest-operating-system-support"></a>Debian 来宾操作系统支持  

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 Debian 版本。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Debian 7、windows 系列|&#10004;| &#10006;|  
|Debian 3.x 系列|&#10004;|&#10004;|  

有关详细信息，请参阅[hyper-v 上的 Debian 虚拟机](../Supported-Debian-virtual-machines-on-Hyper-V.md)。  

### <a name="freebsd-guest-operating-system-support"></a>FreeBSD 来宾操作系统支持

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 FreeBSD 版本。  

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 和10。1|&#10004;| &#10006;|  
|FreeBSD 9.1 和9。3|&#10004;| &#10006;|  
|FreeBSD 8。4|&#10004;| &#10006;|  

有关详细信息，请参阅[hyper-v 上的 FreeBSD 虚拟机](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md)。  

### <a name="oracle-linux-guest-operating-system-support"></a>Oracle Linux 来宾操作系统支持  

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 Red Hat 兼容内核系列版本。  

|Red Hat 兼容内核系列版本|第 1 代|第 2 代|  
|---------------------------------------------|----------------|----------------|  
|Oracle Linux 7. x 系列|&#10004;|&#10004;|
|Oracle Linux 1.x 系列|&#10004;| &#10006;|  

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 Unbreakable Enterprise 内核版本。

|Unbreakable Enterprise Kernel （UEK）版本|第 1 代|第 2 代|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

有关详细信息，请参阅[Oracle Linux hyper-v 上的虚拟机](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="suse-guest-operating-system-support"></a>SUSE 来宾操作系统支持

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 SUSE 版本。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|SUSE Linux Enterprise Server 12 系列|&#10004;|&#10004;|  
|SUSE Linux Enterprise Server 11 系列|&#10004;| &#10006;|  
|打开 SUSE 12。3|&#10004;| &#10006;|  

有关详细信息，请参阅[SUSE 虚拟机上的 hyper-v](../Supported-SUSE-virtual-machines-on-Hyper-V.md)。  

### <a name="ubuntu-guest-operating-system-support"></a>Ubuntu 来宾操作系统支持

下表显示了可用作第1代和第2代虚拟机的来宾操作系统的 Ubuntu 版本。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 及更高版本|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

有关详细信息，请参阅[hyper-v 上的 Ubuntu 虚拟机](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md)。  

## <a name="how-can-i-boot-the-virtual-machine"></a>如何启动虚拟机？

下表显示第1代和第2代虚拟机支持的启动方法。  

|Boot 方法|第 1 代|第 2 代|  
|---------------|----------------|----------------|  
|PXE 通过使用标准网络适配器启动| &#10006;|&#10004;|  
|使用旧版网络适配器启动 PXE|&#10004;| &#10006;|  
|从 SCSI 虚拟硬盘（。VHDX）或虚拟 DVD （。ISO-C| &#10006;|&#10004;|  
|从 IDE 控制器虚拟硬盘（。VHD）或虚拟 DVD （。ISO-C|&#10004;| &#10006;|  
|从软盘启动（.VFD|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>使用第2代虚拟机的好处是什么？

下面是使用第2代虚拟机时获得的一些优势：  
- **安全启动**这是一项功能，用于验证启动加载程序是否由 UEFI 数据库中的受信任的颁发机构签名，以帮助防止未经授权的固件、操作系统或 UEFI 驱动程序在启动时运行。 默认情况下，针对第 2 代虚拟机启用安全启动。 如果需要运行安全启动不支持的来宾操作系统，则可以在创建虚拟机后将其禁用。  有关详细信息，请参阅[安全启动](https://technet.microsoft.com/library/dn486875.aspx)。  

    若要保护 Boot 第2版 Linux 虚拟机的安全，需要在创建虚拟机时选择 UEFI CA 安全启动模板。  

- **更大的启动卷**第2代虚拟机的最大启动卷为 64 TB。 这是所支持的最大磁盘大小。VHDX. 对于第1代虚拟机，最大启动卷为2TB。的 VHDX 和2040GB。硬盘. 有关详细信息，请参阅[Hyper-v 虚拟硬盘格式概述](https://technet.microsoft.com/library/hh831446.aspx)。  

  对于第2代虚拟机，你可能还会看到虚拟机启动和安装时间方面的一些改进。

## <a name="whats-the-difference-in-device-support"></a>设备支持的区别是什么？

下表比较了第1代虚拟机和第2代虚拟机之间可用的设备。  

|第 1 代设备|第 2 代替换设备|第 2 代增强功能|  
|-----------------------|----------------------------|-----------------------------|  
|IDE 控制器|虚拟 SCSI 控制器|从 .vhdx（64 TB 的最大大小和联机调整大小功能）中启动|  
|IDE CD-ROM|虚拟 SCSI CD-ROM|每个 SCSI 控制器最多支持 64 个 SCSI DVD 设备。|  
|传统 BIOS|UEFI 固件|安全启动|  
|旧版网络适配器|合成网络适配器|使用 IPv4 和 IPv6 的网络启动|  
|软盘控制器和 DMA 控制器|不支持软盘控制器|不可用|  
|适用于 COM 端口的通用异步收发器 (UART)|用于调试的可选 UART|更快且更可靠|  
|i8042 键盘控制器|基于软件的输入|使用较少的资源，因为没有任何模拟。 还减少来自来宾操作系统的攻击面。|  
|PS/2 键盘|基于软件的键盘|使用较少的资源，因为没有任何模拟。 还减少来自来宾操作系统的攻击面。|  
|PS/2 鼠标|基于软件的鼠标|使用较少的资源，因为没有任何模拟。 还减少来自来宾操作系统的攻击面。|  
|S3 视频|基于软件的视频|使用较少的资源，因为没有任何模拟。 还减少来自来宾操作系统的攻击面。|  
|PCI 总线|不再需要|不可用|  
|可编程中断控制器 (PIC)|不再需要|不可用|  
|可编程间隔计时器 (PIT)|不再需要|不可用|  
|超级 I/O 设备|不再需要|不可用|  

## <a name="more-about-generation-2-virtual-machines"></a>有关第2代虚拟机的详细信息

下面是有关使用第2代虚拟机的一些其他提示。

### <a name="attach-or-add-a-dvd-drive"></a>附加或添加 DVD 驱动器

- 不能将物理 CD 或 DVD 驱动器附加到第2代虚拟机。 第 2 代虚拟机中的虚拟 DVD 驱动器仅支持 ISO 映像文件。 若要创建 Windows 环境的 ISO 映像文件，可以使用 Oscdimg 命令行工具。 有关详细信息，请参阅 [Oscdimg 命令行选项](https://msdn.microsoft.com/library/hh824847.aspx)。
- 使用新的-VM Windows PowerShell cmdlet 创建新的虚拟机时，第2代虚拟机没有 DVD 驱动器。 可以在虚拟机运行时添加 DVD 驱动器。

### <a name="use-uefi-firmware"></a>使用 UEFI 固件

- 物理 Hyper-v 主机上不需要安全启动或 UEFI 固件。 Hyper-v 提供与 Hyper-v 主机上的功能无关的虚拟机的虚拟固件。
- 第2代虚拟机中的 UEFI 固件不支持安全启动的安装模式。
- 我们不支持在第2代虚拟机中运行 UEFI shell 或其他 UEFI 应用程序。 使用非 Microsoft UEFI shell 或 UEFI 应用程序从技术上讲是可行的（如果它们直接从源进行编译）。 如果未对这些应用程序进行适当的数字签名，则必须对该虚拟机禁用安全启动。

### <a name="work-with-vhdx-files"></a>使用 VHDX 文件

- 当虚拟机正在运行时，可以调整包含第2代虚拟机启动卷的 VHDX 文件的大小。
- 我们不支持或建议你创建可同时启动第1代和第2代虚拟机的 VHDX 文件。  
- 虚拟机代次是虚拟机的属性，而不是虚拟硬盘的属性。 因此，你无法判断某个 VHDX 文件是由第1代还是第2代虚拟机创建的。  
- 使用第2代虚拟机创建的 VHDX 文件可以连接到第1代虚拟机的 IDE 控制器或 SCSI 控制器。 但是，如果这是可启动的 VHDX 文件，则第1代虚拟机不会启动。

### <a name="use-ipv6-instead-of-ipv4"></a>使用 IPv6 而不是 IPv4

默认情况下，第 2 代虚拟机使用 IPv4。 若要改为使用 IPv6，请运行[Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx) Windows PowerShell cmdlet。 例如，下面的命令将名为 TestVM 的虚拟机的首选协议设置为 IPv6：  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>为内核调试添加 COM 端口

在添加之前，COM 端口在第2代虚拟机中不可用。 可以通过 Windows PowerShell 或 Windows Management Instrumentation （WMI）来执行此操作。 以下步骤演示如何在 Windows PowerShell 中执行此操作。

添加 COM 端口：  

1. 禁用安全启动。 内核调试与安全启动不兼容。 请确保虚拟机处于关闭状态，然后使用[set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet。 例如，以下命令将在虚拟机 TestVM 上禁用安全启动：  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. 添加 COM 端口。 使用[set-vmcomport](https://technet.microsoft.com/library/hh848616.aspx) cmdlet 执行此操作。 例如，以下命令将虚拟机 TestVM 上的第一个 COM 端口配置为连接到本地计算机上的命名管道 TestPipe：  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> Hyper-v 管理器中虚拟机的设置中未列出已配置的 COM 端口。

## <a name="see-also"></a>请参阅  

- [Hyper-v 上的 Linux 和 FreeBSD 虚拟机](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [在具有 VMConnect 的 Hyper-v 虚拟机上使用本地资源](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Windows Server 2016 中的 Hyper-v 可伸缩性规划](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
