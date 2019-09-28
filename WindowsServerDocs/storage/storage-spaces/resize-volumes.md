---
title: 扩展存储空间直通中的卷
description: 如何使用 Windows 管理中心和 PowerShell 在存储空间直通中调整卷的大小。
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 05/07/2019
ms.openlocfilehash: 20482fe1728b12d4fe56dcfa397352fbb4b4f981
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366095"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>扩展存储空间直通中的卷
> 适用于：Windows Server 2019、Windows Server 2016

本主题提供使用 Windows 管理中心调整[存储空间直通](storage-spaces-direct-overview.md)群集上的卷大小的说明。

观看有关如何调整卷大小的快速视频。

> [!VIDEO https://www.youtube-nocookie.com/embed/hqyBzipBoTI]

## <a name="extending-volumes-using-windows-admin-center"></a>使用 Windows 管理中心扩展卷

1. 在 Windows 管理中心，连接到存储空间直通群集，然后从 "**工具**" 窗格中选择 "**卷**"。
2. 在 "卷" 页上，选择 "**清单**" 选项卡，然后选择要调整大小的卷。

    在 "卷详细信息" 页上，指出了卷的存储容量。 还可以直接从仪表板打开 "卷详细信息" 页。 在仪表板上的 "警报" 窗格中，选择警报，该警报将通知你卷的存储容量是否不足，然后选择 "**转向卷**"。

4. 在卷详细信息页的顶部，选择 "**调整大小**"。
5. 输入新的大小，然后选择 "**调整**大小"。

    在 "卷详细信息" 页上，指出了卷的较大存储容量，并清除了仪表板上的警报。

## <a name="extending-volumes-using-powershell"></a>使用 PowerShell 扩展卷

### <a name="capacity-in-the-storage-pool"></a>存储池中的容量

在调整卷大小之前，请确保存储池中有足够的容量，以容纳其新的更大占用空间。 例如，将三向镜像卷的大小从 1 TB 调整为 2 TB 时，其占用空间将从 3 TB 增长到 6 TB。 要成功调整大小，存储池中将至少需要 (6 - 3) = 3 TB 的可用容量。

### <a name="familiarity-with-volumes-in-storage-spaces"></a>熟悉存储空间中的卷

在存储空间直通中，每个卷都由一些堆叠对象组成：群集共享卷 (CSV)（这是一个卷）、分区、磁盘（这是一个虚拟磁盘）以及一个或多个存储层（如果适用）。 若要调整卷的大小，你将需要调整其中一些对象的大小。

![SMAPI 中的卷](media/resize-volumes/volumes-in-smapi.png)

若要熟悉它们，请尝试在 PowerShell 中使用相应的名词运行 **Get-** 。

例如：

```PowerShell
Get-VirtualDisk
```

若要跟踪堆栈中对象之间的关联，请通过管道将一个 **Get-** cmdlet 传递给下一个 cmdlet。

例如，下面介绍如何从虚拟磁盘开始一直调整到其卷：

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume 
```

### <a name="step-1--resize-the-virtual-disk"></a>第 1 步 - 调整虚拟磁盘的大小

虚拟磁盘可以使用或者不使用存储层，具体取决于它的创建方式。

若要进行检查，请运行以下 cmdlet：

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

如果此 cmdlet 未返回任何内容，则虚拟磁盘未使用存储层。

#### <a name="no-storage-tiers"></a>无存储层

如果虚拟磁盘没有存储层，你可以直接使用 **Resize-VirtualDisk** cmdlet 调整虚拟磁盘的大小。

在 **-Size** 参数中提供新的大小。

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

当你调整 **VirtualDisk** 的大小时，**Disk** 也会自动跟着调整大小。

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

#### <a name="with-storage-tiers"></a>具有存储层

如果虚拟磁盘使用存储层，你可以使用 **Resize-StorageTier** cmdlet 分别调整每一层的大小。

通过跟踪虚拟磁盘中的关联来获取存储层的名称。

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier | Select FriendlyName
```

然后，在 **-Size** 参数中为每个层提供新的大小。

```PowerShell
Get-StorageTier <FriendlyName> | Resize-StorageTier -Size <Size>
```

> [!TIP]
> 如果你的层是不同的物理媒体类型（例如 **MediaType = SSD** 和 **MediaType = HDD**），则需要确保存储池中每种媒体类型都具有足够的容量，以在每层上容纳更大的新占用空间。

当你调整 **StorageTier** 的大小时，**VirtualDisk** 和 **Disk** 也会自动跟着调整大小。

![Resize-StorageTier](media/resize-volumes/Resize-StorageTier.gif)

### <a name="step-2--resize-the-partition"></a>第 2 步 - 调整分区大小

接下来，使用 **Resize-Partition**cmdlet 调整分区大小。 虚拟磁盘应该有两个分区：第一个分区应该保留而不应修改；你需要调整大小的分区具有 **PartitionNumber = 2** 和 **Type = Basic**。

在 **-Size** 参数中提供新的大小。 我们建议使用支持的最大大小，如下所示。

```PowerShell
# Choose virtual disk
$VirtualDisk = Get-VirtualDisk <FriendlyName>

# Get its partition
$Partition = $VirtualDisk | Get-Disk | Get-Partition | Where PartitionNumber -Eq 2

# Resize to its maximum supported size 
$Partition | Resize-Partition -Size ($Partition | Get-PartitionSupportedSize).SizeMax
```

当你调整 **Partition** 的大小时，**Volume** 和 **ClusterSharedVolume** 也会自动跟着调整大小。

![Resize-Partition](media/resize-volumes/Resize-Partition.gif)

就这么简单！

> [!TIP]
> 你可以运行 **Get-Volume** 来验证卷是否具有新的大小。

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的存储空间直通](storage-spaces-direct-overview.md)
- [规划存储空间直通中的卷](plan-volumes.md)
- [在存储空间直通中创建卷](create-volumes.md)
- [删除存储空间直通中的卷](delete-volumes.md)
