---
title: 运行 WSUS 副本模式
description: 'Windows Server Update Service （WSUS）主题-如何配置副本模式 '
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5323210962298ff3f2d0b159cba7726adfbb89d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361621"
---
# <a name="running-wsus-replica-mode"></a>运行 WSUS 副本模式

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在副本模式下运行的 WSUS 服务器继承在管理服务器上创建的更新审批和计算机组。 在使用副本模式的方案中，通常会有一台管理服务器，并且一个或多个从属副本 WSUS 服务器会根据站点或组织拓扑分布在整个组织中。 你批准更新并在管理服务器上创建计算机组，然后副本模式服务器将镜像该服务器。 副本模式服务器只能在 WSUS 安装过程中设置，如果你实施了此方案，则很可能是因为你的组织中的更新审批和计算机组的集中管理是很重要的。

如果 WSUS 服务器在副本模式下运行，则只能在服务器上执行有限的管理功能，这些功能主要包括：

-   添加和删除计算机组中的计算机。 计算机组成员身份不会分发到副本服务器，仅计算机组本身。 因此，在副本模式服务器上，将继承在管理服务器上创建的计算机组。 不过，计算机组将为空。 然后，你必须将连接到副本服务器的客户端计算机分配到计算机组。

-   设置同步计划

-   指定代理服务器设置

-   指定更新源。 这可以是管理服务器以外的服务器

-   查看可用的更新

-   监视服务器上的更新、同步、计算机状态和 WSUS 设置

-   运行副本模式服务器上可用的所有标准 WSUS 报表



