---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: 规划域控制器放置
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 68406d4973dd585bf0a98562c987c1b1512c095c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883358"
---
# <a name="planning-domain-controller-placement"></a>规划域控制器放置

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

已收集的所有网络信息将用于设计站点拓扑后，计划你想要将域控制器，包括目录林根级域控制器、 区域的域控制器、 操作主机角色拥有者，和全局编录服务器。  
  
在 Windows Server 2008 中，您还可以充分利用只读域控制器 (Rodc)。 RODC 是一种新型域控制器承载 Active Directory 数据库的只读分区。 除帐户密码外 RODC 保存所有 Active Directory 对象和可写域控制器保留的属性。 但是，不能对存储在 RODC 的数据库进行更改。 必须在可写域控制器上所做更改，并将其再将其复制回 RODC。  
  
RODC 主要设计用于部署在远程或分支机构环境，通常具有相对较少的用户，物理安全性差，到中心站点，并且几乎不知道的信息的人员的相对较差的网络带宽技术 (IT)。 中改进的安全性和效率更高访问网络资源部署 Rodc 的结果。 有关 RODC 功能的详细信息，请参阅 AD DS:只读域控制器 ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616))。 有关如何部署 RODC 的信息，请参阅只读域控制器循序渐进指南 ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728))。  
  
> [!NOTE]  
> 本指南不介绍如何确定域控制器和在每个站点中的呈现每个域的域控制器硬件要求的正确号码。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [规划林根域控制器放置](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [规划区域性域控制器放置](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [规划全局编录服务器的位置](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [规划操作主机角色放置](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


