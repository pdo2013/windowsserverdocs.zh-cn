---
title: 了解和部署永久性内存
description: 永久性内存以及如何将其设置用于存储空间直通在 Windows Server 2019 的详细的信息。
keywords: 存储空间直通，永久性内存、 pmem、 存储、 S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: ed4b2669ad35a2ce0f818c65f7024ce905d9e4d6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280046"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>了解和部署永久性内存

>适用于：Windows Server 2019

永久性内存 （或 PMem） 是一种新内存技术，可提供唯一的经济实惠的大容量和持久性。 本主题提供背景 PMem 和步骤，若要使用 Windows Server 2019 使用存储空间直通部署它。

## <a name="background"></a>后台

PMem 是一种类型的非易失性 DRAM (NVDIMM) DRAM，速度但会保留其内存的内容经过 （内存内容保持，即使系统电源出现故障发生的意外的断电造成，用户启动关机，在系统崩溃时的电源周期等）。 正因为如此，从离开的位置恢复是明显更快的因为 RAM 的内容不需要重新加载。 另一个唯一特征是 PMem 是字节可寻址的这意味着您还可以使用它作为存储 （这是您可能会听到 PMem 称为存储类内存的原因）。


若要查看其中一些益处，让我们看看这来自 Microsoft ignite 大会 2018年演示：

[![Microsoft ignite 大会 2018 Pmem 演示](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

任何一定提供容错能力的存储系统使分布式的而必须遍历网络，因此会带来后端写入放大写入的副本。 出于此原因，绝对最大 IOPS 基准数字通常会实现仅读取，尤其是在存储系统具有的常识优化，以从本地副本，只要有可能，读取的存储空间直通 does。

**通过 100%读取群集提供了 13,798,674 IOPS。**

![13.7 m IOPS 记录屏幕快照](media/deploy-pmem/iops-record.png)

如果仔细观看视频，您会注意到 thatwhat 的更多不可思议时的延迟是： 在 Windows 中的文件系统甚至是在超过 13.7 M IOPS，报告将始终小于 40 祍的延迟 ！ （这是符号为微秒，百万分之一秒。）这比什么典型所有闪存供应商非常自豪地公布今天更快地是一个数量级。

在一起，存储空间直通在 Windows Server 2019 和 Intel® Optane™ DC 永久性内存中提供突破性的性能。 可预测和极低延迟的 13.7 M IOPS，超过此行业领先 HCI 基准测试是多个双精度我们以前行业领先的基准的 6.7 M IOPS。 什么是多个，这次我们需要的只是 12 个服务器节点，少于两年前的 25%。

![IOPS 提升](media/deploy-pmem/iops-gains.png)

使用的硬件已使用三向镜像的 12 服务器群集和分隔 ReFS 卷**12** x Intel® S2600WFT **384 GiB**内存，2 x 28 core"CascadeLake" **1.5 TB**Intel® Optane™ DC 持久内存中的缓存中，作为**32 TB** NVMe (4 x 8 TB Intel® DC P4510) 作为容量**2** x Mellanox ConnectX 4 25 Gbps

下表提供了完整的性能数字： 

| 基准检验                   | 性能         |
|-----------------------------|---------------------|
| 4k 100%随机读取         | 为 13.8 百万的 IOPS   |
| 4 K 90 / 10%随机读/写 | 9.45 百万的 IOPS   |
| 2MB 顺序读         | 549 GB/秒的吞吐量 |

### <a name="supported-hardware"></a>受支持的硬件

下表显示了 Windows Server 2019 和 Windows Server 2016 的受支持的永久性内存硬件。 请注意，Intel Optane 专门支持内存模式和应用程序直接模式。 Windows Server 2019 支持混合模式操作。

| 永久性内存技术                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM N**在应用程序直接模式下                                       | 支持                | 支持                |
| **Intel Optane™ DC 永久性内存**在应用程序直接模式下             | 不支持            | 支持                |
| **Intel Optane™ DC 永久性内存**两个级别的内存中模式 (2LM) | 不支持            | 支持                |

现在，让我们深入了解如何配置永久性内存。

## <a name="interleave-sets"></a>交错集

### <a name="understanding-interleave-sets"></a>了解交错集

前面曾提到，NVDIMM N 驻留在标准的 DIMM （内存） 槽、 放置数据更接近于处理器 （因此，减少延迟和提取更好的性能）。 若要生成对此，交错一是两个或多个 NVDIMMs 创建设置为提供带区形式读/写操作，以便更高的吞吐量 N 向 interleave 时。 最常见的安装是双向或 4 路交错执行。

通常可以在某个平台的 BIOS，使多个永久性内存设备显示为单个逻辑磁盘到 Windows Server 中创建交错的集合。 永久性内存的每个逻辑磁盘通过运行包含一组交错的物理设备：

```PowerShell
Get-PmemDisk
```

示例输出如下所示：

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

我们可以看到逻辑 pmem 磁盘 #2 的 Id20 和 Id120 的物理设备和逻辑 pmem 磁盘 #3 的 Id1020 和 Id1120 的物理设备。 我们还可以为 Get-PmemPhysicalDevice 来获取所有其物理 NVDIMMs 中按如下所示设置交错馈送特定 pmem 磁盘。


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

示例输出如下所示：

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>配置交错集

若要配置的交错集，请运行以下 PowerShell cmdlet:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

这将显示所有未分配给系统上的逻辑永久性内存磁盘的永久性内存区域。

若要查看所有永久性内存设备信息在系统中，包括设备类型、 位置、 运行状况和操作状态等您可以运行以下 cmdlet 在本地服务器上：

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile
                                                                                                                      memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

由于我们有可用的未使用的 pmem 区域，我们可以创建新的永久性内存磁盘。 我们可以创建多个永久性内存磁盘使用的未使用的区域：

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

这是的之后，我们可以看到的结果，通过运行：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

值得注意是我们可以运行具有**Get-physicaldisk |其中 MediaType-Eq SCM**而不是**Get PmemDisk**以获得相同的结果。 新创建的永久性内存磁盘对应 1:1 到 PowerShell 和 Windows Admin Center 中显示的驱动器。

### <a name="using-persistent-memory-for-cache-or-capacity"></a>永久性内存用于缓存或容量

存储空间直通上 Windows Server 2019 支持使用缓存或容量驱动器作为永久性内存。 请参阅此[文档](understand-the-cache.md)有关设置缓存和容量的驱动器的详细信息。

## <a name="creating-a-dax-volume"></a>创建 DAX 卷

### <a name="understanding-dax"></a>了解 DAX

有两种方法用于访问持久的内存。 它们分别是：

1. **直接访问 (DAX)** ，该内存以获取最低延迟等操作。 应用程序直接修改持久的内存，绕过堆栈。 请注意，这只能在使用 NTFS。
2. **阻止访问**，应用程序兼容性的存储空间等进行操作。 数据流通过这种设置，在堆栈，这可以用于 NTFS 和 ReFS。

出现这种可以如下所示：

![DAX 堆栈](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>配置 DAX

我们将需要使用 PowerShell cmdlet 来创建 DAX 卷上持久的内存。 通过使用 **-IsDax**开关，我们可以将卷格式化为 DAX 启用。

```PowerShell
Format-Volume -IsDax:$true
```

下面的代码段将帮助您在永久性内存磁盘上创建 DAX 卷。

```PowerShell
# Here we use the first pmem disk to create the volume as an example
$disk = (Get-PmemDisk)[0] | Get-PhysicalDisk | Get-Disk
# Initialize the disk to GPT if it is not initialized
If ($disk.partitionstyle -eq "RAW") {$disk | Initialize-Disk -PartitionStyle GPT}
# Create a partition with drive letter 'S' (can use any available drive letter)
$disk | New-Partition -DriveLetter S -UseMaximumSize


   DiskPath: \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{53f56307-b6
bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                               Size Type
---------------  ----------- ------                                               ---- ----
2                S           16777216                                        251.98 GB Basic

# Format the volume with drive letter 'S' to DAX Volume
Format-Volume -FileSystem NTFS -IsDax:$true -DriveLetter S

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
S                        NTFS           Fixed     Healthy      OK                    251.91 GB 251.98 GB

# Verify the volume is DAX enabled
Get-Partition -DriveLetter S | fl


UniqueId             : {00000000-0000-0000-0000-000100000000}SCMLD\VEN_8980&DEV_097A&SUBSYS_89804151&REV_0018\3&1B1819F6&0&03018089F
                       B63494DB728D8418B3CBBF549997891:WIN-8KGI228ULGA
AccessPaths          : {S:\, \\?\Volume{cf468ffa-ae17-4139-a575-717547d4df09}\}
DiskNumber           : 2
DiskPath             : \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{5
                       3f56307-b6bf-11d0-94f2-00a0c91efb8b}
DriveLetter          : S
Guid                 : {cf468ffa-ae17-4139-a575-717547d4df09}
IsActive             : False
IsBoot               : False
IsHidden             : False
IsOffline            : False
IsReadOnly           : False
IsShadowCopy         : False
IsDAX                : True                   # <- True: DAX enabled
IsSystem             : False
NoDefaultDriveLetter : False
Offset               : 16777216
OperationalStatus    : Online
PartitionNumber      : 2
Size                 : 251.98 GB
Type                 : Basic
```

## <a name="monitoring-health"></a>监视运行状况

当使用永久性内存时，有的监视体验中的一些差异：

1. 永久性内存不会创建性能计数器，因此如果不会看到显示 Windows Admin Center 中的图表的物理磁盘。
2. 永久性内存不会创建 Storport 505 数据，因此不会获得主动离群值检测。

除了，监视体验是与任何其他物理磁盘相同。 您可以通过运行查询的永久性内存磁盘运行状况：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Unhealthy    None          True         {20, 120}         2
3          252 GB Healthy      None          True         {1020, 1120}      0

Get-PmemDisk | Get-PhysicalDisk | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails

SerialNumber               HealthStatus OperationalStatus  OperationalDetails
------------               ------------ ------------------ ------------------
802c-01-1602-117cb5fc      Healthy      OK
802c-01-1602-117cb64f      Warning      Predictive Failure {Threshold Exceeded,NVDIMM_N Error}
```

**HealthStatus**显示永久性内存磁盘是否正常运行。 **UnsafeshutdownCount**跟踪可能会导致数据丢失此逻辑磁盘的关闭数。 它是此磁盘的所有基础的永久性内存设备的不安全关闭计数之和。 我们还可以使用以下命令查询运行状况信息。 **OperationalStatus**并**OperationalDetails**提供有关运行状况状态的详细信息。

若要查询的永久内存设备的运行状况：

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

这将显示哪些永久性内存设备处于不正常状态。 不正常的设备 (**DeviceId**) 20 匹配在上面的示例用例。 **PhysicalLocation**从 BIOS 可以帮助识别哪些永久性内存设备处于错误状态。

## <a name="replacing-persistent-memory"></a>替换为永久性内存

更高版本，我们介绍了如何查看的永久性内存运行状况状态。 如果您需要更换发生故障的模块，你将需要重新预配永久性内存磁盘 （请参阅我们上面所述的步骤）。

故障排除时，可能需要使用**删除 PmemDisk**，这将删除特定的永久性内存磁盘。 我们可以删除所有当前的永久性磁盘，通过：

```PowerShell
Get-PmemDisk | Remove-PmemDisk

cmdlet Remove-PmemDisk at command pipeline position 1
Supply values for the following parameters:
DiskNumber: 2

This will remove the persistent memory disk(s) from the system and will result in data loss.
Remove the persistent memory disk(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
Removing the persistent memory disk. This may take a few moments.
```

请务必注意，删除永久性内存磁盘会导致数据丢失该磁盘上。

可能需要的另一个 cmdlet 是**Initialize PmemPhysicalDevice**，将初始化物理永久性内存设备上的标签存储区域。 这可以用于清除损坏的标签永久性内存设备上存储信息。

```PowerShell
Get-PmemPhysicalDevice | Initialize-PmemPhysicalDevice

This will initialize the label storage area on the physical persistent memory device(s) and will result in data loss.
Initializes the physical persistent memory device(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): A
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
```

务必要注意，使用此命令应作为最后的手段来修复永久性内存相关的问题。 它将导致数据丢失，对持久的内存。

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [在 Windows 中的存储类内存 (NVDIMM N) 运行状况管理](storage-class-memory-health.md)
- [了解缓存](understand-the-cache.md)