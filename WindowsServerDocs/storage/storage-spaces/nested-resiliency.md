---
title: 嵌套的复原能力的存储空间直通
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2018
ms.openlocfilehash: 206d5d19ec55774f9055e7265d4c87d276c60590
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879568"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>嵌套的复原能力的存储空间直通

> 适用于：Windows Server 2019

嵌套的复原能力是一项新功能的[存储空间直通](storage-spaces-direct-overview.md)中，使两个服务器群集能够承受多个硬件故障时不会丢失存储可用性，因此用户、 应用、 Windows Server 2019和虚拟机继续运行而不发生中断。 本主题介绍了其工作原理，提供了分步说明，若要开始和答案的最常见问题。

## <a name="prerequisites"></a>先决条件

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![绿色的复选标记图标。](media/nested-resiliency/supported.png) 如果，考虑嵌套的复原能力：

- 群集运行 Windows Server 2019;和
- 您的群集具有 2 个服务器节点

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![红色 X 图标。](media/nested-resiliency/unsupported.png) 如果不能使用嵌套的复原能力：

- 群集运行 Windows Server 2016;或
- 您的群集具有 3 个或多个服务器节点

## <a name="why-nested-resiliency"></a>为什么嵌套的复原能力

使用嵌套的复原能力的卷可以**即使多个硬件故障发生在同一时间将保持联机和可访问**，与不同的是经典[双向镜像](storage-spaces-fault-tolerance.md)复原能力。 例如，如果两个驱动器失败一次或一台服务器出现故障时，在驱动器出现故障，使用嵌套的复原能力的卷将保持联机和可访问。 对于超聚合基础结构，这将增加应用和虚拟机; 的运行的时间对于文件服务器工作负荷，这意味着用户享有不间断地的访问他们的文件。

![存储可用性](media/nested-resiliency/storage-availability.png)

弊端是具有该嵌套的复原能力**降低比经典的双向镜像的容量效率**，这意味着获取较少的可用空间。 有关详细信息，请参阅[容量效率](#capacity-efficiency)下面一节。

## <a name="how-it-works"></a>工作原理

### <a name="inspiration-raid-51"></a>灵感：RAID 5 + 1

RAID 5 + 1 是了解嵌套复原能力提供有用的背景的分布式的存储复原能力的已建立窗体。 在 RAID 5 + 1，每个服务器本地的复原能力中提供的 RAID 5 或*单一奇偶校验*，以防止任何单个驱动器丢失。 然后，RAID 1，进一步提供复原能力或*双向镜像*，两个服务器，以防止丢失的任意一台服务器之间。

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>两个新的恢复能力选项

存储空间直通在 Windows Server 2019 提供软件，而无需专用的 RAID 硬件中实现的两个新的恢复能力选项：

- **嵌套的双向镜像。** 每个服务器中本地复原能力提供双向镜像，然后由两个服务器之间的双向镜像提供更多的复原能力。 它是实质上是四向镜像，与每个服务器中的两个副本。 嵌套的双向镜像提供高度可靠的性能： 写入转到所有副本，并读取来自任何副本。

  ![嵌套的双向镜像](media/nested-resiliency/nested-two-way-mirror.png)

- **嵌套的镜像加速奇偶校验。** 合并嵌套双向镜像，从更高版本，具有嵌套的奇偶校验。 每个服务器中的最多的数据的本地恢复能力提供的单一[算术的按位奇偶校验](storage-spaces-fault-tolerance.md#parity)，除非新而使用双向镜像的最新写入。 然后，进一步的所有数据的复原能力被提供的服务器之间的双向镜像。 了解如何加速镜像的奇偶校验的工作原理的详细信息，请参阅[镜像加速奇偶校验](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity)。

  ![嵌套的镜像加速奇偶校验](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>容量效率

容量效率是到可用空间比率[卷占用空间](plan-volumes.md#choosing-the-size-of-volumes)。 它描述容量开销因引起的复原能力，并取决于所选择的恢复能力选项。 作为一个简单的示例，同时双向镜像复原能力是 100%容量高效 (1TB 数据占用 1 TB 的物理存储容量) 不存储数据是 50%有效 (1 TB 的数据占用 2 TB 的物理存储容量)。

- **嵌套的双向镜像**写入四个副本的所有内容，这意味着存储 1 TB 的数据，您需要 4 TB 的物理存储容量。 虽然它的简单是有吸引力，但嵌套双向镜像的 25%的容量效率是最低的存储空间直通中的任何复原选项。

- **嵌套镜像加速奇偶校验**来实现更高的容量效率，大约 35%到 40%，依赖于两个因素： 在每个服务器和镜像和奇偶校验卷指定的组合驱动器容量的数量。 此表提供了查找常见的配置：

  | 每个服务器的容量驱动器 | 10%镜像 | 20%镜像 | 30%镜像 |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7+                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **如果您感兴趣，下面是完整的数学计算的示例。** 假设我们有六个容量驱动器中的每个两个服务器，并且我们想要创建一个包含 10 GB 的镜像和奇偶校验的 90 GB 的 100 GB 卷。 服务器本地双向镜像是有效的高达 50.0%，这意味着 10 GB 的镜像数据占用 20 GB 存储每个服务器上。 镜像到两个服务器，其总占用量为 40 GB。 服务器本地单奇偶校验，这种情况下，为 5/6 = 83.3%有效，这意味着 90 GB 的奇偶校验数据采用 108 GB 存储每个服务器上。 镜像到两个服务器，其总占用量为 216 GB。 因此，总占用量是 [(10 GB / 50.0%) + (90 GB / 83.3%)] × 2 = 256 GB，则表示 39.1%整体容量效率。

请注意，经典双向镜像 （约 50%) 的容量效率嵌套镜像加速奇偶校验和 （最多 40%)差别不大。 根据您的需求，略低的容量效率可能值得大大增加了存储可用性。 您选择复原每个卷，因此可以混合使用嵌套的复原卷和同一群集中的经典的双向镜像卷。

![权衡](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

可以在 PowerShell 中使用熟悉的存储空间 cmdlet 来创建嵌套的复原能力的卷。

### <a name="step-1-create-storage-tier-templates"></a>第 1 步：创建存储层模板

首先，创建新存储层模板中使用`New-StorageTier`cmdlet。 只需一次，执行此操作，然后创建每个新卷可以引用这些模板。 指定`-MediaType`容量驱动器的和 （可选）`-FriendlyName`所选。 不要修改其他参数。

如果你的容量驱动器硬盘驱动器 (HDD)，启动 PowerShell，以管理员身份，并运行：

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

如果容量驱动器是固态硬盘 (SSD)，设置`-MediaType`到`SSD`相反。 不要修改其他参数。

> [!TIP]
> 验证是否已成功创建的层`Get-StorageTier`。

### <a name="step-2-create-volumes"></a>步骤 2：创建卷

然后，创建新卷使用`New-Volume`cmdlet。

#### <a name="nested-two-way-mirror"></a>嵌套的双向镜像

若要使用嵌套的双向镜像，引用`NestedMirror`层模板和指定的大小。 例如：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>嵌套的镜像加速奇偶校验

若要使用嵌套的镜像加速奇偶校验，可以同时引用`NestedMirror`和`NestedParity`层模板并指定两个大小，一个用于每个部分卷 （镜像首先，奇偶校验第二个）。 例如，若要创建一个是 20%嵌套双向镜像和 80%的 500 GB 卷嵌套奇偶校验，运行：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>步骤 3:继续在 Windows Admin Center

使用嵌套的复原能力的卷将出现在[Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)与清除标记，如下面的屏幕截图中所示。 一旦创建了这些，您可以管理和监视它们就像任何其他卷中存储空间直通使用 Windows Admin Center。

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>可选：将扩展到缓存驱动器

使用其默认设置，嵌套的复原能力可防止丢失的同时，在多个容量驱动器或一台服务器并同时在一个容量驱动器。 若要将此保护扩展到[缓存驱动器](understand-the-cache.md)具有一个额外的考虑因素： 因为缓存驱动器通常提供读取*和写入*缓存*多个*容量驱动器确保另一台服务器出现故障时，可以容许的缓存驱动器丢失的唯一方法是只需缓存写入数据，但这会影响性能。

若要解决这种情况下，存储空间直通还可以自动禁用写入缓存时的两个服务器群集中的一台服务器已关闭，然后再重新启用写入缓存服务器备份后。 若要允许例程的重新启动，且没有性能影响，编写之前 30 分钟，服务器已关闭，不禁用缓存。

若要设置此行为 （可选），请启动 PowerShell，以管理员身份，并运行：

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

一旦设置为`True`，缓存行为是：

| 情形                       | 缓存行为                           | 可以容忍缓存驱动器丢失？ |
|---------------------------------|------------------------------------------|--------------------------------|
| 安装这两个服务器                 | 缓存读取和写入，完整的性能 | 是                            |
| 服务器关闭前的 30 分钟   | 缓存读取和写入，完整的性能 | 否 （暂时）               |
| 第一个 30 分钟后          | 缓存读取仅，性能会受到影响   | 是                            |

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>可以将转换现有卷之间双向镜像和嵌套的复原能力？

否，不能复原类型之间转换的卷。 对于 Windows Server 2019 的新部署，决定哪些复原类型最适合您的需要提前。 如果您要升级 Windows Server 2016 中，可以创建嵌套的复原能力的新卷、 将数据迁移，然后删除较旧的卷。

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>可以使用多个容量驱动器的类型使用嵌套的复原能力？

是的只需指定`-MediaType`每个层相应地期间[步骤 1](#step-1-create-storage-tier-templates)上面。 例如，使用 NVMe、 SSD 和 HDD 的相同群集中，NVMe 提供缓存而后两个提供容量： 设置`NestedMirror`层设置为`-MediaType SSD`并`NestedParity`层设置为`-MediaType HDD`。 在这种情况下，请注意，奇偶校验的容量效率取决于仅，HDD 驱动器的数量，你需要至少 4 个其每个服务器。

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>可以使用 3 个或多个服务器使用嵌套的复原能力？

如果群集具有 2 个服务器不可以，只能使用嵌套的复原能力。

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>使用嵌套的复原能力时，需要多少个驱动器？

最小所需的存储空间直通的驱动器数是每个服务器节点的 4 个容量驱动器以及每个服务器节点 （如果有） 的 2 个缓存驱动器。 这是 Windows Server 2016 相比并无变化。 无其他嵌套的复原能力，并保留容量的建议太不变。

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>嵌套的复原能力更改驱动器更换的工作原理？

否。

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>嵌套的复原能力更改节点替换服务器的工作原理？

否。 若要替换的服务器节点和其驱动器，请按照以下顺序操作：

1. 停用传出服务器中的驱动器
2. 将新的服务器，具有其驱动器添加到群集
3. 将重新平衡存储池
4. 删除传出服务器和其驱动器

有关详细信息，请参阅[删除服务器](remove-servers.md)主题。

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [了解存储空间直通中的容错能力](storage-spaces-fault-tolerance.md)
- [计划中存储空间直通的卷](plan-volumes.md)
- [在存储空间直通创建的卷](create-volumes.md)
