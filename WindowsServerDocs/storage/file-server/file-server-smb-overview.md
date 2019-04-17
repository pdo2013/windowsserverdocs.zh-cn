---
title: 在 Windows Server 中使用 SMB 3 协议文件共享的概述
description: 使用文件共享和文件提供与 Windows Server SMB 3 协议的概述。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233483"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>在 Windows Server 中使用 SMB 3 协议文件共享的概述

>适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主题介绍在 Windows Server® 2012年、 Windows Server 2012 R2 和 Windows Server 2016 SMB 3.0 功能 — 实用新使用最重要的功能或更新与早期版本和硬件相比此版本中的功能要求。

## <a name="feature-description"></a>功能说明

服务器消息块 (SMB) 协议是网络文件共享协议，让计算机上的应用程序可读取和写入文件以及从计算机网络中的服务器程序请求服务。 SMB 协议可在其 TCP/IP 协议或其他网络协议上使用。 使用 SMB 协议时，应用程序（或应用程序用户）可访问远程服务器上的文件或其他资源。 这让应用程序可以读取、创建和更新远程服务器上的文件。 它还可以与任何设置为接收 SMB 客户端请求的服务器程序通信。 Windows Server 2012 引入了新 3.0 版 SMB 协议。

## <a name="practical-applications"></a>实际应用程序

本节讨论一些新的实践方法为使用新 SMB 3.0 协议。

* **虚拟化 (通过 SMB Hyper-V™) 的文件存储**。 HYPER-V 可通过 SMB 3.0 协议的文件共享中存储虚拟机文件，如配置、 虚拟硬盘 (VHD) 文件和快照。 这可以用于独立的文件服务器和 HYPER-V 以及共享的文件存储用于群集的群集的文件服务器。
* **通过 SMB 的 Microsoft SQL Server**。 SQL Server 可存储 SMB 文件共享上的用户数据库文件。 目前，这与 SQL Server 2008 R2 的独立 SQL 服务器支持。 即将发布版本的 SQL Server 将添加群集的 SQL 服务器和系统数据库的支持。
* **为最终用户数据的传统存储**。 SMB 3.0 协议提供的信息工作者 （或客户端） 的增强功能工作负荷。 这些增强功能包括减少时广域网 (WAN) 上访问数据和防止数据窃听攻击分支机构用户体验的应用程序延迟。

## <a name="new-and-changed-functionality"></a>新增功能和更改的功能

有关 Windows Server 2012 R2 中的新的和更改功能的信息，请参阅[What's New in SMB 在 Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>)。

在 Windows Server 2012 和 Windows Server 2016 SMB 包含下表中的新 SMB 3.0 协议和描述了这些很多新改进。

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
<th><p>小结</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>SMB 透明故障转移</p></td>
<td><p>新增</p></td>
<td><p>使管理员可以执行的节点的硬件或软件维护群集的文件服务器在不中断存储这些文件共享上的数据的服务器应用程序的情况。 此外，如果硬件或软件故障发生群集节点上，SMB 客户端以透明方式重新连接到另一个群集节点不中断服务器应用程序数据存储到这些文件共享上的情况。</p></td>
</tr>
<tr class="even">
<td><p>SMB 扩展</p></td>
<td><p>新增</p></td>
<td><p>使用群集共享卷 (CSV) 版本 2，管理员可创建同时访问数据文件提供直接 I/O 使用，通过文件服务器群集中的所有节点的文件共享。 此提供更好地利用网络带宽和负载平衡的文件服务器客户端，并优化服务器应用程序的性能。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 多通道</p></td>
<td><p>新增</p></td>
<td><p>如果多个路径有 SMB 3.0 客户端和 SMB 3.0 服务器之间，则启用聚合的网络带宽和网络的容错能力。 这样充分利用所有可用网络带宽和为适应网络故障的服务器应用程序。</p></td>
</tr>
<tr class="even">
<td><p>SMB 直通</p></td>
<td><p>新增</p></td>
<td><p>支持网络适配器的具有 RDMA 功能，并且可以在运行完全速度非常低延迟、 使用非常少的 CPU 时的使用。 对于如 HYPER-V 或 Microsoft SQL Server 的工作负荷，这样将类似于本地存储的远程文件服务器。</p></td>
</tr>
<tr class="odd">
<td><p>服务器应用程序的性能计数器</p></td>
<td><p>新增</p></td>
<td><p>新 SMB 性能计数器提供详细，每个共享信息吞吐量、 延迟和 I/O 每秒 (IOPS) 允许管理员分析 SMB 3.0 文件共享存储其数据的性能。 这些计数器专门的服务器应用程序，如 HYPER-V 和 SQL Server 存储远程文件共享上的文件。</p></td>
</tr>
<tr class="even">
<td><p>性能优化</p></td>
<td><p>已更新</p></td>
<td><p>SMB 3.0 客户端和 SMB 3.0 服务器经过优化的小型随机读/写 I/O 使用，这是公共服务器应用程序，例如 SQL Server OLTP 中。 此外，大型最大传输单元 (MTU) 处于默认情况下，显著增强了大型顺序在传输中，如 SQL Server 数据仓库、 数据库备份或还原部署或复制虚拟硬盘性能。</p></td>
</tr>
<tr class="odd">
<td><p>特定于 SMB 的 Windows PowerShell cmdlet</p></td>
<td><p>新增</p></td>
<td><p>SMB 的 Windows PowerShell cmdlet，管理员可以从命令行管理的文件服务器上，端到端的文件共享。</p></td>
</tr>
<tr class="even">
<td><p>SMB 加密</p></td>
<td><p>新增</p></td>
<td><p>提供 SMB 数据的端到端加密，并防止窃听出现在不受信任的网络上的数据。 需要没有新的部署成本和 Internet 协议安全性 (IPsec)、 专用的硬件或 WAN 加速器不需要。 它可能配置根据每个共享，或为整个文件服务器，并可能启用的各种方案数据遍历网络不受信任的位置。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 目录租赁</p></td>
<td><p>新增</p></td>
<td><p>在分支机构提高应用程序响应时间。 由于更长时间的生活中检索元数据将从客户端对服务器的往返减少目录租约，使用目录高速缓存。 因为服务器上的目录信息发生更改时通知客户端维护缓存一致性。 方案<em>HomeFolder</em> （读/写与无共享） 和<em>出版物</em>（只读与共享） 的工作原理。</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>硬件要求

SMB 透明故障转移有以下要求：

* 运行 Windows Server 2012 或 Windows Server 2016 至少两个节点配置故障转移群集。 群集必须通过在验证向导中包含的群集验证测试。
* 文件共享必须创建具有连续可用性 (CA) 属性，默认值。
* 必须在 CSV 卷路径，即可获得 SMB 向外扩展创建文件共享。
* 客户端计算机必须运行 Windows® 8 或 Windows Server 2012，这两种包括支持连续可用性的更新的 SMB 客户端。

>[!NOTE]
>下层客户端可以连接到文件共享具有 CA 属性中，但不是将这些客户端支持透明故障转移。

SMB 多通道具有以下要求：

* 运行 Windows Server 2012 至少两台计算机是必需的。 没有额外的功能需要安装 — 技术位于默认情况下。
* 有关建议的网络配置信息，请参阅本概述主题末尾的请参阅一节。

SMB 直接具有以下要求：

* 运行 Windows Server 2012 至少两台计算机是必需的。 没有额外的功能需要安装 — 技术位于默认情况下。
* 网络适配器，与 RDMA 功能是必需的。 Currently, these adapters are available in three different types: iWARP, Infiniband, or RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>详细信息

以下列表提供 web 上关于 SMB 和 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的相关的技术的其他资源。

* [Windows Server 中的存储](../storage.md)
* [向外扩展文件服务器应用程序数据](../../failover-clustering/sofs-overview.md)
* [直接提高性能的 SMB 的文件服务器](smb-direct.md)
* [部署基于 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [部署 SMB 多通道](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [部署快速、 高效文件服务器供服务器应用程序](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB： 故障排除指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)