---
title: 应用程序数据用横向扩展文件服务器概述
description: Windows Server 201 R2 和 Windows Server 2012 横向扩展文件服务器功能的概述。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 80bf85caac6f84e0d6da0c6139e39f3823b3a961
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868707"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>应用程序数据用横向扩展文件服务器概述

>适用于：Windows Server 2012 R2、Windows Server 2012

横向扩展文件服务器是一项设计用于提供横向扩展文件共享的功能，该类共享可供基于文件的服务器应用程序存储连续使用。 横向扩展文件共享允许从同一群集的多个节点上共享同一文件夹。 本方案重点介绍如何计划和部署横向扩展文件服务器。

可以通过使用以下方法之一来部署和配置群集文件服务器：

- **应用程序数据的横向扩展文件服务器**此群集文件服务器功能是在 Windows Server 2012 中引入的，它使你可以在文件共享上存储服务器应用程序数据（例如 Hyper-v 虚拟机文件），并获得类似的可靠性、可用性、可管理性和高级别从存储区域网络中获得的性能。 所有文件共享均为在线状态，且同时位于全部节点上。 与此类群集文件服务器相关的文件共享被称为横向扩展文件共享。 这有时也称为活动/活动。 部署 Hyper-V over Server Message Block (SMB) 或 Microsoft SQL Server over SMB 时，建议采用该类文件服务器。
- **一般用途文件服务器** 该类服务器是群集文件服务器的延续，自从引入故障转移群集之后，Windows Server 已支持该服务器。 该类服务器属于群集文件服务器，因此与群集文件服务器相关的所有共享均为在线状态，且在同一时间位于某一个节点上。 这有时也称为活动/被动或双活动。 与此类型群集文件服务器相关的文件共享被称为群集文件共享。 部署信息工作者方案时，建议采用该类文件服务器。

## <a name="scenario-description"></a>方案说明

利用横向扩展文件共享，可以从某个群集的多个节点共享同一文件夹。 例如，如果你有一个使用服务器消息块（SMB）扩展的四节点文件服务器群集，则运行 Windows Server 2012 R2 或 Windows Server 2012 的计算机可以从四个节点中的任一节点访问文件共享。 该功能通过利用新 Windows 服务器故障转移群集功能和 Windows 文件服务器协议 SMB 3.0 的功能得以实现。 文件服务器管理员可为服务器应用程序提供横向扩展文件共享及持续可用文件服务，而且只需简单地增加在线服务器的数量即可快速响应增加的需求。 所有此类功能均可在生产环境中实现，且对服务器应用程序来说完全透明。

横向扩展文件服务器提供的主要优点包括：

- **主动-主动文件共享**。 所有群集节点均可接受 SMB 客户端请求并为其提供服务。 通过使文件共享的内容可通过全部群集节点同时访问，SMB 3.0 群集和客户端共同协作在计划的维护和非计划故障（服务中断）期间为备用群集节点提供透明的故障转移。
- **增加了带宽**。 最大共享带宽是所有文件服务器群集节点的总带宽。 与 Windows Server 早期版本不同的是，总带宽不再受限于单个群集节点的带宽，而是由后备存储系统的容量来确定带宽限制。 你可以通过添加节点来增加总带宽。
- 无**停机时间的 CHKDSK**。 Windows Server 2012 中的 CHKDSK 大大增强，大大缩短了文件系统脱机修复的时间。 群集共享卷 (CSV) 更进一步，直接省去了脱机阶段。 CSV 文件系统 (CSVFS) 可在不影响文件系统中带有开放句柄的应用程序的情况下使用 CHKDSK。
- **群集共享卷缓存**。 Windows Server 2012 中的 Csv 引入了读取缓存支持，这在某些情况下（例如虚拟桌面基础结构（VDI））可以显著提高性能。
- **更简单的管理**。 通过横向扩展文件服务器创建横向扩展文件服务器，然后添加必要的 Csv 和文件共享。 无需创建多个群集文件服务器，每个服务器均具有单独的群集磁盘，且无需开发放置策略来确保各个群集节点上的活动。
- **自动重新平衡横向扩展文件服务器客户端**。 在 Windows Server 2012 R2 中，自动重新平衡可以提高横向扩展文件服务器的可伸缩性和可管理性。 将按照每个文件共享（而不是每个服务器）跟踪 SMB 客户端连接，然后将客户端重定向到群集节点，并让用户最方便地访问文件共享使用的卷。 这样便会减少文件服务器节点之间的重定向流量，从而提高效率。 在建立初始连接后以及在重新配置群集存储时，将重定向客户端。

## <a name="in-this-scenario"></a>本方案内容

以下主题可用于帮助你部署横向扩展文件服务器：

- [规划横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [步骤 1：在横向扩展文件服务器中规划存储](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [步骤 2：在横向扩展文件服务器中规划网络](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [部署横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [步骤 1：安装横向扩展文件服务器的先决条件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [步骤 2：配置横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [步骤 3：配置 Hyper-v 以使用横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [步骤 4：配置要使用的 Microsoft SQL Server 横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

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
<td>不可用</td>
</tr>
<tr class="odd">
<td>文件系统</td>
<td>复原文件系统（<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>）</td>
<td>建议用于存储空间直通</td>
<td>建议用于存储空间直通</td>
</tr>
<tr class="even">
<td>文件系统</td>
<td>群集共享卷文件系统 (CSV)</td>
<td>不可用</td>
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
<td>不建议<em></td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>脱机文件（客户端缓存）</td>
<td>是</td>
<td>不建议</em></td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>漫游用户配置文件</td>
<td>是</td>
<td>不建议<em></td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>主目录</td>
<td>是</td>
<td>不建议</em></td>
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

\*使用连续可用的文件共享时，文件夹重定向、脱机文件、漫游用户配置文件或主目录会生成大量必须立即写入磁盘（无缓冲）的写入操作，从而降低性能，与常规用途文件共享。 连续可用的文件共享与文件服务器资源管理器和运行 Windows XP 的电脑也不兼容。 此外，在用户失去对共享的访问权限后，脱机文件可能不会转换为脱机模式3-6 分钟，这可能会让尚未使用 "始终脱机" 模式脱机文件的用户不快。

## <a name="practical-applications"></a>实际应用程序

横向扩展文件服务器非常适用于服务器应用程序存储。 下面列出了可以将其数据存储在横向扩展文件共享上的服务器应用程序的几个示例：

- Internet Information Services (IIS) Web 服务器可以将网站的配置和数据存储在横向扩展文件共享上。 有关详细信息，请参阅 [共享配置](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264)。
- Hyper-V 可以将配置和实时虚拟磁盘存储在横向扩展文件共享上。 有关详细信息，请参阅 [部署基于 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。
- SQL Server 可以将实时数据库文件存储在横向扩展文件共享上。 有关详细信息，请参阅 [安装 SQL Server 并使用 SMB 文件共享作为存储选项](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option)。
- Virtual Machine Manager (VMM) 可以将库共享（其中包含虚拟机模板和相关文件）存储在横向扩展文件共享上。 但是，库服务器本身不能是横向扩展文件服务器，它必须位于独立的服务器或不使用横向扩展文件服务器群集角色的故障转移群集上。

如果将横向扩展文件共享用作库共享，则只能使用与横向扩展文件服务器兼容的技术。 例如，不能使用 DFS 复制来复制横向扩展文件共享上托管的库共享。 还有一点很重要，那就是横向扩展文件服务器安装了最新的软件更新。

若要将横向扩展文件共享用作库共享，首先添加一个具有本地共享或根本没有共享的库服务器（比如虚拟机）。 然后在添加库共享时，选择在横向扩展文件服务器上托管的文件共享。 此共享应由 VMM 管理，并且专门创建用于库服务器。 此外，请确保在横向扩展文件服务器上安装最新的更新。 有关添加 VMM 库服务器和库共享的详细信息，请参阅[将配置文件添加到 VMM 库](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801)。 有关文件和存储服务当前可用的修补程序的列表，请参阅 [Microsoft 知识库文章 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie)。

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
<td>故障转移群集在 Windows Server 2012 中添加了以下功能，以支持横向扩展文件服务器：分布式网络名称、横向扩展文件服务器资源类型、群集共享卷（CSV）2和横向扩展文件服务器高可用性角色。 有关这些功能的详细信息，请<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">参阅&#39;Windows Server 2012 中的故障转移群集中的新增功能 [重定向]</a>。</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">服务器消息块</a></td>
<td>SMB 3.0 在 Windows Server 2012 中添加了以下功能，以支持横向扩展文件服务器：SMB 透明故障转移、SMB 多通道和 SMB 直通。<br />
<br />
有关 Windows Server 2012 R2 中 SMB 的新功能和更改的功能的详细信息，请参阅<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">&#39;WINDOWS server 中 smb 的新增</a>功能。</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>详细信息

- [软件定义的存储设计注意事项指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [提高服务器、存储和网络可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署基于 SMB 的 Hyper-v](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [为服务器应用程序部署快速且高效的文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [要不要横向扩展，这才是问题所在](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) （博客文章）
- [文件夹重定向、脱机文件和漫游用户配置文件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)