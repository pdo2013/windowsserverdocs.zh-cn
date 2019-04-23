---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: 站点拓扑所有者角色
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 69443c2fc1af855c7df002e0ac91d43986eff6da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832108"
---
# <a name="site-topology-owner-role"></a>站点拓扑所有者角色

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

管理站点拓扑的管理员称为站点拓扑所有者。 站点拓扑所有者了解站点之间的网络条件并有权在 Active Directory 域服务 (AD DS) 来实现对站点拓扑的更改中更改设置。 站点拓扑更改会影响复制拓扑中的更改。 站点拓扑所有者的职责包括：  
  
-   如果网络连接发生更改，请控制对站点拓扑的更改。  
  
-   获取和维护网络连接和路由器从网络组有关的信息。 站点拓扑所有者必须维护子网地址、 子网掩码和每个所属于的位置的列表。 站点拓扑所有者还必须了解任何有关网络速度和产能影响站点拓扑，以有效地设置的站点链接成本的问题。  
  
-   移动 Active Directory 服务器对象，表示如果域控制器的 IP 地址更改为不同站点中不同的子网或子网本身分配到不同站点的站点之间的域控制器。 在任一情况下，站点拓扑所有者必须手动将域控制器的 Active Directory 服务器对象移到新站点。  
  


