---
title: 使用故障转移群集的群集共享卷
description: 如何使用群集共享卷故障转移群集中。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f5bd0ad05bdc2573a5ea0abbe165de2d3e7f5c8f
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233513"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>使用故障转移群集的群集共享卷

>适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

群集共享卷 (CSV) 启用同时具有对同一 LUN （磁盘） 的设置为 NTFS 卷的读 / 写访问权限的故障转移群集中的多个节点。 （在 Windows Server 2012 R2 中，磁盘可以设置为 NTFS 或弹性文件系统 (ReFS)。）使用 CSV，群集的角色可以故障转移快速从一个节点到另一个节点不要求驱动器所有权更改或卸载和重新装入卷。 CSV 还有助于简化的管理可能大量 Lun 故障转移群集中。

CSV 提供的通用、 群集文件系统，其中分层 NTFS （或在 Windows Server 2012 R2 ReFS）。 CSV 应用程序包括：

- 群集群集 HYPER-V 虚拟机的虚拟硬盘 (VHD) 文件
- 向外扩展文件共享存储的向外扩展文件服务器群集角色的应用程序数据。 此角色的应用程序数据的示例包括 HYPER-V 虚拟机文件和 Microsoft SQL Server 数据。 （请注意，ReFS 不支持向外扩展文件服务器。）有关向外扩展文件服务器的详细信息，请参阅[向外扩展应用程序数据的文件服务器](sofs-overview.md)。

>[!NOTE]
>CSV 不支持在 SQL Server 2012 和 SQL server 的早期版本的 Microsoft SQL Server 群集工作负荷。

Windows Server 2012 中已显著增强 CSV 功能。 例如，已删除依赖 Active Directory 域服务。 支持已添加**chkdsk**的功能改进、 与防病毒和备份应用程序互操作性以及与常规存储功能，如 BitLocker 加密卷和存储空间的集成。 Windows Server 2012 中引入的 CSV 功能的概述，请参阅[What's New in 故障转移群集中 Windows Server 2012 \[redirected\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

Windows Server 2012 R2 引入了其他功能，如分布式 CSV 所有权，通过服务器服务可用性、 更大的灵活性您可以更好地分配给 CSV 缓存的物理内存量增加恢复能力diagnosibility，并增强包括支持 ReFS 和消除的互操作性。 有关详细信息，请参阅[What's New in 故障转移群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

>[!NOTE]
>有关重复数据消除 CSV 上使用的虚拟桌面基础结构 (VDI) 方案的信息，请参阅博客文章[以 VDI 存储在 Windows Server 2012 R2 重复部署数据消除](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx)和[扩展重复数据消除中的新工作负荷Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx)。

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>查看要求和故障转移群集中使用 CSV 注意事项

在故障转移群集中使用 CSV 之前, 查看网络、 存储和其他要求和本节中的注意事项。

### <a name="network-configuration-considerations"></a>网络配置注意事项

在配置支持 CSV 的网络时，请考虑以下。

- **多个网络和多个网络适配器**。 若要启用如果网络故障的容错能力，我们建议多个群集网络携带 CSV 流量或配置搭配使用网络适配器。
    
    如果群集节点到群集不应使用的网络连接，则应禁用它们。 例如，我们建议您禁用 iSCSI 网络用于群集阻止这些网络上的 CSV 流量。 若要禁用在网络中，在故障转移群集管理器中，选择**网络**、 选择网络，选择**属性**操作，，然后选择**不允许此网络上的群集网络通信**。 此外，您可以使用[Get-群集网络](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps)Windows PowerShell cmdlet 配置网络的**角色**属性。
- **网络适配器属性**。 在执行群集通信的所有适配器属性中，确保启用的以下设置：

  - **Microsoft 网络客户端**和**文件和打印机共享 Microsoft 网络**。 这些设置支持服务器消息块 (SMB) 3.0，默认情况下用来执行 CSV 节点之间的流量。 若要启用 SMB，还确保服务器服务和工作站服务正在运行，它们配置为自动启动，每个群集节点上。

    >[!NOTE]
    >在 Windows Server 2012 R2 中，有每个故障转移群集节点的多个服务器服务实例。 没有默认实例的处理传入流量 SMB 客户端访问常规文件共享，并且仅处理间节点 CSV 流量的第二个 CSV 实例。 此外，如果节点上的服务器服务变得不正常，CSV 所有权自动过渡到另一个节点。

    SMB 3.0 包括 SMB 多通道和 SMB 直接功能，使跨越多个群集中并利用支持远程直接内存访问 (RDMA) 的网络适配器的网络到流 CSV 流量。 默认情况下 SMB 多通道用于 CSV 流量。 有关详细信息，请参阅[服务器消息块 overview](../storage/file-server/file-server-smb-overview.md)。
  - **Microsoft 故障转移群集虚拟适配器性能筛选器**。 此设置会提高节点能够执行时需要通 CSV，例如，当连接失败时阻止节点直接连接到 CSV 磁盘的 I/O 重定向。 有关详细信息，请参阅本主题后面的[关于 I/O 同步和 CSV 通信中的 I/O 重定向](#about-i/o-synchronization-and-i/o-redirection-in-csv-communication)。
- **群集网络优先顺序**。 通常，我们建议您不要更改网络的群集配置首选项。
- **IP 子网配置**。 任何特定的子网配置，不则需要为网络中使用 CSV 的节点。 CSV 可以支持 multisubnet 群集。
- **基于策略的服务质量 (QoS)**。 我们建议使用 CSV 时配置 QoS 优先级策略和对每个节点的网络流量的最小带宽策略。 有关详细信息，请参阅[服务质量 (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>)。
- **存储网络**。 存储网络建议，查看存储供应商提供的准则。 CSV 的存储的其他注意事项，请参阅本主题后面的[存储和磁盘配置要求](#storage-and-disk-configuration-requirements)。

硬件、 网络和故障转移群集的存储要求的概述，请参阅[故障转移群集的硬件要求和存储选项](clustering-requirements.md)。

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>有关 I/O 同步和 CSV 通信中的 I/O 重定向

- **I/O 同步**： CSV 允许多个节点具有相同的共享存储的同时读 / 写访问权限。 当节点执行 CSV 卷上的磁盘输入/输出 (I/O) 时，节点直接与存储进行通信，例如，通过存储区域网络 (SAN)。 但是，在任何时候，单个节点 （称为协调节点）"拥有"LUN 与关联的物理磁盘资源。 CSV 卷协调节点故障转移群集管理器中显示为**所有者节点**下**磁盘**。 将显示在[Get ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) Windows PowerShell cmdlet 的输出。

  >[!NOTE]
  >在 Windows Server 2012 R2 CSV 所有权均匀分布故障转移群集节点基于每个节点拥有 CSV 卷的数目。 此外，所有权是自动重新平衡有条件，如 CSV 故障转移、 节点加入群集、 将新节点添加到群集，重新启动群集节点中，或后已关闭它启动故障转移群集时。

  CSV 卷上的文件系统中某些小型更改时，必须在每个物理访问 LUN，而不是只在单个协调节点上的节点上同步此元数据。 例如，当 CSV 卷上的虚拟机是启动、 创建或删除，或迁移虚拟机，需要在每个物理访问虚拟机的节点上同步此信息。 这些元数据更新操作会并行跨群集网络使用 SMB 3.0。 这些操作不需要所有物理的节点通信与共享存储。

- **I/O 重定向**： 存储连接失败和某些存储操作可以禁止给定的节点直接与存储进行通信。 若要维护函数，该节点不与存储时，节点，重定向到当前装载磁盘位置的协调节点的磁盘 I/O 通过群集网络。 如果当前的协调节点遇到存储连接故障，建立新节点作为协调节点时暂时排队所有磁盘 I/O 操作。

服务器将使用以下 I/O 重定向模式，具体取决于这种情况之一：

- **文件系统重定向**重定向是每个卷 — 例如，当 CSV 快照备份应用程序时执行 CSV 卷手动放置在重定向 I/O 模式。
- **阻止重定向**重定向文件块级别是 — 例如，当为批量丢失存储连接时才。 显著比文件系统重定向块重定向。

在 Windows Server 2012 R2 中，您可以在每个节点基础上查看 CSV 卷的状态。 例如，您可以看到 I/O 是否直接或重定向，或是否 CSV 卷不可用。 如果 CSV 卷处于 I/O 重定向模式，您还可以查看原因。 使用 Windows PowerShell cmdlet**获取 ClusterSharedVolumeState**查看此信息。

>[!NOTE]
> * Windows Server 2012 中因为 CSV 设计中的改进 CSV 执行直接 I/O 模式中的更多操作比在 Windows Server 2008 R2 中时发生。
> * 由于与 SMB 3.0 功能，如 SMB 多通道和 SMB 直接 CSV 的集成，重定向的 I/O 通信量可以跨越多个群集网络进行流式处理。
> * 您应计划群集网络以允许期间 I/O 重定向到协调器节点的网络流量的潜在增加。

### <a name="storage-and-disk-configuration-requirements"></a>存储和磁盘配置要求

若要使用 CSV，您的存储和磁盘必须满足以下要求：

- **文件系统格式**。 在 Windows Server 2012 R2 中，必须与 NTFS 或 ReFS 分区的基本磁盘 CSV 卷的磁盘或存储空间。 Windows Server 2012 中用于 CSV 批量磁盘或存储空间必须与 NTFS 分区的基本磁盘。

  CSV 具有以下附加要求：
    
  - 在 Windows Server 2012 R2 中，您无法对断行 FAT 或 FAT32 CSV 使用磁盘。
  - 在 Windows Server 2012 中不能为断行 FAT、 FAT32 或 ReFS CSV 使用磁盘。
  - 如果您想要使用的存储空间为 CSV，您可以配置简单空格或镜像空格。 在 Windows Server 2012 R2 中，您还可以配置奇偶校验空格。 （在 Windows Server 2012 中 CSV 不支持奇偶校验空格。）
  - CSV 不能用作仲裁见证磁盘。 有关群集仲裁的详细信息，请参阅[Understanding 仲裁中存储空格直接](../storage/storage-spaces/understand-quorum.md)。
  - 磁盘添加为 CSV 后，它指定 CSVFS 格式 （的 CSV 文件系统）。 这样，群集和其他软件来区分 CSV 存储从其他 NTFS 或 ReFS 存储。 通常，CSVFS 支持 NTFS 或 ReFS 相同的功能。 但是，不支持某些功能。 例如，在 Windows Server 2012 R2 中，不能启用 CSV 压缩。 Windows Server 2012 中不能启用重复数据消除或 CSV 压缩。
- **群集中的资源类型**。 对于 CSV 卷，您必须使用物理磁盘资源类型。 默认情况下，在这种方式情况下自动配置添加到群集存储的磁盘或存储空间。
- **选择的 CSV 磁盘或其他磁盘群集存储中**。 选择一个或多个磁盘的群集的虚拟机时, 请考虑将如何使用每个磁盘。 如果在磁盘上将用于存储由 HYPER-V，例如 VHD 文件或配置文件的文件可以从 CSV 磁盘或群集存储中的其他可用磁盘中进行选择。 如果在磁盘上将物理磁盘的直接附加到虚拟机 （也称为传递磁盘），不能选择 CSV 磁盘上，并且必须选择从群集存储中的其他可用磁盘。
- **用于标识磁盘路径名称**。 CSV 中的磁盘标识路径名称。 每个路径看起来为编号卷**\\ClusterStorage**文件夹下的节点的系统驱动器上。 此路径是时从群集中的任何节点都查看相同的。 如果需要您可以重命名卷。

有关 CSV 的存储要求，请查看存储供应商提供的准则。 其他存储 CSV 的规划注意事项，请参阅本主题后面的[规划用于故障转移群集中的 CSV](#plan-to-use-csv-in-a-failover-cluster) 。

### <a name="node-requirements"></a>节点要求

若要使用 CSV，节点必须满足以下要求：

- **的系统磁盘驱动器号**。 所有节点上的系统磁盘驱动器字母必须相同。
- **身份验证协议**。 必须在所有节点上启用 NTLM 协议。 默认情况下启用。

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>规划用于 CSV 故障转移群集

本节列出了规划注意事项和故障转移群集运行 Windows Server 2012 R2 或 Windows Server 2012 中使用 CSV 建议。

>[!IMPORTANT]
>有关如何配置您的特定存储设备以 CSV 提出的建议的存储供应商。 如果本主题中的信息不同存储供应商的建议，使用从存储供应商的建议。

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Lun、 卷和 VHD 文件的排列方式

若要使利用 CSV 为群集虚拟机提供存储，它是来回顾一下如何将配置物理服务器时排列 Lun （磁盘）。 相应的虚拟机配置时，尝试以类似方式排列 VHD 文件。

请考虑物理服务器为其组织的磁盘和文件，如下所示：

- 系统文件，包括一个物理磁盘上的页面文件
- 在另一个物理磁盘上的数据文件

对于等效的群集虚拟机，您应以类似方式组织卷和文件：

- 系统文件上一个 CSV, VHD 文件中包括页面文件
- 在另一个 CSV VHD 文件中的数据文件

如果在可能的情况，您可以添加另一虚拟机，，，应记住 Vhd 的相同排列的虚拟机上。

### <a name="number-and-size-of-luns-and-volumes"></a>Lun 和卷的数量和大小

如果您计划使用 CSV 故障转移群集的存储配置，请考虑以下建议：

- 要决定多少个 Lun 配置，请参阅存储供应商。 例如，存储供应商可能会建议您与一个分区配置每个 LUN 并置于一个 CSV 卷。
- 不存在的可以支持在单个 CSV 卷的虚拟机数的限制。 但是，您应考虑您打算将群集和每个虚拟机的工作负荷 （每秒 I/O 操作数） 中的虚拟机的数量。 请考虑以下示例：

  - 一个组织部署将支持虚拟桌面基础结构 (VDI)，即相对较少的工作负荷的虚拟机。 群集使用高性能存储。 群集管理员后咨询存储供应商，, 决定放置相对较大数量的每个 CSV 卷的虚拟机。
  - 其他组织部署数量较大的虚拟机将支持常用的数据库应用程序，它是更大的工作负荷。 群集使用性能较低的存储。 群集管理员后咨询存储供应商，, 决定放置相对较少数量的每个 CSV 卷的虚拟机。
- 当您计划特定的虚拟机的存储配置时，请考虑磁盘要求的服务、 应用程序或将支持在虚拟机的角色。 了解这些要求有助于您避免可能会导致性能不佳的磁盘争用。 虚拟机的存储配置紧密应类似于物理服务器运行相同的服务、 应用程序或角色时所使用的存储配置。 有关详细信息，请参阅本主题前面的[排列的 Lun、 卷和 VHD 文件](#arrangement-of-luns,-volumes,-and-vhd-files)。

    您可以通过大量独立的物理硬盘上存储来减少磁盘争用。 因此，选择您的存储硬件和咨询您的供应商，以优化您的存储的性能。
- 根据您的群集工作负载和自己需要的 I/O 操作，则可以考虑配置仅虚拟机访问每个 LUN，而其他虚拟机不具有连接，而是专用计算操作的百分比。

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>故障转移群集上添加 CSV 磁盘

故障转移群集中默认情况下启用 CSV 功能。 若要添加到 CSV 磁盘上，您必须将磁盘添加到**可用存储**组的群集 （如果尚未添加），并将磁盘到 CSV 群集上。 您可以使用故障转移群集管理器或故障转移群集 Windows PowerShell cmdlet 来执行这些过程。

### <a name="add-a-disk-to-available-storage"></a>将磁盘添加到可用的存储区

1. 在故障转移群集管理器中，在控制台树中，展开的群集，名称，然后展开**存储**。
2. 右键单击**磁盘**，，然后选择**添加磁盘**。 显示可用于故障转移群集中添加的磁盘将显示列表。
3. 选择您要添加的磁盘，然后选择**确定**。

    磁盘现在分配给**可用存储**组中。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Windows PowerShell 等效命令 （将在磁盘上添加到可用存储）

以下 Windows PowerShell cmdlet 执行的上述步骤相同的功能。 在单个行中，输入每个 cmdlet，即使它们可能会出现换跨多个行此处由于格式约束。

下面的示例标识便可添加到群集，磁盘，然后将它们添加到**可用存储**组。

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>添加到 CSV 中可存储的磁盘

1. 在故障转移群集管理器中，在控制台树中，展开群集的名称，展开**存储**，，然后选择**磁盘**。
2. 选择一个或更多的磁盘已分配给**可用存储**，右键单击所选内容，然后选择**添加到群集共享卷**。

    磁盘现在分配给**群集共享卷**组群集中。 向每个群集节点作为 %systemdisk%ClusterStorage 文件夹下的编号卷 （装入点） 公开磁盘。 卷出现 CSVFS 文件系统中。

>[!NOTE]
>您可以重命名 CSV 卷 %systemdisk%ClusterStorage 文件夹中。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Windows PowerShell 等效命令 （将在磁盘上添加到 CSV）

以下 Windows PowerShell cmdlet 执行的上述步骤相同的功能。 在单个行中，输入每个 cmdlet，即使它们可能会出现换跨多个行此处由于格式约束。

下面的示例向*群集磁盘 1*的**可用存储**CSV 在本地群集。

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>启用 CSV 缓存的读取密集型工作负荷 （可选）

CSV 缓存提供了在只读无缓冲的 I/O 操作的块级别通过分配作为写通过高速缓存的系统内存 (RAM) 缓存。 （无缓冲的 I/O 操作不会缓存的缓存管理器。）这可以提高应用程序，如 HYPER-V，访问 VHD 时经营无缓冲的 I/O 操作的性能。 CSV 缓存可以提高性能的读取请求不进行缓存写入请求。 启用 CSV 缓存还可用于向外扩展文件服务器方案。

>[!NOTE]
>我们建议您启用所有群集 HYPER-V 和向外扩展文件服务器部署的 CSV 缓存。

默认情况下，Windows Server 2012 中禁用 CSV 缓存。 在 Windows Server 2012 R2 中，默认情况下启用 CSV 缓存。 但是，您仍必须分配保留阻止缓存的大小。

下表介绍控制 CSV 缓存的两个配置设置。

<table>
<thead>
<tr class="header">
<th>Windows Server 2012 R2 中的属性名称</th>
<th>在 Windows Server 2012 中的属性名称</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>BlockCacheSize</strong></td>
<td><strong>SharedVolumeBlockCacheSizeInMB</strong></td>
<td>这是允许您定义多少内存容量 （兆字节） 保留为 CSV 缓存群集中的每个节点上的群集常见属性。 例如，如果已定义的值为 512 是 512 MB 的系统内存是每个节点上保留。 （在多个群集中，使用 512 MB。 建议的值）默认设置是 0 （禁用）。</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>这是在群集物理磁盘资源的专用属性。 它允许您启用 CSV 缓存添加到 CSV 单个磁盘上。 Windows Server 2012 中的默认设置是 0 （禁用）。 若要启用 CSV 缓存磁盘上的，配置的值为 1。 在 Windows Server 2012 R2 中，默认情况下启用此设置。</td>
</tr>
</tbody>
</table>

您可以通过添加下**群集 CSV 卷高速缓存**的计数器来监视性能监视器中的 CSV 缓存。

#### <a name="configure-the-csv-cache"></a>配置 CSV 缓存

1. 以管理员身份启动 Windows PowerShell。
2. 要定义*512* MB，以保留每个节点上的缓存，请键入以下命令：

    - 对于 Windows Server 2012 R2:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - 对于 Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. 在 Windows Server 2012 中要启用 CSV 名为*群集磁盘 1*上的 CSV 缓存，请输入以下信息：

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * Windows Server 2012 中，您可以分配仅为 CSV 缓存物理内存总量的 20%。 在 Windows Server 2012 R2 中，您可以分配 80%。 向外扩展文件服务器通常不是内存约束，因为您可以使用适用于 CSV 缓存的额外内存完成大型性能提升。
> * 若要避免资源争用，您应后重新启动每个节点群集中修改分配给 CSV 缓存的内存。 在 Windows Server 2012 R2 中，重新启动不再是必需的。
> * 启用或禁用的设置才会生效，单个磁盘上的 CSV 缓存后必须脱机的物理磁盘资源，并将其重新联机。 （默认情况下，在 Windows Server 2012 R2 中，CSV 启用缓存。） 
> * 有关 CSV 缓存包含有关性能计数器信息的详细信息，请参阅博客文章[如何启用 CSV 高速缓存](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/)。

## <a name="back-up-csv"></a>备份 CSV

有多种方法来备份存储在 CSV 故障转移群集中的信息。 您可以使用 Microsoft 备份应用程序或非 Microsoft 应用程序。 一般情况下，CSV 不实施群集存储断行 NTFS 或 ReFS 以外的特殊备份要求。 CSV 备份也不会干扰 CSV 存储操作。

选择备份应用程序和 CSV 备份计划时，您应考虑以下因素：

- 可以从 CSV 卷连接的任何节点运行 CSV 卷的卷级备份。
- 备份应用程序可以使用软件快照或硬件快照。 根据备份应用程序支持这些功能，备份可以使用应用程序一致性和崩溃一致的卷影复制服务 (VSS) 快照。
- 如果您要备份 CSV 具有多个正在运行的虚拟机，您通常应该选择管理基于操作系统的备份方法。 如果备份应用程序支持，多个虚拟机可以同时备份。
- CSV 支持运行的 Windows Server 2012 R2 Backup、 Windows Server 2012 Backup 或 Windows Server 2008 R2 备份的备份 requestors。 但是，Windows Server Backup 通常提供仅基本备份解决方案，对于具有更大的群集的组织可能不适用于。 Windows Server Backup 不支持应用程序一致的虚拟机备份 CSV 上。 它支持仅崩溃一致的音量级别备份。 （如果还原崩溃一致的备份，请在虚拟机将它，如果在确切的时间的备份崩溃虚拟机具有相同的状态。）CSV 卷上的虚拟机的备份会成功，但将指出这不受支持记录错误事件。
- 备份故障转移群集时，您可能需要的管理凭据。

>[!IMPORTANT]
>确保仔细查看哪些数据备份应用程序备份和还原，它支持，哪些 CSV 功能并在每个应用程序的资源要求群集节点。

>[!WARNING]
>如果您需要还原 CSV 卷上的备份数据，请注意的功能和维护和群集节点上还原应用程序一致的数据的备份应用程序的限制。 例如，与某些应用程序，如果 CSV 文件恢复不同于其中 CSV 卷备份，该节点的节点上可能无意覆盖重要状态相关的数据应用程序节点上还原正在进行。

## <a name="more-information"></a>详细信息

- [故障转移群集](failover-clustering.md)
- [部署群集的存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)