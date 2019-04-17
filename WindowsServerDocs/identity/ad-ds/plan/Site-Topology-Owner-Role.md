---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: "网站拓扑负责人的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7c98d313cac28ab07380791a384a87bffacfbda2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="site-topology-owner-role"></a>网站拓扑负责人的角色

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

管理站点拓扑的管理员称为网站拓扑所有者。 网站拓扑所有者理解的条件的站点之间的网络，并且具有机构更改 Active Directory 域服务 (广告 DS) 来实现站点拓扑更改的设置。 更改站点拓扑影响复制拓扑中的更改。 网站拓扑所有者责任包括：  
  
-   如果网络连接更改，控制站点拓扑更改。  
  
-   获取和维护网络连接和路由器网络组的信息。 网站拓扑所有者必须维护子网地址、 子网掩码和对每个所属的位置的列表。 网站拓扑所有者还必须了解网络速度和容量有关的任何问题影响的站点拓扑有效地设置成本网站的链接。  
  
-   移动到其他网站，在不同的子网更改域控制器的 IP 地址是否子网本身分配给其他站点表示的站点之间的域控制器的 Active Directory 服务器对象。 不论采取何种情况，网站拓扑所有者必须手动将域控制器 Active Directory 服务器对象移动到新的站点。  
  


