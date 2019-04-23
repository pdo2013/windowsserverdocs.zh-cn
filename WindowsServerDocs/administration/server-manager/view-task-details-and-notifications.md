---
title: 查看任务详细信息和通知
description: 服务器管理器
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 44fd23b917d08aad663c0d3fb8da4bebef47783e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831508"
---
# <a name="view-task-details-and-notifications"></a>查看任务详细信息和通知

>适用于：Windows Server 2016

在服务器管理器中在 Windows Server 2012 R2 或 Windows Server 2012 中执行管理任务，如添加角色和功能时，启动服务、 刷新在服务器管理器控制台中，显示的数据或创建自定义的一组服务器，通知显示在**通知**区域中的服务器管理器控制台标头。 通知，并**任务的详细信息**您可以从打开的对话框**通知**菜单上的，通过单击标志图标，显示用户任务或请求的状态，是否任务失败，并帮助你向您展示通过指向任务失败相关的详细的错误消息疑难解答。

## <a name="the-notifications-area"></a>通知区域
**通知**区域服务器管理器菜单栏中，标志图标标记，将显示在服务器管理器中启动的任务的结果。 通知会告知你开始在服务器管理器的任务是成功还是失败。 当你可以查看通知时，标志图标旁会显示可用的通知数。 如果任务失败、只能部分完成（例如，如果无法在所有想要执行任务的远程服务器上完成它）或完成但有警告，那么 **“通知”** 标志将变为红色。 显示通知的任务包括以下任务。

-   手动刷新显示在服务器管理器 （显示通知自动刷新只在失败时才） 的数据

-   启动或停止服务

-   安装或卸载角色、 角色服务和功能

-   在开始最佳做法分析器 (BPA) 扫描

-   添加要管理的远程服务器 （通知失败时将显示联系或刷新远程服务器显示的数据）

**“通知”** 菜单中的项目显示进度条、对任务的简单描述、任务目标服务器的名称（或者在选中多个目标服务器的情况下，其中一台目标服务器的名称）、到相关控件或对话框的链接（若适用）以及 **“任务”** 菜单。 **“任务”** 菜单显示了适用于活动通知（鼠标光标悬停于其上的通知）的命令。 例如，如果停止了服务，你可以在通知的 **“任务”** 菜单中单击 **“重新启动”** 以重启服务。

通知是用于安装或卸载角色、 角色服务和功能特别有用。 例如，如果远程服务器上启动功能安装，则可以关闭添加角色和功能向导时安装仍在进行，但活动任务仍保留在**通知**列表。 **通知**项目显示了安装过程中，一个进度栏，并允许您重新打开添加角色和功能向导中，如果需要通过单击**添加角色和功能向导**。 此列表中的项目通知你安装是否失败，或者是否需要额外的配置步骤来完成功能部署。

通知也发挥着重要作用中解决问题的任务或进程在服务器管理器。 详细了解如何使用中的消息**通知**区域并**任务的详细信息**对话框中，若要对不成功的任务或进程，请参阅[服务器管理器故障排除指南](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)。

若要删除一条通知，你不再想要查看从**通知**列表中，将鼠标光标悬停在通知，，然后单击**删除任务**(**X**)。

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>查看和故障排除任务，通过使用任务的详细信息
**任务的详细信息**底部的命令**通知**菜单打开**任务的详细信息**对话框中，它提供了任务事件 （从开始的完整描述，停止、 警告、 成功或失败）。 像其他列表控件在服务器管理器中，如**事件**，**服务**，并**最佳实践分析工具**磁贴，可以筛选和创建的任务上运行的查询的将显示在**任务的详细信息**对话框。 (有关筛选和创建列表控件查询的详细信息，请参阅[筛选器、 排序和查询服务器管理器磁贴中的数据](filter-sort-and-query-data-in-server-manager-tiles.md)。)在顶部窗格中，你可以查看显示在 **“通知”** 菜单中的通知，并查看对同一任务生成了多少通知。 顶部窗格中选择一个通知，底部窗格中显示有关通知的完整详细信息。

底部窗格对于解决失败的任务特别有用。 如果服务器管理器无法连接到或属于服务器池的服务器获取数据，此窗格中的条目常常包含详细的信息，包括基础 Windows 远程管理 (WinRM)、 网络或安全问题的完整文本的阻止服务器管理器与目标服务器进行通信。

## <a name="see-also"></a>请参阅
[筛选、 排序和查询服务器管理器磁贴中的数据](filter-sort-and-query-data-in-server-manager-tiles.md)
[服务器管理器故障排除指南](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
