---
title: 在故障转移群集中使用群集共享卷
description: 如何在故障转移群集中使用群集共享卷。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f5bd0ad05bdc2573a5ea0abbe165de2d3e7f5c8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857698"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>在故障转移群集中使用群集共享卷

>适用于：Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2016

群集共享卷 (CSV) 允许故障转移群集中多个节点同时具有对已设置为 NTFS 卷的同一 LUN（磁盘）的读写访问权限。 （在 Windows Server 2012 R2 中，可以将磁盘设置为 NTFS 或弹性文件系统 (ReFS)。）借助 CSV，群集角色可以从一个节点快速故障转移到另一个节点，而无需对驱动器所有权进行更改，或卸载和重新装载卷。 CSV 还帮助简化管理故障转移群集中潜在大量 LUN 的管理的操作。

CSV 提供了常规用途的群集文件系统，NTFS （或 Windows Server 2012 R2 中的 ReFS） 上进行分层的。 CSV 应用程序包括：

- 群集 Hyper-V 虚拟机的群集虚拟硬盘 (VHD) 文件
- 用于存储横向扩展文件服务器群集角色的应用程序数据的横向扩展文件共享。 此角色的应用程序数据的示例包括 Hyper-V 虚拟机文件和 Microsoft SQL Server 数据。 （请注意横向扩展文件服务器不支持 ReFS。）有关横向扩展文件服务器的详细信息，请参阅[应用程序数据的横向扩展文件服务器](sofs-overview.md)。

>[!NOTE]
>CSV 不支持 SQL Server 2012 和 SQL Server 早期版本中的 Microsoft SQL Server 群集工作负载。

在 Windows Server 2012 中，CSV 功能得到显著增强。 例如，已删除 Active Directory 域服务上的依赖关系。 已添加了对 **chkdsk**中的功能改进、与防病毒和备份应用程序的互操作性以及与常规存储功能（如 BitLocker 加密卷和存储空间）集成的支持。 Windows Server 2012 中引入的 CSV 功能的概述，请参阅[What's New in Windows Server 2012 中故障转移群集\[重定向\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

Windows Server 2012 R2 引入了附加功能，例如分发了 CSV 所有权、 通过服务器服务，你可以更好地分配给 CSV 缓存的物理内存量更大的灵活性的可用性增强的复原能力诊断能力，并包括对 ReFS 和重复数据删除支持的增强互操作性。 有关详细信息，请参阅[What's New in 故障转移群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

>[!NOTE]
>有关将 CSV 上的数据重复删除用于虚拟桌面基础结构 (VDI) 方案的信息，请参阅博客文章 [在 Windows Server 2012 R2 中部署 VDI 存储的数据重复删除](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) 和 [在 Windows Server 2012 R2 中将数据重复删除扩展到新的工作负荷](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx)。

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>查看在故障转移群集中使用 CSV 的要求和注意事项

在故障转移群集中使用 CSV 之前，请查看本部分中的网络、存储以及其他要求和注意事项。

### <a name="network-configuration-considerations"></a>网络配置注意事项

当你配置支持 CSV 的网络时，请考虑以下内容。

- **多个网络和多个网络适配器**。 若要在发生网络故障时启用容错能力，我们建议多个群集网络执行 CSV 通信，或者配置成组网络适配器。
    
    如果群集节点已连接到不应由该群集使用的网络，则应该禁用它们。 例如，我们建议你禁用 iSCSI 网络，以供群集禁止这些网络上的 CSV 通信。 若要禁用网络，请在故障转移群集管理器中，选择**网络**，选择的网络，选择**属性**操作，并选择**不允许上进行群集网络通信此网络**。 或者，可以配置**角色**属性的方法是使用[Get-clusternetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) Windows PowerShell cmdlet。
- **网络适配器属性**。 在执行群集通信的所有适配器的属性中，确保以下设置处于启用状态：

  - “Microsoft 网络的客户端” 和“Microsoft 网络的文件和打印机共享” **File 和“Microsoft 网络的文件和打印机共享” Printer Sharing for Microsoft Networks**。 这些设置支持服务器消息块 (SMB) 3.0，默认情况下用于在节点之间执行 CSV 通信。 若要启用 SMB，还请确保服务器服务和工作站服务正在运行，并且将它们配置为在每个群集节点上自动启动。

    >[!NOTE]
    >在 Windows Server 2012 R2 中，有多个服务器服务实例，每个故障转移群集节点。 存在两个实例：可处理来自访问常规文件共享的 SMB 客户端的传入通信和仅可处理节点间 CSV 通信的第二个 CSV 实例。 此外，如果节点上的服务器服务变得不正常，CSV 所有权将自动转换到另一个节点。

    SMB 3.0 包括 SMB 多通道和 SMB 直通功能，使 CSV 通信能够在群集中跨多个网络进行流式传输并且利用支持远程直接内存访问 (RDMA) 的网络适配器。 默认情况下，SMB 多通道用于 CSV 通信。 有关详细信息，请参阅[服务器消息块概述](../storage/file-server/file-server-smb-overview.md)。
  - “Microsoft 故障转移群集虚拟适配器性能筛选器”。 此设置可改进节点功能以在需要到达 CSV 时执行 I/O 重定向，例如，当连接故障阻止节点直接连接到 CSV 磁盘时。 有关详细信息，请参阅[About I/O 同步和 CSV 通信中的 I/O 重定向](#about-i/o-synchronization-and-i/o-redirection-in-csv-communication)本主题中更高版本。
- **群集网络优先级**。 通常，我们建议你不要更改网络的群集配置首选项。
- **IP 子网配置**。 使用 CSV 的网络中的节点不需要特定的子网配置。 CSV 可以支持多子网群集。
- **基于策略的服务质量 (QoS)**。 我们建议你在使用 CSV 时，针对每个节点的网络通信配置 QoS 优先级策略和最小带宽策略。 有关详细信息，请参阅[服务质量 (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>)。
- **存储网络**。 有关存储网络建议，请查看你的存储供应商提供的指南。 有关 CSV 存储的其他注意事项，请参阅[存储和磁盘配置要求](#storage-and-disk-configuration-requirements)本主题中更高版本。

有关故障转移群集的硬件、网络和存储要求的概述，请参阅 [Failover Clustering Hardware Requirements and Storage Options](clustering-requirements.md)。

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>有关 CSV 通信中的 I/O 同步和 I/O 重定向

- **I/O 同步**:CSV 使多个节点同时具有读写访问权限对同一共享存储。 当某个节点在 CSV 卷上执行磁盘输入/输出 (I/O) 时，该节点将直接与存储进行通信（例如，通过存储区域网络 (SAN)）。 但是，单个节点（称为协调器节点）随时“拥有”与该 LUN 关联的物理磁盘资源。 CSV 卷的协调器节点作为“磁盘”  下的“所有者节点” 显示在故障转移群集管理器中。 它还显示的输出[Get-clustersharedvolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) Windows PowerShell cmdlet。

  >[!NOTE]
  >在 Windows Server 2012 R2 中，CSV 所有权均匀地分布在故障转移群集节点基于每个节点所拥有的 CSV 卷数。 此外，当存在以下条件时自动重新平衡所有权：CSV 故障转移、某个节点重新加入该群集、将新节点添加到该群集、重新启动群集节点，或者在关闭故障转移群集后启动该群集。

  当 CSV 卷上的文件系统中出现某些很小的更改时，则不仅在单个协调器节点上，而且必须在访问 LUN 的每个物理节点上同步此元数据。 例如，当启动、创建或删除 CSV 卷上的虚拟机或迁移虚拟机时，需要在每访问虚拟机的每个物理节点上同步此信息。 通过使用 SMB 3.0，这些元数据更新操作会并行发生在群集网络上。 这些操作不需要所有物理节点与共享存储进行通信。

- **I/O 重定向**:存储连接故障和某些存储操作可以阻止给定节点直接与存储进行通信。 若要在节点无法与存储进行通信时保持功能，该节点通过群集网络将磁盘 I/O 重定向到当前装载该磁盘的协调器节点。 如果当前协调器节点遇到存储连接故障，则在建立作为协调器节点的新节点时，所有磁盘 I/O 操作将暂时排队。

服务器使用以下的 I/O 重定向模式之一，具体取决于相应情况：

- **文件系统重定向** 在每个卷上进行重定向 — 例如，如果备份应用程序在将 CSV 卷手动放置在重定向 I/O 模式中时拍摄的 CSV 快照。
- **阻止重定向** 在文件块级别上进行重定向 — 例如，当卷丢失存储连接时。 块重定向比文件系统重定向快很多。

在 Windows Server 2012 R2 中，可以基于每个节点查看 CSV 卷的状态。 例如，你可以看到 I/O 是直接定向还是重定向，或者 CSV 卷是否不可用。 如果 CSV 卷在 I/O 重定向模式下，则还可以查看原因。 使用 Windows PowerShell cmdlet **Get-ClusterSharedVolumeState** 查看此信息。

>[!NOTE]
> * 在 Windows Server 2012 中，由于 CSV 设计中的改进 CSV 执行在直接 I/O 模式下比 Windows Server 2008 R2 中发生的更多操作。
> * 由于借助将 CSV 与 SMB 3.0 功能（如 SMB 多通道和 SMB 直通）相集成，重定向 I/O 通信可以在多个群集网络上进行流式传输。
> * 你应该计划群集网络，以允许在 I/O 重定向期间潜在增加协调器节点的网络通信。

### <a name="storage-and-disk-configuration-requirements"></a>存储和磁盘配置要求

若要使用 CSV，你的存储和磁盘必须满足以下要求：

- **文件系统格式**。 在 Windows Server 2012 R2 中，CSV 卷的磁盘或存储空间必须是使用 NTFS 或 ReFS 分区的基本磁盘。 在 Windows Server 2012 中，CSV 卷的磁盘或存储空间必须使用 NTFS 分区的基本磁盘。

  CSV 具有以下附加要求：
    
  - 在 Windows Server 2012 R2 中，不能使用通过 FAT 或 FAT32 进行格式化的 csv 磁盘。
  - 在 Windows Server 2012 中，不能使用通过 FAT、 FAT32 或 ReFS 进行格式化的 csv 磁盘。
  - 如果你想要使用 CSV 的存储空间，则可以配置简单空间或镜像空间。 在 Windows Server 2012 R2 中，您还可以配置奇偶校验空间。 （在 Windows Server 2012 中，CSV 不支持奇偶校验空间。）
  - CSV 不能用作仲裁见证磁盘。 有关群集仲裁的详细信息，请参阅[存储空间直通中了解仲裁](../storage/storage-spaces/understand-quorum.md)。
  - 将磁盘添加为 CSV 后，采用 CSVFS 格式（适用于 CSV 文件系统）指定它。 这允许群集和其他软件将 CSV 存储从其他 NTFS 或 ReFS 存储中区分开。 通常情况下，CSVFS 支持与 NTFS 或 ReFS 相同的功能。 但是，某些功能不受支持。 例如，在 Windows Server 2012 R2 中，你不能启用压缩 CSV 上。 在 Windows Server 2012 中，不能启用重复数据删除或压缩 CSV 上。
- **群集中的资源类型**。 对于 CSV 卷，必须使用物理磁盘资源类型。 默认情况下，添加到群集存储的磁盘或存储空间将会采用此方式自动配置。
- **选择群集存储中的 CSV 磁盘或其他磁盘**。 当选择群集虚拟机的一个或多个磁盘时，请考虑将如何使用每个磁盘。 如果将磁盘用于存储由 Hyper-V 创建的文件（如 VHD 文件或配置文件），则可以从 CSV 磁盘或群集存储中的其他可用磁盘中进行选择。 如果磁盘是直接附加到虚拟机的物理磁盘（也称为传递磁盘），则你不能选择 CSV 磁盘，并且必须从群集存储中其他可用磁盘中进行选择。
- **用于标识磁盘的路径名称**。 使用路径名称标识 CSV 中的磁盘。 每个路径看起来作为下带编号的卷上的节点的系统驱动器 **\\ClusterStorage**文件夹。 当从群集中的任何节点进行查看时，此路径相同。 可以重命名这些卷（如果需要）。

有关 CSV 的存储要求，请查看你的存储供应商提供的指南。 有关 CSV 的其他存储计划注意事项，请参阅本主题后面的[计划在故障转移群集中使用 CSV](#plan-to-use-csv-in-a-failover-cluster)。

### <a name="node-requirements"></a>节点要求

若要使用 CSV，你的节点必须满足以下要求：

- **系统磁盘的驱动器号**。 在所有节点上，系统磁盘的驱动器号必须相同。
- **身份验证协议**。 必须在所有节点上启用 NTLM 协议。 默认情况下该协议处于启用状态。

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>计划在故障转移群集中使用 CSV

本部分列出了计划注意事项和运行 Windows Server 2012 R2 或 Windows Server 2012 的故障转移群集中使用 CSV 的建议。

>[!IMPORTANT]
>向存储供应商询问有关如何为 CSV 配置特定存储单位的建议。 如果存储供应商提供的建议与本主题中的信息不同，请使用存储供应商提供的建议。

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>LUN、卷和 VHD 文件的排列

若要最有效地使用 CSV 来为群集虚拟机提供存储，它有助于在你配置物理服务器时查看排列 LUN（磁盘）的方式。 当你配置相应的虚拟机时，请尝试采用相似方式排列 VHD 文件。

请考虑你可以如下所示组织磁盘和文件的物理服务器：

- 一个物理磁盘上的系统文件（包括页面文件）
- 另一个物理磁盘上的数据文件

对于等效的群集虚拟机，你应采用相似方式整理卷和文件：

- 一个 CSV 上的 VHD 文件中的系统文件（包括页面文件）
- 另一个 CSV 上的 VHD 文件中的数据文件

如果你添加另一台虚拟机，则应该保留该虚拟机上 VHD 的相同排列（如果可能）。

### <a name="number-and-size-of-luns-and-volumes"></a>LUN 以及卷的数量和大小

当你为使用 CSV 的故障转移群集计划存储配置时，请考虑以下建议：

- 若要决定配置多少 LUN，请咨询你的存储供应商。 例如，你的存储供应商可能会建议你使用一个分区配置每个 LUN，并在其上放置一个 CSV 卷。
- 对在单个 CSV 卷上受支持的虚拟机的数量没有限制。 但是，应该为每台虚拟机考虑你计划在群集和工作负载（每秒 I/O 操作）中使用的虚拟机的数量。 请考虑以下示例：

  - 一个组织正在部署将支持虚拟桌面基础结构 (VDI) 的虚拟机，它的工作负载相对较轻。 该群集使用高性能存储。 群集管理员在咨询存储供应商后，可确定在每个 CSV 卷上放置相对较大数量的虚拟机。
  - 另一个组织正在部署将支持大量使用的数据库应用程序的大量虚拟机，它的工作负载较重。 该群集使用低性能存储。 群集管理员在咨询存储供应商后，可确定在每个 CSV 卷上放置相对较小数量的虚拟机。
- 当为特定虚拟机计划存储配置时，请考虑虚拟机将支持的服务、应用程序或角色的磁盘要求。 了解这些要求可帮助你避免导致性能下降的磁盘争用。 虚拟机的存储配置应该非常类似于你将用于运行相同的服务、应用程序或角色的物理服务器的存储配置。 有关详细信息，请参阅[排列方式的 Lun、 卷和 VHD 文件](#arrangement-of-luns,-volumes,-and-vhd-files)本主题前面的。

    你还可以通过拥有大量独立的物理硬盘的存储来缓解磁盘争用。 相应地选择你的存储硬件，并咨询你的供应商如何优化存储的性能。
- 根据群集工作负载及其 I/O 操作需求，你可以考虑仅配置一部分虚拟机来访问每个 LUN，同时不会连接其他虚拟机，这些虚拟机专门用于计算操作。

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>将磁盘添加到故障转移群集上的 CSV

默认情况下，CSV 功能在故障转移群集中处于启用状态。 若要将磁盘添加到 CSV，你必须将磁盘添加到群集的“可用存储”  组（如果尚未添加），然后将磁盘添加到该群集上 CSV。 可以使用故障转移群集管理器或故障转移群集 Windows PowerShell cmdlet 来执行这些过程。

### <a name="add-a-disk-to-available-storage"></a>将磁盘添加到可用存储

1. 在故障转移群集管理器的控制台树中，展开该群集的名称，然后展开“存储” 。
2. 右键单击**磁盘**，然后选择**添加磁盘**。 将出现一个列表，显示可添加的以供在故障转移群集中使用的磁盘。
3. 选择你想要添加，并选择的磁盘**确定**。

    现在将磁盘分配给“可用存储”组。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Windows PowerShell 等效命令 （将磁盘添加到可用存储）

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

以下示例将标识已准备好添加到群集的磁盘，然后将它们添加到“可用存储”组。

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>可用存储中的磁盘添加到 CSV

1. 在故障转移群集管理器中，在控制台树中，展开该群集的名称，展开**存储**，然后选择**磁盘**。
2. 选择一个或多个磁盘分配给**可用存储**，右键单击所选内容，并选择**添加到群集共享卷**。

    现在将磁盘分配给群集中的“群集共享卷”  组。 在 %SystemDisk%ClusterStorage 文件夹下，向每个群集节点公开作为带编号的卷（装入点）的磁盘。 这些卷将出现在 CSVFS 文件系统中。

>[!NOTE]
>你可以在 %SystemDisk%ClusterStorage 文件夹中重命名 CSV 卷。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Windows PowerShell 等效命令 （将磁盘添加到 CSV）

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

以下示例将“可用存储”中的**群集磁盘 1** 添加到本地群集上的 CSV。

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>为读密集型工作负载启用 CSV 缓存（可选）

CSV 缓存通过将系统内存 (RAM) 分配为直写缓存，在只读无缓冲 I/O 操作的块级别上提供了缓存。 （无缓冲 I/O 操作不会通过缓存管理器缓存。）这可以提高应用程序（如 unbuffered）的性能，从而在访问 VHD 时执行无缓冲 I/O 操作。 CSV 缓存可以提高读取请求的性能，而无需缓存写入请求。 对于横向扩展文件服务器方案，启用 CSV 缓存也非常有用。

>[!NOTE]
>我们建议为所有群集 Hyper-V 和横向扩展文件服务器部署启用 CSV 缓存。

默认情况下，Windows Server 2012 中禁用 CSV 缓存。 在 Windows Server 2012 R2 中，默认情况下启用 CSV 缓存。 但是，你必须仍然分配要保留的块缓存大小。

下表介绍了控制 CSV 缓存的两个配置设置。

<table>
<thead>
<tr class="header">
<th>Windows Server 2012 R2 中的属性名称</th>
<th>Windows Server 2012 中的属性名称</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>BlockCacheSize</strong></td>
<td><strong>SharedVolumeBlockCacheSizeInMB</strong></td>
<td>这是群集公用属性，它允许你定义要为群集中每个节点上的 CSV 保留的内存量（以兆字节为单位）。 例如，如果定义了值 512，则在每个节点上保留 512 MB 的系统内存。 （在许多群集中，512 MB 是推荐的值。）默认设置为 0 （对应于已禁用）。</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>这是群集物理磁盘资源的专用属性。 它允许你在添加到 CSV 的单个磁盘上启用 CSV 缓存。 Windows Server 2012 中的默认设置为 0 （对应于已禁用）。 若要在磁盘上启用 CSV 缓存，请配置值 1。 默认情况下，在 Windows Server 2012 R2 中，启用此设置。</td>
</tr>
</tbody>
</table>

通过在“群集 CSV 卷缓存” 下添加计数器，可在性能监视器中监视 CSV 缓存。

#### <a name="configure-the-csv-cache"></a>配置 CSV 缓存

1. 以管理员身份启动 Windows PowerShell。
2. 若要定义要在每个节点上保留的 *512* MB 的缓存，请键入以下内容：

    - 适用于 Windows Server 2012 R2:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - 对于 Windows Server 2012：

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. 在 Windows Server 2012，若要启用名为 CSV 上的 CSV 缓存*群集磁盘 1*，请输入以下命令：

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * 在 Windows Server 2012 中，可以分配给 CSV 缓存的总物理 RAM 的仅有 20%。 在 Windows Server 2012 R2 中，您可以分配 80%。 由于横向扩展文件服务器通常不受内存的约束，因此可以通过使用 CSV 缓存的额外内存来完成较大的性能增益。
> * 若要避免资源争用，您应重新启动每个节点群集中后修改分配给 CSV 缓存的内存。 在 Windows Server 2012 R2 中，重新启动不再需要。
> * 在单个磁盘上启用或禁用CSV 缓存后，必须使物理磁盘资源脱机并将其重新联机，才能使设置生效。 （默认情况下，在 Windows Server 2012 R2 中，CSV 缓存已启用。） 
> * 有关包括性能计数器相关信息的 CSV 缓存的详细信息，请参阅博客文章 [如何启用 CSV 缓存](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/)。

## <a name="back-up-csv"></a>备份 CSV

有多种方法可备份存储在故障转移群集中的 CSV 上的信息。 你可以使用 Microsoft 备份应用程序或非 Microsoft 应用程序。 通常，对于那些使用 NTFS 或 ReFS 格式化的群集存储之外的存储，CSV 不会施加特殊的备份要求。 CSV 备份也不会中断其他 CSV 存储操作。

当你选择 CSV 的备份应用程序和备份计划时，应考虑以下因素：

- 可以从连接到 CSV 卷的任何节点运行 CSV 卷的卷级备份。
- 备份应用程序可以使用软件快照或硬件快照。 备份可以使用应用程序一致和崩溃一致的卷影复制服务 (VSS) 快照，具体取决于备份应用程序能否支持它们。
- 如果你正在备份具有多个运行的虚拟机的 CSV，通常应选择基于管理操作系统的备份方法。 如果你的备份应用程序支持它，则可以同时备份多个虚拟机。
- CSV 支持运行 Windows Server 2012 R2 备份、 Windows Server 2012 Backup 或 Windows Server 2008 R2 备份的备份请求者。 但是，Windows Server Backup 通常仅提供可能不适合具有较大群集的组织的基本备份解决方案。 Windows Server Backup 不支持 CSV 上应用程序一致的虚拟机备份。 它仅支持崩溃一致的卷级备份。 （如果还原崩溃一致的备份，虚拟机将处于它发生崩溃时（进行备份的准确时间）所处的状态。）CSV 卷上的虚拟机的备份将成功，但会记录错误事件，指示这不受支持。
- 当备份故障转移群集时，你可能需要管理凭据。

>[!IMPORTANT]
>请确保仔细查看备份应用程序将备份和还原的数据、它支持的 CSV 功能，以及每个群集节点上的应用程序的资源要求。

>[!WARNING]
>如果需要在 CSV 卷上还原备份数据，请注意备份应用程序在群集节点上维护和还原应用程序一致的数据时的功能和限制。 例如，借助某些应用程序，如果在不同于备份 CSV 卷所在的节点上还原 CSV，则你可能会无意中覆盖正在进行还原的节点上的有关应用程序状态的重要数据。

## <a name="more-information"></a>详细信息

- [故障转移群集](failover-clustering.md)
- [部署群集的存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)