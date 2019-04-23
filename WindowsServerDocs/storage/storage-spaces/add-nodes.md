---
ms.assetid: 898d72f1-01e7-4b87-8eb3-a8e0e2e6e6da
title: 向存储空间直通添加服务器或驱动器
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2017
description: 如何将服务器或驱动器添加到存储空间直通群集
ms.localizationpriority: medium
ms.openlocfilehash: ae639b920788911dbc16952d7b61aab85b0a391b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833448"
---
# <a name="adding-servers-or-drives-to-storage-spaces-direct"></a>向存储空间直通添加服务器或驱动器

>适用于：Windows Server 2019、Windows Server 2016

本主题介绍如何向存储空间直通添加服务器或驱动器。

## <a name="adding-servers"></a> 添加服务器

添加服务器（通常称作横向扩展）可增加存储容量、提高存储性能并实现更高的存储效率。 如果你的部署是超聚合的，则添加服务器还可为你的工作负载提供更多计算资源。

![将服务器添加到四个节点群集的动画](media/add-nodes/Scaling-Out.gif)

通过添加服务器，典型的部署很容易实现横向扩展。 只需两个步骤：

1. 使用故障转移群集管理单元或者在 PowerShell 中使用 **Test-Cluster** cmdlet，运行 [群集验证向导](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx)（以管理员身份运行）。 包括你要添加的新服务器 *\<NewNode>*。

   ```PowerShell
   Test-Cluster -Node <Node>, <Node>, <Node>, <NewNode> -Include "Storage Spaces Direct", Inventory, Network, "System Configuration"
   ```

   由此确认正在运行 Windows Server 2016 Datacenter Edition 的新服务器与现有服务器一样已加入 Active Directory 域服务域，具有全部所需角色和功能，并已正确配置了网络。

   >[!IMPORTANT]
   > 如果你正在重复使用包含不再需要的旧数据或元数据的驱动器，则使用“磁盘管理”或 **Reset-PhysicalDisk** cmdlet 清除它们。 如果检测到旧数据或元数据，则不将这些驱动器放入池中。

2. 在群集上运行以下 cmdlet 以完成服务器添加：

```
Add-ClusterNode -Name NewNode 
```

   >[!NOTE]
   > 自动池取决于你只有一个池。 如果你已避开标准配置来创建多个池，则将需要使用 **Add-PhysicalDisk** 自行将新驱动器添加到首选池。

### <a name="from-2-to-3-servers-unlocking-three-way-mirroring"></a>从 2 个服务器横向扩展到 3 个服务器：解锁三向镜像

![将第三个服务器添加到两个节点群集](media/add-nodes/Scaling-2-to-3.png)

通过两个服务器仅可创建双向镜像卷（相较于分布式 RAID-1）。 通过三个服务器可以创建三向镜像卷，以便提供更好的容错能力。 建议尽可能使用三向镜像。

双向镜像卷无法就地升级到三向镜像。 相反，你可以创建一个新卷并将数据迁移（复制，例如通过使用 [存储副本](../storage-replica/server-to-server-storage-replication.md)）到该卷中，然后删除旧卷。

若要开始创建三向镜像卷，有几个很好的选项。 你可以根据意愿使用这些选项。 

#### <a name="option-1"></a>选项 1

创建时在每个新卷上指定 **PhysicalDiskRedundancy = 2**。

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2
```

#### <a name="option-2"></a>选项 2

你可以在该池的名为 **Mirror** 的 **ResiliencySetting** 对象上设置 **PhysicalDiskRedundancyDefault = 2**。 然后，任何新镜像卷都将自动使用*三向*镜像，即使你未指定。

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Mirror | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size>
```

#### <a name="option-3"></a>选项 3

在名为 *Capacity* 的 **StorageTier** 模板上设置 **PhysicalDiskRedundancy = 2**，然后通过引用层创建卷。

```PowerShell
Set-StorageTier -FriendlyName Capacity -PhysicalDiskRedundancy 2 

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Capacity -StorageTierSizes <Size>
```

### <a name="from-3-to-4-servers-unlocking-dual-parity"></a>从 3 个服务器横向扩展到 4 个服务器：解锁双奇偶校验

![将第四个服务器添加到一个三节点群集](media/add-nodes/Scaling-3-to-4.png)

通过四个服务器可以使用双奇偶校验，通常也称为“删除编码”（相较于分布式 RAID-6）。 这提供了与三向镜像相同的默认容错，但存储效率更好。 要了解详细信息，请参阅 [容错和存储效率](storage-spaces-fault-tolerance.md)。

如果你来自更小的部署，则有几个很好的选项可供开始创建双奇偶校验卷。 你可以根据意愿使用这些选项。

#### <a name="option-1"></a>选项 1

创建时在每个新卷上指定 **PhysicalDiskRedundancy = 2** 和 **ResiliencySettingName = Parity**。

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity
```

#### <a name="option-2"></a>选项 2

对名为 **Parity** 的池的 **ResiliencySetting** 对象设置 **PhysicalDiskRedundancy = 2**。 然后，任何新奇偶校验卷都将自动使用*双*奇偶校验，即使你未指定。

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Parity | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -ResiliencySettingName Parity
```

通过四个服务器，你也可以使用镜像加速的奇偶校验开始，其中单个卷为部分镜像和部分奇偶校验。

为此，你将需要更新你的 **StorageTier** 模板，以同时具有 *Performance* 和 *Capacity* 层，因为如果首先在四个服务器运行 **Enable-ClusterS2D**，则会创建层。 具体来说，这两个层应设置容量设备（例如 SSD 或 HDD）的 **MediaType**，且 **PhysicalDiskRedundancy = 2**。 *Performance* 层应为 **ResiliencySettingName = Mirror**，*Capacity* 层应为 **ResiliencySettingName = Parity**。

#### <a name="option-3"></a>选项 3

你可能会发现，最简单方式就是删除现有层模板并创建两个新模板。 这将不会影响通过引用层模板创建的预先存在的任何卷：它只是一个模板。

```PowerShell
Remove-StorageTier -FriendlyName Capacity

New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Mirror -FriendlyName Performance
New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity -FriendlyName Capacity
```

就这么简单！ 现在可以通过引用这些层模板开始创建镜像加速的奇偶校验了。

#### <a name="example"></a>示例

```PowerShell
New-Volume -FriendlyName "Sir-Mix-A-Lot" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes <Size, Size> 
```

### <a name="beyond-4-servers-greater-parity-efficiency"></a>超过 4 个服务器：奇偶校验效率更高

如扩展超过四个服务器，则新卷可以受益于越来越大的奇偶校验编码效率。 例如，在六和七个服务器之间效率可从 50.0% 提高到 66.7%，因为变得可以使用 Reed Solomon 4 + 2（而不是 2 + 2）。 无需任何操作即可开始享用全新效率；每次创建卷时都会自动确定最佳编码。

但是，任何预先存在的卷将*不*被“转换”为新的更广的编码。 一个主要的原因是，这样做将需要大量计算，而这会对整个部署中的*每一个位*都产生影响。 若要以较高的效率对预存在的数据进行编码，你可以将其迁移到新卷。

有关详细信息，请参阅 [容错和存储效率](storage-spaces-fault-tolerance.md)。

### <a name="adding-servers-when-using-chassis-or-rack-fault-tolerance"></a>使用机箱或机架容错时添加服务器

如果你的部署使用机箱或机架容错，则必须在将它们添加到群集之前指定新服务器的机箱或机架。 这将告知“存储空间直通”如何最好地分发数据以最大化容错能力。

1. 通过打开提升的 PowerShell 会话，然后使用以下命令，其中 *\<NewNode &gt;* 是新的群集节点名称，为节点创建一个临时故障域：

   ```PowerShell
   New-ClusterFaultDomain -Type Node -Name <NewNode> 
   ```

2. 按照 *\<ParentName>* 所指定的位置将此临时故障域移动到新服务器在现实环境中位于的机箱或机架中：

   ```PowerShell
   Set-ClusterFaultDomain -Name <NewNode> -Parent <ParentName> 
   ```

   有关详细信息，请参阅 [Windows Server 2016 中的容错域感知](../../failover-clustering/fault-domains.md)

3. 如 [添加服务器](#adding-servers) 中所述将服务器添加到群集。 当新服务器加入群集时，它会自动（使用其名称）与占位符容错域相关联。

## <a name="adding-drives"></a> 添加驱动器

添加驱动器（也称为纵向扩展）将增加存储容量并提高性能。 如果你有可用插槽，则无需添加服务器便可将驱动器添加到每个服务器，从而扩展你的存储容量。 你随时可以独立添加缓存驱动器或容量驱动器。

   >[!IMPORTANT]
   > 强烈建议所有服务器都具有相同的存储配置。

![动画显示添加到系统驱动器](media/add-nodes/Scale-Up.gif)

若要纵向扩展，请连接驱动器并验证 Windows 是否能够发现它们。 它们应该出现在 PowerShell 的 **Get-PhysicalDisk** cmdlet 输出中，其 **CanPool** 属性设置为 **True**。 如果它们显示为 **CanPool = False**，则你可以通过查看其 **CannotPoolReason** 属性来了解原因。

```PowerShell
Get-PhysicalDisk | Select SerialNumber, CanPool, CannotPoolReason
```

在短时间内，符合条件的驱动器将自动被“存储空间直通”声明并添加到存储池，卷将自动 [跨所有驱动器均匀地重新分发](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)。 此时，你已完成操作并准备[扩展现有卷](resize-volumes.md)或[创建新卷](create-volumes.md)。

如果驱动器未出现，可手动扫描检测硬件改动。 使用“**操作**”菜单下的“**设备管理器**”可以完成此操作。 如果它们包含旧数据或元数据，请考虑对其重格式化。 这可以使用“磁盘管理”或 **Reset-PhysicalDisk** cmdlet 来完成。

   >[!NOTE]
   > 自动池取决于你只有一个池。 如果你已避开标准配置来创建多个池，则将需要使用 **Add-PhysicalDisk** 自行将新驱动器添加到首选池。

## <a name="optimizing-drive-usage-after-adding-drives-or-servers"></a>添加驱动器或服务器后优化驱动器使用情况

随着时间推移，驱动器在添加或删除，在池中的驱动器间的数据分布可以变得不均匀。 在某些情况下，这可能导致某些变满，而在池中其他驱动器有使用量低很多的驱动器。

为了帮助保持甚至跨池中的驱动器分配，存储空间直通自动优化了驱动器使用情况后将驱动器或服务器添加到池 （这是一个手动过程对于使用共享 SAS 存储设备的存储空间系统）。 优化开始之后将新驱动器添加到池的 15 分钟。 池优化运行为低优先级后台操作，因此可能需要花费数小时或数天才能完成，特别是如果您使用大型的硬盘驱动器。

优化使用两个作业的另一个名为*优化*另一个名为*重新平衡*-，你可以监视其进度，使用以下命令：

```powershell
Get-StorageJob
```

您可以手动优化的存储池[Optimize-storagepool](https://docs.microsoft.com/powershell/module/storage/optimize-storagepool?view=win10-ps) cmdlet。 以下是一个示例：

```powershell
Get-StoragePool <PoolName> | Optimize-StoragePool
```
