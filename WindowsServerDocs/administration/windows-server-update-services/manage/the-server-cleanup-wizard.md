---
title: 服务器清理向导
description: Windows Server Update Service （WSUS）主题-如何使用服务器清理向导来管理磁盘空间
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e285e59a27b6bf0ef1bf3b1ab0f78a96efa60c87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361535"
---
# <a name="the-server-cleanup-wizard"></a>服务器清理向导

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

"服务器清理向导" 已集成到用户界面，可用于帮助你管理磁盘空间。 此向导可执行以下操作：

- 删除未使用的更新和更新修订将删除所有较旧的更新和尚未批准的更新修订版本。

- 删除未与服务器联系的计算机删除在三十天内未与服务器联系的所有客户端计算机。

- 删除不需要的更新文件删除更新或下游服务器不需要的所有更新文件。

- 拒绝过期的更新会拒绝 Microsoft 已过期的所有更新。

- 拒绝被取代的更新将拒绝满足以下所有条件的所有更新：

  -   被取代的更新不是必需的

  -   被取代的更新已在服务器上30天或以上

  -   任何客户端当前未报告被取代的更新

  -   被取代的更新已在90天或更多的时间内显式部署到计算机组

  -   必须批准此取代更新才能安装到计算机组

  > [!WARNING]
  >  在 WSUS 层次结构中，强烈建议先在最靠下的下游/副本 WSUS 服务器上运行清理进程，然后再向上移动层次结构。 在每个下游服务器上运行清理之前，在任何上游服务器上错误运行清理可能导致上游数据库和下游数据库中存在的数据不匹配。 数据不匹配可能导致上游和下游服务器之间的同步失败。 
  > 
  > [!IMPORTANT]
  >  如果使用 "服务器清理向导" 删除不必要的内容，则还会删除从 Microsoft 更新目录站点下载的所有专用更新文件。 运行 "服务器清理向导" 之后，必须重新导入这些文件。 

如果使用自动批准规则批准更新，则这些更新可能仍处于 "已批准" 状态，并且不会被服务器清理向导删除。 若要删除处于 "已批准" 状态的自动批准的更新，WSUS 管理员至少必须将 "被取代的更新" 的审批状态手动设置为 "未批准"，以便它们可由服务器清理向导赤纬。 "服务器清理向导" 将确保更新更新，并确保在将更新标记为 "已拒绝" 之前，不会根据需要向客户端系统报告该更新。




