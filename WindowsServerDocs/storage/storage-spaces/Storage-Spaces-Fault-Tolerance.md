---
title: 存储空间直通中的容错和存储效率
ms.prod: windows-server-threshold
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/11/2017
ms.assetid: 5e1d7ecc-e22e-467f-8142-bad6d82fc5d0
description: 存储空间直通中的复原选项（包括镜像和奇偶校验）的讨论。
ms.localizationpriority: medium
ms.openlocfilehash: 4e6a29e82a85ec9570cda827060dfe1cdf192c53
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1284972"
---
# <a name="fault-tolerance-and-storage-efficiency-in-storage-spaces-direct"></a>存储空间直通中的容错和存储效率

>适用于：Windows Server 2016

本主题介绍了[存储空间直通](storage-spaces-direct-overview.md)中可用的复原选项，并概括了缩放要求、存储效率、一般优势，并对每个选项权衡利弊。 它还介绍了某些使用说明以使用户入门，并且提及某些出色论文、博客和其他可了解详细信息的内容。

如果熟悉存储空间，可能希望跳至[摘要](#summary)部分。

## <a name="overview"></a>概述

在本质上，存储空间涉及为数据提供容错，通常称为“复原”。 实现方式与 RAID 的方式相似，但跨服务器分配并在软件中实现除外。

对于 RAID，存储空间可使用几种不同方法可以这样做，这需要在容错、存储效率和计算复杂性之间权衡利弊。 这些大致可分为两类：“镜像”和“奇偶校验”，后者有时称为“擦除编码”。

## <a name="mirroring"></a>镜像

镜像保留所有数据的多个副本，提供容错。 这最像 RAID-1。 如何划分和放置数据很复杂（请参阅[此博客](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) 了解详细信息），但绝对可以说，任何使用镜像存储的数据都整体编写多次。 每个副本均写入不同的物理硬件（不同服务器中的不同驱动器）中，而这些硬件都认为将独立发生故障。

在 Windows Server 2016 中，存储空间提供两类镜像：“双向”和“三向”。

### <a name="two-way-mirror"></a>双向镜像

双向镜像为所有数据编写两个副本。 存储效率为 50%，即若要写入 1 TB 的数据，需要至少 2 TB 的物理存储容量。 同样地，需要至少两个[硬件“容错域”](../../failover-clustering/fault-domains.md)，对存储空间直通而言，这意味着两台服务器。

![双向镜像](media/Storage-Spaces-Fault-Tolerance/two-way-mirror-180px.png)

   >[!WARNING]
   > 如果你有两个以上的服务器，我们建议改用三向镜像。

### <a name="three-way-mirror"></a>三向镜像

三向镜像为所有数据编写三个副本。 存储效率为 33.3%，即若要写入 1 TB 的数据，需要至少 3 TB 的物理存储容量。 同样地，需要至少三个硬件容错域，对存储空间直通而言，这意味着三台服务器。

三向镜像可以安全容忍[一次至少出现两个硬件（驱动器或服务器）问题](#examples)。 例如，如果你在另一个驱动器或服务器突然发生故障时重新启动一个服务器，则所有数据都将保持安全且可连续访问。

![三向镜像](media/Storage-Spaces-Fault-Tolerance/three-way-mirror-180px.png)

## <a name="parity"></a>奇偶校验

奇偶校验编码通常称为“擦除编码”，使用按位运算提供容错，这会变得[异常复杂](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/LRC12-cheng20webpage.pdf)。 奇偶校验的工作方式相比于镜像而言不太明显，并且有很多在线资源（例如第三方的[擦除编码的傻瓜指南](http://smahesh.com/blog/2012/07/01/dummies-guide-to-erasure-coding/)）可帮助理解。 这足以说明，它在不影响容错的前提下提供更好的存储效率。

在 Windows Server 2016 中，存储空间提供两类奇偶校验：“单”奇偶校验和“双”奇偶校验，后者在更大范围内使用名为“本地重建代码”的高级技术。

> [!IMPORTANT]
> 我们建议对大多数性能敏感的工作负载使用镜像。 若要了解有关根据工作负荷平衡性能和容量的详细信息，请参阅[计划卷](plan-volumes.md#choosing-the-resiliency-type)。

### <a name="single-parity"></a>单奇偶校验
单奇偶校验仅保留一个按位奇偶校验符号，这一次仅对一个失败提供容错。 这最像 RAID-5。 若要使用单奇偶校验，需要至少三个硬件容错域，对存储空间直通而言，这意味着三台服务器。 因为三向镜像在相同范围内提供更多容错，所以我们不鼓励使用单奇偶校验。 但是，如果坚持使用，可随时利用，并且完全受支持。

   >[!WARNING]
   > 我们不鼓励使用单奇偶校验，因为它一次只能安全地写入 一个硬件故障：如果你在另一个驱动器或服务器突然发生故障时重新启动一个服务器，则将遇到停机。 如果仅拥有三台服务器，我们建议使用三向镜像。 如果拥有四台或更多服务器，请参阅接下来的部分。

### <a name="dual-parity"></a>双奇偶校验

双奇偶校验实现 Reed-Solomon 错误更正代码，保留两个按位奇偶校验符号，因此提供与三向镜像提供的相同容错（即最多同时两个失败），但存储效率更高。 这最像 RAID-6。 若要使用双奇偶校验，需要至少四个硬件容错域，对存储空间直通而言，这意味着四台服务器。 在该范围内，存储效率为 50%，即若要写入 2 TB 的数据，需要至少 4 TB 的物理存储容量。

![双奇偶校验](media/Storage-Spaces-Fault-Tolerance/dual-parity-180px.png)

双奇偶校验的存储效率增加了用户所拥有的硬件容错域，即从 50% 增加到最高 80%。 例如，在七（对于存储空间直通而言，这意味着七台服务器）时，效率跃至 66.7%，即若要存储 4 TB 数据，只需 6 TB 的物理存储容量。

![双奇偶校验范围](media/Storage-Spaces-Fault-Tolerance/dual-parity-wide-180px.png)

有关双奇偶校验的效率和每个范围的本地重建代码，请参阅[摘要](#summary)部分。

### <a name="local-reconstruction-codes"></a>本地重建代码

Windows Server 2016 中的存储空间引入了 Microsoft Research 开发的名为“本地重建代码”或 LRC 的高级技术。 在大范围内，双奇偶校验使用 LRC 将编码/解码划分为几个较小的组，以减少从失败写入或恢复所需的开销。

对于硬盘驱动器 (HDD)，组大小为四个符号；对于固态硬盘 (SSD)，组大小为六个符号。 例如，以下是硬盘驱动器和 12 个硬件容错域（意味着 12 台服务器）布局（即有两组的四个数据符号）的外观。 它实现了 72.7% 的存储效率。

![本地重建代码](media/Storage-Spaces-Fault-Tolerance/local-reconstruction-codes-180px.png)

我们推荐这篇由我们的 [Claus Joergensen](https://twitter.com/clausjor) 编写的深入但非常易读的操作实例[本地重建代码如何处理各种失败方案，并且它们为什么具有吸引力](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)。

## <a name="mirror-accelerated-parity"></a>镜像加速奇偶校验

从 Windows Server 2016 开始，一个存储空间直通卷可以一部分是镜像，另一部分是奇偶校验。 写入首先在镜像部分中进行，稍后逐渐移到奇偶校验部分。 实际上，这是[使用镜像加速擦除编码](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)。

若要混合三向镜像和双奇偶校验，需要至少四个容错域，这意味着四台服务器。

镜像加速奇偶校验的存储效率介乎使用所有镜像或所有奇偶校验得出的结果之间，并且取决于选择的比例。 例如，此演示第 37 分钟标记处的演示文稿显示 12 台服务器的[各种混合实现了 46%、54% 和 65% 的效率](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)。

> [!IMPORTANT]
> 我们建议对大多数性能敏感的工作负荷使用镜像。 若要了解有关根据工作负荷平衡性能和容量的详细信息，请参阅[计划卷](plan-volumes.md#choosing-the-resiliency-type)。

## <a name="summary"></a>小结

本部分总结了存储空间直通可用的复原类型、使用每种类型的最低范围要求、每种类型可容忍的失败数以及相应的存储效率。

### <a name="resiliency-types"></a>复原类型

|    复原          |    失败容差       |    存储效率      |
|------------------------|----------------------------|----------------------------|
|    双向镜像      |    1                       |    50.0%                   |
|    三向镜像    |    2                       |    33.3%                   |
|    双奇偶校验         |    2                       |    50.0% - 80.0%           |
|    混合               |    2                       |    33.3% - 80.0%           |

### <a name="minimum-scale-requirements"></a>最小范围要求

|    复原          |    所需的最小故障域数   |
|------------------------|-------------------------------------|
|    双向镜像      |    2                                |
|    三向镜像    |    3                                |
|    双奇偶校验         |    4                                |
|    混合               |    4                                |

   >[!TIP]
   > 除非使用[机箱或机架容错](../../failover-clustering/fault-domains.md)，否则容错域数量指的是服务器数量。 每台服务器中的驱动器数量不会影响可使用的复原类型，前提是达到存储空间直通的最低要求。 

### <a name="dual-parity-efficiency-for-hybrid-deployments"></a>混合部署的双奇偶校验效率

此表显示包含硬盘驱动器 (HDD) 和固态硬盘 (SSD) 的混合部署在每个范围的双奇偶校验和本地重建代码的存储效率。

|    容错域      |    布局           |    效率   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50.0%        |
|    5                  |    RS 2+2           |    50.0%        |
|    6                  |    RS 2+2           |    50.0%        |
|    7                  |    RS 4+2           |    66.7%        |
|    8                  |    RS 4+2           |    66.7%        |
|    9                  |    RS 4+2           |    66.7%        |
|    10                 |    RS 4+2           |    66.7%        |
|    11                 |    RS 4+2           |    66.7%        |
|    12                 |    LRC (8, 2, 1)    |    72.7%        |
|    13                 |    LRC (8, 2, 1)    |    72.7%        |
|    14                 |    LRC (8, 2, 1)    |    72.7%        |
|    15                 |    LRC (8, 2, 1)    |    72.7%        |
|    16                 |    LRC (8, 2, 1)    |    72.7%        |

### <a name="dual-parity-efficiency-for-all-flash-deployments"></a>全闪存部署的双奇偶校验效率

此表显示仅包含固态硬盘 (SSD) 的全闪存部署在每个范围的双奇偶校验和本地重建代码的存储效率。 奇偶校验布局在全闪存配置中可使用较大的大小，并实现更好的存储效率。

|    容错域      |    布局           |    效率   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50.0%        |
|    5                  |    RS 2+2           |    50.0%        |
|    6                  |    RS 2+2           |    50.0%        |
|    7                  |    RS 4+2           |    66.7%        |
|    8                  |    RS 4+2           |    66.7%        |
|    9                  |    RS 6+2           |    75.0%        |
|    10                 |    RS 6+2           |    75.0%        |
|    11                 |    RS 6+2           |    75.0%        |
|    12                 |    RS 6+2           |    75.0%        |
|    13                 |    RS 6+2           |    75.0%        |
|    14                 |    RS 6+2           |    75.0%        |
|    15                 |    RS 6+2           |    75.0%        |
|    16                 |    LRC (12, 2, 1)   |    80.0%        |

## <a name="examples"></a>示例

除非只拥有两台服务器，否则我们建议使用三向镜像和/或双奇偶校验，因为它们提供更好的容错。 特别是它们可确保所有数据的安全并可一直访问，即使在两个容错域（对存储空间直通而言，这意味着两台服务器）受到同时产生的失败影响时也是如此。

### <a name="examples-where-everything-stays-online"></a>所有内容保持联机的示例

这六个示例显示三向镜像和/或双奇偶校验**可以**容忍的失败。

- **1.**    丢失了一个驱动器（包括缓存驱动器）
- **2.**    丢失了一台服务器

![容错示例 1 和 2](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-12.png)

- **3.**    丢失了一台服务器和一个驱动器
- **4.**    不同服务器丢失了两个驱动器

![容错示例 3 和 4](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-34.png)

- **5.**    丢失了超过两个驱动器，前提是最多两台服务器受影响
- **6.**    丢失了两台服务器

![容错示例 5 和 6](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-56.png)

...在每种情况下，所有卷均保持联机。 （请确保群集保留仲裁。）

### <a name="examples-where-everything-goes-offline"></a>所有内容脱机的示例

在生命周期内，存储空间可容忍任何数量的失败，因为如果时间充足，在每个失败后，它都可以还原到完整复原。 但是，在任何指定时刻，失败可安全影响最多两个容错域。 因此，以下是三向和/或双奇偶校验**无法**容忍的失败示例。

- **7.** 三台或更多服务器同时丢失驱动器
- **8.** 同时丢失三台或更多服务器

![容错示例 7 和 8](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-78.png)

## <a name="usage"></a>用途

查看[在存储空间直通中创建卷](create-volumes.md)。

## <a name="see-also"></a>另请参阅

以下所有链接均是本主题正文某个位置的内联。

- [Windows Server 2016 中的存储空间直通](storage-spaces-direct-overview.md)
- [Windows Server 2016 中的容错域感知](../../failover-clustering/fault-domains.md)
- [Microsoft Research 开发的 Azure 擦除编码](https://www.microsoft.com/en-us/research/publication/erasure-coding-in-windows-azure-storage/)
- [本地重建代码和加快奇偶校验卷](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)
- [存储管理 API 中的卷](https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/)
- [Microsoft Ignite 2016 上的存储效率展示](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)
- [存储空间直通的容量计算器预览](http://aka.ms/s2dcalc)
