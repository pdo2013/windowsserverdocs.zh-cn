---
title: 选择分支缓存设计
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4fe40b3d9ece771a46af8ecc70297b8713d65875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-branchcache-design"></a>选择分支缓存设计

>适用于：Windows Server（半年通道），Windows Server 2016

若要了解有关分支缓存模式并选择最佳的部署模式，你可以使用本主题。  
  
可以使用本指南中的以下模式和模式组合部署分支缓存。  
  
-   适用于分布式的缓存模式配置所有分支机构。  
  
-   所有分公司配置为托管的缓存模式，并且具有站点上托管的缓存服务器。  
  
-   某些分支机构配置分布式的缓存模式和一些分支机构具有站点上托管的缓存服务器，并且配置为托管的缓存模式。  
  
下图配置为分布式的缓存模式一个分支机构和配置为托管的缓存模式一个分支机构演示模式双安装。  
  
![选择分支缓存设计](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
部署分支缓存之前，请选择你喜欢的每个分支机构在你的组织的模式。  
  


