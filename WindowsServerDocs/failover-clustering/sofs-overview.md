---
title: 向外扩展文件服务器应用程序数据概述 （英文）
description: Windows Server 201 R2、 Windows Server 2012 和 Windows Server 2016 的向外扩展文件服务器功能的概述。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081902"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>向外扩展文件服务器应用程序数据概述 （英文）

>适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

向外扩展文件服务器是一种功能，旨在提供持续可用于基于文件的服务器应用程序存储的向外扩展文件共享。 向外扩展文件共享提供共享同一文件夹中的同一群集的多个节点中的能力。 此方案重点介绍如何规划和部署扩展文件服务器。

您可以部署和配置群集的文件服务器使用以下方法之一：

- **向外扩展应用程序数据的文件服务器**Windows Server 2012 中引入了此群集的文件服务器功能和它让您存储服务器应用程序数据，如文件共享上的 HYPER-V 虚拟机文件并获取可靠性、 可用性、 可管理性和高级别类似您希望从存储区域网络的性能。 所有文件共享都的所有节点上同时联机。 此类型的群集的文件服务器相关联的文件共享称为向外扩展文件共享。 这有时称为活动-活动。 通过 SMB 服务器消息块 (SMB) 或 Microsoft SQL Server 上部署任一 HYPER-V 时，这是建议的文件服务器类型。
- **一般使用文件服务器**这是群集的文件服务器具有已中支持的 Windows Server 故障转移群集的简介以来的延续。 一次，此类型的群集的文件服务器，因此所有共享群集的文件服务器相关联处于联机状态的一个节点上。 这是有时称为主动-被动或双活动。 此类型的群集的文件服务器相关联的文件共享称为群集的文件共享。 部署信息工作人员方案时，这是建议的文件服务器类型。

## <a name="scenario-description"></a>方案说明

通过向外扩展文件共享，您可以共享群集中的多个节点的同一文件夹。 例如，如果您有四节点文件服务器群集正在使用服务器消息块 (SMB) 向外扩展，运行 Windows Server 2012 R2 或 Windows Server 2012 的计算机可以从任何四个节点访问文件共享。 这是通过利用新 Windows Server 故障转移群集的特性和功能的 Windows 文件服务器协议，实现 SMB 3.0。 文件服务器管理员可以提供向外扩展文件共享和连续可用文件服务的服务器应用程序，并通过只需将多个服务器联机，从而快速响应增加的要求。 所有这些可以在生产环境中，完成，是完全透明的服务器应用程序。

向外扩展中的文件服务器所提供的主要优点包括：

- **活动-活动文件共享**。 所有群集节点可以接受并为 SMB 客户端请求提供服务。 通过使同时共享内容可通过所有群集节点访问的文件，SMB 3.0 群集和客户端合作透明故障转移到其他群集节点计划内的维护和计划外的故障期间提供服务中断。
- **提高了带宽**。 最大共享带宽是所有文件服务器群集节点的总带宽。 与早期版本的 Windows Server，不同的总带宽不再受限于一个群集节点; 的带宽但是，而不显示，备份存储系统的功能定义的约束。 您可以通过添加节点增加的总带宽。
- **与零停机时间的 CHKDSK**。 Windows Server 2012 中的 CHKDSK 显著增强显著缩短文件系统是脱机的修复的时间。 簇状共享的卷 (Csv) 通过消除脱机阶段需要进一步此一个步骤。 CSV 文件系统 (CSVFS) 可以使用 CHKDSK，而不会影响使用文件系统上打开的句柄应用程序。
- **群集共享卷高速缓存**。 Windows Server 2012 中的 Csv 引入了读取缓存，这样可以显著提高性能在某些情况下，如虚拟桌面基础结构 (VDI) 的支持。
- **简化管理**。 通过向外扩展文件服务器，您创建的向外扩展文件服务器，并添加必要 Csv 和文件共享。 不再需要创建多个群集的文件服务器，每个单独群集磁盘，然后开发位置策略，以确保每个群集节点上的活动。
- **自动重新平衡的扩展文件服务器客户端**。 在 Windows Server 2012 R2 中，自动重新平衡提高可伸缩性和可管理性的向外扩展文件服务器。 SMB 客户端连接跟踪每文件共享 (而不是每台服务器)，并且客户端然后重定向到群集节点对卷使用文件共享的最佳访问权限。 这将通过减少重定向文件服务器节点之间的流量提高效率。 客户端将重定向关注的初始连接和系统群集存储将重新配置。

## <a name="in-this-scenario"></a>在此方案

可帮助您部署的向外扩展文件服务器相关的以下主题：

- [规划向外扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [步骤 1： 规划存储在向外扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [在向外扩展文件服务器的网络规划步骤 2:](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [向外扩展文件服务器部署](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [步骤 1： 安装必备组件向外扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [步骤 2： 配置文件向外扩展服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [步骤 3： 配置 HYPER-V 用于向外扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [步骤 4： 配置 Microsoft SQL Server 使用向外扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>何时使用向外扩展文件服务器

您不应使用向外扩展文件服务器如果您的工作负载生成大量的元数据操作，如打开文件，关闭文件、 创建新文件，或重命名现有文件。 典型的信息工作人员会产生大量的元数据操作。 如果您感兴趣的可扩展性和它会提供的简单性和仅需要与扩展文件服务器支持的技术，您应使用的向外扩展文件服务器。

下表列出了 SMB 3.0、 常见 Windows 文件系统、 文件服务器数据管理技术和常见的工作负荷中的功能。 您可以看到这种技术支持向外扩展文件服务器，还是需要传统群集的文件服务器 （也称为常规使用的文件服务器）。

<table>
<thead>
<tr class="header">
<th>技术领域</th>
<th>功能</th>
<th>常规使用文件服务器群集</th>
<th>向外扩展文件服务器</th>
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
<td>是 （如果已启用连续可用性）</td>
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
<td>复原文件系统 (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>建议使用存储空格直接</td>
<td>建议使用存储空格直接</td>
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
<td>重复数据消除 (Windows Server 2012)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>重复数据消除 (Windows Server 2012 R2)</td>
<td>是</td>
<td>是 (仅适用于 VDI)</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>DFS Namespace (DFSN) 根服务器根</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>DFS Namespace (DFSN) 文件夹目标服务器</td>
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
<td>文件服务器资源管理器 （屏幕和配额）</td>
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
<td>动态访问控制 （基于声明的访问权限限定）</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>文件夹重定向</td>
<td>是</td>
<td>不建议使用 *</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>脱机文件 （客户端缓存）</td>
<td>是</td>
<td>不建议使用 *</td>
</tr>
<tr class="even">
<td>文件管理</td>
<td>漫游用户配置文件</td>
<td>是</td>
<td>不建议使用 *</td>
</tr>
<tr class="odd">
<td>文件管理</td>
<td>主目录</td>
<td>是</td>
<td>不建议使用 *</td>
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

\ * 文件夹重定向、 脱机、 漫游用户配置文件中，或主目录生成大量必须立即写入磁盘 （不带缓冲） 的写入时使用连续可用的文件共享，减少与性能通用文件共享。 连续可用的文件共享也是与文件服务器资源管理器和运行 Windows XP 的 Pc 不兼容。 此外，脱机文件可能不会转换为脱机模式 3-6 分钟后用户将失去对共享，无法失望不尚未使用的脱机文件始终脱机模式下的用户的访问。

## <a name="practical-applications"></a>实际应用程序

向外扩展文件服务器非常适合于服务器应用程序存储。 下面列出了可以向外扩展文件共享上存储其数据的服务器应用程序的一些示例：

- Internet Information Services (IIS) Web 服务器可存储配置和数据网站向外扩展文件共享上。 有关详细信息，请参阅[共享配置](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264)。
- HYPER-V 可以向外扩展文件共享上存储的配置和实时虚拟磁盘。 有关详细信息，请参阅[部署 HYPER-V 通过 SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。
- SQL Server 可存储向外扩展文件共享上的活动数据库文件。 有关详细信息，请参阅[安装 SQL Server SMB 文件共享作为存储选项](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option)。
- Virtual Machine Manager (VMM) 可以向外扩展文件共享上存储库共享 （其中包含虚拟机模板和相关的文件）。 但是，库服务器本身不能向外扩展文件服务器 — 它必须是独立服务器或不使用向外扩展文件服务器群集角色的故障转移群集上。

如果您使用的向外扩展文件共享作为库共享，可以使用与扩展文件服务器兼容的技术。 例如，不能使用 DFS 复制复制库共享向外扩展文件共享上承载。 也很重要的向外扩展文件服务器具有最新的软件更新安装。

若要向外扩展文件共享的库共享用作，首先添加库服务器 （可能虚拟机） 与本地共享或无共享根本。 然后时添加库共享，请选择向外扩展文件服务器承载的文件共享。 VMM 管理和创建用于以独占方式通过库服务器，应为此共享。 此外请务必在向外扩展文件服务器上安装最新的更新。 有关添加 VMM 库服务器和库共享的详细信息，请参阅[VMM 库添加配置文件](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801)。 当前可用的修补程序文件和存储服务的列表，请参阅[Microsoft 知识库文章 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie)。

>[!NOTE]
>一些用户，例如信息工作者具有对性能产生影响最大的工作负荷。 例如，操作，如打开和关闭文件，创建新的文件和重命名现有文件，供多个用户执行时对性能产生影响。 如果具有连续可用性启用文件共享，则它提供数据完整性，但它还会影响的总体性能。 连续可用性要求将通过的数据写入磁盘以确保发生在向外扩展文件服务器群集节点故障时的完整性。 因此，用户的几个大型文件复制到文件服务器可期待连续可用的文件共享上的性能会显著降低。

## <a name="features-included-in-this-scenario"></a>此方案中包括的功能

下表列出了此方案的一部分的功能，并介绍如何支持。

<table>
<thead>
<tr class="header">
<th>功能</th>
<th>支持此方案的方式</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">故障转移群集</a></td>
<td>故障转移群集 Windows Server 2012 支持向外扩展文件服务器中添加以下功能： 分布式网络名称、 向外扩展文件服务器资源类型、 群集共享卷 (CSV) 2 和向外扩展文件服务器高可用性角色。 有关这些功能的详细信息，请参阅<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">What's New in [重定向] 的 Windows Server 2012 中的故障转移群集</a>。</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">服务器消息块</a></td>
<td>SMB 3.0 添加以支持的 Windows Server 2012 中的以下功能向外扩展文件服务器： SMB 透明故障转移、 SMB 多通道和 SMB 直接。<br />
<br />
SMB Windows Server 2012 R2 中新的和更改功能的更多信息，请参阅<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What's New in SMB 在 Windows Server</a>。</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>详细信息

- [Software Defined 存储设计注意事项指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [增加服务器、 存储和网络可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署基于 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [部署快速、 高效文件服务器供服务器应用程序](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [要向外扩展或不适用于扩展，这是问题](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)（博客文章）
- [文件夹重定向、脱机文件和漫游用户配置文件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)