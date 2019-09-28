---
title: Hyper-v 存储 i/o 性能
description: Hyper-v 性能优化中的存储 i/o 性能注意事项
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7c5a7b667f24ee929a80010dc51508033f991ed5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370059"
---
# <a name="hyper-v-storage-io-performance"></a>Hyper-v 存储 i/o 性能

本部分介绍在虚拟机中优化存储 i/o 性能的不同选项和注意事项。 存储 i/o 路径通过主机虚拟化层扩展到主机存储堆栈，然后再扩展到物理磁盘上。 下面是有关如何在每个阶段进行优化的说明。

## <a name="virtual-controllers"></a>虚拟控制器

Hyper-v 提供了三种类型的虚拟控制器：IDE、SCSI 和虚拟主机总线适配器（Hba）。

## <a name="ide"></a>IDE

IDE 控制器将 IDE 磁盘公开给虚拟机。 IDE 控制器是模拟的，它是在没有虚拟机 Integration Services 的情况下可用于运行旧版 Windows 的来宾 Vm 的唯一控制器。 使用与虚拟机 Integration Services 一起提供的 IDE 筛选器驱动程序执行的磁盘 i/o 明显优于与仿真 IDE 控制器一起提供的磁盘 i/o 性能。 建议仅将 IDE 磁盘用于操作系统磁盘，因为它们具有性能限制，因为可以向这些设备颁发最大的 i/o 大小。

## <a name="scsi-sas-controller"></a>SCSI （SAS 控制器）

SCSI 控制器将 SCSI 磁盘公开给虚拟机，每个虚拟 SCSI 控制器最多可支持64设备。 为了获得最佳性能，建议将多个磁盘附加到单个虚拟 SCSI 控制器并创建更多的控制器，因为这些控制器需要扩展连接到虚拟机的磁盘数。 SCSI 路径不会模拟，这使得它成为操作系统磁盘以外的任何磁盘的首选控制器。 实际上对于第2代 Vm，这是唯一可能的控制器类型。 在 Windows Server 2012 R2 中引入，此控制器被报告为 SAS 以支持共享的 VHDX。

## <a name="virtual-fibre-channel-hbas"></a>虚拟光纤通道 Hba

虚拟光纤通道 Hba 可以配置为允许虚拟机的直接访问，以便通过以太网光纤通道和光纤通道（FCoE） Lun。 虚拟光纤通道磁盘会绕过根分区中的 NTFS 文件系统，从而减少了存储 i/o 的 CPU 使用量。

在多个虚拟机之间共享的大型数据驱动器和驱动器（适用于来宾群集方案）是虚拟光纤通道磁盘的最佳候选项。

虚拟光纤通道磁盘需要在主机上安装一个或多个光纤通道的主机总线适配器（Hba）。 每个主机 HBA 都需要使用支持 Windows Server 2016 Virtual 光纤通道/NPIV 功能的 HBA 驱动程序。 SAN fabric 应支持 NPIV，用于虚拟光纤通道的 HBA 端口应在支持 NPIV 的光纤通道拓扑中进行设置。

为了最大限度地提高随多个 HBA 一起安装的主机的吞吐量，建议你在 Hyper-v 虚拟机中配置多个虚拟 Hba （每个虚拟机最多可配置四个 Hba）。 Hyper-v 将自动尽力平衡虚拟 Hba，以托管访问同一虚拟 SAN 的 Hba。

## <a name="virtual-disks"></a>虚拟磁盘

磁盘可以通过虚拟控制器公开给虚拟机。 这些磁盘可以是虚拟硬盘，是磁盘的文件抽象或主机上的传递磁盘。

## <a name="virtual-hard-disks"></a>虚拟硬盘

有两个虚拟硬盘格式： VHD 和 VHDX。 其中每种格式都支持三种类型的虚拟硬盘文件。

## <a name="vhd-format"></a>VHD 格式

VHD 格式是过去版本中 Hyper-v 支持的唯一虚拟硬盘格式。 Windows Server 2012 中引入了 VHD 格式，以允许更好地对齐，这使得新的大型扇区磁盘的性能明显更好。

在 Windows Server 2012 或更高版本上创建的任何新 VHD 都具有最佳的 4 KB 对齐方式。 此对齐格式与以前的 Windows Server 操作系统完全兼容。 但是，对于来自不能识别大小为 4 KB 的分析器的新分配，对齐属性将断开（例如，以前版本的 Windows Server 或非 Microsoft 分析器中的 VHD 分析器）。

从以前的版本移动的任何 VHD 都不会自动转换为此改进后的 VHD 格式。

若要转换为新的 VHD 格式，请运行以下 Windows PowerShell 命令：

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

您可以检查系统上所有 Vhd 的 "对齐" 属性，并且应将其转换为最佳的 4 KB 对齐方式。 使用原始 VHD 中的数据通过 "**从源创建**" 选项创建新的 vhd。

若要使用 Windows Powershell 检查对齐方式，请检查对齐线，如下所示：

``` syntax
Get-VHD –Path E:\vms\testvhd\test.vhd

Path                    : E:\vms\testvhd\test.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69245440
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 10
Alignment               : 0
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

若要使用 Windows PowerShell 验证对齐方式，请检查对齐线，如下所示：

``` syntax
Get-VHD –Path E:\vms\testvhd\test-converted.vhd

Path                    : E:\vms\testvhd\test-converted.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69369856
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 0
Alignment               : 1
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

## <a name="vhdx-format"></a>VHDX 格式

VHDX 是 Windows Server 2012 中引入的一种新的虚拟硬盘格式，可用于创建高达 64 tb 的可恢复高性能虚拟磁盘。 此格式的优点包括：

-   支持的虚拟硬盘存储容量高达 64 tb。

-   可通过记录对 VHDX 元数据结构的更新，保护数据在电源发生故障时不受损坏。

-   能够存储有关文件的自定义元数据，用户可能想要记录这些元数据，如操作系统版本或应用的修补程序。

VHDX 格式还提供以下性能优势：

-   改进了虚拟硬盘格式的对齐方式，以便能在大型扇区磁盘上更好地工作。

-   动态磁盘和差异磁盘的块大小较大，这允许这些磁盘领域工作负荷的需求。

-   4 KB 逻辑扇区虚拟磁盘，可在用于 4 KB 扇区的应用程序和工作负荷使用时提高性能。

-   表示数据的效率，这会使文件大小变小，并允许基础物理存储设备回收未使用的空间。 （Trim 要求传递或 SCSI 磁盘和与剪裁兼容的硬件。）

升级到 Windows Server 2016 时，建议你将所有 VHD 文件转换为 VHDX 格式，因为这些优势。 只有在虚拟机可能会移到不支持 VHDX 格式的 Hyper-v 以前版本的情况下，才能将文件保留为 VHD 格式是唯一的情况。

## <a name="types-of-virtual-hard-disk-files"></a>虚拟硬盘文件的类型

有三种类型的 VHD 文件。 以下各节介绍了这些类型之间的性能特性和权衡。

在选择 VHD 文件类型时，应考虑以下建议：

-   使用 VHD 格式时，建议使用固定类型，因为与其他 VHD 文件类型相比，它具有更好的复原能力和性能特征。

-   当使用 VHDX 格式时，建议使用动态类型，因为它除了提供仅在需要时分配空间的空间节省，还提供复原保证。

-   如果承载卷上的存储未处于主动监视状态，以确保在运行时扩展 VHD 文件时存在足够的磁盘空间，则也建议使用固定类型，而不考虑格式。

-   虚拟机的快照创建用于存储写入磁盘的差异 VHD。 只有几个快照可以提升存储 i/o 的 CPU 使用率，但可能不会明显影响性能，但在高 i/o 密集型服务器工作负荷中除外。 但是，由于从 VHD 中读取可能需要在多个差异 Vhd 中检查请求的块，因此具有大量快照可能会明显影响性能。 保持快照链的简短对于维护良好的磁盘 i/o 性能非常重要。

## <a name="fixed-virtual-hard-disk-type"></a>固定虚拟硬盘类型

创建 VHD 文件时，将首先分配 VHD 的空间。 这种类型的 VHD 文件不太可能分段，这减少了单个 i/o 被拆分为多个 i/o 时的 i/o 吞吐量。 由于读取和写入不需要查找块的映射，因此它具有三个 VHD 文件类型的最小 CPU 开销。

## <a name="dynamic-virtual-hard-disk-type"></a>动态虚拟硬盘类型

VHD 的空间按需分配。 磁盘中的块作为未分配的块启动，不受文件中的任何实际空间支持。 首次写入块时，虚拟化堆栈必须为块分配 VHD 文件中的空间，然后更新元数据。 这增加了写入所需的磁盘 i/o 数量并增加了 CPU 使用率。 在元数据中查找块映射时，读取和写入现有块会导致磁盘访问和 CPU 开销。

## <a name="differencing-virtual-hard-disk-type"></a>差异虚拟硬盘类型

VHD 指向父 VHD 文件。 不写入的任何块写入会导致 VHD 文件中的空间被分配，就像动态扩展的 VHD 一样。 如果已将块写入，则从 VHD 文件处理读取。 否则，将从父 VHD 文件中为其提供服务。 在这两种情况下，将读取元数据以确定块的映射。 此 VHD 的读取和写入会消耗更多 CPU，并导致比固定 VHD 文件更多的 i/o。

## <a name="block-size-considerations"></a>块大小注意事项

块大小会显著影响性能。 最佳做法是将块大小与使用磁盘的工作负荷的分配模式相匹配。 例如，如果应用程序在 16 MB 的块区中分配，则虚拟硬盘块大小为 16 MB 是最佳的。 只有 VHDX 格式的&gt;虚拟硬盘上才能有 2 MB 的块大小。 对于随机 i/o 工作负荷的分配模式，更大的块大小会显著增加主机上的空间使用率。

## <a name="sector-size-implications"></a>扇区大小影响

大多数软件行业都依赖于超过512个字节的磁盘扇区，但标准正在迁移到 4 KB 的磁盘扇区。 为了降低由于扇区大小的变化而导致的兼容性问题，硬盘驱动器供应商引入了一种转换大小，称为512仿真驱动器（512e）。

这些模拟驱动器提供 4 KB 磁盘扇区本机驱动器提供的一些优点，例如改进的格式效率和用于错误纠正代码（ECC）的改进方案。 它们产生的兼容性问题较少，因为它在磁盘接口上公开 4 KB 扇区大小。

## <a name="support-for-512e-disks"></a>支持512e 磁盘

512e 磁盘只能以物理扇区的形式执行写入，也就是说，它不能直接写入颁发给它的512byte 扇区。 磁盘中可执行这些写入的内部过程可遵循以下步骤：

-   磁盘会将 4 KB 物理扇区读取到其内部缓存中，其中包含写入中提到的512字节逻辑扇区。

-   将 4 KB 缓冲区中的数据修改为包含更新的 512 个字节扇区。

-   磁盘会将已更新的 4 KB 缓冲区写入磁盘上的物理扇区。

此过程称为读修改写入（RMW）。 RMW 进程对总体性能的影响取决于工作负荷。 由于以下原因，RMW 进程会导致虚拟硬盘性能下降：

-   动态和差异虚拟硬盘在其数据负载之前具有512字节扇区的位图。 此外，页脚、页眉和父定位符将与512字节扇区对齐。 虚拟硬盘驱动程序通常会发出512字节写入命令以更新这些结构，从而导致前面所述的 RMW 过程。

-   应用程序通常以 4 KB 大小（NTFS 的默认群集大小）的倍数发出读取和写入。 由于动态和差异虚拟硬盘的数据负载块前面有一个512字节扇区的位图，所以 4 KB 块与物理 4 KB 边界不对齐。 下图显示了与物理 4 KB 边界不对齐的 VHD 4 KB 块（突出显示）。

![vhd 4 kb 块](../../media/perftune-guide-vhd-4kb-block.png)

由当前分析器发出的用于更新有效负载数据的每个 4 KB 写入命令都将对磁盘上的两个块执行两次读取，然后将更新这些块并随后写回到这两个磁盘块。 Windows Server 2016 中的 hyper-v 通过准备前面提到的结构来对齐 VHD 格式的 4 KB 边界，降低了 VHD 堆栈上的512e 磁盘的某些性能影响。 这可以避免在访问虚拟硬盘文件中的数据以及更新虚拟硬盘元数据结构时 RMW 的效果。

如前文所述，从以前版本的 Windows Server 复制的 Vhd 不会自动对齐到 4 KB。 可以使用 VHD 接口中提供的 "从源磁盘**复制**" 选项，手动将其转换为最佳对齐方式。

默认情况下，Vhd 的物理扇区大小为512字节。 这样做是为了确保在应用程序和 Vhd 从以前版本的 Windows Server 移动时不影响物理扇区大小的从属应用程序。

默认情况下，将使用 4 KB 物理扇区大小创建带有 VHDX 格式的磁盘，以优化其性能配置文件常规磁盘和大型扇区磁盘。 若要充分利用 4 KB 扇区，建议使用 VHDX 格式。

## <a name="support-for-native-4kb-disks"></a>支持本机 4 KB 磁盘

Windows Server 2012 R2 和更高版本中的 hyper-v 支持 4 KB 本机磁盘。 但仍可以在 4 KB 本机磁盘上存储 VHD 磁盘。 这是通过在虚拟存储堆栈层实现软件 RMW 算法实现的，该方法将512字节访问和更新请求转换为相应的 4 KB 访问和更新。

由于 VHD 文件只能将自身作为512字节逻辑扇区大小的磁盘公开，因此很有可能会出现发出512字节 i/o 请求的应用程序。 在这些情况下，RMW 层将满足这些请求并导致性能下降。 对于带有逻辑扇区大小为512字节的 VHDX 格式的磁盘也是如此。

可以配置一个 VHDX 文件，使其作为 4 KB 逻辑扇区大小的磁盘公开，这在将磁盘托管到 4 KB 本机物理设备上时，这将是最佳的性能配置。 应小心谨慎，以确保使用虚拟磁盘的来宾和应用程序支持 4 KB 逻辑扇区大小。 VHDX 格式将在 4 KB 逻辑扇区大小设备上正常工作。

## <a name="pass-through-disks"></a>传递磁盘

虚拟机中的 VHD 可以直接映射到物理磁盘或逻辑单元号（LUN），而不是映射到 VHD 文件。 优点是此配置会绕过根分区中的 NTFS 文件系统，从而减少存储 i/o 的 CPU 使用量。 风险是，物理磁盘或 Lun 在不同于 VHD 文件的计算机之间移动可能更困难。

应避免使用传递磁盘，因为虚拟机迁移方案中引入了限制。

## <a name="advanced-storage-features"></a>高级存储功能

### <a name="storage-quality-of-service-qos"></a>服务存储质量 (QoS)

从 Windows Server 2012 R2 开始，Hyper-v 包括为虚拟机上的存储设置某些服务质量（QoS）参数的功能。 存储 QoS 在多租户环境中提供了存储性能隔离，以及在存储 I/O 性能不满足定义的阈值时通知你的机制，以便高效运行你的虚拟机工作负载。

存储 QoS 能够为你的虚拟硬盘指定每秒输入/输出操作次数 (IOPS) 的最大值。 管理员可以限制存储 I/O，以防止某个租户占用过多的存储资源，这可能影响其他租户。

还可以设置最小 IOPS 值。 当指定虚拟硬盘的 IOPS 小于其最佳性能所需的阈值时，他们将收到通知。

还使用与存储相关的参数更新虚拟机指标基础结构，以允许管理员监视性能和与退款相关的参数。

最大值和最小值以规范化 IOPS 为依据，其中每 8 KB 的数据计为一个 i/o。

其中一些限制如下：

-   仅适用于虚拟磁盘

-   差异磁盘在不同的卷上不能有父虚拟磁盘

-   副本-与主站点分开配置的副本站点的 QoS

-   不支持共享 VHDX

有关存储服务质量的详细信息，请参阅[hyper-v 的存储服务质量](https://technet.microsoft.com/library/dn282281.aspx)。

### <a name="numa-io"></a>NUMA I/O

Windows Server 2012 和更高版本支持大型虚拟机，并且任何大型虚拟机配置（例如，具有64虚拟处理器的 Microsoft SQL Server 配置）在 i/o 吞吐量方面也需要可伸缩性。

以下重要改进首次在 Windows Server 2012 存储堆栈和 Hyper-v 中引入，提供了大型虚拟机的 i/o 可伸缩性需求：

-   在来宾设备与主机存储堆栈之间增加的通信通道数的增加。

-   一种更有效的 i/o 完成机制，涉及虚拟处理器间的中断分布，以避免昂贵的 interprocessor 中断。

Windows Server 2012 中引入了几个注册表项，位于 HKLM\\系统\\CurrentControlSet\\Enum\\VMBUS\\{device id}\\{instance id}\\StorChannel，它允许调整通道数。 它们还将处理 i/o 完成的虚拟处理器与应用程序分配给 i/o 处理器的虚拟 Cpu 对齐。 注册表设置在设备硬件密钥上基于每个适配器进行配置。

-   **ChannelCount （DWORD）** 要使用的通道总数，最大值为16。 它的默认值为上限，即虚拟处理器/16 的数目。

-   **ChannelMask （QWORD）** 通道的处理器关联。 如果未设置或设置为0，则默认为用于普通存储或网络通道的现有通道分发算法。 这可确保存储通道不会与网络通道发生冲突。

### <a name="offloaded-data-transfer-integration"></a>卸载数据传输集成

Vhd 的重要维护任务（如合并、移动和压缩）都依赖于复制大量数据。 当前复制数据的方法需要在其他位置进行读写操作，这是一个非常耗时的过程。 它还使用主机上的 CPU 和内存资源，这些资源可能已用于为虚拟机服务。

存储区域网络 (SAN) 供应商正致力于提供几乎能瞬间复制大量数据的功能。 此存储设计用于允许磁盘上的系统将特定数据集从一个位置移动到另一个位置。 此硬件功能称为卸载数据传输。

Windows Server 2012 和更高版本中的 hyper-v 支持卸载数据传输（ODX）操作，以便这些操作可以从来宾操作系统传递到主机硬件。 这可确保工作负荷可以使用启用 ODX 的存储，就像它在非虚拟化环境中运行一样。 Hyper-v 存储堆栈还会在维护操作期间为 Vhd （如合并磁盘和存储迁移元操作，其中移动了大量数据）发出 ODX 操作。

### <a name="unmap-integration"></a>取消映射集成

虚拟硬盘文件作为文件存在于存储卷上，它们与其他文件共享可用空间。 由于这些文件的大小往往很大，它们占用的空间可能会迅速增长。 对更多物理存储的需求会影响 IT 硬件预算。 尽可能优化物理存储的使用非常重要。

在 Windows Server 2012 之前，当应用程序在虚拟硬盘中删除内容，而该虚拟硬盘实际上放弃了内容的存储空间时，来宾操作系统中的 Windows 存储堆栈和 Hyper-v 主机的限制会阻止此情况信息被传递到虚拟硬盘和物理存储设备。 这会阻止 Hyper-v 存储堆栈通过基于 VHD 的虚拟磁盘文件优化空间使用量。 它还会阻止基础存储设备回收已删除数据以前占用的空间。

从 Windows Server 2012 开始，Hyper-v 支持取消映射通知，这使得 VHDX 文件在表示其中的数据时更有效。 这会生成较小的文件，并允许基础物理存储设备回收未使用的空间。

只有特定于 Hyper-v 的 SCSI、启用 IDE 和虚拟光纤通道控制器允许来宾的取消映射命令访问主机虚拟存储堆栈。 在虚拟硬盘上，只有设置为 VHDX 格式的虚拟磁盘才支持来宾的取消映射命令。

出于此原因，我们建议在不使用虚拟光纤通道磁盘时，使用附加到 SCSI 控制器的 VHDX 文件。

## <a name="see-also"></a>请参阅

-   [Hyper-V 术语](terminology.md)

-   [Hyper-V 体系结构](architecture.md)

-   [Hyper-V 服务器配置](configuration.md)

-   [Hyper-V 处理器性能](processor-performance.md)

-   [Hyper-V 内存性能](memory-performance.md)

-   [Hyper-V 网络 I/O 性能](network-io-performance.md)

-   [检测虚拟化环境中的瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
