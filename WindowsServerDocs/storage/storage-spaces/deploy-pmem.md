---
title: 了解和部署永久性内存
description: 有关永久性内存的详细信息以及如何在 Windows Server 2019 中通过存储空间直通对其进行设置的详细信息。
keywords: 存储空间直通、永久性内存、pmem、存储、S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 497fa201c500919fc857d25166d37ce87613d0f0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872009"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>了解和部署永久性内存

>适用于：Windows Server 2019

永久性内存（或 PMem）是一种新类型的内存技术，可提供经济实惠的大容量和持久性的独特组合。 本主题提供有关 PMem 的背景以及与 Windows Server 2019 一起部署存储空间直通的步骤。

## <a name="background"></a>后台

PMem 是一种非易失性 DRAM （NVDIMM），它的速度为 DRAM，但通过电源周期保留其内存内容（即使在出现意外断电、用户启动关机、系统崩溃、等等）。 因此，从停止的位置恢复的速度要快得多，因为无需重新加载 RAM 的内容。 另一种独特的特征是，PMem 是字节可寻址的，这意味着你还可以将其用作存储（这就是你可能会听到 PMem 称为存储类内存的原因）。


若要查看其中一些优点，请查看 Microsoft Ignite 2018 中的此演示：

[![Microsoft Ignite 2018 Pmem 演示](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

任何提供容错的存储系统都需要提供分布式写入的副本，该副本必须遍历网络并导致后端写入放大。 出于此原因，通常只通过读取来实现绝对最大的 IOPS 基准编号，尤其是在存储系统有可能的情况下从本地副本中读取时，尤其是在可能的情况下，这存储空间直通。

**如果读取了 100%，则群集提供 13798674 IOPS。**

![13.7 m IOPS 记录屏幕快照](media/deploy-pmem/iops-record.png)

如果你密切观看视频，你会注意到，thatwhat 的 jaw 是延迟：即使在超过13.7 的 IOPS，Windows 中的文件系统报告的延迟始终小于40μs！ （这是毫秒数的符号，一秒的秒。）这比典型的所有闪存供应商骄傲公布的速度要快得多。

同时，Windows Server 2019 和 Intel® Optane 中的存储空间直通™ DC 持久性内存提供了突破性的性能。 这项业界领先的13.7 的 HCI 基准，具有可预测和极低的延迟，这比我们先前业界领先的 6.7 M IOPS 基准更多了两倍。 而且，这一次我们只需要12个服务器节点，25% 的时间少于两年前。

![IOPS 收益](media/deploy-pmem/iops-gains.png)

使用的硬件是使用三向镜像和分隔的 ReFS 卷、 **12** x INTEL® S2600WFT、 **384 GiB** memory、2 x 28-核心 "CASCADELAKE"、 **1.5 TB** Intel® Optane™ DC 永久性内存作为缓存、 **32 TB** NVMe （4 x 8TB Intel® DC P4510）容量， **2** x Mellanox ConnectX-4 25 Gbps

下表具有完整的性能数字： 

| 测                   | 性能         |
|-----------------------------|---------------------|
| 4K 100% 随机读取         | 13800000 IOPS   |
| 4K 90/10% 随机读/写 | 9450000 IOPS   |
| 2MB 按顺序读取         | 549 GB/秒的吞吐量 |

### <a name="supported-hardware"></a>受支持的硬件

下表显示了 Windows Server 2019 和 Windows Server 2016 支持的永久性内存硬件。 请注意，Intel Optane 专门支持内存模式和应用直接模式。 Windows Server 2019 支持混合模式操作。

| 永久性内存技术                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| 应用中的**Nvdimm-n**模式                                       | 支持                | 支持                |
| Intel Optane 在应用程序-直接模式下**™ DC 永久性内存**             | 不支持            | 支持                |
| Intel Optane 在二级内存模式下**™ DC 永久性内存**（2LM） | 不支持            | 支持                |

现在，让我们深入了解如何配置永久性内存。

## <a name="interleave-sets"></a>交错集

### <a name="understanding-interleave-sets"></a>了解交错集

请记住，NVDIMM N 位于标准 DIMM （内存）槽中，将数据放置在靠近处理器的位置（从而降低延迟并获取更好的性能）。 若要构建这一点，有两个或更多 NVDIMMs 创建一个 N 向交叉交错集，以提供条带读/写操作来提高吞吐量。 最常见的设置是双向或四向交叉。

通常可以在平台的 BIOS 中创建交错集，使多个永久性内存设备显示为 Windows Server 的单个逻辑磁盘。 每个永久性内存逻辑磁盘都包含一组交错的物理设备，方法是运行：

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

我们可以看到，逻辑 pmem 磁盘 #2 包含 Id20 和 Id120 的物理设备，并且逻辑 pmem 磁盘 #3 具有 Id1020 和 Id1120 的物理设备。 我们还可以将特定的 pmem 磁盘送进到 PmemPhysicalDevice，以便在交错集中获取其所有物理 NVDIMMs，如下所示。


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

若要配置交错集，请运行以下 PowerShell cmdlet：

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

这会显示未分配给系统中的逻辑持久性内存磁盘的所有永久性内存区域。

若要查看系统中的所有永久性内存设备信息（包括设备类型、位置、运行状况和操作状态等），可以在本地服务器上运行以下 cmdlet：

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

由于我们有可用的未使用的 pmem 区域，因此可以创建新的持久性内存磁盘。 可以通过以下方式使用未使用的区域创建多个持久性内存磁盘：

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

完成此操作后，可以通过运行以下内容来查看结果之后：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

值得注意的是，我们可以运行**PhysicalDisk |其中，媒体存储-Eq SCM** ，而不是**PmemDisk**以获得相同的结果。 新创建的永久性内存磁盘对应于 PowerShell 和 Windows 管理中心中显示的驱动器1:1。

### <a name="using-persistent-memory-for-cache-or-capacity"></a>使用持久性内存作为缓存或容量

Windows Server 2019 上的存储空间直通支持使用持久性内存作为缓存或容量驱动器。 有关设置缓存和容量驱动器的详细信息，请参阅此[文档](understand-the-cache.md)。

## <a name="creating-a-dax-volume"></a>创建 DAX 卷

### <a name="understanding-dax"></a>了解 DAX

可以通过两种方法来访问永久性内存。 它们分别是：

1. **直接访问（DAX）** ，其操作类似于内存来获得最低延迟。 应用直接修改持久性内存，绕过堆栈。 请注意，这只能与 NTFS 一起使用。
2. **阻止访问**，它的操作类似于应用程序兼容性的存储。 在此设置中，数据流经堆栈，这可以与 NTFS 和 ReFS 一起使用。

下面是一个示例：

![DAX 堆栈](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>配置 DAX

我们将需要使用 PowerShell cmdlet 在永久性内存上创建 DAX 卷。 通过使用 **-IsDax**开关，我们可以将卷格式化为启用 DAX。

```PowerShell
Format-Volume -IsDax:$true
```

以下代码片段将帮助你在永久性内存磁盘上创建 DAX 卷。

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

使用持久性内存时，监视体验有一些不同之处：

1. 永久性内存不会创建物理磁盘性能计数器，因此看不到在 Windows 管理中心的图表上是否出现。
2. 永久性内存不会创建 Storport 505 数据，因此不会获得主动离群检测。

除此之外，监视体验与任何其他物理磁盘相同。 可以通过运行以下内容来查询永久性内存磁盘的运行状况：

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

**HealthStatus**显示永久性内存磁盘是否正常。 **UnsafeshutdownCount**跟踪可能导致此逻辑磁盘上的数据丢失的关闭次数。 这是此磁盘的所有底层持久性内存设备的不安全关闭计数的总和。 我们还可以使用以下命令来查询运行状况信息。 **OperationalStatus**和**OperationalDetails**提供有关运行状况状态的详细信息。

查询永久性内存设备的运行状况：

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

这表明哪些永久性内存设备处于不正常状态。 不正常的设备（**DeviceId**）20与上述示例中的大小写匹配。 BIOS 中的**PhysicalLocation**可帮助确定哪些永久性内存设备处于错误状态。

## <a name="replacing-persistent-memory"></a>替换持久性内存

以上我们介绍了如何查看持久性内存的运行状况状态。 如果需要更换发生故障的模块，则需要重新预配永久性内存磁盘（请参阅上文所述的步骤）。

进行故障排除时，可能需要使用**PmemDisk**，这将删除特定的持久性内存磁盘。 可以通过以下方式删除所有当前持久磁盘：

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

请务必注意，删除永久性内存磁盘将导致该磁盘上的数据丢失。

可能需要的另一个 cmdlet 为**PmemPhysicalDevice**，它将初始化物理持久性内存设备上的标签存储区域。 这可用于清除永久性内存设备上损坏的标签存储信息。

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

请注意，应将此命令用作解决永久性内存相关问题的最后手段。 这将导致持久性内存丢失数据。

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [Windows 中的存储类内存（NVDIMM-N）运行状况管理](storage-class-memory-health.md)
- [了解缓存](understand-the-cache.md)