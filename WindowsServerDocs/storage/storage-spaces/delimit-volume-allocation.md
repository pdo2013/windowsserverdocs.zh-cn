---
title: 分隔存储空间直通中的卷分配
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: faf9547833764e9075e86515d1f486a5a3f61ff8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872077"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>分隔存储空间直通中的卷分配
> 适用于：Windows Server 2019

Windows Server 2019 引入了一个选项，用于在存储空间直通中手动分隔卷的分配。 这样做可能会在某些情况下显著增加容错能力，但会带来一些额外的管理注意事项和复杂性。 本主题说明了它的工作原理并提供了 PowerShell 中的示例。

   > [!IMPORTANT]
   > 此功能是 Windows Server 2019 中的新增功能。 它在 Windows Server 2016 中不可用。 

## <a name="prerequisites"></a>先决条件

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![绿色复选标记图标。](media/delimit-volume-allocation/supported.png) 如果是以下情况，请考虑使用此选项：

- 群集包含六个或更多服务器;与
- 群集只使用[三向镜像](storage-spaces-fault-tolerance.md#mirroring)复原

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![红色 X 图标。](media/delimit-volume-allocation/unsupported.png) 如果是以下情况，请不要使用此选项：

- 你的群集包含六个以上的服务器;或
- 群集使用[奇偶](storage-spaces-fault-tolerance.md#parity)校验或[镜像加速奇偶校验](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)复原

## <a name="understand"></a>了解

### <a name="review-regular-allocation"></a>查看：常规分配

通过常规的三向镜像，卷划分为多个较小的 "碎片"，复制三次，并均匀地分布在群集中每个服务器上的每个驱动器上。 有关更多详细信息，请阅读[此深入探讨博客](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)。

![此图显示了将卷划分为三个碎片堆栈，并跨每个服务器平均分布。](media/delimit-volume-allocation/regular-allocation.png)

此默认分配最大程度地提高了并行读取和写入量，从而提高了性能，并使其简易性非常有吸引力：每台服务器都是繁忙的，每个驱动器都是相同的，所有卷都保持联机或脱机。 每个卷都保证能经受多达两个并发故障，如[这些示例](storage-spaces-fault-tolerance.md#examples)所示。

但是，通过此分配，卷不会经受三个并发故障。 如果三台服务器一次失败，或者三台服务器中的驱动器同时出现故障，则卷将无法访问，因为至少有一些碎片（极高的概率）分配给了出现故障的三个驱动器或服务器。

在下面的示例中，服务器1、3和5同时出现故障。 尽管许多碎片都有保留下来的副本，但有些没有：

![显示六台服务器中的三个服务器，以红色突出显示，整个卷为红色。](media/delimit-volume-allocation/regular-does-not-survive.png)

卷处于脱机状态，在恢复服务器之前将无法访问。

### <a name="new-delimited-allocation"></a>新建：分隔分配

使用分隔分配，你可以指定要使用的服务器子集（三向镜像使用最少三个）。 卷被分为三次（如之前）复制的碎片，而不是在每个服务器之间分配，而是**只将碎片分配给指定的服务器的子集**。

![此图显示了将卷划分为三个碎片堆栈，并将其分布到了六个服务器。](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>优点

通过此分配，卷可能会经受三个并发故障：事实上，在这种情况下，其生存概率从 0% （通过常规分配）增加到 95% （带分隔分配）！ 直观地说，这是因为它不依赖于服务器4、5或6，因此不受其故障的影响。

在上面的示例中，服务器1、3和5同时出现故障。 由于分隔分配确保了 server 2 包含每个楼板的副本，因此每个楼板都有一个保留下来的副本，并且卷处于联机状态并且可访问：

![显示了六个服务器中的三个（红色突出显示），但整个音量为绿色。](media/delimit-volume-allocation/delimited-does-survive.png)

生存概率取决于服务器的数量和其他因素–有关详细信息，请参阅[分析](#analysis)。

#### <a name="disadvantages"></a>缺点

分隔分配会带来一些额外的管理注意事项和复杂性：

1. 管理员负责界定每个卷的分配，以平衡服务器之间的存储使用率，并使流量的严重程度达到平衡，如 "[最佳做法](#best-practices)" 一节中所述。

2. 通过分隔分配，保留**每个服务器一个容量驱动器的等效值（无最大值）** 。 这比用于常规分配的[已发布建议](plan-volumes.md#choosing-the-size-of-volumes)更多，其中以至于耗光了4个容量驱动器总计。

3. 如果服务器发生故障，需要更换，如[删除服务器及其驱动器](remove-servers.md#remove-a-server-and-its-drives)中所述，管理员负责通过添加新的服务器并删除失败的卷来更新受影响的卷的 delimitation-以下示例。

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

可以使用`New-Volume` cmdlet 在存储空间直通中创建卷。

例如，若要创建常规的三向镜像卷：

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>创建卷并界定其分配

创建三向镜像卷并界定其分配：

1. 首先，将群集中的服务器分配给变量`$Servers`：

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > 在存储空间直通中，术语 "存储缩放单位" 指连接到一台服务器的所有原始存储，包括直接连接驱动器和带有驱动器的直接连接的外部机箱。 在此上下文中，它与 "服务器" 相同。

2. 使用新`-StorageFaultDomainsToUse`参数指定要使用的服务器，并通过将索引`$Servers`到中。 例如，要将分配界定为第一个、第二个和第三个服务器（索引0、1和2）：

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>查看分隔分配

若要查看如何分配*MyVolume* ，请使用`Get-VirtualDiskFootprintBySSU.ps1` [附录](#appendix)中的脚本：

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

请注意，只有 Server1、Server2 和 Server3 包含*MyVolume*的碎片。

### <a name="change-a-delimited-allocation"></a>更改分隔分配

使用新`Add-StorageFaultDomain`的和`Remove-StorageFaultDomain` cmdlet 来更改分配的分隔方式。

例如，要将*MyVolume*移到一台服务器上：

1. 指定第四个服务器**可以**存储*MyVolume*的碎片：

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. 指定第一台服务器**无法**存储*MyVolume*的碎片：

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. 重新平衡存储池以使更改生效：

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![显示碎片将一起从服务器1、2和3迁移到服务器2、3和4的关系图。](media/delimit-volume-allocation/move.gif)

你可以通过`Get-StorageJob`监视重新平衡的进度。

完成后，验证*MyVolume*是否已通过再次运行`Get-VirtualDiskFootprintBySSU.ps1`进行了移动。

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

请注意，Server1 不会再包含*MyVolume*的碎片–而是 Server04。

## <a name="best-practices"></a>最佳实践

下面是使用带分隔符的卷分配时应遵循的最佳做法：

### <a name="choose-three-servers"></a>选择三个服务器

将每个三向镜像卷分割为三个服务器，而不是更多。

### <a name="balance-storage"></a>平衡存储

平衡分配给每个服务器的存储量，并考虑卷大小。

### <a name="every-delimited-allocation-unique"></a>每个分隔分配唯一

为了最大限度地提高容错能力，请确保每个卷的分配都是唯一的，这意味着它不会与另一个卷共享其*所有*服务器（有一些重叠问题）。 对于 N 台服务器，有 "N 个选择 3" 的唯一组合–对于某些常见的群集大小，这意味着：

| 服务器数（N） | 唯一分隔分配数（N 选择3） |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > 请考虑这对简称有帮助[，并选择表示法](https://betterexplained.com/articles/easy-permutations-and-combinations/)。

下面是最大化容错的示例–每个卷都具有唯一的分隔分配：

![唯一分配](media/delimit-volume-allocation/unique-allocation.png)

相反，在下一个示例中，前三个卷使用相同的分隔分配（对服务器1、2和3），最后三个卷使用相同的分隔分配（到服务器4、5和6）。 这并不能最大程度地提高容错能力：如果三台服务器发生故障，则多个卷可能会脱机，并且无法同时访问。

![非唯一分配](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Analysis

此部分派生了一个卷处于联机状态且可访问（或等效于处于联机和可访问状态的总体存储的预期小数部分）的数学概率，作为失败次数和群集大小的函数。

   > [!NOTE]
   > 本部分是可选阅读内容。 如果想看到数学计算，请继续阅读！ 但如果没有，别担心：[在 PowerShell 中使用](#usage-in-powershell)和[最佳做法](#best-practices)，只需成功实现分隔分配。

### <a name="up-to-two-failures-is-always-okay"></a>最多可以有两个故障

每个三向镜像卷同时保留多达两个故障，如[这些示例](storage-spaces-fault-tolerance.md#examples)所示，而不考虑其分配。 如果两个驱动器发生故障，或者两个服务器发生故障，或者每个服务器发生故障，则每个三向镜像卷都保持联机和可访问，即使采用常规分配也是如此。

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>超过一半的群集故障永远无法正常工作

相反，在极端情况下，群集中超过一半的服务器或驱动器无法同时发生故障，[仲裁会丢失](understand-quorum.md)，并且每个三向镜像卷都将处于脱机状态且不可访问，不管其分配情况如何。

### <a name="what-about-in-between"></a>与之间的区别是什么？

如果一次发生了三次或更多故障，但至少有一半的服务器和驱动器仍处于开启状态，则带分隔分配的卷可能会保持联机并可访问，具体取决于哪些服务器发生故障。 让我们运行数字来确定具体的几率。

为简单起见，假定卷是根据上述最佳实践单独和相同的分布式（IID），而且有足够的唯一组合可用于每个卷的分配。 任何给定的卷置的概率也是预期的线性置的总体存储的预计分数。 

如果 a **N**台服务器**发生故障**，则分配给**3**个服务器的卷将在每**次都处于**脱机**状态的情况**下变为脱机状态。 发生**F**故障的方法有 **（N）** ，其中 **（F 选择3）** 导致卷脱机且变为不可访问。 概率可表示为：

![P_offline = Fc3/NcF](media/delimit-volume-allocation/probability-volume-offline.png)

在所有其他情况下，该卷保持联机和可访问：

![P_online = 1 –（Fc3/NcF）](media/delimit-volume-allocation/probability-volume-online.png)

下表评估了某些常见群集大小的概率和最多5个故障，这表明，在每种情况下，在考虑到的每种情况下，分隔分配会提高容错能力。

### <a name="with-6-servers"></a>带有6台服务器

| 分配                           | 导致1个故障的概率 | 出现2次故障的概率 | 导致3个故障的概率 | 出现4次故障的概率 | 5次失败的概率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分布在所有6个服务器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 仅限3个服务器          | 100%                               | 100%                                | 95.0%                               | 0                                  | 0                                  |

   > [!NOTE]
   > 6个以上的服务器故障后，群集将失去仲裁。

### <a name="with-8-servers"></a>具有8个服务器

| 分配                           | 导致1个故障的概率 | 出现2次故障的概率 | 导致3个故障的概率 | 出现4次故障的概率 | 5次失败的概率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分布在所有8个服务器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 仅限3个服务器          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0                                  |

   > [!NOTE]
   > 超过4个失败的服务器总数后，群集将失去仲裁。

### <a name="with-12-servers"></a>具有12个服务器

| 分配                            | 导致1个故障的概率 | 出现2次故障的概率 | 导致3个故障的概率 | 出现4次故障的概率 | 5次失败的概率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分布在所有12个服务器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 仅限3个服务器           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>含16台服务器

| 分配                            | 导致1个故障的概率 | 出现2次故障的概率 | 导致3个故障的概率 | 出现4次故障的概率 | 5次失败的概率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 常规，分布在所有16个服务器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 仅限3个服务器           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="can-i-delimit-some-volumes-but-not-others"></a>是否可以分隔某些卷，但不能分隔其他卷？

是。 无论是否分隔分配，都可以选择每个卷。

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>分隔分配是否会改变驱动器更换的工作方式？

不会，它与常规分配相同。

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [存储空间直通中的容错能力](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>附录

此脚本可帮助你查看卷的分配方式。

如以上所述，请复制/粘贴并另存为`Get-VirtualDiskFootprintBySSU.ps1`。

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
