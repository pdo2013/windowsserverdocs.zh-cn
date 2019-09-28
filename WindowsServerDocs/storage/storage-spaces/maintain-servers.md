---
title: 使存储空间直通服务器脱机以进行维护
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
Keywords: 存储空间直通, S2D, 维护
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 20439a06c255a73f20a297f765e6ed11abfde6f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402823"
---
# <a name="taking-a-storage-spaces-direct-server-offline-for-maintenance"></a>使存储空间直通服务器脱机以进行维护

> 适用于：Windows Server 2019、Windows Server 2016

本主题提供有关如何正确重启或关闭带有[存储空间直通](storage-spaces-direct-overview.md)的服务器的指南。

如果具有存储空间直通，使服务器脱机（将其关机）还意味着使在群集中的所有服务器之间共享的存储部分脱机。 执行此操作需要暂停（挂起）你要使其脱机的服务器，将角色移动到群集中的其他服务器，并验证所有数据在群集中的其他服务器上可用，以便数据在整个维护过程中保持安全和可访问。

使其脱机之前，使用以下步骤正确暂停存储空间直通群集中的服务器。 

   > [!IMPORTANT]
   > 要在存储空间直通群集上安装更新，请使用群集感知更新 (CAU)，它将自动执行本主题中的步骤，使你无需在安装更新时执行。 有关详细信息，请参阅[群集感知更新 (CAU)](https://technet.microsoft.com/library/hh831694.aspx)。

## <a name="verifying-its-safe-to-take-the-server-offline"></a>验证使服务器脱机是否安全

使服务器脱机以进行维护之前，验证所有卷是否正常。

为此，请使用管理员权限打开一个 PowerShell 会话，然后运行以下命令以查看卷状态：

```PowerShell
Get-VirtualDisk 
```

下面是此输出的执行示例：
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

验证每个卷（虚拟磁盘）的 **HealthStatus** 属性为 **Healthy**。

要在故障转移群集管理器中执行此操作，请转到**存储** > **磁盘**。

验证每个卷（虚拟磁盘）的**状态**列显示**联机**。

## <a name="pausing-and-draining-the-server"></a>暂停并清空服务器

重启或关闭服务器之前，请暂停并清空（移除）任意角色（例如，在服务器上运行的虚拟机）。 这也为存储空间直通提供了顺利刷新并提交数据的机会，以确保关机对于任何在该服务器上运行的应用程序而言都是透明的。

   > [!IMPORTANT]
   > 请在重启或关闭之前始终暂停或清空群集服务器。

在 PowerShell 中运行以下 cmdlet（以管理员身份）可暂停并清空。

```PowerShell
Suspend-ClusterNode -Drain
```

要在故障转移群集管理器中执行此操作，请转到**节点**，右键单击该节点，然后依次选择**暂停** > **清空角色**。

![暂停清空](media/maintain-servers/pause-drain.png)

所有虚拟机将开始实时迁移到群集中的其他服务器。 这可能需要几分钟。

   > [!NOTE]
   > 正确暂停和清空群集节点时，Windows 将执行自动安全检查，以确保可以安全继续。 如果有不正常的卷，安全检查会停止，并提醒你继续操作不安全。

![安全检查](media/maintain-servers/safety-check.png)

## <a name="shutting-down-the-server"></a>正在关闭服务器

服务器完成清空后，它将在故障转移群集管理器和 PowerShell 中显示为**暂停**。

![已暂停](media/maintain-servers/paused.png)

现在，你可以像往常一样（例如，通过使用 Restart-Computer 或 Stop-Computer PowerShell cmdlet）安全重启或关机。

```PowerShell
Get-VirtualDisk 

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

当节点关闭或启动/停止节点上的群集服务时，操作状态为 "未完成" 或 "降级" 是正常的。 你的所有卷都保持联机和可访问。

## <a name="resuming-the-server"></a>恢复服务器

当你准备好使服务器再次开始托管工作负载时，请恢复它。

在 PowerShell 中运行以下 cmdlet（以管理员身份）恢复。

```PowerShell
Resume-ClusterNode
```

要移回之前在此服务器中运行的角色，请使用可选的 **-故障回复**标志。

```PowerShell
Resume-ClusterNode –Failback Immediate
```

要在故障转移群集管理器中执行此操作，请转到**节点**，右键单击该节点，然后依次选择**恢复** > **故障回复角色**。

![故障回复角色](media/maintain-servers/resume-failback.png)

## <a name="waiting-for-storage-to-resync"></a>等待要重新同步的存储

当服务器恢复时，任何不能使用的新写入都需要重新同步。 此过程自动发生。 使用智能更改跟踪无需扫描或同步*所有*数据，只需扫描或同步更改。 此过程会受到限制，以缓解生产负载产生的影响。 这一过程可能需要数分钟才能完成，具体取决于服务器暂停的时间和写入的新数据量。

你必须等待重新同步完成才能使群集中的任意其他服务器脱机。

在 PowerShell 中运行以下 cmdlet（以管理员身份）监视进度。

```PowerShell
Get-StorageJob
```
下面是一些显示重新同步（修复）作业的示例输出：
```
Name   IsBackgroundTask ElapsedTime JobState  PercentComplete BytesProcessed BytesTotal
----   ---------------- ----------- --------  --------------- -------------- ----------
Repair True             00:06:23    Running   65              11477975040    17448304640
Repair True             00:06:40    Running   66              15987900416    23890755584
Repair True             00:06:52    Running   68              20104802841    22104819713
```

**BytesTotal** 显示需要重新同步的存储数。 **PercentComplete** 显示进度。

   > [!WARNING]
   > 在修复作业完成之前，使其他服务器脱机都是不安全的。

在此期间，卷将继续显示为**警告**，这是正常情况。 

例如，如果使用 `Get-VirtualDisk` cmdlet，你可能会看到以下输出：
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                InService         Warning      True           1 TB
MyVolume2    Mirror                InService         Warning      True           1 TB
MyVolume3    Mirror                InService         Warning      True           1 TB
```

作业完成后，使用 `Get-VirtualDisk` cmdlet 再次验证卷是否显示**正常**。 下面是一些示例输出：

```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

现在可以安全地暂停和重启群集中的其他服务器。

## <a name="how-to-update-storage-spaces-direct-nodes-offline"></a>如何脱机更新存储空间直通节点
使用以下步骤快速为存储空间直通系统进行路径。 它涉及到计划维护时段并使系统关闭修补。 如果需要快速应用重要的安全更新，或者需要确保在维护时段内完成修补，则可以使用此方法。 此过程会关闭存储空间直通群集，对其进行修补，并再次将其打开。 权衡是托管资源的停机时间。

1. 规划维护时段。
2. 使虚拟磁盘脱机。
3. 停止群集以使存储池脱机。 运行**停止群集**cmdlet，或使用故障转移群集管理器停止群集。
4. 在每个节点上的 services.msc 中将群集服务设置为 "**已禁用**"。 这会阻止群集服务在修补后启动。
5. 将 Windows Server 累积更新和任何所需的服务堆栈更新应用到所有节点。 （您可以同时更新所有节点，而无需在群集关闭后等待）。  
6. 重新启动节点，确保一切正常。
7. 在每个节点上将群集服务重新设置为**自动**。
8. 启动群集。 运行**启动群集**cmdlet，或使用故障转移群集管理器。 

   请花几分钟时间。  请确保存储池处于正常状态。
9. 使虚拟磁盘恢复联机。
10. 通过运行**VirtualDisk** cmdlet 来监视虚拟磁盘的状态 。


## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [群集感知更新（CAU）](https://technet.microsoft.com/library/hh831694.aspx)
