---
title: 选择一个 BranchCache 设计
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 330dcbee26f52ff69cd85ef8dc78d2e161b943d1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811908"
---
# <a name="choosing-a-branchcache-design"></a>选择一个 BranchCache 设计

>适用于：Windows 服务器 （半年频道），Windows Server 2016

若要了解有关 BranchCache 模式并选择你的部署的最佳模式，你可以使用本主题。  
  
可以使用本指南中的以下模式和组合模式下部署 BranchCache。  
  
-   所有分支机构都配置为使用分布式的缓存模式。  
  
-   所有分支机构配置为托管的缓存模式，并且站点上有一个托管的缓存服务器。  
  
-   某些分支办事处配置为使用分布式的缓存模式和某些分支机构站点上有一个托管的缓存服务器，并且配置为使用托管的缓存模式。  
  
下图演示双模式安装中，使用配置为使用分布式的缓存模式的一个分支机构和一个分支机构配置为托管的缓存模式。  
  
![选择一种 BranchCache 设计](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
部署 BranchCache 之前，请选择您喜欢的每个分支机构在组织中的模式。  
  


