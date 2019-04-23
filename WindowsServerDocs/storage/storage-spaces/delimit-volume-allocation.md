---
title: 分隔存储空间直通中的卷的分配
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: c93cbf4ba418f6702cf8747508605952d993508d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889048"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>分隔存储空间直通中的卷的分配
> 适用于：Windows Server Insider Preview，生成 17093 及更高版本

Windows Server Insider Preview 引入了一个选项来手动分隔的存储空间直通中的卷分配。 这样做以便可显著提高容错能力，某些情况下，但施加了一些增加了的管理注意事项和复杂性。 本主题说明如何工作，并在 PowerShell 中提供了示例。

   > [!IMPORTANT]
   > 此功能是 Windows Server Insider Preview，生成 17093 及更高版本中的新增功能。 不在 Windows Server 2016 中可用。 我们邀请了 IT 专业人员加入[Windows Server 预览体验计划](https://aka.ms/serverinsider)向我们提供反馈，我们如何使 Windows Server 为你的组织更好地工作。

## <a name="prerequisites"></a>系统必备

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![绿色的复选标记图标。](media/delimit-volume-allocation/supported.png) 如果使用此选项，请考虑：

- 您的群集具有六个或多个服务器;和
- 仅使用你的群集[三向镜像](storage-spaces-fault-tolerance.md#mirroring)复原能力

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![红色 X 图标。](media/delimit-volume-allocation/unsupported.png) 如果不使用此选项：

- 您的群集具有少于六个服务器;或
- 群集使用[奇偶校验](storage-spaces-fault-tolerance.md#parity)或[镜像加速奇偶校验](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)复原能力

## <a name="understand"></a>了解

### <a name="review-regular-allocation"></a>检查： 常规分配

使用正则三向镜像卷分为许多小"slabs"是复制三次，并且均匀地分布在群集中的每个服务器中的每个驱动器。 有关更多详细信息，请阅读[此深入探讨博客](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)。

![显示的卷被划分为三个堆栈的 slabs 和均匀分布在每个服务器之间的关系图。](media/delimit-volume-allocation/regular-allocation.png)

此默认分配最大化并行读取和写入，从而导致更好的性能，并且将在其简单性非常受欢迎： 每个服务器正忙同样，每个驱动器已同样满，并且所有卷保持联机或脱机在一起。 保证每个卷作为保留最多两个并发故障[这些示例](storage-spaces-fault-tolerance.md#examples)说明。

但是，使用此分配的卷不能三个并发从故障中恢复。 如果三个服务器失败，或同时在三个服务器中的驱动器发生故障，卷将变得无法访问，因为至少一些 slabs （使用很有可能） 分配给的确切的三个驱动器或失败的服务器。

在下面的示例中，服务器 1、 3 和 5 失败一次。 尽管许多 slabs 具有存活的副本，但有些则不：

![关系图显示三个六个服务器以红色和总数量突出显示为红色。](media/delimit-volume-allocation/regular-does-not-survive.png)

卷处于脱机状态，直到恢复服务器变得无法访问。

### <a name="new-delimited-allocation"></a>新增内容： 分隔分配

使用带分隔符的分配，你指定服务器使用 （最少三个用于三向镜像） 的子集。 卷划分为三次，如前面一样，而不是分配在每个服务器之间复制的 slabs **slabs 分配给你指定的服务器的子集仅**。

![显示的卷被划分为三个堆栈的 slabs 和仅为三个六个服务器分布的关系图。](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>优点

使用此分配的卷是有可能从三个并发故障中恢复： 事实上，其可能性的生存也相应增加从 （与正则分配） 0%到 95%（使用带分隔符的分配） 在这种情况下 ！ 直观地说，这是因为这样不受其故障不依赖于服务器 4、 5 或 6。

在上述示例中，服务器 1、 3 和 5 失败一次。 分隔的分配确保该服务器 2 包含副本的每个碎片，因为每个碎片都具有未发生故障的副本和卷保持联机并可访问：

![关系图显示三个六个服务器以红色突出显示，但总数量为绿色。](media/delimit-volume-allocation/delimited-does-survive.png)

生存概率取决于服务器和其他因素的数量-请参阅[分析](#analysis)有关详细信息。

#### <a name="disadvantages"></a>缺点

带分隔符的分配施加一些增加了的管理注意事项和复杂性：

1. 管理员负责分隔要进行跨服务器平衡存储利用率和都能保持高概率的生存，每个卷的分配，如中所述[最佳做法](#best-practices)部分。

2. 使用分隔分配保留的等效**每个服务器 （与无最大值） 的一个容量驱动器**。 这是多个[发布建议](plan-volumes.md#choosing-the-size-of-volumes)常规分配时，总共的上限是最大程度上四个驱动器。

3. 如果一台服务器出现故障并且需要更换，如中所述[删除在服务器和其驱动器](remove-servers.md#remove-a-server-and-its-drives)，管理员是负责更新受影响的卷的界定方式添加新服务器并删除失败的一个 – 示例下面。

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

可以使用`New-Volume`cmdlet 在存储空间直通中创建的卷。

例如，若要创建正则三向镜像卷：

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>创建一个卷并分隔其分配

若要创建三向镜像卷并分隔其分配：

1. 首先将你的群集中的服务器分配给变量`$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > 在存储空间直通，术语存储缩放单位是指附加到一台服务器，包括直接连接的驱动器和驱动器使用直接附加外部存储设备的所有原始存储。 在此上下文中，它是与服务器相同。

2. 指定要使用与新的服务器`-StorageFaultDomainsToUse`参数和由编入索引`$Servers`。 例如，若要分隔的第一个、 第二，分配和第三个服务器 （索引 0、 1 和 2）：

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>请参阅分隔的分配

若要查看如何*MyVolume*是分配，使用`Get-VirtualDiskFootprintBySSU.ps1`中编写脚本[附录](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

请注意，仅 Server1、 Server2 和 Server3 包含的 slabs *MyVolume*。

### <a name="change-a-delimited-allocation"></a>更改分隔的分配

使用新`Add-StorageFaultDomain`和`Remove-StorageFaultDomain`cmdlet 以更改如何分隔分配。

例如，若要将移动*MyVolume*转移一台服务器：

1. 指定的第四个服务器**可以**存储的 slabs *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. 指定的第一台服务器**不能**存储的 slabs *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. 重新平衡存储池，以使更改生效：

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![关系图显示 slabs 将服务器 1、 2 和 3 中 en 情况迁移到服务器 2、 3 和 4。](media/delimit-volume-allocation/move.gif)

您可以使用重新平衡的进度`Get-StorageJob`。

完成后，确认*MyVolume*已通过运行`Get-VirtualDiskFootprintBySSU.ps1`试。

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

请注意，Server1 不包含的 slabs *MyVolume*不再 – 相反，Server04 会。

## <a name="best-practices"></a>最佳做法

下面是在使用分隔卷分配时应遵循的最佳实践：

### <a name="choose-three-servers"></a>选择三个服务器

不是更分隔为三个服务器，每个三向镜像卷。

### <a name="balance-storage"></a>平衡存储

平衡多少存储空间分配给每个服务器，考虑卷大小。

### <a name="every-delimited-allocation-unique"></a>每个唯一的分隔的分配

若要最大化容错能力，使每个卷分配唯一的这意味着它不会共享*所有*它与另一个卷 （某种重叠是可行的） 的服务器。 与 N 服务器有"N 选择 3"唯一组合 – 这意味着对于一些常见的群集大小：

| 服务器 (N) 数量 | 唯一数分隔的分配 （N 选择 3） |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > 请考虑此有用的评审[组合，然后选择表示法](https://betterexplained.com/articles/easy-permutations-and-combinations/)。

下面是用于最大化容错能力的一个示例： 每个卷具有唯一的分隔的分配：

![unique-allocation](media/delimit-volume-allocation/unique-allocation.png)

相反，在下一步的示例中前, 三个卷使用相同的分隔的分配 （为服务器 1、 2 和 3） 和最后三个卷使用相同的分隔的分配 （到 4、 5 和 6 的服务器）。 这不会最大化容错能力： 如果三个服务器失败，可能会脱机并次变得无法访问多个卷。

![non-unique-allocation](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>分析

本部分中派生卷保持联机并可访问的数学概率 （或反之，整个存储空间保持联机并可访问的预期的部分） 作为数故障和群集大小的函数。

   > [!NOTE]
   > 本部分是可选的读取。 如果你热衷若要查看数学计算，请继续阅读 ！ 但如果没有，请不要担心：[在 PowerShell 中的使用情况](#usage-in-powershell)并[最佳实践](#best-practices)只需成功实现带分隔符的分配。

### <a name="up-to-two-failures-is-always-okay"></a>最多两次故障是始终可行

每个三向镜像卷可以最多两个从故障中恢复在同一时间，作为[这些示例](storage-spaces-fault-tolerance.md#examples)说明，而不考虑其分配。 如果两个驱动器发生故障，或两个服务器失败，或一个每、 每三向镜像卷保持联机并可访问，即使使用正则分配。

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>半个多群集故障是永远不会正常

相反，在超过一半的服务器或群集中的驱动器，失败的极端情况下[仲裁是丢失](understand-quorum.md)和每个三向镜像卷进入脱机状态，并且变得无法访问，而不考虑其分配。

### <a name="what-about-in-between"></a>呢之间？

如果三个或多个故障发生在一次，但至少一半的服务器和驱动器仍能启动，使用带分隔符的分配的卷可能会保持联机并可访问，具体取决于哪些服务器具有失败。 让我们运行的编号，以确定精确的几率。

为简单起见，假定卷独立且相同分布式 (IID) 根据更高版本的最佳实践和该足够的独特组合可用于每个卷的分配是唯一的。 任何给定的卷可以幸存，但的概率也是可以幸存，但通过线性的假定条件下的总存储区的预期的部分。 

给定**N**这些服务器**F**分配给卷时出现故障， **3**它们进入脱机如果-和-仅限-如果所有**3** 即属于**F**但失败。 有 **（N 选择 F）** 方式**F**故障发生，其中 **（F 选择 3）** 导致卷进入脱机状态并变得无法访问。 概率可以表示为：

![P_offline = Fc3 / NcF](media/delimit-volume-allocation/probability-volume-offline.png)

在所有其他情况下，在卷保持联机并可访问：

![P_online = 1 – (Fc3 / NcF)](media/delimit-volume-allocation/probability-volume-online.png)

下表评估某些常见的群集大小和多达 5 失败，显示带分隔符的分配会增加相比正则被视为每种情况下分配容错能力的概率。

### <a name="with-6-servers"></a>使用 6 个服务器

| 分配                           | 存活的 1 故障的概率 | 存活的 2 个故障的概率 | 存活的 3 个故障的概率 | 存活的 4 个故障的概率 | 存活的 5 个故障的概率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分散到所有 6 个服务器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔到只有 3 个服务器          | 100%                               | 100%                                | 95.0%                               | 0%                                  | 0%                                  |

   > [!NOTE]
   > 带 6 服务器总数超过 3 发生故障之后, 群集失去仲裁。

### <a name="with-8-servers"></a>使用 8 台服务器

| 分配                           | 存活的 1 故障的概率 | 存活的 2 个故障的概率 | 存活的 3 个故障的概率 | 存活的 4 个故障的概率 | 存活的 5 个故障的概率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分布在所有 8 服务器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔到只有 3 个服务器          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0%                                  |

   > [!NOTE]
   > 4 个以上发生故障之后超出 8 服务器总数、 群集失去仲裁。

### <a name="with-12-servers"></a>使用 12 个服务器

| 分配                            | 存活的 1 故障的概率 | 存活的 2 个故障的概率 | 存活的 3 个故障的概率 | 存活的 4 个故障的概率 | 存活的 5 个故障的概率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分布在所有的 12 个服务器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔到只有 3 个服务器           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>与 16 台服务器

| 分配                            | 存活的 1 故障的概率 | 存活的 2 个故障的概率 | 存活的 3 个故障的概率 | 存活的 4 个故障的概率 | 存活的 5 个故障的概率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分布在所有 16 台服务器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔到只有 3 个服务器           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="can-i-delimit-some-volumes-but-not-others"></a>可以界定某些卷而不是其他？

是。 您可以选择每个卷，来分隔分配。

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>带分隔符的分配更改驱动器更换的工作原理？

否，它是与使用常规分配相同。

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [存储空间直通中的容错能力](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>附录

此脚本可帮助你查看你的卷的分配方式。

若要使用它，如上文所述，复制/粘贴和另存为`Get-VirtualDiskFootprintBySSU.ps1`。

```PowerShell
Function ConvertTo-PrettyCapacity {
    Param (
        [Parameter(
            Mandatory = $True,
            ValueFromPipeline = $True
            )
        ]
    [Int64]$Bytes,
    [Int64]$RoundTo = 0
    )
    If ($Bytes -Gt 0) {
        $Base = 1024
        $Labels = ("bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        $Order = [Math]::Floor( [Math]::Log($Bytes, $Base) )
        $Rounded = [Math]::Round($Bytes/( [Math]::Pow($Base, $Order) ), $RoundTo)
        [String]($Rounded) + " " + $Labels[$Order]
    }
    Else {
        "0"
    }
    Return
}

Function Get-VirtualDiskFootprintByStorageFaultDomain {

    ################################################
    ### Step 1: Gather Configuration Information ###
    ################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Gathering configuration information..." -Status "Step 1/4" -PercentComplete 00

    $ErrorCannotGetCluster = "Cannot proceed because 'Get-Cluster' failed."
    $ErrorNotS2DEnabled = "Cannot proceed because the cluster is not running Storage Spaces Direct."
    $ErrorCannotGetClusterNode = "Cannot proceed because 'Get-ClusterNode' failed."
    $ErrorClusterNodeDown = "Cannot proceed because one or more cluster nodes is not Up."
    $ErrorCannotGetStoragePool = "Cannot proceed because 'Get-StoragePool' failed."
    $ErrorPhysicalDiskFaultDomainAwareness = "Cannot proceed because the storage pool is set to 'PhysicalDisk' fault domain awareness. This cmdlet only supports 'StorageScaleUnit', 'StorageChassis', or 'StorageRack' fault domain awareness."

    Try  {
        $GetCluster = Get-Cluster -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetCluster
    }

    If ($GetCluster.S2DEnabled -Ne 1) {
        throw $ErrorNotS2DEnabled
    }

    Try  {
        $GetClusterNode = Get-ClusterNode -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetClusterNode
    }

    If ($GetClusterNode | Where State -Ne Up) {
        throw $ErrorClusterNodeDown
    }

    Try {
        $GetStoragePool = Get-StoragePool -IsPrimordial $False -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetStoragePool
    }

    If ($GetStoragePool.FaultDomainAwarenessDefault -Eq "PhysicalDisk") {
        throw $ErrorPhysicalDiskFaultDomainAwareness
    }

    ###########################################################
    ### Step 2: Create SfdList[] and PhysicalDiskToSfdMap{} ###
    ###########################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing physical disk information..." -Status "Step 2/4" -PercentComplete 25

    $SfdList = Get-StorageFaultDomain -Type ($GetStoragePool.FaultDomainAwarenessDefault) | Sort FriendlyName # StorageScaleUnit, StorageChassis, or StorageRack

    $PhysicalDiskToSfdMap = @{} # Map of PhysicalDisk.UniqueId -> StorageFaultDomain.FriendlyName
    $SfdList | ForEach {
        $StorageFaultDomain = $_
        $_ | Get-StorageFaultDomain -Type PhysicalDisk | ForEach {
            $PhysicalDiskToSfdMap[$_.UniqueId] = $StorageFaultDomain.FriendlyName
        }
    }

    ##################################################################################################
    ### Step 3: Create VirtualDisk.FriendlyName -> { StorageFaultDomain.FriendlyName -> Size } Map ###
    ##################################################################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing virtual disk information..." -Status "Step 3/4" -PercentComplete 50

    $GetVirtualDisk = Get-VirtualDisk | Sort FriendlyName

    $VirtualDiskMap = @{}

    $GetVirtualDisk | ForEach {
        # Map of PhysicalDisk.UniqueId -> Size for THIS virtual disk
        $PhysicalDiskToSizeMap = @{}
        $_ | Get-PhysicalExtent | ForEach {
            $PhysicalDiskToSizeMap[$_.PhysicalDiskUniqueId] += $_.Size
        }
        # Map of StorageFaultDomain.FriendlyName -> Size for THIS virtual disk
        $SfdToSizeMap = @{}
        $PhysicalDiskToSizeMap.keys | ForEach {
            $SfdToSizeMap[$PhysicalDiskToSfdMap[$_]] += $PhysicalDiskToSizeMap[$_]
        }
        # Store
        $VirtualDiskMap[$_.FriendlyName] = $SfdToSizeMap
    }

    #########################
    ### Step 4: Write-Out ###
    #########################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Formatting output..." -Status "Step 4/4" -PercentComplete 75

    $Output = $GetVirtualDisk | ForEach {
        $Row = [PsCustomObject]@{}

        $VirtualDiskFriendlyName = $_.FriendlyName
        $Row | Add-Member -MemberType NoteProperty "VirtualDiskFriendlyName" $VirtualDiskFriendlyName

        $TotalFootprint = $_.FootprintOnPool | ConvertTo-PrettyCapacity
        $Row | Add-Member -MemberType NoteProperty "TotalFootprint" $TotalFootprint

        $SfdList | ForEach {
            $Size = $VirtualDiskMap[$VirtualDiskFriendlyName][$_.FriendlyName] | ConvertTo-PrettyCapacity
            $Row | Add-Member -MemberType NoteProperty $_.FriendlyName $Size
        }

        $Row
    }

    # Calculate width, in characters, required to Format-Table
    $RequiredWindowWidth = ("TotalFootprint").length + 1 + ("VirtualDiskFriendlyName").length + 1
    $SfdList | ForEach {
        $RequiredWindowWidth += $_.FriendlyName.Length + 1
    }

    $ActualWindowWidth = (Get-Host).UI.RawUI.WindowSize.Width

    If (!($ActualWindowWidth)) {
        # Cannot get window width, probably ISE, Format-List
        Write-Warning "Could not determine window width. For the best experience, use a Powershell window instead of ISE"
        $Output | Format-Table
    }
    ElseIf ($ActualWindowWidth -Lt $RequiredWindowWidth) {
        # Narrower window, Format-List
        Write-Warning "For the best experience, try making your PowerShell window at least $RequiredWindowWidth characters wide. Current width is $ActualWindowWidth characters."
        $Output | Format-List
    }
    Else {
        # Wider window, Format-Table
        $Output | Format-Table
    }
}

Get-VirtualDiskFootprintByStorageFaultDomain
```
