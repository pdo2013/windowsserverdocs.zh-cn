---
ms.assetid: 13210461-1e92-48a1-91a2-c251957ba256
title: 驱动器固件更新疑难解答
ms.prod: windows-server-threshold
ms.author: toklima
ms.manager: masriniv
ms.technology: storage
ms.topic: article
author: toklima
ms.date: 04/18/2017
ms.openlocfilehash: 8283b87e9505b1d3f47ddc823016fbcc7c0c29e6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867046"
---
# <a name="troubleshooting-drive-firmware-updates"></a>驱动器固件更新疑难解答

>适用于：Windows 10、Windows Server （半年频道）、

Windows 10 版本 1703 和更高版本以及 Windows Server（半年频道）能够更新已使用固件可升级 AQ（其他限定符）通过 PowerShell 认证的 HDD 和 SSD 的固件。

你可以在此处找到有关此功能的详细信息：

- [更新 Windows Server 2016 中的驱动器固件](update-firmware.md)
- [无需停机即可更新驱动器固件存储空间直通](https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct)

固件更新可能会因各种原因而失败。 这篇文章旨在帮助进行高级疑难解答。

  > [!Note]
  > 根据问题，本文中的信息可能不足以完全调试所有可能的故障情况。

## <a name="common-issues"></a>常见问题
在体系结构方面，此新功能依赖在 PowerShell 调入到的 Windows 存储堆栈中实现的 API。 该存储堆栈依赖驱动程序和硬件来正确实现行业定义的命令。 这会产生可能出现故障的多个点。 最常观察到的问题是：

1.  给定的驱动器没有正确实现行业标准命令（不具有 AQ）
2.  执行更新所需的 API 没有实现或出错（如果使用第三方驱动程序）
3.  API 运行，但固件本身存在问题（无效/映像损坏...）

以下部分概述了疑难解答信息，具体取决于使用 Microsoft 还是第三方驱动程序。

## <a name="identifying-inappropriate-hardware"></a>确定不适当的硬件
识别设备是否支持正确命令集的最快方式是启动 PowerShell，然后将磁盘的代表 PhysicalDisk 对象传递到 Get-StorageFirmwareInfo cmdlet 中。 下面是一个示例：

```powershell
Get-PhysicalDisk -SerialNumber 15140F55976D | Get-StorageFirmwareInformation
```
下面是示例输出：

```
PhysicalDisk          : MSFT_PhysicalDisk (ObjectId = "{1}\\TOKLIMA-DL380\root/Microsoft/Windo...)
SupportsUpdate        : True
NumberOfSlots         : 1
ActiveSlotNumber      : 0
SlotNumber            : {0}
IsSlotWritable        : {True}
FirmwareVersionInSlot : {0013}
```

至少对于 SATA 和 NVMe 设备，SupportsUpdate 字段将指示是否可使用内置 PowerShell 功能更新固件。

对于 SAS 连接的设备，SupportsUpdate 字段将始终报告“True”，因为在使用行业标准命令时无法查询相应的命令支持。

要验证 SAS 设备是否支持所需的命令集，有两个选择：
1.  使用相应的固件映像通过 Update-StorageFirmware cmdlet 试一下，或者
2.  请查阅 Windows Server 目录，以确定哪些 SAS 设备已成功获得 FW 更新 AQ （ https://www.windowsservercatalog.com/)

### <a name="remediation-options"></a>修正选项
如果你正在测试的给定设备不支持相应的命令集，则询问你的供应商，了解是否可获得提供所需命令集的已更新固件，或者查看 Windows Server 目录，确定用于获得源的可实现相应命令集的设备。

## <a name="troubleshooting-with-3rd-party-drivers-sas"></a>第三方驱动程序 (SAS) 疑难解答
与硬件最紧密交互的软件组件是 Windows 存储堆栈中的微型端口驱动程序。 对于某些存储协议，如 SATA 和 NVMe，Microsoft 提供了原始 Windows 驱动程序。 这些驱动程序可提供额外的调试信息。 但第三方硬件和软件供应商可自由地为它们的设备编写自己的微型端口驱动程序，并且它们对调试信息的支持级别可能不同。

要识别固件下载发生的情况，以及激活沿存储堆栈发送的 API，无论是什么微型端口驱动程序，都请查看以下事件日志通道：

事件查看器 - 应用程序和服务日志 - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-ClassPnP/Operational**

此通道记录有关向下发送到微型端口驱动程序的 Windows API 的信息，以及它们的响应。 例如，当尝试将固件映像下载到 SATA 设备，并且此设备通过未正确实现从 SAS 到 SATA 的所需转换的 SAS HBA 连接时，会展示直接显示在下面的错误情况：

```powershell
Get-PhysicalDisk -SerialNumber 44GS103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
```
下面是输出示例：

```
Update-StorageFirmware : Failed

Extended information:
A warning or error has been encountered during storage firmware update.
Incorrect function.

Activity ID: {1224482b-2315-4a38-81eb-27bb7de19c00}
At line:1 char:47
+ ... S103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.en ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Update-StorageFirmware], CimException
+ FullyQualifiedErrorId : StorageWMI 4,Microsoft.Management.Infrastructure.CimCmdlets.InvokeCimMethodCommand,Update-StorageFirmware
```
    
PowerShell 将引发错误，并且已收到调用的函数（即内核 API）不正确的信息。 这可能意味着第三方 SAS 微型端口驱动程序（在本例中为 true）没有实现 API，或者该 API 因其他原因而失败，例如下载分段不一致。

```
EventData
DeviceGUID  {132EDB55-6BAC-A3A0-C2D5-203C7551D700}
DeviceNumber    1
Vendor  ATA 
Model   TOSHIBA THNSNJ12
FirmwareVersion 6101
SerialNumber    44GS103UT5EW
DownLevelIrpStatus  0xc0000185
SrbStatus   132
ScsiStatus  2
SenseKey    5
AdditionalSenseCode 36
AdditionalSenseCodeQualifier    0
CdbByteCount    10
CdbBytes    3B0E0000000001000000
NumberOfRetriesDone 0
```

通道中的 ETW 事件507显示 SCSI SRB 请求失败，并提供 SenseKey 为 "5" （非法请求）的附加信息，并且 AdditionalSense 信息为 "36" （CDB 中非法的字段）。

   > [!Note]
   > 这些信息是由相关微型端口直接提供的，这些信息的准确性将取决于微型端口驱动程序的实现和复杂程度。

如果微型端口驱动程序没有消除它们之间的歧义，则不同的错误情况可能展示同一错误代码。 例如，尝试通过 SAS HBA 将无效固件映像下载到 SATA 设备（预计该设备会失败）可能被转换为相同的故障代码。

在混合了协议并出现转换的情况下，即 SATA 在 SAS 后，最好测试直接连接到 SATA 控制器的 SATA 设备，从而排除它是潜在问题的可能性。

### <a name="remediation-options"></a>修正选项
如果第三方驱动程序被认为没有实现所需的 API 或转换，则可换成 Microsoft 提供的 SATA (StorAHCI.sys) 和 NVMe (StorNVMe.sys) 的替代项，或者联系提供了此 SAS 驱动程序的 OEM 或 HBA 供应商，询问是否存在具有正确支持的更新版本。

## <a name="additional-troubleshooting-with-microsoft-drivers-satanvme"></a>Microsoft 驱动程序 (SATA/NVMe) 的其他疑难解答
当使用 StorAHCI.sys 或 StorNVMe.sys 等 Windows 本机驱动程序为存储设备提供动力时，可获得有关在固件更新操作过程中可能发生的故障的额外信息。

除了 ClassPnP 操作通道，StorAHCI 和 StorNVMe 还会在以下 ETW 通道中记录设备的协议特定返回代码：

事件查看器 - 应用程序和服务日志 - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-StorPort/Diagnose**

诊断日志在默认情况下不显示，可在事件查看器中单击“查看”，然后从下拉菜单中选择“显示分析和调试日志”来激活/显示诊断日志。

要收集这些高级日志条目，则启用该日志，重现固件更新失败，然后保存诊断日志。

下面是 SATA 设备上固件更新的示例失败，因为要下载的映像无效（事件 ID：258）：

``` 
EventData
MiniportName    storahci
MiniportEventId 19
MiniportEventDescription    Firmware Activate Completion
PortNumber  0
Bus 2
Target  0
LUN 0
Irp 0xffff8c84cd45aca0
Srb 0xffffab0024030bc0
Parameter1Name  SrbStatus
Parameter1Value 130
Parameter2Name  ReturnCode
Parameter2Value 0
Parameter3Name  FeaturesReg
Parameter3Value 15
Parameter4Name  SectorCountReg
Parameter4Value 0
Parameter5Name  DriveHeadReg
Parameter5Value 160
Parameter6Name  CommandReg
Parameter6Value 146
Parameter7Name  NULL
Parameter7Value 0
Parameter8Name  NULL
Parameter8Value 0
```

上述事件在参数值 2 至 6 中包含详细的设备信息。 在这里，我们将查看各个 ATA 注册值。 可使用 ATA ACS 规范来解码针对下载微代码命令失败的以下值：
- 返回代码：0（0000 0000）（由于未传输有效负载，因此没有任何意义）
- 功能：15（0000 1111）（第1位设置为 "1"，表示 "中止"）
- SectorCount:0（0000 0000）（不适用）
- DriveHead:160（1010 0000）（无-仅设置过时的位）
- Command146（1001 0010）（Bit 1 设置为 "1"，表示感知数据的可用性）

这告诉我们，设备终止了固件更新操作。

在将 NVMe 设备与 Windows 本机 NVMe 驱动程序 (StorNVMe.sys) 搭配使用时，该通道中可获得类似级别的调试信息。
