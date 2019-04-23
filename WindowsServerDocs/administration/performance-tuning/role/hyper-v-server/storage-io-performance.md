---
title: HYPER-V 存储 I/O 性能
description: 中的 HYPER-V 性能优化存储 i/o 性能注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fedc23083914bcf97a8cde12b78c0b174143de25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831308"
---
# <a name="hyper-v-storage-io-performance"></a>HYPER-V 存储 I/O 性能

本部分介绍不同的选项和优化存储的虚拟机中的 I/O 性能注意事项。 主机的存储堆栈，然后写入物理磁盘，存储 I/O 路径从来宾的存储堆栈，主机虚拟化层，通过扩展。 以下是有关如何优化，可能在每个阶段的说明。

## <a name="virtual-controllers"></a>虚拟控制器

HYPER-V 提供三种类型的虚拟控制器：IDE、 SCSI、 和虚拟主机总线适配器 (Hba)。

## <a name="ide"></a>IDE

IDE 控制器公开到虚拟机的 IDE 磁盘。 模拟的 IDE 控制器，并且仅适用于来宾 Vm 运行较旧版本的 Windows 不使用虚拟机集成服务的控制器。 磁盘与虚拟机 Integration Services 一起使用提供的 IDE 筛选器驱动程序执行的 I/O 是明显优于磁盘 I/O 性能与模拟的 IDE 控制器提供。 我们建议，因为它们具有可颁发给这些设备的最大 I/O 大小由于性能限制，仅为操作系统磁盘使用 IDE 磁盘。

## <a name="scsi-sas-controller"></a>SCSI （SAS 控制器）

SCSI 控制器公开到虚拟机，SCSI 磁盘和每个虚拟 SCSI 控制器可支持最多 64 个设备。 为了获得最佳性能，我们建议您将多个磁盘附加到单个虚拟 SCSI 控制器，然后创建其他控制器，因为它们需要连接到虚拟机的磁盘数量进行缩放。 SCSI 路径，将不模拟这使得任何不是操作系统磁盘的磁盘的首选的控制器。 实际上与第 2 代 Vm，它是控制器的唯一的可能类型。 在 Windows Server 2012 R2 中引入，此控制器将报告为 SAS 以支持共享的 VHDX。

## <a name="virtual-fibre-channel-hbas"></a>虚拟光纤通道主机总线适配器

虚拟光纤通道主机总线适配器可以配置为允许对用于虚拟机连接到光纤通道和光纤通道直接访问通过 Ethernet (FCoE) Lun。 虚拟光纤通道磁盘绕过 NTFS 文件系统在根分区中，从而减少了存储 I/O 的 CPU 使用率。

较大的数据驱动器和 （适用于来宾群集方案） 的多个虚拟机之间共享的驱动器的虚拟光纤通道磁盘的首要候选项。

虚拟光纤通道磁盘需要一个或多个光纤通道主机总线适配器 (Hba) 若要在主机上安装。 每个主机 HBA 时需要使用支持的功能，Windows Server 2016 虚拟光纤通道/NPIV 的 HBA 驱动程序。 SAN 结构应支持 NPIV，和用于虚拟光纤通道 HBA 端口应当支持 NPIV 的光纤通道拓扑中设置。

若要最大化与多个 HBA 一起安装的主机上的吞吐量，建议配置的 HYPER-V 虚拟机内部的多个虚拟 Hba （可以为每个虚拟机配置最多四个 Hba）。 HYPER-V 将自动进行最大努力来平衡到访问同一个虚拟 SAN 的主机 Hba 的虚拟 Hba。

## <a name="virtual-disks"></a>虚拟磁盘

可以通过虚拟控制器的虚拟机公开磁盘。 这些磁盘可能是文件抽象的磁盘或传递磁盘在主机上的虚拟硬盘。

## <a name="virtual-hard-disks"></a>虚拟硬盘

有两个虚拟硬盘格式，VHD 和 VHDX。 每个这些格式支持三种类型的虚拟硬盘文件。

## <a name="vhd-format"></a>VHD 格式

VHD 格式是在过去的版本的 HYPER-V 支持的唯一的虚拟硬盘格式。 在 Windows Server 2012 中引入，具有对 VHD 格式进行修改，允许更好地对齐方式，这会导致新的大型扇区磁盘上的显著提高性能。

在 Windows Server 2012 或更高版本创建任何新 VHD 具有最佳 4KB 对齐方式。 此对齐的格式与早期版本的 Windows Server 操作系统完全兼容。 但是的对齐方式属性将会断开从均为 4 KB 不对齐方式识别如 （从早期版本的 Windows Server VHD 分析器） 或非 Microsoft 分析器的分析程序的新分配。

从以前的版本移动任何 VHD 不会不会自动获取转换为这一新改进 VHD 格式。

若要将转换为新的 VHD 格式，请运行以下 Windows PowerShell 命令：

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

您可以检查在系统上的所有 Vhd 的对齐方式属性，并应将它转换为最佳 4KB 对齐方式。 通过使用来自原始 VHD 的数据创建新的 VHD**创建从源**选项。

若要使用 Windows Powershell 检查对齐方式，检查对齐行，如下所示：

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

若要验证使用 Windows PowerShell 的对齐方式，请检查对齐行，如下所示：

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

VHDX 是可用于创建可复原的高性能虚拟磁盘最多 64 千吉字节 Windows Server 2012 中引入的新虚拟硬盘格式。 此格式的优点包括：

-   支持的虚拟硬盘的最多为 64 千吉字节的存储容量。

-   可通过记录对 VHDX 元数据结构的更新，保护数据在电源发生故障时不受损坏。

-   能够存储有关用户可能想记录，如操作系统版本或应用的修补程序文件的自定义元数据。

VHDX 格式还提供了以下性能优势：

-   改进了虚拟硬盘格式的对齐方式，以便能在大型扇区磁盘上更好地工作。

-   动态磁盘和差异磁盘，它允许这些磁盘能够满足工作负荷的需求的更大块大小。

-   4 KB 逻辑扇区虚拟磁盘，可以提高性能时用于应用程序和工作负荷设计的 4 KB 扇区。

-   高效地表示数据，这会导致较小文件大小并允许基础物理存储设备回收未使用的空间。 （剪裁需要传递或 SCSI 磁盘和剪裁兼容的硬件）。

当你升级到 Windows Server 2016 时，我们建议将所有 VHD 文件都转换为 VHDX 格式，由于这些优势。 虚拟机可能会潜在地移动到以前的版本不支持 VHDX 格式的 HYPER-V 时，最好采用 VHD 格式保留这些文件的唯一情形。

## <a name="types-of-virtual-hard-disk-files"></a>类型的虚拟硬盘文件

有三种类型的 VHD 文件。 以下各节是性能特征和类型之间的权衡。

以下建议应考虑选择 VHD 文件类型方面的考虑因素：

-   使用 VHD 格式时，我们建议使用固定的类型，因为它具有更好的复原能力和性能特征相比其他 VHD 文件类型。

-   使用 VHDX 格式时，我们建议使用动态类型，因为它提供复原保证除了节省空间分配空间，仅当需要为此，与相关联。

-   固定的类型时，也建议，而不考虑格式，当承载卷上的存储不主动监视以确保足够的磁盘空间时在运行时扩展 VHD 文件存在。

-   虚拟机的快照创建差异 VHD 来存储写入磁盘。 包含一些快照可以不明显提升存储 I/o，但可能的 CPU 使用率仅会影响除/O 占用的服务器工作负荷的性能。 但是，具有大型快照链会显著影响性能因为从 VHD 读取可能需要检查多个差异 Vhd 中请求的块。 保持简短的快照链保持正常的磁盘 I/O 性能至关重要。

## <a name="fixed-virtual-hard-disk-type"></a>固定虚拟硬盘类型

创建 VHD 文件时，首先分配为 VHD 的空间。 此类型的 VHD 文件就很难片段，这减少了 I/O 吞吐量，当单个 I/O 拆分为多个 I/o。 它具有三种 VHD 文件类型的最小 CPU 开销，因为读取和写入不需要查找块的映射。

## <a name="dynamic-virtual-hard-disk-type"></a>动态虚拟硬盘类型

按需分配为 VHD 的空间。 在磁盘中的块开始为未分配的块和不受任何文件中的实际空间。 第一个块时写入到中，虚拟化堆栈必须分配的块，VHD 文件中的空间，然后更新元数据。 这需要磁盘 I/o 的数量增加的写入，并提高 CPU 使用率。 读取和写入到现有块都只产生磁盘访问和 CPU 开销查找块的映射的元数据中时。

## <a name="differencing-virtual-hard-disk-type"></a>差异虚拟硬盘类型

VHD 指向父 VHD 文件。 对块未编写为与动态扩展 VHD 一样在 VHD 文件中，所分配的空间会导致任何写操作。 如果已写入块是提供读取服务从 VHD 文件。 否则，它们提供服务的父 VHD 文件。 在这两种情况下，读取元数据来确定的块的映射。 读取和写入此 VHD 可使用更多的 CPU，并导致更多的 I/o 比固定的 VHD 文件。

## <a name="block-size-considerations"></a>块大小考虑事项

块大小可能会显著影响性能。 它是最佳匹配到正在使用该磁盘的工作负荷的分配模式的块大小。 例如，如果应用程序 16 mb 的块区中分配的它将最佳具有虚拟硬盘的块大小为 16 MB。 块大小&gt;2MB 是可能仅在具有 VHDX 格式的虚拟硬盘上。 比随机 I/O 工作负荷的分配模式的更大块大小将会显著增大主机上的空间使用情况。

## <a name="sector-size-implications"></a>扇区大小影响

软件行业的大多数人已经习惯使用 512 字节的磁盘扇区，但标准移动到 4 KB 磁盘扇区。 若要减少兼容性问题可能会引发的扇区大小的更改，硬盘驱动器供应商引入了称为 512 仿真驱动器 (512e) 的过渡大小。

这些仿真驱动器提供一些由 4 KB 磁盘扇区本机驱动器，如经过改进的格式效率和纠错码 (ECC) 的改进的方案提供优势。 它们都具有较少的通过公开 4 KB 扇区大小的磁盘接口会出现兼容性问题。

## <a name="support-for-512e-disks"></a>512e 磁盘的支持

512e 磁盘可以执行仅基于物理扇区写 — 即，不能直接写入颁发给它的 512 字节扇区。 在实现这些写入磁盘中的内部过程遵循以下步骤：

-   磁盘读取到其内部缓存中，其中包含 512 个字节逻辑扇区写到引用的 4 KB 物理扇区。

-   将 4 KB 缓冲区中的数据修改为包含更新的 512 个字节扇区。

-   该磁盘在磁盘上执行更新 4 KB 缓冲区返回到其物理扇区的写入。

此过程称为读取-修改-写入 (RMW)。 RMW 过程的整体性能影响取决于工作负荷。 RMW 过程会导致性能下降的虚拟硬盘原因如下：

-   动态和差异虚拟硬盘的有其数据负载之前 512 字节扇区的位图。 此外，页脚、 页眉和父定位符与对齐 512 字节扇区。 它是常见的虚拟硬盘驱动程序发出 512 个字节写入命令以更新这些结构，从而导致前面所述的 RMW 过程。

-   应用程序通常问题读取和写入的 4 KB 大小 （NTFS 的默认群集大小） 的倍数。 因为动态的数据负载块前面 512 字节扇区的位图，并且差异虚拟硬盘，不到物理 4 KB 边界对齐 4 KB 的块。 下图显示了一个 VHD 4 KB 块 （突出显示），它是与物理 4 KB 边界不对齐。

![vhd 4 kb 的块，](../../media/perftune-guide-vhd-4kb-block.png)

当前解析程序来更新负载数据发出的每个 4 KB 写入命令会导致两个磁盘，然后更新并随后写回到两个磁盘块上的两个块读取。 Windows Server 2016 中的 HYPER-V 通过为 4 KB 边界采用 VHD 格式的对齐方式准备前面提到的结构来缓解一些对 VHD 堆栈上 512e 磁盘上的性能效果。 访问虚拟硬盘文件中的数据时和更新的虚拟硬盘的元数据结构时，这可以避免 RMW 效果。

前面曾提到，从以前版本的 Windows Server 复制的 Vhd 将不自动对齐到 4 KB。 您可以手动将它们转换为以最佳方式使用对齐**来自源的复制**磁盘 VHD 接口中可用的选项。

默认情况下都是通过物理扇区大小为 512 字节公开 Vhd。 这样做是为了确保物理扇区大小依赖应用程序的应用程序和 Vhd 移动从早期版本的 Windows Server 时不会受到影响。

默认情况下，使用 4 KB 物理扇区大小，以优化其性能配置文件正则磁盘和大扇区磁盘创建具有 VHDX 格式的磁盘。 为了充分利用 4 KB 扇区建议使用 VHDX 格式。

## <a name="support-for-native-4kb-disks"></a>对本机 4 KB 磁盘的支持

Hyper-v 在 Windows Server 2012 R2 和更高版本支持 4 KB 本机磁盘。 但仍可在 4 KB 本机磁盘上存储 VHD 磁盘。 这是通过实现软件 RMW 算法中的虚拟存储堆栈层，将 512 个字节访问和更新请求转换为相应的 4 KB 访问和更新。

VHD 文件可以仅将公开本身为 512 个字节逻辑扇区大小的磁盘，因为它是很有可能，将发出 512 个字节输入/输出请求的应用程序。 在这些情况下，RMW 层将满足这些请求，并导致性能下降。 这也是使用的逻辑扇区大小为 512 字节的 VHDX 格式的磁盘，则返回 true。

可以配置要为 4 KB 逻辑扇区大小磁盘，公开的 VHDX 文件，这将在 4 KB 本机物理设备上托管磁盘时是性能的最佳配置。 应格外小心以确保来宾操作系统和应用程序正在使用的虚拟磁盘提供支持按 4KB 的逻辑扇区大小。 VHDX 格式设置将在 4 KB 逻辑扇区大小的设备上正常工作。

## <a name="pass-through-disks"></a>传递磁盘

虚拟机中的 VHD 可以直接映射到物理磁盘或逻辑单元号 (LUN)，而不是到 VHD 文件。 好处是，此配置将绕过 NTFS 文件系统在根分区中，从而减少了存储 I/O 的 CPU 使用率。 风险是物理磁盘或 Lun 可以会更难比 VHD 文件在计算机之间移动。

由于引入了虚拟机的迁移方案的限制，应避免使用传递磁盘。

## <a name="advanced-storage-features"></a>高级的存储功能

### <a name="storage-quality-of-service-qos"></a>服务存储质量 (QoS)

从 Windows Server 2012 R2 开始，HYPER-V 包括为虚拟机上设置存储某些服务质量 (QoS) 参数的功能。 存储 QoS 在多租户环境中提供了存储性能隔离，以及在存储 I/O 性能不满足定义的阈值时通知你的机制，以便高效运行你的虚拟机工作负载。

存储 QoS 能够为你的虚拟硬盘指定每秒输入/输出操作次数 (IOPS) 的最大值。 管理员可以限制存储 I/O，以防止某个租户占用过多的存储资源，这可能影响其他租户。

此外可以设置最小 IOPS 值。 当指定虚拟硬盘的 IOPS 小于其最佳性能所需的阈值时，他们将收到通知。

还使用与存储相关的参数更新虚拟机指标基础结构，以允许管理员监视性能和与退款相关的参数。

根据其中每个 8 KB 的数据计为一个 I/O 的标准化 IOPS 指定最大和最小值。

某些限制如下所示：

-   仅适用于虚拟磁盘

-   差异磁盘的其他卷上不能有父虚拟磁盘

-   副本-副本站点单独配置从主站点的 QoS

-   不支持共享的 VHDX

有关存储服务质量的详细信息，请参阅[质量的存储服务的 HYPER-V](https://technet.microsoft.com/library/dn282281.aspx)。

### <a name="numa-io"></a>NUMA I/O

Windows Server 2012 及支持大型虚拟机和任何大型虚拟机配置 （例如，使用 64 个虚拟处理器运行 Microsoft SQL Server 的配置） 之外还需要在输入/输出吞吐量方面的可伸缩性。

在 Windows Server 2012 存储堆栈和 HYPER-V 中首先引入了以下重大改进提供了大型虚拟机的 I/O 可伸缩性需求：

-   创建来宾设备和主机的存储堆栈之间的通信通道数增加。

-   涉及中断分配到不同的虚拟处理器，以避免成本高昂的 interprocessor 中断效率更高 I/O 完成机制。

在 Windows Server 2012 中引入，有几个注册表条目，位于 HKLM\\系统\\CurrentControlSet\\Enum\\VMBUS\\{设备 id}\\{instance-id}\\StorChannel，允许可进行调整的通道数。 它们还对齐处理由应用程序 I/O 处理器分配的虚拟 Cpu 到 I/O 完成虚拟处理器。 设备的硬件密钥上基于每个适配器配置注册表设置。

-   **ChannelCount (DWORD)** 使用最多 16 个信道的总数。 将默认的上限，即每个 16 虚拟处理器数目。

-   **ChannelMask (QWORD)** 处理器关联的通道。 如果它未设置或者设置为 0，则默认为使用常规存储或网络通道的现有通道分发算法。 这可确保存储通道不会与网络通道冲突。

### <a name="offloaded-data-transfer-integration"></a>卸载的数据传输的集成

重要维护任务的 Vhd，如合并、 移动和压缩，取决于复制大量数据。 当前复制数据的方法需要在其他位置进行读写操作，这是一个非常耗时的过程。 它还使用在可能使用到服务的虚拟机的主机上的 CPU 和内存资源。

存储区域网络 (SAN) 供应商正致力于提供几乎能瞬间复制大量数据的功能。 此存储旨在允许指定特定数据集从一个位置到另一个移动的磁盘上的系统。 此硬件功能被称为卸载数据传输。

HYPER-V 在 Windows Server 2012 和更高版本支持卸载数据传输 (ODX) 操作，以便这些操作可以从来宾操作系统传递到主机硬件。 这可确保工作负荷可以使用已启用 ODX 的存储，就像它在非虚拟化环境中运行。 HYPER-V 存储堆栈还会 ODX 操作期间的 Vhd 的维护操作例如合并磁盘和存储迁移元数据操作的移动大量数据的位置。

### <a name="unmap-integration"></a>取消映射集成

虚拟硬盘文件作为存储卷上的文件都存在，并且它们与其他文件共享可用空间。 因为这些文件的大小通常很大，它们占用的空间可能迅速增大。 更多的物理存储需求会影响 IT 硬件预算。 请务必优化的物理存储尽可能多地使用。

在 Windows Server 2012 之前的应用程序删除虚拟硬盘，其中有效地放弃了内容的存储空间中的内容时来宾操作系统和 HYPER-V 主机中的 Windows 存储堆栈有限制，防止此错误从传达给磁盘的虚拟硬盘和物理存储设备的信息。 这可以阻止 HYPER-V 存储堆栈优化的基于 VHD 的虚拟磁盘文件的空间使用情况。 它还阻止基础存储设备回收已删除的数据之前曾占用的空间。

从 Windows Server 2012 开始，HYPER-V 支持取消映射通知，允许更高效地表示该数据在其中 VHDX 文件。 这会导致较小文件大小，并允许基础物理存储设备回收未使用的空间。

仅 Hyper V 特定 SCSI、 启用了 IDE 和虚拟光纤通道控制器允许 unmap 命令从来宾到达主机虚拟存储堆栈。 虚拟硬盘上仅虚拟磁盘格式化为 VHDX 支持取消映射来自来宾的命令。

出于这些原因，我们建议你使用时不使用虚拟光纤通道磁盘附加到 SCSI 控制器的 VHDX 文件。

## <a name="see-also"></a>请参阅

-   [HYPER-V 术语](terminology.md)

-   [HYPER-V 体系结构](architecture.md)

-   [HYPER-V 服务器配置](configuration.md)

-   [HYPER-V 处理器性能](processor-performance.md)

-   [HYPER-V 内存性能](memory-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [虚拟化环境中检测瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
