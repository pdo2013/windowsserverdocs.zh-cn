---
title: 了解群集和池仲裁
description: 了解群集和池仲裁，使用特定的示例，以介绍如何使用复杂。
keywords: 存储空间直通，仲裁见证，S2D，群集仲裁，池仲裁群集，池
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024050"
---
# 了解群集和池仲裁

>适用于： Windows Server 2019、 Windows Server 2016

[Windows Server 故障转移群集](../../failover-clustering/failover-clustering-overview.md)提供工作负荷的高的可用性。 这些资源被认为是托管资源的节点设置; 如果高可用性但是，群集通常需要超过一半的节点以在运行，这称作具有*仲裁*。

仲裁旨在防止*拆分式*方案出现这种情况时网络中没有分区和节点的子集无法与彼此通信。 这可能会导致两个子集节点尝试拥有工作负荷并写入到同一磁盘这会导致大量的问题。 但是，这将阻止使用故障转移群集的概念，这将强制仅有一个要继续运行，因此这些组中的只有一个均保持联机的节点这些组的仲裁。

仲裁确定群集可以维持同时仍保持联机的失败的次数。 仲裁旨在处理方案的子集群集节点之间的通信问题时，以便多个服务器不要尝试同时托管资源组，并在同一时间写入到同一磁盘。 通过此仲裁的概念，群集将强制群集服务停止在一个节点以确保只有一个真正的特定资源组所有者的子集。 一旦节点均已停止，这会再次与节点的主组，它们将自动重新加入群集并启动其群集服务。

在 Windows Server 2016 中，有两个系统的组件具有其自己的仲裁机制：

- <strong>群集仲裁</strong>： 这在群集级别运行 （即可丢失节点，并使设置保持群集）
- <strong>池仲裁</strong>： 此操作的池级别上时启用存储空间直通 （即可丢失节点和驱动器，并使设置保持池）。 存储池被设计用于在群集和非群集方案中，这是它们具有不同的仲裁机制的原因。

## 群集仲裁概述

下表概述了每个方案的群集仲裁结果：

| 服务器节点 | 能够经受住一个服务器节点故障 | 一台服务器节点故障，则另一台能够经受住 | 可以利用可承受两个同时服务器节点故障 |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 对半                               | 否                                                | 否                                                 |
| 2 + 见证  | 是                                 | 否                                                | 否                                                 |
| 3            | 是                                 | 对半                                             | 否                                                 |
| 3 + 见证  | 是                                 | 是                                               | 否                                                 |
| 4            | 是                                 | 是                                               | 对半                                              |
| 4 + 见证  | 是                                 | 是                                               | 是                                                |
| 5 和更高版本  | 是                                 | 是                                               | 是                                                |

### 群集仲裁建议

- 如果你有两个节点，见证是<strong>必需</strong>。
- 如果你有三个或四个节点，见证是<strong>强烈建议</strong>。
- 如果你具有 Internet 访问权限，请使用<strong>[云见证](../../failover-clustering/deploy-cloud-witness.md)</strong>
- 如果你是具有其他计算机和文件共享的 IT 环境中，使用文件共享见证

## 群集仲裁的工作原理

当节点出现故障时，或者当节点的一些子集与失去联系另一个子集，剩下的节点需要验证它们构成*大多数*保持联机的群集。 如果它们不能验证，将脱机它们。

但的*大多数*概念仅适用于完全时的群集中节点总数为奇数 （例如，五个节点的群集中的三个节点）。 因此，群集的节点 （说，一个四个节点的群集） 为偶数呢？

有两种方法群集可以使奇怪*的投票总数*：

1. 首先，它可以*设置*一个转通过添加额外的投票的*见证*。 这要求用户设置。
2.  或者，它可以奔*向下*一个由零位调整 （根据需要自动发生） 的一个 unlucky 节点的投票。

每当剩余节点成功验证它们*大多数*，将更新的*大多数*定义以成为只需幸存者。 这样，群集可以丢失一个节点中，则另一个，则另一个，等等。 此概念的*总数的投票*适应后连续故障称为<strong>*动态仲裁*</strong>。  

### 动态见证

动态见证切换见证，以确保*总数投票，则*为奇数的投票。 如果有奇数的投票，见证没有投票。 如果没有有用投票，则为偶数，见证则投票。 动态见证大大降低群集将奔溃由于见证失败的风险。 群集决定是否使用基于在群集中可用的投票节点数的见证投票。

动态仲裁适用于动态见证如下所述的方法。

### 动态仲裁行为

- 如果你有<strong>即使</strong>节点并没有见证，*一个节点获取其归零的投票*数。 例如，只有三种四个节点获取投票，因此*的投票总数*为 3，并且与投票，则两个幸存者被认为大多数。
- 如果你有<strong>奇数</strong>节点并没有见证，*它们都获取投票*的数量。
- 如果你有<strong>即使</strong>数量的节点以及见证，*见证投票*，因此总数为奇数。
- 如果你有<strong>奇数</strong>数量的节点以及见证，*不会进行表决见证*。

动态仲裁使能够为动态避免丢失的投票大多数并允许运行 （称为最后一个男士战绩外） 的一个节点的群集节点分配投票。 例如，让我们来看一个四个节点的群集。 假定该仲裁需要 3 个投票。 

在此情况下，群集将已关闭如果你丢失了两个节点。

![显示四个群集节点，其中每个获取投票图](media/understand-quorum/dynamic-quorum-base.png)

但是，动态仲裁防止此情况发生。 现在基于可用节点的数量确定所需的仲裁*的投票的总次数*。 因此，使用动态仲裁，群集将保持设置中，即使在丢失三个节点。

![图表显示四个群集节点，节点无法在时间和需要调整每个失败后的投票数。](media/understand-quorum/dynamic-quorum-step-through.png)

上述方案适用于不具有存储空间直通启用常规群集。 但是，启用存储空间直通后，群集可以仅支持两个节点失败。 这是中详细介绍了[池仲裁部分](#poolQuorum)。

### 示例

#### 而无需见证的两个节点。 
一个节点的投票为零，因此*大多数*投票确定的总<strong>1 投票</strong>。 如果非投票节点意外出现故障，survivor 具有 1/1 和群集责任生效期间存续。 如果投票节点意外出现故障，survivor 具有 0/1 和群集出现故障。 投票节点顺利电源的情况下，投票转移到另一个节点和群集责任生效期间存续。 *<strong>这是很重要配置见证的原因。</strong>*

![带有两个节点，而无需见证的用例中所述的仲裁](media/understand-quorum/2-node-no-witness.png)

- 能够经受住一个服务器故障： <strong>50%机会</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>否</strong>。
- 可以同时利用承受两个服务器故障：<strong>否</strong>。 

#### 与见证的两个节点。 
两个节点投票，以及见证投票，因此*大多数*确定的总<strong>3 投票，则</strong>。 如果任一节点出现故障，survivor 具有 2/3 和群集责任生效期间存续。

![仲裁见证通过介绍在两个节点的用例](media/understand-quorum/2-node-witness.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>否</strong>。
- 可以同时利用承受两个服务器故障：<strong>否</strong>。 

#### 而无需见证的三个节点。
所有节点进行都投票，因此*大多数*确定的总<strong>3 投票，则</strong>。 如果任何节点出现故障，幸存者是 2/3，而且群集责任生效期间存续。 群集成为而无需见证的两个节点，此时，则你处于方案 1。

![带有而无需见证的三个节点的用例中所述的仲裁](media/understand-quorum/3-node-no-witness.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住： <strong>50%机会</strong>。
- 可以同时利用承受两个服务器故障：<strong>否</strong>。 

#### 与见证的三个节点。
所有节点进行都投票，因此不会最初都投票见证。 *大多数*确定的总<strong>3 投票，则</strong>。 一个失败后群集有两个节点的见证 – 这是返回到第二种情况。 因此，现在两个节点和见证投票。

![仲裁见证使用带有三个节点的用例中所述](media/understand-quorum/3-node-witness.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>是</strong>。
- 可以同时利用承受两个服务器故障：<strong>否</strong>。 

#### 四个节点，而无需见证
一个节点的投票为零，因此*大多数*确定的总<strong>3 投票，则</strong>。 一次失败后群集变为三个节点，并为方案 3 中所。

![带有而无需见证的四个节点的用例中所述的仲裁](media/understand-quorum/4-node-no-witness.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>是</strong>。
- 可以同时利用承受两个服务器故障： <strong>50%机会</strong>。 

#### 与见证的四个节点。
所有节点投票和见证投票，因此*大多数*确定的总<strong>5 投票，则</strong>。 一个故障后，你可以在方案 4。 后两个同时发生的故障，则跳过第二种情况下。

![仲裁见证通过介绍在用例中有四个节点](media/understand-quorum/4-node-witness.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>是</strong>。
- 可以同时利用承受两个服务器故障：<strong>是</strong>。 

#### 5 个节点及更高。
所有节点都投票，或一个都投票，任何使奇怪的总。 存储空间直通不能处理超过两个节点下，因此此时，没有见证是所需或非常有用。

![使用 5 个节点及更高的情况中所述的仲裁](media/understand-quorum/5-nodes.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>是</strong>。
- 可以同时利用承受两个服务器故障：<strong>是</strong>。 

现在，我们了解仲裁的工作原理，让我们看一下仲裁证人的类型。

### 仲裁见证类型

故障转移群集支持三种类型的仲裁证人：

- <strong>[云见证](../../failover-clustering\deploy-cloud-witness.md)</strong> -可访问 Azure 中的存储 Blob 的群集的所有节点。 它维护 witness.log 文件中的群集信息，但不会存储副本的群集数据库。
- <strong>文件共享见证</strong>– 配置运行 Windows Server 的文件服务器的 SMB 文件共享。 它维护 witness.log 文件中的群集信息，但不会存储副本的群集数据库。
- <strong>磁盘见证</strong>-一个小的群集的磁盘即群集可用存储组中。 此磁盘为高度可用，并且可以节点之间的故障转移。 它包含群集数据库的副本。  <strong>*如果具有存储空间直通见证磁盘不受支持*</strong>。

## <a id="poolQuorum"></a>池仲裁概述

我们只需讨论了群集仲裁，在群集级别运行。 现在，让我们来深入到池仲裁，池级别 （即你可能会丢失节点和驱动器，并拥有保持池） 运行。 存储池被设计用于在群集和非群集方案中，这是它们具有不同的仲裁机制的原因。

下表概述了每个方案的池仲裁结果：

| 服务器节点 | 能够经受住一个服务器节点故障 | 一台服务器节点故障，则另一台能够经受住 | 可以利用可承受两个同时服务器节点故障 |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 否                                  | 否                                                | 否                                                 |
| 2 + 见证  | 是                                 | 否                                                | 否                                                 |
| 3            | 是                                 | 否                                                | 否                                                 |
| 3 + 见证  | 是                                 | 否                                                | 否                                                 |
| 4            | 是                                 | 否                                                | 否                                                 |
| 4 + 见证  | 是                                 | 是                                               | 是                                                |
| 5 和更高版本  | 是                                 | 是                                               | 是                                                |

## 池仲裁的工作原理

当驱动器发生故障时，或者当驱动器的一些子集与失去联系另一个子集，未发生故障的驱动器需要验证它们构成池保持联机的*大多数*。 如果它们不能验证，将脱机它们。 池是脱机或保持联机具体取决于它是否具有足够的磁盘为仲裁的实体 （50%+ 1）。 池资源所有者 （活动群集节点） 可以是 + 1。

但池仲裁工作方式，从群集仲裁在以下方面：

- 池使用一个节点在群集中作为决胜局作为见证台能够经受一半前面的驱动器 （是池资源所有者，此节点）
- 池不具有动态仲裁
- 池并不实现它自己的删除投票版本

### 示例

#### 使用对称布局的四个节点。 
每个 16 个驱动器有一个投票，两个节点又有一个投票，（因为它是池资源所有者）。 *大多数*确定的总<strong>16 投票，则</strong>。 如果节点 3 和 4 奔溃，未发生故障的子集都有 8 个驱动器和池资源所有者，这是 9/16 投票。 因此，该池仍然有效。

![池仲裁 1](media/understand-quorum/pool-1.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>是</strong>。
- 可以同时利用承受两个服务器故障：<strong>是</strong>。 

#### 使用对称布局和驱动器故障的四个节点。 
每个 16 个驱动器都有一个投票和节点 2 还具有一个投票 （因为它是池资源所有者）。 *大多数*确定的总<strong>16 投票，则</strong>。 首先，驱动器 7 出现故障。 如果节点 3 和 4 奔溃，未发生故障的子集具有 7 驱动器和池资源所有者，这是 8/16 投票。 因此，池没有大多数和出现故障。

![池仲裁 2](media/understand-quorum/pool-2.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>否</strong>。
- 可以同时利用承受两个服务器故障：<strong>否</strong>。 

#### 使用非对称布局的四个节点。 
每个 24 个驱动器有一个投票，两个节点又有一个投票，（因为它是池资源所有者）。 *大多数*确定的总<strong>24 投票，则</strong>。 如果节点 3 和 4 奔溃，未发生故障的子集都有 8 个驱动器和池资源所有者，这是 9 月 24 投票。 因此，池没有大多数和出现故障。

![池仲裁 3](media/understand-quorum/pool-3.png)

- 能够经受住一个服务器故障：<strong>是</strong>。
- 一台服务器故障，则另一台能够经受住：<strong>依赖于</strong>（不能经受如果两个节点 3 和 4 会下降，但可以经受所有其他方案。
- 可以同时利用承受两个服务器故障：<strong>依赖于</strong>（不能经受如果两个节点 3 和 4 会下降，但可以经受所有其他方案。

### 池仲裁建议

- 确保你的群集中每个节点都对称 （每个节点具有相同数量的驱动器）
- 启用三向镜像或双奇偶校验，以便可容忍节点失败，也可以使虚拟磁盘。 请参阅我们的[卷指南页面](plan-volumes.md)的更多详细信息。
- 如果超过两个节点都已关闭或两个节点和另一个节点上的磁盘是向下，系统可能无法访问其数据的所有三个副本卷和因此些时候脱机并且不可用。 建议将再回到服务器或更换磁盘快速以确保大多数复原的卷中的所有数据。

## 详细信息

- [配置和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)
- [部署云见证](../../failover-clustering/deploy-cloud-witness.md)