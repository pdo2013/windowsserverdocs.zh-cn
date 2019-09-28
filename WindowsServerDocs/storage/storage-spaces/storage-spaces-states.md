---
title: 存储空间直通运行状况和运行状态
description: 如何查找并了解存储空间直通和存储空间（包括物理磁盘、池和虚拟磁盘）的不同运行状况和操作状态，以及如何处理它们。
keywords: 存储空间，已分离，虚拟磁盘，物理磁盘，降级
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 4b525555333a8aeee416e9ab55981c17137a52ea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365994"
---
# <a name="troubleshoot-storage-spaces-direct-health-and-operational-states"></a>存储空间直通运行状况和运行状态的疑难解答

> 适用于：Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server （半年频道），Windows 10，Windows 8。1

本主题介绍存储池、虚拟磁盘（位于存储空间中的卷下）的运行状况和操作状态，以及[存储空间直通](storage-spaces-direct-overview.md)和[存储空间](overview.md)中的驱动器。 尝试排查各种问题（例如由于只读配置而无法删除虚拟磁盘）时，这些状态可能非常有用。 它还讨论了为什么无法将驱动器添加到池（CannotPoolReason）。

存储空间具有三个主要对象-添加到*存储池*的*物理磁盘*（硬盘驱动器、ssd 等），虚拟化存储，以便可以从池中的可用空间创建*虚拟磁盘*，如下所示。 池元数据将写入池中的每个驱动器。 卷是在虚拟磁盘上创建的，并存储文件，但我们不会在此处讨论卷。

![将物理磁盘添加到存储池，然后将虚拟磁盘添加到池空间](media/storage-spaces-states/storage-spaces-object-model.png)

可以在服务器管理器或 PowerShell 中查看运行状况和运行状态。 下面是一个示例，其中显示了缺少其大部分群集节点的存储空间直通群集上的各种（主要是坏）运行状况和操作状态（右键单击列标题以添加**操作状态**）。 这不是很高兴的群集。

![服务器管理器在存储空间直通群集中显示两个丢失节点的结果-大量缺少的物理磁盘和虚拟磁盘处于不正常状态](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>存储池状态

每个存储池的运行状况状态为 "**正常**"、"**警告**" 或 "**未知**" @no__t 不**正常**，以及一个或多个操作状态。

若要了解池所处的状态，请使用以下 PowerShell 命令：

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

以下示例输出显示处于 "未知" 运行状况状态且具有只读操作状态的存储池：

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

以下部分列出了运行状况和运行状态。

### <a name="pool-health-state-healthy"></a>池运行状况状态：正常

|操作状态    |描述|
|---------            |---------  |
|确定|存储池运行状况良好。|

### <a name="pool-health-state-warning"></a>池运行状况状态：警告

如果存储池处于**警告**运行状况状态，则意味着池可访问，但一个或多个驱动器出现故障或丢失。 因此，您的存储池可能会降低复原能力。

|操作状态    |描述|
|---------            |---------  |
|降级|存储池中的驱动器已失败或丢失。 此条件仅在托管池元数据的驱动器上发生。 <br><br>**操作**:检查驱动器的状态，并在出现其他故障之前更换所有故障驱动器。|

### <a name="pool-health-state-unknown-or-unhealthy"></a>池运行状况状态：未知或不正常

如果存储池处于 "**未知**" 或 "不**正常**" 运行状况状态，则表示存储池为只读，不能修改，直到将池返回到 "**警告**" 或 **"正常"** 运行状况状态。

|操作状态    |只读原因 |描述|
|---------            |---------       |--------   |
|只读|完整|如果存储池失去其[仲裁](understand-quorum.md)，这表示池中的大多数驱动器都出现故障或处于脱机状态，则可能会发生这种情况。 当某个池失去仲裁时，存储空间会自动将池配置设置为只读，直到有足够的驱动器再次可用。<br><br>**采取** <br>1.重新连接任何缺失的驱动器，如果使用存储空间直通，则使所有服务器联机。 <br>2.通过打开具有管理权限的 PowerShell 会话，然后键入：<br><br> <code> @ no__t-1 <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false @ no__t-4|
||策略|管理员将存储池设置为只读。<br><br>**采取**若要在故障转移群集管理器中将群集存储池设置为读写访问权限，请转到 "**池**"，右键单击该池，然后选择 "**联机**"。<br><br>对于其他服务器和 Pc，请打开具有管理权限的 PowerShell 会话，然后键入：<br><br><code> @ no__t-1 <PoolName> \| Set-StoragePool -IsReadOnly $false @ no__t-4<br><br> |
||Starting|存储空间正在启动或等待驱动器连接到池中。 这应该是临时状态。 完全启动后，池应转换为其他操作状态。<br><br>**采取**如果池处于 "*正在启动*" 状态，请确保池中的所有驱动器都已正确连接。|

另请参阅[修改具有只读配置的存储池](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx)。

## <a name="virtual-disk-states"></a>虚拟磁盘状态

在存储空间中，卷放置在划分空间不足的虚拟磁盘（存储空间）上，以供池中使用。 每个虚拟磁盘的运行状况状态为 "**正常**"、"**警告**"、"不**正常**" 或 "**未知**" 以及一个或多个操作状态。

若要了解虚拟磁盘的状态，请使用以下 PowerShell 命令：

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

下面是一个输出示例，显示分离的虚拟磁盘和降级/未完成的虚拟磁盘：

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

以下部分列出了运行状况和运行状态。

### <a name="virtual-disk-health-state-healthy"></a>虚拟磁盘运行状况状态：正常

|操作状态    |描述|
|---------            |---------          |
|确定    |虚拟磁盘处于正常状态。|
|最佳    |数据不会跨驱动器均匀写入。 <br><br>**操作**:通过运行[StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) cmdlet 优化存储池中的驱动器使用情况。|

### <a name="virtual-disk-health-state-warning"></a>虚拟磁盘运行状况状态：警告

当虚拟磁盘处于**警告**运行状况状态时，表示数据的一个或多个副本不可用，但存储空间仍可读取至少一个数据副本。

|操作状态    |描述|
|---------            |---------          |
|服务中            |Windows 正在修复虚拟磁盘，例如添加或删除驱动器后。 修复完成后，虚拟磁盘应恢复为 "正常" 运行状况状态。|
|完整           |虚拟磁盘的复原能力降低，因为一个或多个驱动器出现故障或丢失。 不过, 缺少的驱动器包含数据的最新副本。<br><br> **操作**: <br>1.重新连接任何缺失的驱动器、更换任何故障的驱动器，如果使用存储空间直通，请将任何处于脱机状态的服务器联机。 <br>2.如果使用的不是存储空间直通，请使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet 修复虚拟磁盘。<br> 如果在重新连接或更换驱动器后需要，存储空间直通会自动启动修复。|
|降级             |虚拟磁盘的复原能力会降低，因为一个或多个驱动器出现故障或丢失，并且这些驱动器上有过时的数据副本。 <br><br>**操作**: <br> 1.重新连接任何缺失的驱动器、更换任何故障的驱动器，如果使用存储空间直通，请将任何处于脱机状态的服务器联机。 <br> 2.如果使用的不是存储空间直通，请使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet 修复虚拟磁盘。 <br>如果在重新连接或更换驱动器后需要，存储空间直通会自动启动修复。|

### <a name="virtual-disk-health-state-unhealthy"></a>虚拟磁盘运行状况状态：Unhealthy

如果虚拟磁盘运行状况不**正常**，则虚拟磁盘上的部分或全部数据当前无法访问。

|操作状态    |描述|
|---------            |--------   |
|无冗余|虚拟磁盘丢失了数据，因为太多驱动器出现故障。<br><br>**操作**:替换失败的驱动器，然后从备份还原数据。|

### <a name="virtual-disk-health-state-informationunknown"></a>虚拟磁盘运行状况状态：信息/未知

如果管理员使虚拟磁盘脱机或虚拟磁盘已变为，则虚拟磁盘还可以处于**信息**运行状况状态（如存储空间控制面板项中所示）或**未知**的运行状况状态（如 PowerShell 中所示）处于.

|操作状态    |分离原因 |描述|
|---------            |---------       |--------   |
|Detached             |按策略            |管理员使虚拟磁盘脱机，或将虚拟磁盘设置为需要手动连接，在这种情况下，每次 Windows 重新启动时，都必须手动附加虚拟磁盘。<br><br>**操作**:使虚拟磁盘恢复联机。 若要在虚拟磁盘位于群集存储池时执行此操作，请在故障转移群集管理器选择**存储**@no__t**池**@no__t 3 个**虚拟磁盘**上，选择显示**脱机**状态的虚拟磁盘，然后选择 "**引入"在线**。 <br><br>若要在不在群集中时使虚拟磁盘重新联机，请以管理员身份打开 PowerShell 会话，然后尝试使用以下命令：<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>若要在 Windows 重新启动后自动附加所有非群集虚拟磁盘，请以管理员身份打开 PowerShell 会话，然后使用以下命令：<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |多数磁盘不正常 |此虚拟磁盘使用的驱动器太多，已丢失或具有陈旧数据。   <br><br>**操作**: <br> 1.重新连接任何缺失的驱动器，如果使用存储空间直通，请将任何处于脱机状态的服务器联机。 <br> 2.所有驱动器和服务器都处于联机状态后，请更换任何出现故障的驱动器。 有关详细信息，请参阅[运行状况服务](../../failover-clustering/health-service-overview.md)。 <br>如果在重新连接或更换驱动器后需要，存储空间直通会自动启动修复。<br>3.如果使用的不是存储空间直通，请使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet 修复虚拟磁盘。  <br><br>如果有多个磁盘发生故障，超过了你的数据的副本，并且未在故障之间修复虚拟磁盘，则虚拟磁盘上的所有数据都将永久丢失。 在这种情况下，请删除虚拟磁盘、创建新的虚拟磁盘，然后从备份还原。|
|                     |完整 |没有足够的驱动器来读取虚拟磁盘。    <br><br>**操作**: <br> 1.重新连接任何缺失的驱动器，如果使用存储空间直通，请将任何处于脱机状态的服务器联机。 <br> 2.所有驱动器和服务器都处于联机状态后，请更换任何出现故障的驱动器。 有关详细信息，请参阅[运行状况服务](../../failover-clustering/health-service-overview.md)。 <br>如果在重新连接或更换驱动器后需要，存储空间直通会自动启动修复。<br>3.如果使用的不是存储空间直通，请使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet 修复虚拟磁盘。<br><br>如果有多个磁盘发生故障，超过了你的数据的副本，并且未在故障之间修复虚拟磁盘，则虚拟磁盘上的所有数据都将永久丢失。 在这种情况下，请删除虚拟磁盘、创建新的虚拟磁盘，然后从备份还原。|
| |Timeout|附加虚拟磁盘花费了太长时间 <br><br> **采取**不应经常发生这种情况，因此你可能会尝试查看条件是否按时间传递。 或者，可以尝试使用[VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) cmdlet 断开虚拟磁盘的连接，然后使用[VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) cmdlet 将其重新连接。 |

## <a name="drive-physical-disk-states"></a>驱动器（物理磁盘）状态

以下部分描述了驱动器可以处于的运行状况状态。 池中的驱动器在 PowerShell 中表示为*物理磁盘*对象。

### <a name="drive-health-state-healthy"></a>驱动器运行状况状态:正常

|操作状态    |描述|
|---------            |---------          |
|确定|驱动器正常。|
|服务中|驱动器正在执行一些内部的内部维护操作。 操作完成后，驱动器应返回到 *"正常"* 运行状况状态。|

### <a name="drive-health-state-warning"></a>驱动器运行状况状态:警告

处于警告状态的驱动器可以成功读取和写入数据, 但会出现问题。

|操作状态    |描述|
|---------            |---------          |
|通信丢失|驱动器丢失。 如果使用存储空间直通，则可能是由于服务器已关闭。<br><br>**操作**:如果使用存储空间直通，请将所有服务器恢复联机状态。 如果未解决此问题，请重新连接驱动器，将其替换，或尝试通过使用 Windows 错误报告 >[物理磁盘](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out)的故障排除中所述的步骤获取详细的诊断信息。|
|正在从池删除|存储空间正在从其存储池中删除驱动器。 <br><br> 这是临时状态。 删除完成后，如果驱动器仍连接到系统，驱动器将转换为原始池中的另一操作状态（通常为正常）。|
|正在启动维护模式|在管理员将驱动器置于维护模式后，存储空间正在将驱动器置于维护模式。 这是一种临时状态-驱动器即将处于*维护模式*状态。|
|处于维护模式|管理员将驱动器置于维护模式，从而停止从驱动器进行的读取和写入操作。 这通常在更新驱动器固件或测试失败时执行。<br><br>**操作**:若要使驱动器退出维护模式，请使用[StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) cmdlet。|
|停止维护模式|管理员将驱动器退出维护模式，并且存储空间正在使驱动器恢复联机。 这是一种临时状态-驱动器应很快处于另一状态 *-理想状态*。|
|预测性故障|驱动器报告它接近故障。<br><br>**操作**:更换驱动器。|
|IO 错误|访问驱动器时出现临时错误。<br><br>**操作**: <br>1.如果驱动器未转换回**正常**状态，可以尝试使用[PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet 来擦除驱动器。 <br> 2.使用[VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk)还原受影响的虚拟磁盘的复原能力。 <br>3.如果这种情况持续发生，请更换驱动器。|
|暂时性错误|驱动器出现临时错误。 这通常意味着驱动器无响应，但这也可能意味着存储空间保护分区未正确地从驱动器中删除。 <br><br>**操作**: <br>1.如果驱动器未转换回**正常**状态，可以尝试使用[PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet 来擦除驱动器。 <br> 2.使用[VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk)还原受影响的虚拟磁盘的复原能力。 <br>3.如果出现这种情况，请更换驱动器，或者按照使用 Windows 错误报告 >[物理磁盘无法联机](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)中的步骤来尝试获取有关此驱动器的详细诊断信息。|
|异常延迟|驱动器的执行速度慢，如存储空间直通中的运行状况服务。<br><br>**操作**:如果这种情况持续发生，请更换驱动器，使其不会整体降低存储空间的性能。

### <a name="drive-health-state-unhealthy"></a>驱动器运行状况状态:Unhealthy

当前不能写入或访问处于不正常状态的驱动器。

|操作状态    |描述|
|---------            |---------          |
|不可用|存储空间不能使用此驱动器。 有关详细信息，请参阅[存储空间直通硬件要求](storage-spaces-direct-hardware-requirements.md);如果使用的不是存储空间直通，请参阅[存储空间概述](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements)。|
|分割|驱动器已与池分离。<br><br>**操作**:重置驱动器，清除驱动器中的所有数据，并将其作为空驱动器添加回池。 为此，请以管理员身份打开 PowerShell 会话，运行[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，然后运行[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。 <br><br>若要获取有关此驱动器的详细诊断信息，请按照使用 Windows 错误报告 >[物理磁盘无法联机](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)中的步骤进行故障排除。|
|过时的元数据|存储空间在驱动器上找到旧的元数据。<br><br>**操作**:这应该是临时状态。 如果驱动器未转换回正常，则可以运行[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)来启动对受影响的虚拟磁盘的修复操作。 如果这不能解决问题，你可以用[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet 重置驱动器，从驱动器中擦除所有数据，然后运行[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。|
|无法识别的元数据|存储空间在驱动器上发现无法识别的元数据，这通常意味着驱动器上有来自不同池中的元数据。<br><br>**操作**:若要擦除驱动器并将其添加到当前池，请重置驱动器。 若要重置驱动器，请以管理员身份打开 PowerShell 会话，运行[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，然后运行[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。|
|媒体失败|驱动器失败, 存储空间将不再使用。<br><br>**操作**:更换驱动器。 <br><br>若要获取有关此驱动器的详细诊断信息，请按照使用 Windows 错误报告 >[物理磁盘无法联机](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)中的步骤进行故障排除。|
|设备硬件故障|此驱动器上出现硬件故障。 <br><br>**操作**:更换驱动器。|
|更新固件|Windows 正在更新驱动器上的固件。 这是一种临时状态, 通常不到一分钟, 在此期间, 池中的其他驱动器将处理所有读取和写入操作。 有关详细信息，请参阅[更新驱动器固件](../update-firmware.md)。|
|Starting|驱动器正在为操作做好准备。 这应该是一种临时状态-完成后, 驱动器应转换为其他操作状态。|

## <a name="reasons-a-drive-cant-be-pooled"></a>无法将驱动器汇集到池中的原因

有些驱动器并未准备好在存储池中。 查看物理磁盘的 `CannotPoolReason` 属性可以了解驱动器不符合池要求的原因。 下面是一个示例 PowerShell 脚本，用于显示 CannotPoolReason 属性：

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

下面是一个示例输出：

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

下表提供了有关每个原因的更多详细信息。

|Reason|描述|
|---|---|
|在池中|驱动器已属于一个存储池。 <br><br>驱动器一次只能属于一个存储池。 若要在另一个存储池中使用此驱动器，请首先从其现有池中删除该驱动器，这会告诉存储空间将驱动器上的数据移到池中的其他驱动器上。 如果驱动器已从其池断开连接，但未通知存储空间，则重置驱动器。 <br><br>若要安全地从存储池中删除驱动器，请使用[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)，或服务器管理器 >**文件和存储服务** > **存储池**，>**物理磁盘**，右键单击该驱动器，然后选择 "**删除"磁盘**。<br><br>若要重置驱动器，请使用[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk)。|
|不正常|驱动器不处于正常状态, 可能需要将其替换。|
|可移动媒体|驱动器被分类为可移动驱动器。 <br><br>存储空间不支持被 Windows 识别为可移动的媒体，如蓝光驱动器。 尽管许多固定驱动器都位于可移动插槽中，但通常情况下，由 Windows 作为可移动*分类*的媒体不适用于存储空间。|
|正在由群集使用|驱动器当前正在由故障转移群集使用。|
|脱机|驱动器处于脱机状态。 <br><br>若要使所有脱机驱动器联机并设置为读/写，请以管理员身份打开 PowerShell 会话，并使用以下脚本：<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|容量不足|这通常发生在分区占用驱动器上的可用空间时。 <br><br>**操作**:删除驱动器上的所有卷，并清除驱动器上的所有数据。 实现此目的的一种方法是使用[明文](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps)PowerShell cmdlet。|
|正在进行验证|[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)正在检查驱动器上的驱动器或固件是否已被服务器管理员使用。|
|验证失败|[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)无法检查驱动器上的驱动器或固件是否已被服务器管理员使用。|
|固件不合规|物理驱动器上的固件不在服务器管理员使用[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)指定的已批准固件修订列表中。 |
|硬件不合规|该驱动器不在由服务器管理员使用[运行状况服务](../../failover-clustering/health-service-overview.md#supported-components-document)指定的已批准存储模型的列表中。|

## <a name="see-also"></a>请参阅

- [存储空间直通](storage-spaces-direct-overview.md)
- [存储空间直通硬件要求](storage-spaces-direct-hardware-requirements.md)
- [了解群集和池仲裁](understand-quorum.md)