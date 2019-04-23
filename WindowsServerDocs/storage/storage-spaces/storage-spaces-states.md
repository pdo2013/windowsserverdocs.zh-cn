---
title: 存储空间直通的运行状况和操作状态
description: 如何找到并了解不同的运行状况和操作状态的存储空间直通和存储空间 （包括物理磁盘、 池和虚拟磁盘），以及如何处理它们。
keywords: 存储空间，分离虚拟磁盘、 物理磁盘已降级
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 5090a68270438bd9a06c7d50f9d4abca066d31e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849138"
---
# <a name="troubleshoot-storage-spaces-direct-health-and-operational-states"></a>存储空间直通的运行状况和操作状态进行故障排除

> 适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012，Windows Server （半年频道），Windows 10 中，Windows 8.1

本主题介绍的运行状况和操作状态的存储池，（位于存储空间中的卷的下方） 的虚拟磁盘，并驱动器[存储空间直通](storage-spaces-direct-overview.md)并[存储空间](overview.md)。 当尝试解决各种问题，例如为何无法删除虚拟磁盘由于只读配置时，这些状态会很有价值。 它还介绍了为什么不能将驱动器添加到池 (CannotPoolReason)。

存储空间的三个主要对象的*物理磁盘*(硬盘驱动器，Ssd，等) 添加到*存储池*，以便可以创建虚拟化存储以*的虚拟磁盘*从可用在池中的空间，如下所示。 池的元数据写入到池中的每个驱动器。 卷的虚拟磁盘顶层创建和存储的文件，但我们不打算讨论一下此处的卷。

![物理磁盘添加到存储池和虚拟磁盘将通过创建的池空间](media/storage-spaces-states/storage-spaces-object-model.png)

在服务器管理器中，或使用 PowerShell，可以查看运行状况和操作状态。 下面是示例的各种 （主要是错误） 的运行状况和缺少大部分其群集节点的存储空间直通群集上的操作状态 (右键单击要添加的列标头**操作状态**)。 这并不满意的群集。

![服务器管理器中存储空间直通群集-缺失的物理磁盘和中的不正常状态的虚拟磁盘的很多显示缺少的两个节点的结果](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>存储池状态

每个存储池的运行状况状态-**正常**，**警告**，或**未知**/**不正常**、 也作为一个或多个操作状态。

若要确定池处于什么状态，请使用以下 PowerShell 命令：

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

下面是在使用只读的操作状态的未知运行状况状态中显示存储池的示例输出：

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

以下部分列出的运行状况和操作状态。

### <a name="pool-health-state-healthy"></a>池运行状况状态：正常

|操作状态    |描述|
|---------            |---------  |
|确定|存储池处于正常状态。|

### <a name="pool-health-state-warning"></a>池运行状况状态：警告

当存储池处于**警告**运行状况状态，则表示池是可访问，但一个或多个驱动器失败或缺少。 因此，您的存储池可能降低恢复能力。

|操作状态    |描述|
|---------            |---------  |
|降级|在存储池中有故障或丢失的驱动器。 这种情况只会出现托管池元数据的驱动器。 <br><br>**操作**:检查你的驱动器的状态和有其他失败之前替换失败的任何驱动器。|

### <a name="pool-health-state-unknown-or-unhealthy"></a>池运行状况状态：未知或不正常

当存储池处于**未知**或**不正常**运行状况状态时，它表示存储池是只读的并返回到池之前，无法修改**警告**或**确定**运行状况状态。

|操作状态    |只读的原因 |描述|
|---------            |---------       |--------   |
|只读|不完整|发生这种情况的存储池将失去其[仲裁](understand-quorum.md)，这意味着在池中的大多数驱动器出现故障或出于某种原因均处于脱机状态。 当池失去其仲裁时，存储空间自动设置池配置为只读的直到足够的驱动器再次变得可用。<br><br>**操作：** <br>1.重新连接任何缺少的驱动器，并且如果使用存储空间直通，使联机的所有服务器。 <br>2.将池设置回为读写通过使用管理权限打开 PowerShell 会话，然后键入：<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||策略|管理员将存储池设置为只读。<br><br>**操作：** 若要设置为读写访问权限在故障转移群集管理器中的群集的存储池，请转到**池**，右键单击该池，然后选择**联机**。<br><br>对于其他服务器和 Pc，使用管理权限打开 PowerShell 会话，然后键入：<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Starting|存储空间是启动或等待才能连接池中的驱动器。 这应该是临时状态。 完全启动后，池应转换为不同的操作状态。<br><br>**操作：** 如果在池中停留在*正在启动*状态，请确保已正确连接池中的所有驱动器。|

另请参阅[修改存储池具有只读配置](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx)。

## <a name="virtual-disk-states"></a>虚拟磁盘状态

在存储空间中的卷放置从中分割出在池中的可用空间的虚拟磁盘 （存储空间） 上。 每个虚拟磁盘的运行状况状态-**正常**，**警告**，**不正常**，或者**未知**以及一个或多个操作状态。

若要找出什么状态的虚拟磁盘中，请使用以下 PowerShell 命令：

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

下面是示例输出显示了已分离的虚拟磁盘和性能下降或不完整的虚拟磁盘：

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

以下部分列出的运行状况和操作状态。

### <a name="virtual-disk-health-state-healthy"></a>虚拟磁盘运行状况状态：正常

|操作状态    |描述|
|---------            |---------          |
|确定    |虚拟磁盘处于正常状态。|
|非最优    |不是均匀地在驱动器写入数据。 <br><br>**操作**:通过运行优化存储池中的驱动器使用情况[Optimize-storagepool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) cmdlet。|

### <a name="virtual-disk-health-state-warning"></a>虚拟磁盘运行状况状态：警告

当虚拟磁盘处于**警告**运行状况状态，则表示你的数据的一个或多个副本都不可用，但在存储空间仍可以读取数据的至少一个副本。

|操作状态    |描述|
|---------            |---------          |
|在服务中            |添加或删除驱动器后，Windows 如修复虚拟磁盘。 修复完成后，虚拟磁盘应返回到正常的运行状况状态。|
|不完整           |虚拟磁盘的复原能力被减少，因为一个或多个驱动器失败或缺少。 但是，缺少驱动器包含你的数据的最新副本。<br><br> **操作**: <br>1.重新连接任何缺少的驱动器，替换任何失败的驱动器，并使用存储空间直通，如果联机处于脱机状态的任何服务器。 <br>2.如果您不使用存储空间直通下, 一步修复虚拟磁盘使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。<br> 存储空间直通会自动启动修复如果需要后重新连接或更换驱动器。|
|降级             |虚拟磁盘的复原能力被减少，因为一个或多个驱动器失败或缺失，并且有过时的数据副本在这些驱动器上。 <br><br>**操作**: <br> 1.重新连接任何缺少的驱动器，替换任何失败的驱动器，并使用存储空间直通，如果联机处于脱机状态的任何服务器。 <br> 2.如果您不使用存储空间直通下, 一步修复虚拟磁盘使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。 <br>存储空间直通会自动启动修复如果需要后重新连接或更换驱动器。|

### <a name="virtual-disk-health-state-unhealthy"></a>虚拟磁盘运行状况状态：Unhealthy

当虚拟磁盘处于**不正常**运行状况状态、 部分或全部虚拟磁盘上的数据是当前无法访问。

|操作状态    |描述|
|---------            |--------   |
|未冗余|虚拟磁盘已丢失数据，因为过多的驱动器失败。<br><br>**操作**:替换失败的驱动器，然后从备份还原数据。|

### <a name="virtual-disk-health-state-informationunknown"></a>虚拟磁盘运行状况状态：信息/未知

也可以为虚拟磁盘**信息**运行状况状态 （如存储空间控制面板项中所示） 或**未知**运行状况状态 （如下所示在 PowerShell 中） 是否管理员花费了虚拟磁盘脱机或被分离虚拟磁盘。

|操作状态    |已分离的原因 |描述|
|---------            |---------       |--------   |
|Detached             |通过策略            |管理员使虚拟磁盘脱机，或将虚拟磁盘，需要手动附件设置，这种情况下将必须手动在每次重新启动 Windows 时将附加虚拟磁盘。，<br><br>**操作**:使虚拟磁盘返回到联机状态。 若要执行此操作在群集的存储池中的虚拟磁盘时，在故障转移群集管理器中选择**存储** > **池** > **虚拟磁盘**选择显示的虚拟磁盘**脱机**状态，然后选择**联机**。 <br><br>若要使虚拟磁盘返回到联机状态时不在群集中，打开 PowerShell 会话以管理员身份，然后再尝试使用以下命令：<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>若要自动附加所有非群集的虚拟磁盘，Windows 重启后，打开 PowerShell 会话以管理员身份，并使用以下命令：<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |大多数磁盘不正常 |使用此虚拟磁盘的太多驱动器失败、 缺失，或包含陈旧的数据。   <br><br>**操作**: <br> 1.重新连接任何缺少的驱动器，并使用存储空间直通，如果联机处于脱机状态的任何服务器。 <br> 2.所有驱动器和服务器都都处于联机状态后，将为失败的任何驱动器。 请参阅[运行状况服务](../../failover-clustering/health-service-overview.md)有关详细信息。 <br>存储空间直通会自动启动修复如果需要后重新连接或更换驱动器。<br>3.如果您不使用存储空间直通下, 一步修复虚拟磁盘使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。  <br><br>如果更多磁盘失败，也不具有数据的副本和虚拟磁盘不是修复后的中间故障，虚拟磁盘上的所有数据都将永久丢失。 在此令人遗憾的情况下，删除该虚拟磁盘、 创建新的虚拟磁盘，然后从备份中还原。|
|                     |不完整 |没有足够的驱动器都存在要读取的虚拟磁盘。    <br><br>**操作**: <br> 1.重新连接任何缺少的驱动器，并使用存储空间直通，如果联机处于脱机状态的任何服务器。 <br> 2.所有驱动器和服务器都都处于联机状态后，将为失败的任何驱动器。 请参阅[运行状况服务](../../failover-clustering/health-service-overview.md)有关详细信息。 <br>存储空间直通会自动启动修复如果需要后重新连接或更换驱动器。<br>3.如果您不使用存储空间直通下, 一步修复虚拟磁盘使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。<br><br>如果更多磁盘失败，也不具有数据的副本和虚拟磁盘不是修复后的中间故障，虚拟磁盘上的所有数据都将永久丢失。 在此令人遗憾的情况下，删除该虚拟磁盘、 创建新的虚拟磁盘，然后从备份中还原。|
| |Timeout|将虚拟磁盘附加时间太长 <br><br> **操作：** 这不应发生通常情况下，因此您可以尝试查看是否该条件将中的传递时间。 也可以尝试断开连接的虚拟磁盘[Disconnect-virtualdisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) cmdlet，然后使用[Connect-virtualdisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) cmdlet 重新连接它。 |

## <a name="drive-physical-disk-states"></a>驱动器 （物理磁盘） 状态

以下部分介绍可以是驱动器的运行状况状态。 在池中的驱动器都是在 PowerShell 中作为*物理磁盘*对象。

### <a name="drive-health-state-healthy"></a>驱动器运行状况状态：正常

|操作状态    |描述|
|---------            |---------          |
|确定|驱动器处于正常状态。|
|在服务中|驱动器正在执行某些内部管理操作。 操作完成后，驱动器应返回到*确定*运行状况状态。|

### <a name="drive-health-state-warning"></a>驱动器运行状况状态：警告

警告状态可以中的驱动器读取和写入数据已成功但出现问题。

|操作状态    |描述|
|---------            |---------          |
|通信中断|找不到驱动器。 如果使用存储空间直通，这可能是因为服务器已关闭。<br><br>**操作**:如果使用存储空间直通，使所有服务器重新联机。 如果仍未解决它，重新连接该驱动器中，替换它，或尝试获取有关此驱动器的详细诊断信息中使用 Windows 错误报告的故障排除步骤 >[物理磁盘已超时](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out)。|
|从池中删除|从存储池删除驱动器正在存储空间。 <br><br> 这是临时状态。 删除完成时，如果驱动器在仍附加到系统中后, 驱动器将转换为另一个操作状态 （通常是正常的） 在原始池中。|
|启动维护模式|将驱动器放入维护模式，管理员将驱动器放入维护模式后正在存储空间。 这是临时状态-中应该很快就可将驱动器*处于维护模式*状态。|
|处于维护模式|正在停止读取和写入从驱动器，管理员在维护模式下，放置在驱动器。 这通常是之前更新驱动器固件或测试失败时。<br><br>**操作**:若要退出维护模式的驱动器，请使用[禁用 StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) cmdlet。|
|停止维护模式|管理员需要退出维护模式的驱动器和存储空间是在过程中将恢复联机状态的驱动器。 这是临时状态-驱动器应该很快就是在另一种状态的理想情况下*正常*。|
|预计故障|驱动器报告附近出现故障时。<br><br>**操作**:更换驱动器。|
|IO 错误|出现临时错误访问驱动器。<br><br>**操作**: <br>1.如果驱动器不会转换回**确定**状态，则可以尝试使用[Reset-physicaldisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet 来擦除该驱动器。 <br> 2.使用[Repair-virtualdisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk)还原受影响的虚拟磁盘的复原能力。 <br>3.如果发生这使情况，更换驱动器。|
|暂时性错误|出现临时驱动器错误。 这通常意味着在驱动器没有响应，但它还可能表示的存储空间不恰当地从驱动器中删除保护性的分区。 <br><br>**操作**: <br>1.如果驱动器不会转换回**确定**状态，则可以尝试使用[Reset-physicaldisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet 来擦除该驱动器。 <br> 2.使用[Repair-virtualdisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk)还原受影响的虚拟磁盘的复原能力。 <br>3.这将执行过程中，如果更换驱动器，或尝试获取有关此驱动器的详细诊断信息中使用 Windows 错误报告的故障排除步骤 >[物理磁盘无法联机](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|异常等待时间|驱动器运行缓慢，存储空间直通中的运行状况服务进行度量。<br><br>**操作**:如果发生这使情况，因此它不会减少作为一个整体的存储空间性能更换驱动器。

### <a name="drive-health-state-unhealthy"></a>驱动器运行状况状态：Unhealthy

当前处于不正常状态的驱动器不能写入或通过访问。

|操作状态    |描述|
|---------            |---------          |
|不能使用|此驱动器不能使用存储空间。 有关详细信息请参阅[存储空间直通的硬件要求](storage-spaces-direct-hardware-requirements.md); 如果不使用存储空间直通，请参阅[存储空间概述](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements)。|
|拆分|已从池中与驱动器分离。<br><br>**操作**:重置的擦除所有数据从驱动器并将其添加回池为空驱动器的驱动器。 为此，请打开 PowerShell 会话以管理员身份，运行[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，然后运行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。 <br><br>若要获取有关此驱动器的详细诊断信息，请执行中使用 Windows 错误报告的故障排除步骤 >[物理磁盘无法联机](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|元数据已过时|存储空间驱动器上找到旧的元数据。<br><br>**操作**:这应该是临时状态。 如果驱动器不会转换回确定，则可以运行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)上启动修复操作受影响的虚拟磁盘。 如果仍未解决此问题，则可以重置具有驱动器[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，擦除所有数据驱动器，然后再运行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。|
|无法识别的元数据|存储空间上的驱动器，这通常意味着的驱动器上它的不同池从具有元数据中找到无法识别的元数据。<br><br>**操作**:若要擦除驱动器并将其添加到当前的池，重置该驱动器。 若要重置该驱动器，请打开 PowerShell 会话中的作为管理员，运行[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，然后运行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。|
|介质故障|驱动器失败，并且不会再使用存储空间。<br><br>**操作**:更换驱动器。 <br><br>若要获取有关此驱动器的详细诊断信息，请执行中使用 Windows 错误报告的故障排除步骤 >[物理磁盘无法联机](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|设备硬件故障|在此驱动器上出现硬件故障。 <br><br>**操作**:更换驱动器。|
|更新固件|Windows 正在更新的驱动器上的固件。 这只是临时状态的持续时间通常少于一分钟，并在此期间时间在池中其他驱动器处理所有读取和写入。 有关详细信息，请参阅[更新驱动器固件](../update-firmware.md)。|
|Starting|驱动器前的准备操作。 这应该是临时状态-完成后，驱动器应转换为不同的操作状态。|

## <a name="reasons-a-drive-cant-be-pooled"></a>不能共用一个驱动器的原因

只是，某些驱动器未准备好要在存储池中。 您可以找出为什么驱动器不是有资格参与池通过查看`CannotPoolReason`的物理磁盘的属性。 下面是示例 PowerShell 脚本来显示 CannotPoolReason 属性：

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

下面是示例输出：

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

下表提供有关每个原因一些更多详细信息。

|原因|描述|
|---|---|
|在池中|驱动器已属于存储池。 <br><br>驱动器可以属于单个存储池中，一次。 若要在另一个存储池中使用此驱动器，首先从其现有的池，它会告知存储空间，以将数据移动的驱动器上到其他驱动器在池上删除驱动器。 或重置该驱动器，如果驱动器已断开连接从其池而不通知存储空间。 <br><br>若要安全地从存储池中删除驱动器，请使用[Remove-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)，或转到服务器管理器 >**文件和存储服务** > **存储池**，>**物理磁盘**，右键单击该驱动器，然后选择**删除磁盘**。<br><br>若要重置一个驱动器，请使用[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk)。|
|不正常|驱动器未处于正常状态，并且可能需要更换。|
|可移动媒体|驱动器归类为可移动驱动器。 <br><br>存储空间不支持将 Windows 识别为可移动，如蓝光驱动器的媒体。 尽管许多修复驱动器在可移动插槽中，一般情况下，介质指的是*分类*由为可移动的 Windows 不适合与存储空间一起使用。|
|在使用群集|故障转移群集当前使用的驱动器。|
|脱机|驱动器处于脱机状态。 <br><br>若要使所有脱机驱动器联机并设置为读/写，请打开 PowerShell 会话以管理员身份，并使用以下脚本：<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|产能不足|这通常发生时有分区占用的驱动器上的可用空间。 <br><br>**操作**:删除擦除所有数据驱动器上的驱动器上的任何卷。 若要执行此操作的一种方法是使用[Clear-disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) PowerShell cmdlet。|
|正在进行验证|[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)检查以查看是否被批准的驱动器或固件的驱动器上的服务器管理员使用。|
|验证失败|[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)无法检查以查看是否被批准的驱动器或固件的驱动器上的服务器管理员使用。|
|固件不符合|不在由服务器管理员通过使用指定的已批准的固件版本的列表中的物理驱动器上的固件[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)。 |
|硬件不符合|驱动器不在列表中的服务器管理员通过使用指定的已批准的存储模型[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)。|

## <a name="see-also"></a>请参阅

- [存储空间直通](storage-spaces-direct-overview.md)
- [存储空间直通的硬件要求](storage-spaces-direct-hardware-requirements.md)
- [了解群集和池仲裁](understand-quorum.md)