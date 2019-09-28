---
title: 存储空间直通的嵌套复原
ms.prod: windows-server
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/15/2019
ms.openlocfilehash: ea1c4b2c249759634e00f6a1ac2caa34f8085ae1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402870"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>存储空间直通的嵌套复原

> 适用于：Windows Server 2019

嵌套复原是 Windows Server 2019 中[存储空间直通](storage-spaces-direct-overview.md)的一项新功能，它使两个服务器的群集能够同时承受多个硬件故障，而不会丢失存储可用性，因此用户、应用程序和虚拟机继续运行而不中断。 本主题介绍了它的工作原理，提供了入门的分步说明，并回答了常见问题。

## <a name="prerequisites"></a>先决条件

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![绿色复选标记图标。](media/nested-resiliency/supported.png) 如果有以下情况，请考虑嵌套复原：

- 群集运行 Windows Server 2019;与
- 群集正好有2个服务器节点

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![红色 X 图标。](media/nested-resiliency/unsupported.png) 以下情况不能使用嵌套复原：

- 群集运行 Windows Server 2016;或
- 群集有3个或更多服务器节点

## <a name="why-nested-resiliency"></a>嵌套复原的原因

使用嵌套复原的卷可以**始终处于联机状态并且可访问，即使在同一时间发生多个硬件故障**，与经典[双向镜像](storage-spaces-fault-tolerance.md)复原不同。 例如，如果两个驱动器同时出现故障，或者服务器发生故障，而驱动器发生故障，则使用嵌套复原的卷保持联机和可访问。 对于超聚合基础结构，这会增加应用程序和虚拟机的正常运行时间;对于文件服务器工作负荷，这意味着用户无间断地访问其文件。

![存储可用性](media/nested-resiliency/storage-availability.png)

与**传统的双向镜像相比，与传统的双向镜像相比，嵌套复原的容量效率较低**，这意味着您可以获得略微少的可用空间。 有关详细信息，请参阅下面的[容量效率](#capacity-efficiency)部分。

## <a name="how-it-works"></a>工作原理

### <a name="inspiration-raid-51"></a>启发RAID 5 + 1

RAID 5 + 1 是一种已建立的分布式存储复原形式，它提供了有助于了解嵌套复原能力的帮助背景。 在 RAID 5 + 1 中，在每个服务器内，本地复原是由 RAID-5 提供的，也可以是*单个奇偶校验*，以防止丢失任何单个驱动器。 然后，在两个服务器之间通过 RAID-1 或*双向镜像*提供进一步的复原能力，以防止出现任一服务器丢失。

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>两个新的复原选项

Windows Server 2019 中的存储空间直通提供了在软件中实现的两个新复原选项，而无需专用的 RAID 硬件：

- **嵌套的双向镜像。** 在每个服务器中，本地复原都由双向镜像提供，然后在两个服务器之间通过双向镜像提供进一步的复原能力。 它实质上是四向镜像，每个服务器中有两个副本。 嵌套的双向镜像提供不折不扣的性能：写入到所有副本，读取来自任何副本。

  ![嵌套双向镜像](media/nested-resiliency/nested-two-way-mirror.png)

- **嵌套的镜像加速的奇偶校验。** 结合了嵌套的双向镜像和嵌套的奇偶校验。 在每个服务器中，大多数数据的本地复原都是通过单一[按位奇偶校验算法](storage-spaces-fault-tolerance.md#parity)提供的，但新的最近写入将使用双向镜像。 然后，在服务器之间通过双向镜像提供所有数据的复原能力。 有关镜像加速奇偶校验工作原理的详细信息，请参阅[镜像加速的奇偶校验](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity)。

  ![嵌套镜像加速奇偶校验](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>容量效率

容量效率是可用空间与[卷占用](plan-volumes.md#choosing-the-size-of-volumes)量的比值。 它描述了复原的容量开销，取决于所选的复原选项。 简单的示例是，存储无复原功能的数据的容量高达 100% （1 TB 的数据占用 1 TB 的物理存储容量），而双向镜像的效率为 50% （1 TB 的数据占用 2 TB 的物理存储容量）。

- **嵌套的双向镜像**写入所有内容的四个副本，也就是说，若要存储 1 tb 的数据，则需要 4 tb 的物理存储容量。 尽管简单易用，但嵌套式双向镜像的容量效率（25%）是存储空间直通中的任何复原选项的最低级别。

- **嵌套镜像加速奇偶校验**实现了更高的容量效率（大约为 35%-40%），这取决于两个因素：每台服务器中的容量驱动器数量，以及为卷指定的镜像和奇偶校验的组合。 此表提供对常见配置的查找：

  | 每台服务器的容量驱动器 | 10% 镜像 | 20% 镜像 | 30% 镜像 |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7 +                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **如果您感到好奇，以下是完整的数学计算的示例。** 假设两个服务器中的每个服务器都有六个容量驱动器，我们想要创建 1 100 GB 卷，其中包含 10 GB 镜像和 90 GB 的奇偶校验。 服务器本地双向镜像的效率为 50.0%，这意味着，每个服务器上存储的数据量为 10 GB （共 10 GB）。 镜像到这两个服务器，其总占用量为 40 GB。 在这种情况下，服务器本地单一奇偶校验为 5/6 = 83.3%，这意味着 90 GB 的奇偶校验数据将在每台服务器上存储 108 GB。 镜像到这两个服务器，其总占用量为 216 GB。 总体需求量为 [（10 GB/50.0%） + （90 GB/83.3%] × 2 = 256 GB，39.1 用于提高总体容量效率。

请注意，经典双向镜像的容量效率（约 50%）和嵌套的镜像加速奇偶校验（最多 40%）不是很大不同。 根据你的需求，更小的容量效率可能会显著提高存储可用性。 你可以选择每卷弹性，以便在同一群集内混合使用嵌套的复原卷和经典双向镜像卷。

![最佳](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

可以在 PowerShell 中使用熟悉的存储 cmdlet 来创建具有嵌套复原能力的卷。

### <a name="step-1-create-storage-tier-templates"></a>第 1 步：创建存储层模板

首先，使用 @no__t cmdlet 创建新的存储层模板。 只需执行此操作一次，然后创建的每个新卷都可以引用这些模板。 指定容量驱动器的 @no__t 0，还可以选择选择 @no__t 1。 请勿修改其他参数。

如果容量驱动器是硬盘驱动器（HDD），请以管理员身份启动 PowerShell 并运行：

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

如果容量驱动器是固态硬盘（SSD），请将 `-MediaType` 改为 `SSD`。 请勿修改其他参数。

> [!TIP]
> 验证通过 `Get-StorageTier` 成功创建的层。

### <a name="step-2-create-volumes"></a>步骤 2：创建卷

然后，使用 @no__t cmdlet 创建新卷。

#### <a name="nested-two-way-mirror"></a>嵌套双向镜像

若要使用嵌套的双向镜像，请引用 @no__t 的层模板并指定大小。 例如：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>嵌套镜像加速奇偶校验

若要使用嵌套的镜像加速奇偶校验，请同时引用 @no__t 0 和 @no__t 层模板，并指定两个大小，每个大小对应于卷的每个部分（第一个镜像，奇偶校验秒）。 例如，若要创建 20% 嵌套双向镜像和 80% 嵌套的奇偶校验的 1 500 GB 卷，请运行：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>步骤 3:在 Windows 管理中心中继续

使用嵌套复原的卷在[Windows 管理中心](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)中显示，并带有清晰的标签，如以下屏幕截图所示。 一旦创建，就可以使用 Windows 管理中心来管理和监视它们，就像存储空间直通中的其他任何卷一样。

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>可选：扩展到缓存驱动器

利用其默认设置，嵌套复原可防止同时丢失多个容量驱动器，或者同时保护一台服务器和一个容量驱动器。 若要将此保护扩展到[缓存驱动器](understand-the-cache.md)，请注意以下事项：因为缓存驱动器通常为*多个*容量驱动器提供读*写*缓存，所以，确保在其他服务器已关闭，只是不缓存写入，但会影响性能。

若要解决这种情况，存储空间直通提供了在两个服务器群集中的一个服务器关闭时自动禁用写入缓存的选项，然后在服务器备份后重新启用写入缓存。 若要在不影响性能的情况下允许日常重启，则在服务器停机30分钟之前，不会禁用写入缓存。 禁用写入缓存后，写入缓存的内容将写入容量设备。 完成此操作后，服务器可以容忍联机服务器中出现故障的缓存设备，但如果缓存设备出现故障，缓存中的读取可能会延迟或失败。

若要设置此行为（可选），请以管理员身份启动 PowerShell 并运行：

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

一旦设置为**True**，缓存行为就是：

| 情形                       | 缓存行为                           | 能否容忍缓存驱动器丢失？ |
|---------------------------------|------------------------------------------|--------------------------------|
| 这两台服务器                 | 缓存读取和写入，完全性能 | 是                            |
| 服务器停机，前30分钟   | 缓存读取和写入，完全性能 | 否（临时）               |
| 前30分钟后          | 仅缓存读取，性能受到影响   | 是（在缓存写入容量驱动器后）                           |

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>能否在双向镜像和嵌套复原之间转换现有卷？

不能，无法在复原类型之间转换卷。 对于 Windows Server 2019 上的新部署，请提前决定哪种复原类型最适合你的需求。 如果要从 Windows Server 2016 升级，可以创建具有嵌套复原能力的新卷，迁移数据，然后删除较旧的卷。

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>是否可以对多种类型的容量驱动器使用嵌套复原？

是的，只需在上面的[步骤 1](#step-1-create-storage-tier-templates)中指定每个层的 @no__t 0。 例如，在同一个群集中，NVMe、SSD 和 HDD 提供缓存，而后两个提供容量：将 `NestedMirror` 层设置为 `-MediaType SSD`，将 @no__t 2 层设置为 `-MediaType HDD`。 在这种情况下，请注意，奇偶校验容量效率取决于 HDD 驱动器的数量，每台服务器至少需要4个驱动器。

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>是否可以对3个或更多的服务器使用嵌套复原？

否，仅在群集正好有2台服务器的情况下，才使用嵌套复原。

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>需要多少驱动器才能使用嵌套复原？

存储空间直通所需的最小驱动器数为每个服务器节点4个容量驱动器，以及每个服务器节点2个缓存驱动器（如果有）。 这在 Windows Server 2016 中是不变的。 嵌套复原无需额外的要求，并且预留容量的建议也不会发生变化。

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>嵌套复原是否会改变驱动器更换的工作方式？

否。

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>嵌套复原是否会改变服务器节点替换的工作方式？

否。 若要替换服务器节点及其驱动器，请遵循以下顺序：

1. 停用传出服务器中的驱动器
2. 将新服务器及其驱动器添加到群集
3. 存储池将重新平衡
4. 删除传出服务器及其驱动器

有关详细信息，请参阅[删除服务器](remove-servers.md)主题。

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- [了解存储空间直通中的容错](storage-spaces-fault-tolerance.md)
- [计划中的卷存储空间直通](plan-volumes.md)
- [在存储空间直通中创建卷](create-volumes.md)
