---
title: 服务器清理向导
description: Windows Server Update Service (WSUS) 主题-如何使用服务器清理向导来管理磁盘空间
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da87db95303ba7254e9f1f9dbc7219423e9e6b72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884068"
---
# <a name="the-server-cleanup-wizard"></a>服务器清理向导

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

服务器清理向导已集成到用户界面，可用于帮助你管理你的磁盘空间。 此向导可以执行以下操作：

-   删除未使用的更新和更新修订中移除所有较旧的更新和未批准的更新修订。

-   删除不联系服务器删除的计算机未与服务器联系超过 30 天的所有客户端计算机。

-   删除不需要的更新文件删除所有通过更新或下游服务器更新不需要的文件。

-   拒绝过期的更新拒绝已由 Microsoft 已过期的所有更新。

-   拒绝被取代更新拒绝满足以下所有条件的所有更新：

    -   被取代的更新不是强制性

    -   30 天或更多被取代的更新已在服务器上

    -   被取代的更新未当前报告为所需的任何客户端

    -   被取代的更新尚未显式部署到计算机组 90 天或更多

    -   必须为安装到计算机组批准取代更新

    >  [!WARNING]  
    >  在 WSUS 层次结构中，强烈建议首先，在最，较低的下游/副本 WSUS 服务器上运行清除进程，然后在层次结构向上移动。 未正确在每个下游服务器上运行清理之前任何上游服务器上运行清理可能会导致在上游数据库中存在的数据与下游数据库之间不匹配。 数据不匹配可能会导致同步失败上游和下游服务器之间。 

 >  [!IMPORTANT]  
    >  如果使用服务器清理向导删除不需要的内容，也会删除已从 Microsoft Update 目录站点下载的所有专用更新文件。 在运行服务器清理向导后，必须重新这些文件导入。 

更新的批准使用自动批准规则，它们仍可能处于"已批准"状态，并将不会删除由服务器清理向导。 若要删除处于"已批准"状态的自动批准更新，WSUS 管理员必须-最小值-手动设置为"未批准"被取代的更新的批准状态以便它们能够获取赤纬，由服务器清理向导。 服务器清理向导将确保较新的更新批准和任何客户端系统仍然报告，根据需要更新，才能将标记为"已拒绝。"的更新




