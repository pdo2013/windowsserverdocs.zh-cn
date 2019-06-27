---
ms.assetid: 2bab6bf6-90e7-46a7-b917-14a7a8f55366
title: Windows 中的存储类内存 (NVDIMM N) 的运行状况管理
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: JasonGerend
ms.date: 06/25/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4ebec8618c79c43816680387ae5e495f125b3c54
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407555"
---
# <a name="storage-class-memory-nvdimm-n-health-management-in-windows"></a>Windows 中的存储类内存 (NVDIMM N) 的运行状况管理

> 适用于：Windows Server 2019，Windows Server 2016、 Windows Server （半年频道），Windows 10

针对 Windows 中的存储类内存 (NVDIMM N) 设备，本文向系统管理员和 IT 专业人员提供了有关错误处理和运行状况管理的信息，重点强调存储类内存设备和传统存储设备之间的差异。

如果不熟悉 Windows 对存储类内存设备的支持，请观看以下短片了解概况：
- [将非易失性内存 (NVDIMM N) 用作 Windows Server 2016 中的块存储](https://channel9.msdn.com/Events/Build/2016/P466)
- [使用作为 Windows Server 2016 中的字节寻址存储的非易失性内存 (NVDIMM N)](https://channel9.msdn.com/Events/Build/2016/P470)
- [提高 SQL Server 2016 性能与 Windows Server 2016 中的永久性内存](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-Windows-Server-2016-SCM--FAST)

另请参阅[了解和部署存储空间直通中的永久性内存](deploy-pmem.md)。

自 Windows Server 2016 和 Windows 10（1607 版）起，具有本机驱动程序的 Windows 支持符合 JEDEC 的 NVDIMM-N 存储类内存设备。 虽然这些设备的行为与其他磁盘 （HDD 和 SSD）类似，但也存在一些差异。

此处列出的所有情况都极少发生，具体取决于硬件的使用状况。

以下各种实例可能引用了存储空间配置。 令人感兴趣的特别配置是：两个 NVDIMM N 设备在存储空间中用作一个镜像回写式缓存。 要设置此配置，请参阅[配置具有 NVDIMM-N 回写式缓存的存储空间](https://msdn.microsoft.com/library/mt650885.aspx)。

在 Windows Server 2016 中，存储空间 GUI 显示 NVDIMM N 总线类型的状态为“未知”。 创建池、存储 VD 期间无任何功能丢失或失效。 可以通过运行以下命令来验证总线类型：

```powershell
PS C:\>Get-PhysicalDisk | fl
```
cmdlet 输出中的 BusType 参数会正确地将总线类型显示为“SCM”

## <a name="checking-the-health-of-storage-class-memory"></a>检查存储类内存的运行状况
要查询存储类内存的运行状况，请在 Windows PowerShell 会话中使用以下命令。

```powershell
PS C:\> Get-PhysicalDisk | where BusType -eq "SCM" | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails
```

执行此操作将生成此示例输出：

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | 正常 | 确定 | |
| 802c-01-1602-117cb64f | 警告 | 预计故障 | {Threshold Exceeded,NVDIMM\_N Error} |

> [!NOTE]
> 若要查找事件中指定的 NVDIMM-N 设备的物理位置，请在事件查看器中事件的**详细信息**选项卡上，转到 **EventData** > **位置**。 请注意，Windows Server 2016 列出的 NVDIMM N 设备位置不正确，但 Windows Server 版本 1709 中对此进行了修复。

有关了解各种运行状况的帮助，请参阅以下部分。

## <a name="warning-health-status"></a>“警告”运行状况状态

这种情况即：检查存储类内存设备的运行状况时，发现它的“运行状况状态”显示为“**警告**”，如此示例输出中所示：

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | 正常 | 确定 | |
| 802c-01-1602-117cb64f | 警告 | 预计故障 | {Threshold Exceeded,NVDIMM\_N Error} |

下表列出了一些有关这种情况的信息。

| | 描述 |
| --- | --- |
| 可能的情况 | NVDIMM-N 警告阈值违例 |
| 根本原因 | NVDIMM-N 设备追踪各种阈值，例如温度、NVM 生存期和/或能量源生存期。 当超出其中一个阈值时，操作系统将会收到通知。 |
| 常规特性 | 设备保持完全正常运行。 这是警告，不是错误。 |
| 存储空间性能 | 设备保持完全正常运行。 这是警告，不是错误。 |
| 详细信息 | PhysicalDisk 对象的 OperationalStatus 字段。 EventLog – Microsoft-Windows-ScmDisk0101/Operational |
| 要执行的操作 | 视警告阈值违例而定，可能需要谨慎考虑是替换整个 NVDIMM-N 还是替换其中某些部分。 例如，如果 NVM 生存期阈值已经违例，那么替换 NVDIMM-N 是可以的。 |

## <a name="writes-to-an-nvdimm-n-fail"></a>写入 NVDIMM-N 失败

这种情况即：检查存储类内存设备的运行状况时，发现“运行状况状态”显示为“**不正常**”，“操作状态”显示为“**IO 错误**”，如此示例输出中所示：

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | 正常 | 确定 | |
| 802c-01-1602-117cb64f | Unhealthy | {元数据已过时, IO 错误, 暂时性错误} | {丢失数据持久性, 丢失数据, NV...} |

下表列出了一些有关这种情况的信息。

| | 描述 |
| --- | --- |
| 可能的情况 | 持久性丢失/备份电源 |
|根本原因|NVDIMM-N 设备依赖备份电源（通常是电池或超级电容）获得持久性。 如果此备份电源的源不可用或设备出于任何原因（控制器/闪存错误）无法执行备份，那么数据将处于危险之中，Windows 将阻止对受影响设备的任何进一步写入操作。 仍有可能执行“读取”操作以疏散数据。|
|常规特性|NTFS 卷将被卸除。<br>所有受影响的 NVDIMM-N 设备的 PhysicalDisk 运行状况状态字段将显示为“不正常”。|
|存储空间性能|只有一个 NVDIMM-N 受影响的情况下，存储空间仍可正常运行。 如果多个设备受到影响，则将无法写入到存储空间。 <br>所有受影响的 NVDIMM-N 设备的 PhysicalDisk 运行状况状态字段将显示为“不正常”。|
|详细信息|PhysicalDisk 对象的 OperationalStatus 字段。<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|要执行的操作|我们建议备份受影响的 NVDIMM-N 的数据。 要获得读取访问权限，可以手动使磁盘联机（将以只读 NTFS 卷形式出现）。<br><br>要完全清除这种情况，必须解决根本原因（即提供电源或更换 NVDIMM-N，视具体问题而定），NVDIMM-N 上的卷必须脱机并再次进入联机状态或重新启动系统。<br><br>若要使 NVDIMM N 再次在存储空间中可用，请使用 **Reset-PhysicalDisk** cmdlet，它将重新集成设备并启动修复进程。|

## <a name="nvdimm-n-is-shown-with-a-capacity-of-0-bytes-or-as-a-generic-physical-disk"></a>NVDIMM-N 显示“0”字节容量，或显示为“通用物理磁盘”

这种情况即：存储类内存设备显示 0 字节容量，无法初始化或公开为操作状态显示“**通信中断**”的“通用物理磁盘”对象，如此示例输出中所示：

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|正常|确定||
||警告|通信中断||

下表列出了一些有关这种情况的信息。

||描述|
|---|---|
|可能的情况|BIOS 未将 NVDIMM N 公开到操作系统|
|根本原因|NVDIMM N 设备基于 DRAM。 当引用损坏的 DRAM 地址时，大多数 CPU 将启动计算机检查并重新启动服务器。 然后，某些服务器平台取消映射 NVDIMM，以阻止操作系统访问相应平台，并可能会引起其他计算机检查。 如果 BIOS 检测到 NVDIMM N 已失败并且需要更换，这种情况同样也可能发生。|
|常规特性|NVDIMM-N 显示为未初始化、容量为 0 字节，并且无法读取或写入。|
|存储空间性能|存储空间仍可运行（前提是只有 1 个 NVDIMM N 受影响）。<br>NVDIMM-N PhysicalDisk 对象的“运行状况状态”显示为“警告”和“通用物理磁盘”|
|详细信息|PhysicalDisk 对象的 OperationalStatus 字段。 <br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|要执行的操作|NVDIMM-N 设备必须经过替换或净化，以便服务器平台将其再次公开给主机操作系统。 建议替换设备，因为可能会发生其他无法纠正的错误。 使用 **Add-physicaldisk** cmdlet 可以将替换设备添加到存储空间配置。|

## <a name="nvdimm-n-is-shown-as-a-raw-or-empty-disk-after-a-reboot"></a>重启后，NVDIMM-N 显示为 RAW 或空磁盘

这种情况即：检查存储类内存设备的运行状况时，发现“运行状况状态”显示为“**不正常**”，“操作状态”显示为“**无法识别的元数据**”，如此示例输出中所示：

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|正常|确定|{未知}|
|802c-01-1602-117cb64f|Unhealthy|{无法识别的元数据, 元数据已过时}|{未知}|

下表列出了一些有关这种情况的信息。

||描述|
|---|---|
|可能的情况|备份/还原失败|
|根本原因|备份或还原过程失败可能会导致 NVDIMM-N 上的所有数据丢失。 加载操作系统时，它将显示为一个没有分区或文件系统的全新 NVDIMM-N 并呈现为 RAW，这意味着它不具有文件系统。|
|常规特性|NVDIMM-N 将处于只读模式。 执行显式用户操作才能再次使用。|
|存储空间性能|如果只有一个 NVDIMM 受到影响，那么存储空间仍可以运行。<br>NVDIMM-N 物理磁盘对象的运行状况状态将显示为“不正常”，无法用于存储空间。|
|详细信息|PhysicalDisk 对象的 OperationalStatus 字段。<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|要执行的操作|如果用户不想替换受影响的设备，他们可以使用 **Reset-PhysicalDisk** cmdlet 清除受影响的 NVDIMM-N 上的只读条件。 在存储空间环境中，这也会尝试将 NVDIMM-N 重新集成到存储空间并启动修复进程。|

## <a name="interleaved-sets"></a>交错的集合

交错的集合通常在平台的 BIOS 中创建，使多个 NVDIMM-N 设备显示为主机操作系统的单个设备。

Windows Server 2016 和 Windows 10 Anniversary Edition 不支持 NVDIMM-N 的交错的集合。

在撰写本文时，没有主机操作系统在这种集合中正确识别单个 NVDIMM-N、清楚地告知用户哪个特定设备可能导致错误或需要进行维修的机制。


