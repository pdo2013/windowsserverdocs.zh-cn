---
title: 驱动器存储空间直通的对称性的注意事项
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866878"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>驱动器存储空间直通的对称性的注意事项 

> 适用于：Windows Server 2019、Windows Server 2016

[存储空间直通](storage-spaces-direct-overview.md)每台服务器具有完全相同的驱动器时效果最佳。

实际上，我们识别这并不总是可行：存储空间直通旨在根据你的组织的需求的增长运行年和缩放。 现在，可能会购买硬盘驱动器; spacious 3 TB下一年，它可能是不可能以查找与如此之小。 因此，支持一定量的混合和匹配。

本主题说明约束，并提供示例的支持和不受支持的配置。

## <a name="constraints"></a>约束

### <a name="type"></a>在任务栏的搜索框中键入

所有服务器都应都具有相同[类型的驱动器](choosing-drives.md#drive-types)。

例如，如果一台服务器具有 NVMe，它们应*所有*具有 NVMe。

### <a name="number"></a>编号

所有服务器应都具有相同数量的每个类型的驱动器。

例如，如果一台服务器具有六个 SSD，它们应*所有*有六个 SSD。

   > [!NOTE]
   > 是可行的不同暂时在故障期间或者在添加或删除驱动器的驱动器数。

### <a name="model"></a>型号

我们建议使用相同的模型和固件版本，只要有可能的驱动器。 如果不能请仔细选择尽可能类似的驱动器。 我们建议不要具有明显不同的性能或耐用性特征的相同类型的混合和匹配的驱动器 （除非其中一个是缓存和另一个是容量） 因为存储空间直通将 IO 均匀地分布和不区分基础模型上.

   > [!NOTE]
   > 则可以暂时对混合和匹配类似 SATA 和 SAS 驱动器。

### <a name="size"></a>大小

我们建议使用相同的大小只要有可能的驱动器。 使用不同大小的容量驱动器可能会导致一些不可用的容量，并使用不同大小的缓存驱动器可能无法提高缓存性能。 有关详细信息请参阅下一步。

   > [!WARNING]
   > 在服务器之间的不同容量的驱动器大小可能会导致闲置容量。

## <a name="understand-capacity-imbalance"></a>了解： 容量不平衡

存储空间直通是可靠的容量不平衡到跨驱动器和服务器。 即使不均衡非常严重，所有内容将继续工作。 但是，具体取决于若干因素中的每台服务器不可用的容量可能不可用。

若要查看发生此问题，请考虑下面的简化的示意图。 每个彩色的框表示镜像数据的一个副本。 例如，对应的框标记为 A、 A，和 ' 的相同数据的三个副本。 若要遵循服务器容错功能，这些副本*必须*存储在不同的服务器。

### <a name="stranded-capacity"></a>闲置的容量

绘制，服务器 1 (10 TB) 和服务器 2 (10 TB) 已满。 服务器 3 具有较大的驱动器，因此其总容量为更大 (15 TB)。 但是，为服务器 3 上存储更多的三向镜像数据可能需要服务器 1 和服务器 2 上的副本，已完整。 不能使用服务器 3 上的剩余 5 TB 容量 – 这 *"闲置"* 容量。

![三向镜像，三个服务器闲置容量](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>最佳位置

相反，使用 10 TB，10 TB，10 TB 和 15 TB 的容量和三向镜像复原则的四个服务器它*是*可以有效地将副本放在使用所有可用容量，作为绘制一种方法。 只要可能，存储空间直通的分配器将查找并使用最佳位置，从而使任何闲置的容量。

![三向镜像、 四个服务器、 没有闲置的容量](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

闲置的容量是否会影响的服务器、 复原能力、 容量不平衡和其他因素的严重级别数。 **最明智的一般规则是假定的每个服务器中可用的唯一容量保证可用。**

## <a name="understand-cache-imbalance"></a>了解： 缓存不平衡

存储空间直通是稳定地缓存不平衡跨驱动器和服务器。 即使不均衡非常严重，所有内容将继续工作。 此外，存储空间直通始终使用最充分地所有可用的缓存。

但是，使用不同大小的缓存驱动器可能无法提高缓存性能统一或以可预测方式： 仅对 IO[驱动器绑定](understand-the-cache.md#server-side-architecture)使用更大的缓存驱动器可能会看到改进的性能。 存储空间直通将 IO 均匀地分布到绑定并不会区分根据缓存容量的比率。

![缓存不平衡](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > 请参阅[了解缓存](understand-the-cache.md)若要了解有关缓存绑定的详细信息。

## <a name="example-configurations"></a>配置示例

下面是一些支持和不受支持的配置：

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-between-servers"></a>![受支持](media/drive-symmetry-considerations/supported.png) 支持： 服务器之间的不同模型

前两个服务器使用 NVMe 模型"X"，而第三个服务器使用 NVMe 模型"Z"，这是非常相似。

| 服务器 1                    | 服务器 2                    | 服务器 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2x NVMe 模型 X （缓存）    | 2x NVMe 模型 X （缓存）    | 2x NVMe 模型 Z （缓存）    |
| 10 倍的 SSD 模型 Y （容量） | 10 倍的 SSD 模型 Y （容量） | 10 倍的 SSD 模型 Y （容量） |

支持此功能。

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-within-server"></a>![受支持](media/drive-symmetry-considerations/supported.png) 支持： 服务器内不同的模型

每个服务器使用 HDD 模型"Y"和"Z"，这非常类似一些不同的组合。 每个服务器都有 10 个总 HDD。

| 服务器 1                   | 服务器 2                   | 服务器 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 个 SSD 模型 X （缓存）    | 2 个 SSD 模型 X （缓存）    | 2 个 SSD 模型 X （缓存）    |
| 每周 7 天 HDD 模型 Y （容量） | 5 倍 HDD 模型 Y （容量） | 1 个 HDD 模型 Y （容量） |
| 3 倍 HDD 模型 Z （容量） | 5 倍 HDD 模型 Z （容量） | 9 x HDD 模型 Z （容量） |

支持此功能。

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-across-servers"></a>![受支持](media/drive-symmetry-considerations/supported.png) 支持： 在服务器之间的不同大小

前两个服务器使用 4 TB HDD，而第三个服务器使用非常类似 6 TB HDD。

| 服务器 1                | 服务器 2                | 服务器 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe （缓存） | 2 x 800 GB NVMe （缓存） | 2 x 800 GB NVMe （缓存） |
| 4 × 4 TB HDD （容量） | 4 × 4 TB HDD （容量） | 4 x 6 TB HDD （容量） |

支持此功能，尽管它会导致闲置容量。

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-within-server"></a>![受支持](media/drive-symmetry-considerations/supported.png) 支持： 服务器内的不同大小

每个服务器使用 1.2 TB 而非常类似 1.6 TB SSD 的一些不同组合。 每个服务器都有 4 个总 SSD。

| 服务器 1                 | 服务器 2                 | 服务器 3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1.2 TB SSD （缓存）   | 2 x 1.2 TB SSD （缓存）   | 4 x 1.2 TB SSD （缓存）   |
| 1 x 1.6 TB SSD （缓存）   | 2 x 1.6 TB SSD （缓存）   | -                        |
| 20 x 4 TB HDD （容量） | 20 x 4 TB HDD （容量） | 20 x 4 TB HDD （容量） |

支持此功能。

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-types-of-drives-across-servers"></a>![不受支持](media/drive-symmetry-considerations/unsupported.png) 不支持： 不同类型的跨服务器的驱动器

服务器 1 具有 NVMe 但却不可行。

| 服务器 1            | 服务器 2            | 服务器 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe （缓存）    | -                   | -                   |
| -                   | 6 x SSD （缓存）     | 6 x SSD （缓存）     |
| 18 x HDD （容量） | 18 x HDD （容量） | 18 x HDD （容量） |

不支持此功能。 驱动器类型应为每个服务器中相同。

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-number-of-each-type-across-servers"></a>![不受支持](media/drive-symmetry-considerations/unsupported.png) 不支持： 不同数量的服务器上每个类型

服务器 3 具有比其他更多驱动器。

| 服务器 1            | 服务器 2            | 服务器 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe （缓存）    | 2 x NVMe （缓存）    | 4 x NVMe （缓存）    |
| 10 x HDD （容量） | 10 x HDD （容量） | 20 x HDD （容量） |

不支持此功能。 每个类型的驱动器数应在每个服务器中相同。

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-only-hdd-drives"></a>![不受支持](media/drive-symmetry-considerations/unsupported.png) 不支持： 仅 HDD 驱动器

所有服务器都具有仅 HDD 驱动器的连接。

|服务器 1|服务器 2|服务器 3|
|-|-|-| 
|18 x HDD （容量） |18 x HDD （容量）|18 x HDD （容量）|

不支持此功能。 您需要添加至少两个缓存或的驱动器 （NvME SSD） 附加到每台服务器。

## <a name="summary"></a>总结

概括来说，在群集中的每个服务器应具有相同类型的驱动器和相同数量的每种类型。 它支持混合和匹配驱动器型号和驱动器大小根据需要使用上面的注意事项。

| 约束                               |               |
|------------------------------------------|---------------|
| 相同类型的驱动器中的每个服务器     | **必填**  |
| 相同数量的每个服务器中每个类型 | **必填**  |
| 中的每个服务器的相同驱动器模型        | 推荐   |
| 在每个服务器中是相同的驱动器大小         | 推荐   |

## <a name="see-also"></a>请参阅

- [存储空间直通的硬件要求](storage-spaces-direct-hardware-requirements.md)
- [存储空间直通概述](storage-spaces-direct-overview.md)
