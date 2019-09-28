---
title: 查看任务详细信息和通知
description: 服务器管理器
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95117407-2dd3-4f9a-841f-4331be3544c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3dcbac95e60fce75316f8a4427aef54bdfad15b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383018"
---
# <a name="view-task-details-and-notifications"></a>查看任务详细信息和通知

>适用于：Windows Server 2016

在 Windows Server 2012 R2 或 Windows Server 2012 的服务器管理器中，当你执行管理任务（如添加角色和功能、启动服务、刷新在服务器管理器控制台中显示的数据或创建自定义服务器组）时，通知会显示在服务器管理器控制台标头的 "**通知**" 区域中。 通知，以及可通过单击标志图标从 "**通知**" 菜单打开的 "**任务详细信息**" 对话框，显示用户任务或请求的状态，显示任务是否失败，并通过指向有关任务失败的详细错误消息。

## <a name="the-notifications-area"></a>通知区域
服务器管理器菜单栏中的 "**通知**" 区域（由标志图标标记）显示您在服务器管理器中启动的任务结果。 通知会告知你在服务器管理器中启动的任务是成功还是失败。 当你可以查看通知时，标志图标旁会显示可用的通知数。 如果任务失败、只能部分完成（例如，如果无法在所有想要执行任务的远程服务器上完成它）或完成但有警告，那么 **“通知”** 标志将变为红色。 显示通知的任务包括以下任务。

-   手动刷新服务器管理器中显示的数据（只有在刷新失败时，才会显示通知自动刷新）

-   启动或停止服务

-   安装或卸载角色、角色服务和功能

-   启动最佳做法分析器（BPA）扫描

-   添加要管理的远程服务器（对于无法联系或刷新远程服务器显示的数据，将显示通知）

**“通知”** 菜单中的项目显示进度条、对任务的简单描述、任务目标服务器的名称（或者在选中多个目标服务器的情况下，其中一台目标服务器的名称）、到相关控件或对话框的链接（若适用）以及 **“任务”** 菜单。 **“任务”** 菜单显示了适用于活动通知（鼠标光标悬停于其上的通知）的命令。 例如，如果停止了服务，你可以在通知的 **“任务”** 菜单中单击 **“重新启动”** 以重启服务。

通知在安装或卸载角色、角色服务和功能时特别有用。 例如，如果在远程服务器上启动功能安装，可以在安装仍在进行中时关闭 "添加角色和功能向导"，但活动任务仍保留在 "**通知**" 列表中。 "**通知**" 项目显示了安装的进度条，并通过单击 "**添加角色和功能向导**" 来重新打开 "添加角色和功能向导" （如果需要）。 此列表中的项目通知你安装是否失败，或者是否需要额外的配置步骤来完成功能部署。

通知还在解决与服务器管理器中的任务或进程有关的问题方面起着重要的作用。 有关如何使用 "**通知**" 区域和 "**任务详细信息**" 对话框中的消息对不成功的任务或进程进行疑难解答的详细信息，请参阅[服务器管理器故障排除指南](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)。

若要删除不再想从 "**通知**" 列表中看到的通知，请将鼠标光标悬停在通知上，然后单击 "**删除任务**" （**X**）。

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>使用任务详细信息查看和排查任务
"**通知**" 菜单底部的 "**任务详细信息**" 命令会打开 "**任务详细信息**" 对话框，其中提供了任务事件的完整描述（启动、停止、警告、成功或失败）。 与服务器管理器中的其他列表控件，如 "**事件**"、"**服务**" 和 "**最佳做法分析器**" 磁贴一样，你可以筛选并创建在 "**任务详细信息**" 对话框中显示的任务上运行的查询。 （有关筛选和创建列表控件查询的详细信息，请参阅[筛选、排序和查询服务器管理器磁贴中的数据](filter-sort-and-query-data-in-server-manager-tiles.md)。）在顶部窗格中，你可以查看显示在 **“通知”** 菜单中的通知，并查看对同一任务生成了多少通知。 在顶部窗格中选择一个通知将在底部窗格中显示有关通知的完整详细信息。

底部窗格对于解决失败的任务特别有用。 如果服务器管理器无法连接到作为服务器池成员的服务器或获取数据，此窗格中的条目通常会包含详细的消息，包括基本 Windows 远程管理（WinRM）、网络或安全问题的完整文本阻止服务器管理器与目标服务器进行通信。

## <a name="see-also"></a>请参阅
[筛选、排序和查询服务器管理器磁贴中的数据](filter-sort-and-query-data-in-server-manager-tiles.md)
[服务器管理器故障排除指南](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
