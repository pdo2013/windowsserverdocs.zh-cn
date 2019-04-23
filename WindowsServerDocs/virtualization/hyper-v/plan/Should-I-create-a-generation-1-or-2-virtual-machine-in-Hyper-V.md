---
title: 应在 HYPER-V 中创建第 1 或 2 代虚拟机？
description: 提供了如支持注意事项引导方法和其他功能差异，以帮助您选择哪一代满足你的需求。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 48319e057da9c815a77349bba34996f89973d85a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850498"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>应在 HYPER-V 中创建第 1 或 2 代虚拟机？

>适用于：Windows 10、 Windows Server 2016、 Microsoft HYPER-V Server 2016，Windows Server 2019，Microsoft HYPER-V Server 2019

> [!WARNING]
> 如果你打算过上传 Windows 虚拟机 (VM) 从本地到 Microsoft Azure**只有第 1 代 Vm** ，是在 VHD 文件格式，并具有固定大小的磁盘支持。 上传 Windows VHD 或 VHDX 的详细信息，请参阅[准备要上传到 Azure 的 Windows VHD 或 VHDX](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。

若要创建的第 1 代或第 2 代虚拟机所选取决于哪些来宾操作系统上要安装和你想要用于部署虚拟机的启动方法。 我们建议你创建要充分利用安全启动等功能，除非以下语句之一为 true 的第 2 代虚拟机：  

- 你想要从启动的 VHD 不是[UEFI 兼容](https://technet.microsoft.com/library/hh824898.aspx)。  
- 第 2 代并不支持你想要在虚拟机上运行的操作系统。  
- 第 2 代不支持你想要使用的启动方法。  

有关与第 2 代虚拟机提供的功能的详细信息，请参阅[通过生成和来宾的 HYPER-V 功能兼容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。

无法更改虚拟机的代次后创建它。 因此，我们建议，你查看此处的注意事项，以及选择的操作系统、 启动方法和你想要使用之前选择生成功能。  

## <a name="BKMK_OS"></a>支持哪些来宾操作系统？

第 1 代虚拟机支持大多数的来宾操作系统。 第 2 代虚拟机支持最 64 位版本的 Windows 和 Linux 和 FreeBSD 操作系统的较新版本。 使用下列部分来查看哪一代虚拟机的支持来宾操作系统，你想要安装。  

- [Windows 来宾操作系统支持](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Windows)  

- [CentOS 和 Red Hat Enterprise Linux 来宾操作系统支持](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_CentOS)  

- [Debian 来宾操作系统支持](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Debian)  

- [FreeBSD 来宾操作系统支持](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_FreeBSD)  

- [Oracle Linux 来宾操作系统支持](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Oracle)  

- [SUSE 来宾操作系统支持](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_SUSE)  

- [Ubuntu 来宾操作系统支持](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Ubuntu)  

### <a name="BKMK_Windows"></a>Windows 来宾操作系统支持

下表显示了 Windows 的 64 位版本可用作来宾操作系统的第 1 代和第 2 代虚拟机。  

|64 位版本的 Windows|第 1 代|第 2 代|  
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

下表显示了 Windows 的 32 位版本可用作来宾操作系统的第 1 代和第 2 代虚拟机。

|32 位版本的 Windows|第 1 代|第 2 代|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="BKMK_CentOS"></a>CentOS 和 Red Hat Enterprise Linux 来宾操作系统支持

下表显示了哪些版本的 Red Hat Enterprise Linux \(RHEL\)并且 CentOS 您只能使用作为来宾操作系统的第 1 代和第 2 代虚拟机。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|RHEL/CentOS 7.x 系列|&#10004;|&#10004;|  
|RHEL/CentOS 6.x 系列|&#10004;|&#10004;<br />**注意：** 仅支持在 Windows Server 2016 及更高版本。|  
|RHEL/CentOS 5.x 系列|&#10004;| &#10006;|  

有关详细信息，请参阅[CentOS 和 Red Hat Enterprise Linux 上的 HYPER-V 虚拟机](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="BKMK_Debian"></a>Debian 来宾操作系统支持  

下表显示了哪些版本的 Debian 可用作来宾操作系统的第 1 代和第 2 代虚拟机。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Debian 7.x 系列|&#10004;| &#10006;|  
|Debian 8.x 系列|&#10004;|&#10004;|  

有关详细信息，请参阅[HYPER-V 上的 Debian 虚拟机](../Supported-Debian-virtual-machines-on-Hyper-V.md)。  

### <a name="BKMK_FreeBSD"></a>FreeBSD 来宾操作系统支持

下表显示了哪些版本的 FreeBSD 可用作来宾操作系统的第 1 代和第 2 代虚拟机。  

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 和 10.1|&#10004;| &#10006;|  
|FreeBSD 9.1 和 9.3|&#10004;| &#10006;|  
|FreeBSD 8.4|&#10004;| &#10006;|  

有关详细信息，请参阅[FreeBSD 虚拟机上的 HYPER-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md)。  

### <a name="BKMK_Oracle"></a>Oracle Linux 来宾操作系统支持  

下表显示了哪些版本的 Red Hat 兼容内核系列你可以使用作为来宾操作系统的第 1 代和第 2 代虚拟机。  

|Red Hat 兼容内核系列版本|第 1 代|第 2 代|  
|---------------------------------------------|----------------|----------------|  
|Oracle Linux 7.x 系列|&#10004;|&#10004;|
|Oracle Linux 6.x 系列|&#10004;| &#10006;|  

下表显示了哪些版本的坚不可摧企业内核可用作来宾操作系统的第 1 代和第 2 代虚拟机。

|Unbreakable Enterprise Kernel (UEK) 版本|第 1 代|第 2 代|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

有关详细信息，请参阅[HYPER-V 上的 Oracle Linux 虚拟机](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="BKMK_SUSE"></a>SUSE 来宾操作系统支持

下表显示了的 SUSE 版本可用作来宾操作系统的第 1 代和第 2 代虚拟机。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|SUSE Linux Enterprise Server 12 系列|&#10004;|&#10004;|  
|SUSE Linux Enterprise Server 11 系列|&#10004;| &#10006;|  
|Open SUSE 12.3|&#10004;| &#10006;|  

有关详细信息，请参阅[SUSE 虚拟机上的 HYPER-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md)。  

### <a name="BKMK_Ubuntu"></a>Ubuntu 来宾操作系统支持

下表显示了哪些版本的 Ubuntu 可用作来宾操作系统的第 1 代和第 2 代虚拟机。

|操作系统版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 及更高版本|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

有关详细信息，请参阅[Ubuntu 虚拟机上的 HYPER-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md)。  

## <a name="BKMK_Boot"></a>如何启动虚拟机？

下表显示了方法所支持的第 1 代和第 2 代虚拟机的启动。  

|启动方法|第 1 代|第 2 代|  
|---------------|----------------|----------------|  
|PXE 通过使用标准网络适配器启动| &#10006;|&#10004;|  
|使用旧版网络适配器的 PXE 启动|&#10004;| &#10006;|  
|从 SCSI 虚拟硬盘启动 (。VHDX) 或虚拟 DVD (。ISO)| &#10006;|&#10004;|  
|从 IDE 控制器的虚拟硬盘启动 (。VHD) 或虚拟 DVD (。ISO)|&#10004;| &#10006;|  
|从软盘启动 (。VFD)|&#10004;| &#10006;|  

## <a name="BKMK_Advantages"></a>使用第 2 代虚拟机的优点有哪些？

下面是一些在使用第 2 代虚拟机时获得的优势：  
- **安全启动**这是一项功能，用于验证的启动加载器是受信任颁发机构签名 UEFI 数据库中来帮助防止未经授权的固件、 操作系统或 UEFI 驱动程序在启动时运行。 默认情况下，针对第 2 代虚拟机启用安全启动。 如果需要运行来宾操作系统不支持安全启动的可以创建虚拟机后禁用它。  有关详细信息，请参阅[安全启动](https://technet.microsoft.com/library/dn486875.aspx)。  

    到安全启动第 2 代 Linux 虚拟机，需要在创建虚拟机时选择的 UEFI CA 安全启动模板。  

- **更大的启动卷**第 2 代虚拟机的最大启动卷是 64 TB。 这是支持的最大磁盘大小。VHDX。 对于第 1 代虚拟机，最大启动卷是为 2 TB。VHDX 和 2040 GB。VHD。 有关详细信息，请参阅[HYPER-V 虚拟硬盘格式概述](https://technet.microsoft.com/library/hh831446.aspx)。  

 您可能会看到一些改进中与第 2 代虚拟机的虚拟机启动和安装时间。

## <a name="BKMK_DeviceCompare"></a> 在设备支持的区别是什么？

下表比较了第 1 代和第 2 代虚拟机之间提供的设备。  

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

## <a name="BKMK_More"></a> 有关第 2 代虚拟机的详细信息

以下是一些有关使用第 2 代虚拟机的更多技巧。

### <a name="attach-or-add-a-dvd-drive"></a>附加或添加 DVD 驱动器

- 无法将物理 CD 或 DVD 驱动器附加到第 2 代虚拟机。 第 2 代虚拟机中的虚拟 DVD 驱动器仅支持 ISO 映像文件。 若要创建 Windows 环境的 ISO 映像文件，可以使用 Oscdimg 命令行工具。 有关详细信息，请参阅 [Oscdimg 命令行选项](https://msdn.microsoft.com/library/hh824847.aspx)。
- 当使用 NEW-VM Windows PowerShell cmdlet 创建新的虚拟机时，第 2 代虚拟机没有 DVD 驱动器。 虚拟机运行时，你可以添加 DVD 驱动器。

### <a name="use-uefi-firmware"></a>使用 UEFI 固件

- 安全启动或 UEFI 固件不需要在物理 HYPER-V 主机上。 HYPER-V 提供了虚拟到虚拟机固件无关的新增功能的 HYPER-V 主机上。
- 在第 2 代虚拟机中的 UEFI 固件不支持安全启动的模式下安装程序。
- 我们不支持第 2 代虚拟机中运行 UEFI shell 或其他 UEFI 应用程序。 使用非 Microsoft UEFI shell 或 UEFI 应用程序从技术上讲是可行的（如果它们直接从源进行编译）。 如果这些应用程序未正确进行数字签名，则必须为虚拟机禁用安全启动。

### <a name="work-with-vhdx-files"></a>使用的 VHDX 文件

- 可以调整虚拟机运行时包含第 2 代虚拟机启动卷的 VHDX 文件的大小。
- 我们不支持或建议创建可启动到第 1 代和第 2 代虚拟机的 VHDX 文件。  
- 虚拟机代次是虚拟机的属性，而不是虚拟硬盘的属性。 因此无法判断是否由第 1 代或第 2 代虚拟机创建的 VHDX 文件。  
- 创建与生成 2 个虚拟机可以附加到 IDE 控制器或 SCSI 控制器的第 1 代虚拟机的 VHDX 文件。 但是，如果这是一个可启动的 VHDX 文件，不会启动第 1 代虚拟机。

### <a name="use-ipv6-instead-of-ipv4"></a>使用 IPv6 而非 IPv4

默认情况下，第 2 代虚拟机使用 IPv4。 若要改为使用 IPv6，运行[Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx) Windows PowerShell cmdlet。 例如，以下命令设置名为 TestVM 的虚拟机为 IPv6 首选的协议：  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="BKMK_Debug"></a>添加 COM 端口进行内核调试

COM 端口不可用在第 2 代虚拟机中，直到将其添加。 可以使用 Windows PowerShell 或 Windows Management Instrumentation (WMI) 来执行此操作。 这些步骤演示了如何使用 Windows PowerShell 执行此操作。

若要添加的 COM 端口：  

1. 禁用安全启动。 内核调试不兼容安全启动。 请确保虚拟机处于关闭状态，然后使用[Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet。 例如，以下命令在虚拟机 TestVM 上禁用安全启动：  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. 添加的 COM 端口。 使用[Set-vmcomport](https://technet.microsoft.com/library/hh848616.aspx) cmdlet 来执行此操作。 例如，以下命令在虚拟机 TestVM，若要连接到命名的管道 TestPipe，本地计算机上配置的第一个 COM 端口：  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> 配置的 COM 端口的 HYPER-V 管理器中的虚拟机设置中未列出。

## <a name="see-also"></a>请参阅  

- [Linux 和 FreeBSD 虚拟机上的 HYPER-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [通过 VMConnect 的 HYPER-V 虚拟机上使用本地资源](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [规划 Windows Server 2016 中的 HYPER-V 可伸缩性](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)