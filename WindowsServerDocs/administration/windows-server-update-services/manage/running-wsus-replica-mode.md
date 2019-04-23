---
title: 运行 WSUS 副本模式
description: 'Windows Server Update Service (WSUS) 主题-如何配置副本模式 '
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3b4139354a3f0f7b1f1a97107d2f6b28db2b02c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878738"
---
# <a name="running-wsus-replica-mode"></a>运行 WSUS 副本模式

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在副本模式下运行的 WSUS 服务器继承更新批准和管理服务器上创建的计算机组。 在方案中，它使用副本模式、 但通常需要一台管理服务器，和一个或多个从属副本 WSUS 服务器分散在整个组织，根据站点或组织的位置图。 批准更新和管理服务器，然后将镜像副本模式服务器上创建计算机组。 仅在 WSUS 安装程序，可以设置副本模式服务器和如果实施这种情况下，它可能是因为必须更新批准在组织中，并集中管理计算机组。

如果你的 WSUS 服务器在副本模式下运行，你将能够将主要包含的服务器上执行仅有限的管理功能：

-   添加和删除计算机组中的计算机。 计算机组成员身份不分发到副本服务器，只有计算机组本身。 因此，在副本模式服务器上，将继承在管理服务器创建的计算机组。 但是，计算机组将为空。 您然后必须将客户端分配到副本服务器的计算机组到连接的计算机。

-   设置同步计划

-   指定代理服务器设置

-   指定更新源。 这可以是管理服务器之外的服务器

-   查看可用的更新

-   监视更新、 同步、 计算机状态和在服务器上的 WSUS 设置

-   副本模式服务器上运行所有标准 WSUS 报告可用



