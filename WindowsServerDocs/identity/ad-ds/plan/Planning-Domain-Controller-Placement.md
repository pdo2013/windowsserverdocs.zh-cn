---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: "计划域控制器的位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c6d9ca064699f95390e5b95863a6871ea2dc9d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="planning-domain-controller-placement"></a>计划域控制器的位置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

已收集的所有网络信息将用于设计网站拓扑、规划你想要放置域控制器，包括森林根域控制器区域域控制器后操作母版角色持有者和全球目录服务器。  
  
在 Windows Server 2008，你还可以充分利用只读域控制器 (Rodc)。 RODC 是一种新承载只读分区的 Active Directory 数据库域控制器。 帐户密码除外 RODC 保存的 Active Directory 对象和可写的域控制器拥有的属性。 但是，不能对存储在 RODC 数据库进行更改。 更改必须是可写的域控制器上进行并且再将其复制回 RODC。  
  
RODC 主要用于部署在远程或通常有相对较少的用户、差物理安全、中心网站和信息技术 (IT) 知识有限的人员的网络性能较差带宽的分支机构环境。 在提升的安全性和更高效访问网络资源部署 Rodc 结果。 有关 RODC 功能的详细信息，请参阅广告 DS: Read-Only 域控制器 ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616))。 有关如何部署 RODC 的信息，请参阅 Step-by-Step 指南 Read-Only 域控制器 ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728))。  
  
> [!NOTE]  
> 本指南无法解释你如何确定正确数域控制器和每个便会出现在各个站点的域的域控制器硬件要求。  
  
## <a name="in-this-section"></a>在此部分中  
  
-   [计划森林根域控制器的位置](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [计划区域域控制器位置](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [计划全球目录服务器位置](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [计划操作母版角色位置](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


