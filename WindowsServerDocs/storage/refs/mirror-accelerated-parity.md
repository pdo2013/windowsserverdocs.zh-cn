---
title: 镜像加速奇偶校验
ms.prod: windows-server
ms.author: gawatu
ms.manager: masriniv
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 10/17/2018
ms.assetid: ''
ms.openlocfilehash: 2721f1c744c5c03d8e4bce0508fd23fa5237f95f
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591098"
---
# <a name="mirror-accelerated-parity"></a>镜像加速奇偶校验

>适用于： Windows Server 2019、Windows Server 2016

存储空间可通过两种基本技术为数据提供容错功能：镜像和奇偶校验。 在[存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)中，ReFS 引入了镜像加速奇偶校验，可用于创建使用镜像和奇偶校验复原的卷。 镜像加速奇偶校验提供了低成本、节省空间的存储，而不影响性能。 

![Mirror-Accelerated-Parity-Volume](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume.png)

## <a name="background"></a>后台

镜像和奇偶校验复原方案具有截然不同的存储和性能特征：
- 镜像复原允许用户实现快速写入性能，但复制每个副本的数据不会节省空间。 
- 另一方面，奇偶校验必须为每次写入重新计算奇偶校验，这使得随机写入性能受到影响。 但是，奇偶校验使用户可以在存储数据时节省更多的空间。 有关详细信息，请参阅[存储空间容错](../storage-spaces/Storage-Spaces-Fault-Tolerance.md)。

因此，镜像倾向于提供对性能敏感的存储，而奇偶校验可提升存储容量利用率。 在镜像加速奇偶校验中，ReFS 可在单个卷中组合使用这两种复原方案，以充分利用每种复原类型提供节省空间且对性能敏感的存储的优势。

## <a name="data-rotation-on-mirror-accelerated-parity"></a>镜像加速奇偶校验中的数据旋转

ReFS 会主动在镜像与奇偶校验之间实时旋转数据。 这样，便可将传入写入快速写入镜像，然后将其旋转到奇偶校验以进行高效存储。 为此，将在镜像中快速提供传入 IO，同时将冷数据高效地存储在奇偶校验中，以在同一个卷中提供最佳性能和低成本存储。 

若要在镜像与奇偶校验之间旋转数据，ReFS 会以逻辑方式将卷划分为 64 MiB 的区域，这些区域是旋转单元。 下图描述划分为区域的镜像加速奇偶校验卷。 

![Mirror-Accelerated-Parity-Volume-with-Storage-Containers](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume-with-Storage-Containers.png)

在镜像层达到了指定的容量级别后，ReFS 开始将整个区域从镜像旋转到奇偶校验。 如若可能，ReFS 会等待并将数据保留在镜像中，而不是立即将数据从镜像移动到奇偶校验，这使得 ReFS 可以继续为数据提供最佳性能（请参阅下面的“IO 性能”）。 

在数据从镜像移动到奇偶校验时，系统将读取数据，并计算奇偶校验编码，然后将该数据写入奇偶校验。 以下动画使用在旋转期间转换为擦除编码区域的三向镜像区域说明这一点：

![Mirror-Accelerated-Parity-Rotation](media/mirror-accelerated-parity/Container-Rotation.gif)

## <a name="io-on-mirror-accelerated-parity"></a>镜像加速奇偶校验中的 IO
### <a name="io-behavior"></a>IO 行为
**写入**：ReFS 服务的传入写入，采用下面三种不同方法：

1.  **写入镜像：**

    - **1a.** 如果传入写入修改镜像中的现有数据，ReFS 将就地修改该数据。
    - **1b.** 如果传入写入是新写入，并且 ReFS 可以在镜像中成功找到足够多的可用空间，以供此写入使用，则 ReFS 将写入镜像。
    ![Write 到镜像 ](media/mirror-accelerated-parity/Write-to-Mirror.png)

2. **写入镜像，从奇偶校验重新分配：**

    如果传入写入修改了处于奇偶校验的数据，并且 ReFS 可以在镜像中成功找到足够的可用空间来为传入写入提供服务，ReFS 将首先使奇偶校验中以前的数据失效，然后写入镜像。 此失效是一项低成本的快速元数据操作，有助于显著提高写入奇偶校验的性能。
    ![Reallocated 写入 ](media/mirror-accelerated-parity/Reallocated-Write.png)

3. **写入奇偶校验：**
    
    如果 ReFS 在镜像中无法成功找到足够多的可用空间，则 ReFS 会将新数据写入奇偶校验，或直接修改奇偶校验中的现有数据。 下面的“性能优化”部分提供有助于最大限度减少写入奇偶校验的指导。
    ![Write 到奇偶校验 ](media/mirror-accelerated-parity/Write-to-Parity.png)

**读取**：ReFS 将从包含相关数据的层直接读取。 如果使用 HDD 构造奇偶校验，则存储空间直通中的缓存将对此数据进行缓存，以加快未来读取。 

> [!NOTE]
> 读取绝不会导致 ReFS 将数据旋转回镜像层中。 

### <a name="io-performance"></a>IO 性能

**写入**：上述每种写入都有其自己的性能特征。 大致说，写入镜像层比重新分配的写入快得多，而重新分配的写入比直接写入奇偶校验层也要快得多。 此关系由以下不等式表示： 


- **镜像层 > 重新分配写入 > > 奇偶校验层**


**读取**：从奇偶校验读取时，实际上不会对性能产生负面影响：
- 如果使用相同的媒体类型构造镜像和奇偶校验，则读取性能将等效。 
- 如果使用不同的媒体类型（例如镜像的 SSD 和奇偶校验 HDD）构造镜像和奇偶校验，则[存储空间直通中的缓存](../storage-spaces/understand-the-cache.md)将有助于缓存热数据，以加速从奇偶校验的任何读取。

## <a name="refs-compaction"></a>ReFS 压缩

在此秋季半年发布中，ReFS 引入了压缩功能，这大大提高了90% 的镜像加速奇偶校验卷的性能。 

**背景**：以前，当镜像加速奇偶校验卷已满时，这些卷的性能可能会降低。 性能降低的原因是，在整个卷超时过程中混合了热数据和冷数据。 这意味着，在镜像中可存储的热数据减少，因为冷数据占用了镜像中热数据可能以其他方式使用的空间。 将热数据存储在镜像中是保持高性能的关键，因为直接写入镜像比重新分配的写入快得多，还比直接写入奇偶校验快几个数量级。 因此，将冷数据保存在镜像中会降低性能，因为这降低了 ReFS 可直接写入镜像的可能性。 

ReFS 压缩可为热数据释放镜像中的空间，从而使这些性能问题迎刃而解。 压缩先将所有数据从镜像和奇偶校验合并到奇偶校验中。 这减少了卷中的碎片，并增加了镜像中的可寻址空间量。 更重要的是，此过程使 ReFS 可将热数据重新合并到镜像中：
-   当新写入传入时，系统将在镜像中提供这些写入。 因此，新写入的热数据驻留在镜像中。 
-   在对奇偶校验中的数据执行修改写入时，ReFS 将执行重新分配的写入，因此，还将在镜像中提供此写入。 因此，在压缩期间移入奇偶校验的热数据将重新分配回镜像中。 

## <a name="performance-optimizations"></a>性能优化

>[!IMPORTANT]
> 建议将重写 Vhd 置于不同的子目录。 这是因为 ReFS 会将元数据更改写入目录及其文件的级别。 因此，如果在目录之间分发大量写入文件，元数据操作会更小并并行运行，从而减少了应用的延迟。

### <a name="performance-counters"></a>性能计数器

ReFS 可维护性能计数器，以帮助评估镜像加速奇偶校验的性能。 
- 如上文在写入到奇偶校验部分中所述，当找不到镜像中的可用空间时，ReFS 将直接写入奇偶校验。 通常，如果镜像层的填充速度比 ReFS 可将数据旋转到奇偶校验的速度要快，则会出现这种情况。 换而言之，ReFS 旋转跟不上引入速率。 下面的性能计数器可标识 ReFS 直接写入奇偶校验的时间：

  ```
  # Windows Server 2016
  ReFS\Data allocations slow tier/sec
  ReFS\Metadata allocations slow tier/sec

  # Windows Server 2019
  ReFS\Allocation of Data Clusters on Slow Tier/sec
  ReFS\Allocation of Metadata Clusters on Slow Tier/sec
  ```

- 如果这些计数器非零，这表示 ReFS 无法足够快地将数据旋出镜像。 为了帮助解决这一问题，用户可以更改旋转主动性，或增加镜像层的大小。

### <a name="rotation-aggressiveness"></a>旋转入侵

ReFS 在镜像达到了指定的容量阈值后开始旋转数据。
-   此旋转阈值的值越高，ReFS 在镜像层中保留数据的时间就越长。 在镜像层中保留热数据对性能最为有利，但是，ReFS 将无法有效地提供大量的传入 IO。 
-   如果值较低，则 ReFS 可以主动取消暂存数据，并更好地引入传入 IO。 这适用于需要大量引入的工作负载，例如存档存储。 但是，如果值较低，则可能会降低常规用途工作负载的性能。 在不必要的情况下，将数据旋出镜像层会对性能产生负面影响。 

ReFS 引入了可调参数以调整此阈值，该阈值可使用注册表项进行配置。 此注册表项必须在**存储空间直通中的每个节点**上进行配置，并且需要重新启动，任何更改才能生效。 
-   **注册表项**：HKEY_LOCAL_MACHINE\System\CurrentControlSet\Policies
-   **ValueName (DWORD)** ：DataDestageSsdFillRatioThreshold
-   **ValueType**：百分比

如果未设置此注册表项，则 ReFS 将使用默认值 85%。  建议对大多数部署使用此默认值，不建议使用小于 50% 的值。 下面的 PowerShell 命令演示如何使用值 75% 设置此注册表项： 
```PowerShell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75
 ```
 若要在存储空间直通部署中的每个节点上配置此注册表项，可以使用以下 PowerShell 命令：
 ```PowerShell
 $Nodes = 'S2D-01', 'S2D-02', 'S2D-03', 'S2D-04'
 Invoke-Command $Nodes {Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75}
 ```

### <a name="increasing-the-size-of-the-mirrored-tier"></a>增加镜像层的大小

如果增加镜像层的大小，则 ReFS 可将大部分的工作集保留在镜像中。 这将提高 ReFS 可直接写入镜像的可能性，从而帮助提供更出色的性能。 下面的 PowerShell cmdlet 演示如何增加镜像层的大小：
```PowerShell
Resize-StorageTier -FriendlyName “Performance” -Size 20GB
Resize-StorageTier -InputObject (Get-StorageTier -FriendlyName “Performance”) -Size 20GB
```
>[!TIP]
>调整 **StorageTier** 的大小后，请确保调整**分区**和**卷**的大小。 有关详细信息和示例，请参阅[调整卷大小](../storage-spaces/resize-volumes.md)。

## <a name="creating-a-mirror-accelerated-parity-volume"></a>创建镜像加速奇偶校验卷

下面的 PowerShell cmdlet 可创建镜像/奇偶校验比为 20:80 的镜像加速奇偶校验卷，这是推荐用于大多数工作负载的配置。 有关详细信息和示例，请参阅[在存储空间直通中创建卷](../storage-spaces/Create-volumes.md)。

```PowerShell
New-Volume – FriendlyName “TestVolume” -FileSystem CSVFS_ReFS -StoragePoolFriendlyName “StoragePoolName” -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 200GB, 800GB
```

## <a name="see-also"></a>另请参阅

-   [ReFS 概述](refs-overview.md)
-   [ReFS 块克隆](block-cloning.md)
-   [ReFS 完整性流](integrity-streams.md)
-   [存储空间直通概述](../storage-spaces/storage-spaces-direct-overview.md)
