---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: 规划存储空间直通中的卷
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 52c600068d5dd447ff9faa7c40788664e222a83a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366889"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>规划存储空间直通中的卷

> 适用于：Windows Server 2019、Windows Server 2016

本主题提供关于如何规划存储空间直通中的卷以满足工作负载的性能和容量需求（包括选择其文件系统、复原类型和大小）的指南。

## <a name="review-what-are-volumes"></a>审校什么是卷

卷是放置工作负荷所需的文件的位置，例如 Hyper-v 虚拟机的 VHD 或 VHDX 文件。 卷将合并存储池中的驱动器，以引入存储空间直通的容错、可扩展性和性能优势。

   >[!NOTE]
   > 在存储空间直通的整个文档中，我们使用术语“卷”一起表示卷及其下面的虚拟磁盘，包括诸如群集共享卷 (CSV) 和 ReFS 等其他内置 Windows 功能所提供的功能。 成功规划和部署存储空间直通不必了解这些实现级别的区别。

![什么是卷](media/plan-volumes/what-are-volumes.png)

群集中的所有服务器可以同时访问所有卷。 创建后，它们将显示在所有服务器上的**C:\ClusterStorage @ no__t-1**中。

![csv 文件夹屏幕截图](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>选择要创建的卷数

我们建议将卷数设置为群集中的服务器数的倍数。 例如，如果有4台服务器，则与3或5总数相比，使用4个卷时，将获得更一致的性能。 这样，群集可以在服务器之间均匀分配卷“所有权”（一个服务器可以为每个卷处理元数据业务流程）。

建议将卷的总数限制为：

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| 每个群集最多32个卷 | 每个群集最多64个卷 |

## <a name="choosing-the-filesystem"></a>选择文件系统

我们建议为存储空间直通使用新的[复原文件系统 (ReFS)](../refs/refs-overview.md)。 ReFS 是专门针对虚拟化构建的首要文件系统，并且可提供许多优势，包括显著加快性能以及内置数据损坏防护。 它支持几乎所有重要的 NTFS 功能，包括 Windows Server 版本1709及更高版本中的重复数据删除。 有关详细信息，请参阅 ReFS[功能比较表](../refs/refs-overview.md#feature-comparison)。

如果你的工作负载需要 ReFS 尚不支持的功能，则可以改用 NTFS。

   > [!TIP]
   > 具有不同文件系统的卷可以在同一群集中共存。

## <a name="choosing-the-resiliency-type"></a>选择复原类型

存储空间直通中的卷可以提供复原，以防止出现诸如驱动器或服务器故障等硬件问题，并在诸如软件更新等整个服务器维护过程中实现连续可用性。

   > [!NOTE]
   > 你可以选择的复原类型与你已安装的驱动器类型无关。

### <a name="with-two-servers"></a>已安装两个服务器

对于群集中的两个服务器，可以使用双向镜像。 如果运行的是 Windows Server 2019，还可以使用嵌套复原。

双向镜像保存所有数据的两个副本，每个服务器上的驱动器上有一个副本。 其存储效率为 50%-若要写入 1 TB 的数据，则存储池中至少需要 2 TB 的物理存储容量。 双向镜像一次可安全容忍一个硬件故障（一个服务器或驱动器）。

![双向镜像](media/plan-volumes/two-way-mirror.png)

嵌套复原（仅在 Windows Server 2019 上可用）提供具有双向镜像的服务器之间的数据复原能力，然后在具有双向镜像的服务器和镜像加速的奇偶校验中增加复原能力。 即使在一台服务器重新启动或不可用时，嵌套也能提供数据复原能力。 其存储效率为 25%，具有嵌套的双向镜像，大约 35-40%，适用于嵌套的镜像加速奇偶校验。 嵌套复原可以安全地允许两个硬件故障（两个驱动器或服务器以及剩余服务器上的驱动器）。 由于此功能增加了数据复原能力，因此，如果您运行的是 Windows Server 2019，则建议在两服务器群集的生产部署上使用嵌套复原功能。 有关详细信息，请参阅[嵌套复原](nested-resiliency.md)。

![嵌套镜像加速奇偶校验](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="with-three-servers"></a>已安装三个服务器

如果安装了三个服务器，你应使用三向镜像，以便获得更好的容错和更高的性能。 三向镜像将保留所有数据的三个副本，每个服务器的驱动器上都会保留一个副本。 其存储效率为 33.3%，即若要写入 1 TB 的数据，存储池中需要至少 3 TB 的物理存储容量。 三向镜像可以安全写入 [一次至少出现两个硬件（驱动器或服务器）问题](storage-spaces-fault-tolerance.md#examples)。 如果2个节点不可用，则存储池将失去仲裁，因为2/3 磁盘不可用，虚拟磁盘将被不可。 但是，节点可能会关闭，另一个节点上的一个或多个磁盘可能会失败，并且虚拟磁盘仍将保持联机状态。 例如，如果你在另一个驱动器或服务器突然发生故障时重新启动一个服务器，则所有数据都将保持安全且可连续访问。

![三向镜像](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>已安装四个或更多个服务器

使用四台或更多服务器时，可以为每个卷选择是否使用三向镜像、双重奇偶校验（通常称为 "擦除编码"），或者将两者混合为镜像加速的奇偶校验。

双奇偶校验提供了与三向镜像相同的容错，但存储效率更好。 有四台服务器，其存储效率为 50.0%-若要存储 2 TB 的数据，则存储池中需要 4 TB 的物理存储容量。 如果安装了七个服务器，存储效率会上升到 66.7%，并且最高可继续上升到 80.0%。 弊端是奇偶校验编码需要进行更多的计算，这可能会限制其性能。

![双奇偶校验](media/plan-volumes/dual-parity.png)

要使用的复原类型取决于你的工作负载需求。 下面是一个表，其中汇总了适合每个复原类型的工作负荷，以及每个复原类型的性能和存储效率。

| 复原类型 | 容量效率 | 速度 | 工作负载 |
| ------------------- | ----------------------  | --------- | ------------- |
| **制作**         | ![存储效率显示 33%](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>三向镜像：33% <br>双向镜像：50%     |![性能显示 100%](media/plan-volumes/three-way-mirror-perf.png)<br> 最高性能  | 虚拟化工作负荷<br> 数据库<br>其他高性能工作负荷 |
| **镜像加速奇偶校验** |![存储效率大约 50%](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> 取决于镜像和奇偶校验的比例 | ![性能显示大约 20%](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>比镜像慢得多，但最多可达两倍于双重奇偶校验<br> 最适用于大型顺序写入和读取 | 存档和备份<br> 虚拟桌面基础结构     |
| **双重奇偶校验**               | ![存储效率大约 80%](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4台服务器：50% <br>16台服务器：最高 80% | ![性能显示大约 10%](media/plan-volumes/dual-parity-perf.png)<br>写入时 CPU 使用率 & 最高 i/o 延迟<br> 最适用于大型顺序写入和读取 | 存档和备份<br> 虚拟桌面基础结构  |

#### <a name="when-performance-matters-most"></a>当性能最重要时

具有严格的延迟要求或需要大量混合的随机 IOPS 的工作负载（例如 SQL Server 数据库或性能敏感型 Hyper-V 虚拟机）应在使用镜像最大限度提高性能的卷上运行。

   >[!TIP]
   > 镜像的速度比任何其他复原类型都快。 我们将镜像用于几乎所有性能示例。

#### <a name="when-capacity-matters-most"></a>当容量最重要时

不频繁写入的工作负载（如数据仓库或“冷”存储）应在使用双奇偶校验最大限度提高存储效率的卷上运行。 某些其他工作负载（如传统文件服务器、虚拟桌面基础结构 (VDI) 或不会产生大量快速漂移的随机 IO 流量和/或不需要最佳性能的其他工作负载）也可以由你随意决定是否使用双奇偶校验。 与镜像相比，奇偶校验不可避免地会增加 CPU 使用率和 IO 延迟，特别是在写入时。

#### <a name="when-data-is-written-in-bulk"></a>当批量写入数据时

在大规模后续传递（如存档或备份目标）中进行写入的工作负载具有 Windows Server 2016 中新增的另一个选项：一个卷可以混合镜像和双奇偶校验。 写入首先在镜像部分中进行，稍后逐渐移到奇偶校验部分。 这样，可以在较长时间内发生计算密集型奇偶校验编码，从而在大型写入到达时加快引入速度并减小资源使用率。 在调整部分大小时，请考虑同时发生的写入数目（例如，每天一次备份）应该充分适合于镜像部分。 例如，如果每天引入 100 GB 一次，则考虑使用 150 GB 到 200 GB 之间的镜像，对于其他情况，则考虑使用双奇偶校验。

所产生的存储效率取决于你选择的比例。 有关一些示例，请参阅[此演示](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)。

   > [!TIP]
   > 如果通过数据引入来观察写入性能时发生的突然降低，则可能表示镜像部分不够大，或者镜像加速奇偶校验不太适合用例。 例如，如果写入性能从 400 MB/秒降低到 40 MB/秒，则考虑展开镜像部分或切换到三向镜像。

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>关于具有 NVMe、SSD 和 HDD 的部署

在涉及两种驱动器类型的部署中，较快的驱动器提供缓存，而较慢的驱动器提供容量。 该过程将自动进行 – 有关详细信息，请参阅[了解存储空间直通中的缓存](understand-the-cache.md)。 在此类部署中，所有卷最终都位于相同类型的驱动器（容量驱动器）上。

在涉及所有三种驱动器类型的部署中，只有最快的驱动器 (NVMe) 会提供缓存，剩下两种类型的驱动器（SSD 和 HDD）会提供容量。 对于每个卷，你可以选择它是完全驻留在 SSD 层上、完全驻留在 HDD 层上，还是跨越这两种层。

   > [!IMPORTANT]
   > 我们建议使用 SSD 层将对性能最敏感的工作负载放在全闪存中。

## <a name="choosing-the-size-of-volumes"></a>选择卷的大小

建议将每个卷的大小限制为：

| Windows Server 2016 | Windows Server 2019 |
| ------------------- | ------------------- |
| 最大 32 TB         | 最大 64 TB         |

   > [!TIP]
   > 如果使用的备份解决方案依赖于卷影复制服务（VSS）和 Volsnap 软件提供程序（与文件服务器工作负荷相同），则将卷大小限制为 10 TB 将提高性能和可靠性。 使用较新 Hyper-V RCT API 和/或 ReFS 块克隆和/或本机 SQL 备份 API 的备份解决方案的性能可完全达到 32 TB 及更高。

### <a name="footprint"></a>占用空间

卷的大小指的是其可用容量，即它可以存储的数据量。 此大小由 **New-Volume**cmdlet 的 **-Size** 参数提供，然后在运行 **Get-Volume** cmdlet 时显示在 **Size** 属性中。

大小完全不同于卷的*占用空间*，其指的是卷在存储池中占用的总物理存储容量。 占用空间取决于其复原类型。 例如，使用三向镜像的卷具有的占用空间是其大小的三倍。

卷的占用空间需要能够容纳到存储池中。

![大小与占用空间](media/plan-volumes/size-versus-footprint.png)

### <a name="reserve-capacity"></a>保留容量

如果使存储池中的一些容量处于未分配状态，则会给卷提供空间，以便在驱动器发生故障后“就地”进行修复，从而提高数据安全性和性能。 如果有足够的容量，那么甚至在更换故障驱动器之前，即时、就地、并行修复也可以将卷还原到完全恢复状态。 此过程自动发生。

我们建议为每个服务器保留相当于一个容量驱动器的容量，最多可保留 4 个驱动器的容量。 你可以自行决定保留更多容量，但此最低容量建议可以保证在任何驱动器发生故障后均能够成功进行即时、就地、并行修复。

![保留](media/plan-volumes/reserve.png)

例如，如果你安装了 2 个服务器，并且你使用的是 1 TB 的容量驱动器，请留出 2 x 1 = 2 TB 的池作为保留容量。 如果你安装了 3 个服务器和 1TB 的容量驱动器，请留出 3 x 1 = 3 TB 作为保留容量。 如果你安装了 4 个或更多个服务器以及 1TB 的容量驱动器，请留出 4 x 1 = 4 TB 作为保留容量。

   >[!NOTE]
   > 在具有所有三种类型 (NVMe + SSD + HDD) 的驱动器的群集中，我们建议为每个服务器保留相当于一个 SSD 与一个 HDD 总和的容量，每种类型最多可保留 4 个驱动器的容量。

## <a name="example-capacity-planning"></a>例如：容量规划

假设有一个由四个服务器组成的群集。 每个服务器都安装了一些缓存驱动器以及十六个 2 TB 的容量驱动器。

```
4 servers x 16 drives each x 2 TB each = 128 TB
```

从存储池内的此 128 TB 中，我们留出四个驱动器或 8 TB，以便可以进行就地修复，而不需要在驱动器发生故障后匆忙去更换驱动器。 这样，池中还剩下 120 TB 的物理存储容量，我们可以使用这些容量来创建卷。

```
128 TB – (4 x 2 TB) = 120 TB
```

假设我们需要进行部署以托管一些高度活跃的 Hyper-V 虚拟机，但是我们还有许多冷存储 － 即我们需要保留的旧文件和备份。 由于我们安装了四个服务器，所以我们将创建四个卷。

我们在前两个卷（*Volume1* 和 *Volume2*）上放置虚拟机。 我们选择 ReFS 作为文件系统（用于加快创建速度和检查点），选择三向镜像以让复原达到最大性能。 我们将冷存储放在其他两个卷（*Volume 3* 和 *Volume 4*）上。 我们选择 NTFS 作为文件系统（用于重复数据删除），选择双奇偶校验以让复原达到最大容量。

我们不必将所有卷都设为相同大小，但是为了简单起见，我们就这样设置 - 例如，我们可以将它们都设置为 12 TB。

*Volume1* 和 *Volume2* 每个都将占用 12 TB x 33.3% 效率 = 36 TB 的物理存储容量。

*Volume3* 和 *Volume4* 每个都将占用 12 TB x 50.0% 效率 = 24 TB 的物理存储容量。

```
36 TB + 36 TB + 24 TB + 24 TB = 120 TB
```

我们池中的可用物理存储容量完全能够装下这四个卷。 太完美了！

![示例](media/plan-volumes/example.png)

   >[!TIP]
   > 你无需立即创建所有卷。 你始终可以在稍后扩展卷或创建新卷。

为简单起见，此示例从头到尾都使用十进制（基数为 10）单位，这意味着 1 TB = 1,000,000,000,000 字节。 但是，Windows 中的存储数量按二进制（基数为 2）单位显示。 例如，在 Windows 中，每个 2 TB 的驱动器都将显示为 1.82 TiB。 同样，128 TB 的存储池将显示为 116.41 TiB。 这是预期情况。

## <a name="usage"></a>用法

请参阅[在存储空间直通中创建卷](create-volumes.md)。

### <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [为存储空间直通选择驱动器](choosing-drives.md)
- [容错和存储效率](storage-spaces-fault-tolerance.md)
