---
title: 使用 Windows Server 中的 SMB 3 协议的文件共享概述
description: 有关将 SMB 3 协议用于文件共享和使用 Windows Server 的文件服务的概述。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b40c179d242a0c48c6eb176db1225979f9e6a123
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402082"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>使用 Windows Server 中的 SMB 3 协议的文件共享概述

>适用于：Windows Server 2012 R2、Windows Server 2012、Windows Server 2016

本主题介绍 Windows Server®2012、Windows Server 2012 R2 和 Windows Server 2016 中的 SMB 3.0 功能，这是功能的实用用途，与以前的版本相比，此版本中最重要的新功能或更新功能以及硬件要求.

## <a name="feature-description"></a>功能说明

服务器消息块 (SMB) 协议是网络文件共享协议，让计算机上的应用程序可读取和写入文件以及从计算机网络中的服务器程序请求服务。 SMB 协议可在其 TCP/IP 协议或其他网络协议上使用。 使用 SMB 协议时，应用程序（或应用程序用户）可访问远程服务器上的文件或其他资源。 这让应用程序可以读取、创建和更新远程服务器上的文件。 它还可以与任何设置为接收 SMB 客户端请求的服务器程序通信。 Windows Server 2012 引入了新的3.0 版本的 SMB 协议。

## <a name="practical-applications"></a>实际应用程序

本节讨论一些使用新 SMB 3.0 协议的全新实用方法。

* **用于虚拟化的文件存储 (Hyper-V(TM) over SMB)** 。 Hyper-V 可以通过 SMB 3.0 协议在文件共享中存储虚拟机文件，如配置、虚拟硬盘 (VHD) 文件和快照。 这既可用于独立文件服务器，又可用于将 Hyper-V 与群集的共享文件存储配合使用的群集文件服务器。
* **Microsoft SQL Server over SMB**。 SQL Server 可以将用户数据库文件存储在 SMB 文件共享中。 目前，SQL Server 2008 R2 的独立 SQL 服务器支持此功能。 即将推出的 SQL Server 版本将增加群集 SQL 服务器和系统数据库的支持。
* **用于最终用户数据的传统存储**。 SMB 3.0 协议提供 信息工作者 （或客户端）工作负载的增强功能。 这些增强功能包括减少分支机构用户在通过广域网 (WAN) 访问数据时遇到的应用程序延迟，以及防止数据遭受窃听攻击。

## <a name="new-and-changed-functionality"></a>新增功能和更改的功能

有关 Windows Server 2012 R2 中新功能和更改的功能的信息，请参阅[Windows server 中 SMB 的新增](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>)功能。

Windows Server 2012 和 Windows Server 2016 中的 SMB 包括新的 SMB 3.0 协议和许多新改进，如下表中所述。

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>特性/功能</p></th>
<th><p>新功能或更新功能</p></th>
<th><p>总结</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>SMB 透明故障转移</p></td>
<td><p>新增</p></td>
<td><p>让管理员可执行群集文件服务器中节点的硬件或软件维护，且不会中断将数据存储在这些文件共享上的服务器应用程序。 此外，如果群集节点上发生硬件或软件故障，SMB 客户端将以透明方式重新连接到另一个群集节点，而不会中断将数据存储在这些文件共享上的服务器应用程序。</p></td>
</tr>
<tr class="even">
<td><p>SMB 横向扩展</p></td>
<td><p>新增</p></td>
<td><p>使用群集共享卷 (CSV) 版本 2 时，管理员可以通过文件服务器群集中的所有节点，创建可供同时访问含直接 I/O 的数据文件的文件共享。 这可更好地利用文件服务器客户端的网络带宽和负载平衡，以及优化服务器应用程序的性能。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 多通道</p></td>
<td><p>新增</p></td>
<td><p>如果在 SMB 3.0 客户端和 SMB 3.0 服务器之间提供多条路径，则支持网络带宽和网络容错的聚合。 这让服务器应用程序可以充分利用可用网络带宽并在发生网络故障时恢复。</p></td>
</tr>
<tr class="even">
<td><p>SMB 直通</p></td>
<td><p>新增</p></td>
<td><p>支持使用具有 RDMA 功能且可全速运行的网络适配器，其中延迟非常低且 CPU 非常少。 对于 Hyper-V 或 Microsoft SQL Server 等工作负载，这让远程文件服务器如同本地存储一样。</p></td>
</tr>
<tr class="odd">
<td><p>用于服务器应用程序的性能计数器</p></td>
<td><p>新增</p></td>
<td><p>全新 SMB 性能计数器提供有关吞吐量、延迟和 I/O/秒 (IOPS) 的按共享列出的详细信息，从而让管理员可以分析用于存储数据的 SMB 3.0 文件共享的性能。 这些计数器专为将文件存储在远程文件共享上的服务器应用程序而设计，如 Hyper-V 和 SQL Server。</p></td>
</tr>
<tr class="even">
<td><p>性能优化</p></td>
<td><p>已更新</p></td>
<td><p>SMB 3.0 客户端和 SMB 3.0 服务器均已针对小型随机读/写 I/O 优化，这种 I/O 在 SQL Server OLTP 等服务器应用程序中很常见。 此外，默认情况下打开大型最大传输单元 (MTU)，这将大幅提高大型连续传输性能，如 SQL Server 数据仓库、数据库备份或还原、部署或复制虚拟硬盘。</p></td>
</tr>
<tr class="odd">
<td><p>SMB-专用 Windows PowerShell cmdlet</p></td>
<td><p>新增</p></td>
<td><p>借助用于 SMB 的 Windows PowerShell cmdlet，管理员可以从命令行以端对端方式管理文件服务器上的文件共享。</p></td>
</tr>
<tr class="even">
<td><p>SMB 加密</p></td>
<td><p>新增</p></td>
<td><p>提供 SMB 数据的端对端加密并防止数据在未受信任网络中遭受窃听。 无需新部署成本，且无需 Internet 协议安全性 (IPsec)、专用硬件或 WAN 加速器。 它可按共享配置，也可针对整个文件服务器配置，并且可针对数据遍历未受信任网络的各种方案启用。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 目录租用</p></td>
<td><p>新增</p></td>
<td><p>缩短分支机构的应用程序响应时间。 使用目录租用后，缩短了从客户端到服务器的往返时间，因为是从保留时间较长的目录缓存中检索元数据。 缓存一致性得到保持，因为在服务器上的目录信息更改时将通知客户端。 适用于 <em>主文件夹</em> （读/写，无共享）和 <em>发布</em> （只读，带共享）。</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>硬件要求

SMB 透明故障转移具有以下要求：

* 运行 Windows Server 2012 或 Windows Server 2016 且至少配置了两个节点的故障转移群集。 群集必须通过验证向导中包括的群集验证测试。
* 必须使用连续可用性 (CA) 属性（默认值）创建文件共享。
* 必须在 CSV 卷路径上创建文件共享，才能获得 SMB 横向扩展。
* 客户端计算机必须运行 Windows®8或 Windows Server 2012，两者均包括支持连续可用性的更新 SMB 客户端。

>[!NOTE]
>下级客户端可以连接到具有 CA 属性的文件共享，但这些客户端不支持透明故障转移。

SMB 多通道具有以下要求：

* 至少需要两台运行 Windows Server 2012 的计算机。 无需安装额外功能 - 该技术默认情况下处于打开状态。
* 有关建议的网络配置的信息，请参阅本概述主题末尾的“另请参阅”部分。

SMB 直接具有以下要求：

* 至少需要两台运行 Windows Server 2012 的计算机。 无需安装额外功能 - 该技术默认情况下处于打开状态。
* 需要含 RDMA 功能的网络适配器。 目前，这些适配器有三种类型：iWARP、Infiniband 或 RoCE (RDMA over Converged Ethernet)。

## <a name="more-information"></a>详细信息

以下列表提供了有关 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 和相关技术的其他资源。

* [Windows Server 中的存储](../storage.md)
* [应用程序数据的横向扩展文件服务器](../../failover-clustering/sofs-overview.md)
* [使用 SMB 直通提高文件服务器的性能](smb-direct.md)
* [部署基于 SMB 的 Hyper-v](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [部署 SMB 多通道](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [为服务器应用程序部署快速且高效的文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB：疑难解答指南 @ no__t-0