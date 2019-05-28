---
title: 应用程序数据用横向扩展文件服务器概述
description: 适用于 Windows Server 201 R2 和 Windows Server 2012 的横向扩展文件服务器功能的概述。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 3e6d67eee496d19b216a4366af51ab5736229cf0
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476145"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>应用程序数据用横向扩展文件服务器概述

>适用于：Windows Server 2012 R2, Windows Server 2012

横向扩展文件服务器是一项设计用于提供横向扩展文件共享的功能，该类共享可供基于文件的服务器应用程序存储连续使用。 横向扩展文件共享允许从同一群集的多个节点上共享同一文件夹。 本方案重点介绍如何计划和部署横向扩展文件服务器。

可以通过使用以下方法之一来部署和配置群集文件服务器：

- **应用程序数据的横向扩展文件服务器**Windows Server 2012 中引入此群集的文件服务器功能，它允许你将服务器应用程序数据，例如 HYPER-V 虚拟机文件，文件共享上，并获取类似水平的可靠性、 可用性、 可管理性和高性能预期从存储区域网络。 所有文件共享均为在线状态，且同时位于全部节点上。 与此类群集文件服务器相关的文件共享被称为横向扩展文件共享。 这有时也称为活动/活动。 部署 Hyper-V over Server Message Block (SMB) 或 Microsoft SQL Server over SMB 时，建议采用该类文件服务器。
- **一般用途文件服务器** 该类服务器是群集文件服务器的延续，自从引入故障转移群集之后，Windows Server 已支持该服务器。 该类服务器属于群集文件服务器，因此与群集文件服务器相关的所有共享均为在线状态，且在同一时间位于某一个节点上。 这有时也称为活动/被动或双活动。 与此类型群集文件服务器相关的文件共享被称为群集文件共享。 部署信息工作者方案时，建议采用该类文件服务器。

## <a name="scenario-description"></a>方案说明

利用横向扩展文件共享，可以从某个群集的多个节点共享同一文件夹。 例如，如果必须使用服务器消息块 (SMB) 横向扩展的四节点文件服务器群集，运行 Windows Server 2012 R2 或 Windows Server 2012 的计算机可以从任何的四个节点访问文件共享。 该功能通过利用新 Windows 服务器故障转移群集功能和 Windows 文件服务器协议 SMB 3.0 的功能得以实现。 文件服务器管理员可为服务器应用程序提供横向扩展文件共享及持续可用文件服务，而且只需简单地增加在线服务器的数量即可快速响应增加的需求。 所有此类功能均可在生产环境中实现，且对服务器应用程序来说完全透明。

横向扩展文件服务器提供的主要优点包括：

- **活动 / 活动文件共享**。 所有群集节点可以接受，并为 SMB 客户端请求提供服务。 通过使文件共享的内容可通过全部群集节点同时访问，SMB 3.0 群集和客户端共同协作在计划的维护和非计划故障（服务中断）期间为备用群集节点提供透明的故障转移。
- **带宽增加**。 最大共享带宽是全部文件服务器群集节点的带宽总和。 与 Windows Server 早期版本不同的是，总带宽不再受限于单个群集节点的带宽，而是由后备存储系统的容量来确定带宽限制。 你可以通过添加节点来增加总带宽。
- **零停机时间的 CHKDSK**。 Windows Server 2012 中的 CHKDSK 大幅度增强，极大地缩短了文件系统处于脱机状态的修复时间。 群集共享卷 (CSV) 更进一步，直接省去了脱机阶段。 CSV 文件系统 (CSVFS) 可在不影响文件系统中带有开放句柄的应用程序的情况下使用 CHKDSK。
- **群集共享卷缓存**。 Windows Server 2012 中的 Csv 引入了读取缓存，这可以显著提高性能在某些情况下，如在虚拟桌面基础结构 (VDI) 的支持。
- **管理更为简单**。 与横向扩展文件服务器，您将创建横向扩展文件服务器，然后再添加必要的 Csv 和文件共享。 无需创建多个群集文件服务器，每个服务器均具有单独的群集磁盘，且无需开发放置策略来确保各个群集节点上的活动。
- **自动重新平衡横向扩展文件服务器客户端**。 在 Windows Server 2012 R2 中，自动重新平衡可以提高可伸缩性和可管理性的横向扩展文件服务器。 将按照每个文件共享（而不是每个服务器）跟踪 SMB 客户端连接，然后将客户端重定向到群集节点，并让用户最方便地访问文件共享使用的卷。 这样便会减少文件服务器节点之间的重定向流量，从而提高效率。 在建立初始连接后以及在重新配置群集存储时，将重定向客户端。

## <a name="in-this-scenario"></a>本方案内容

以下主题可用于帮助你部署横向扩展文件服务器：

- [规划横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [步骤 1：横向扩展文件服务器中的存储规划](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [步骤 2：横向扩展文件服务器中的网络规划](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [部署横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [步骤 1：安装横向扩展文件服务器的先决条件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [步骤 2：配置横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [步骤 3：配置 HYPER-V 以使用横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [步骤 4：配置 Microsoft SQL Server 以使用横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>何时使用横向扩展文件服务器

如果你的工作量会产生大量的元数据操作，例如打开文件、关闭文件、创建新文件或重命名现有文件，则不应使用横向扩展文件服务器。 一般的信息操作会导致大量元数据操作。 如果你偏重横向扩展文件服务器所提供的可伸缩性和简易性且仅仅需要横向扩展文件服务器所支持的技术，则应使用横向扩展文件服务器。

下表列出了 SMB 3.0 中的功能、常见 Windows 文件系统、文件服务器数据管理技术和常见工作负荷。 你可以看到该技术是否受横向扩展文件服务器支持，或者是否需要传统的群集文件服务器（也称为一般用途文件服务器）：

<table>
<thead>
<tr class="header">
<th>技术领域</th>
<th>功能</th>
<th>一般用途文件服务器群集</th>
<th>横向扩展文件服务器</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>SMB 连续可用性</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB 多通道</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB 直通</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB 加密</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB 透明故障转移</td>
<td>是（如果启用了连续可用性）</td>
<td>是</td>
</tr>
<tr class="even">
<td>文件系统</td>
<td>NTFS</td>
<td>是</td>
<td>NA</td>
</tr>
<tr class="odd">
<td>文件系统</td>
<td>弹性文件系统 (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>建议的存储空间直通</td>
<td>建议的存储空间直通</td>
</tr>
<tr class="even">
<td>文件系统</td>
<td>群集共享卷文件系统 (CSV)</td>
<td>NA</td>
<td>是</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>BranchCache</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>重复数据删除 (Windows Server 2012)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>重复数据删除 (Windows Server 2012 R2)</td>
<td>是</td>
<td>是（仅限 VDI）</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>DFS 命名空间 (DFSN) 根服务器根</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>DFS 命名空间 (DFSN) 文件夹目标服务器</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>DFS 复制 (DFSR)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>文件服务器资源管理器（屏幕和配额）</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>文件分类基础结构</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>动态访问控制（基于声明的访问，CAP）</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>文件夹重定向</td>
<td>是</td>
<td>不建议*</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>脱机文件（客户端缓存）</td>
<td>是</td>
<td>不建议*</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>漫游用户配置文件</td>
<td>是</td>
<td>不建议*</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>主目录</td>
<td>是</td>
<td>不建议*</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>工作文件夹</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>NFS 服务器</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>应用程序</td>
<td>Hyper-V</td>
<td>不建议</td>
<td>是</td>
</tr>
<tr class="odd">
<td>应用程序</td>
<td>Microsoft SQL Server</td>
<td>不建议</td>
<td>是</td>
</tr>
</tbody>
</table>

\* 文件夹重定向、 脱机文件、 漫游用户配置文件或主目录生成大量必须立即写入磁盘 （无缓冲） 的写入时使用的持续可用文件共享，从而降低相比性能一般用途文件共享。 连续可用的文件共享与文件服务器资源管理器和运行 Windows XP 的电脑也不兼容。 此外，在用户失去对共享的访问权限后的 3 到 6 分钟内，脱机文件可能无法切换到脱机模式，这可能会让尚未使用脱机文件的“始终脱机”模式的用户感到灰心。

## <a name="practical-applications"></a>实际应用程序

横向扩展文件服务器非常适用于服务器应用程序存储。 下面列出了可以将其数据存储在横向扩展文件共享上的服务器应用程序的几个示例：

- Internet Information Services (IIS) Web 服务器可以将网站的配置和数据存储在横向扩展文件共享上。 有关详细信息，请参阅 [共享配置](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264)。
- Hyper-V 可以将配置和实时虚拟磁盘存储在横向扩展文件共享上。 有关详细信息，请参阅 [部署基于 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。
- SQL Server 可以将实时数据库文件存储在横向扩展文件共享上。 有关详细信息，请参阅 [安装 SQL Server 并使用 SMB 文件共享作为存储选项](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option)。
- Virtual Machine Manager (VMM) 可以将库共享（其中包含虚拟机模板和相关文件）存储在横向扩展文件共享上。 但是，在库服务器本身不能为横向扩展文件服务器 — 它必须是独立服务器或不使用横向扩展文件服务器群集角色的故障转移群集上。

如果将横向扩展文件共享用作库共享，则只能使用与横向扩展文件服务器兼容的技术。 例如，不能使用 DFS 复制来复制横向扩展文件共享上托管的库共享。 还有一点很重要，那就是横向扩展文件服务器安装了最新的软件更新。

若要将横向扩展文件共享用作库共享，首先添加一个具有本地共享或根本没有共享的库服务器（比如虚拟机）。 然后在添加库共享时，选择托管于横向扩展文件服务器的文件共享。 此共享应由 VMM 管理，并且专门创建用于库服务器。 此外，请确保在横向扩展文件服务器上安装最新的更新。 有关添加 VMM 库服务器和库共享的详细信息，请参阅[将配置文件添加到 VMM 库](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801)。 有关文件和存储服务当前可用的修补程序的列表，请参阅 [Microsoft 知识库文章 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie)。

>[!NOTE]
>某些用户（比如信息工作者）的工作负荷对性能的影响较大。 例如，当多个用户执行诸如打开和关闭文件、创建新文件和重命名现有文件之类的操作时，将对性能产生影响。 如果文件共享启用连续可用性，它将提供数据完整性，但同时也会影响整体性能。 连续可用性要求将数据直接写入磁盘，以便在横向扩展文件服务器中的群集节点出现故障时确保数据的完整性。 因此，将多个大型文件复制到文件服务器的用户可能会发现连续可用的文件共享上的性能明显变慢。

## <a name="features-included-in-this-scenario"></a>本方案中所含的功能

下表列出了本方案的部分功能并说明了支持该方案的工作原理。

<table>
<thead>
<tr class="header">
<th>功能</th>
<th>如何支持本方案</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">故障转移群集</a></td>
<td>故障转移群集 Windows Server 2012，以支持横向扩展文件服务器中添加了以下功能：分布式的网络名称、 横向扩展文件服务器资源类型、 群集共享卷 (CSV) 2 和横向扩展文件服务器高可用性角色。 有关这些功能的详细信息，请参阅<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">What's New in Windows Server 2012 [重定向] 中的故障转移群集</a>。</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">服务器消息块</a></td>
<td>SMB 3.0 以支持横向扩展文件服务器的 Windows Server 2012 中添加了以下功能：SMB 透明故障转移、SMB 多通道和 SMB 直通。<br />
<br />
Windows Server 2012 R2 中 SMB 的新功能和更改功能的详细信息，请参阅<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What's New in Windows Server 中 SMB</a>。</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>详细信息

- [软件定义的存储设计注意事项指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [增强服务器、 存储和网络可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署基于 SMB 的 HYPER-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [为服务器应用程序部署快速且高效的文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [要不要横向扩展，这才是问题所在](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) （博客文章）
- [文件夹重定向、 脱机文件和漫游用户配置文件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)