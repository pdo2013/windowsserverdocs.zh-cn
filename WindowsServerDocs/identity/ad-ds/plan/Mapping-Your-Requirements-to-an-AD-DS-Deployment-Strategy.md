---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: "将你的要求映射到广告 DS 部署策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b4f625f06fede5b9dc751282cda19b68d7f9e535
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>将你的要求映射到广告 DS 部署策略

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你完成查看并确定 Active Directory 域服务 (广告 DS) 设计和部署的要求，并且确定与特定的部署相关其中哪些后，你可以将这些要求映射到特定的广告 DS 部署策略。  
  
使用下表确定哪些广告 DS 部署策略映射到相应的广告 DS 和部署结合针对你的组织的要求。 （"是"意味着的特定要求是必需的部署策略;"无"意味着的特定要求不必要的部署策略）。  
  
此表仅指三个主要广告 DS 部署策略本指南中所述：  
  
-   [部署新的组织中的广告 DS](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [部署 Windows Server 2003 组织中的广告 DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [部署 Windows 2000 组织中的广告 DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
但是，你可以通过使用广告 DS 设计和部署要求的任意组合来了解你的组织的需求创建混合或自定义广告 DS 部署策略。  
  
|广告 DS 设计和部署要求|部署新的组织中的广告 DS|部署 Windows Server 2003 组织中的广告 DS|部署 Windows 2000 组织中的广告 DS|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[对于 Windows Server 2008 广告 DS 设计的逻辑结构](https://technet.microsoft.com/library/cc770806.aspx)|是的|是的|是的|  
|[对于 Windows Server 2008 广告 DS 设计站点拓扑](Designing-the-Site-Topology.md)|是的|是的|是的|  
|计划域控制器容量|是的|是的|是的|  
|[部署 Windows Server 2008 森林根域](https://technet.microsoft.com/library/cc731174.aspx)|是的|不|不|  
|[部署 Windows Server 2008 区域](https://technet.microsoft.com/library/cc755118.aspx)|是的|是的|是的|  
|[启用广告 DS 的高级的功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|是的|是，但在环境中的所有域控制器必须运行 Windows Server 2008 之前对 Windows Server 2008 设置的域中，还是森林功能级别。|是，但在环境中的所有域控制器必须运行 Windows Server 2008 之前对 Windows Server 2008 设置的域中，还是森林功能级别。|  
|[升级到 Windows Server 2008 和 Windows Server 2008 R2 广告 DS 域的 Active Directory 域](https://technet.microsoft.com/library/cc731188.aspx)|不|是的|是的|  
|[重新构造广告 DS 域林之间](https://go.microsoft.com/fwlink/?LinkId=93678)|是，如果你想要迁移到 production 环境先导域，合并与另一个组织和整合两个信息 (IT) 技术基础结构，或合并资源和帐户从 Windows 2000 或 Windows Server 2003 环境就地升级的域。|是，如果你想要合并与另一个组织和整合两个 IT 基础结构或合并资源和帐户从 Windows 2000 或 Windows Server 2003 环境就地升级的域。|是，如果你想要合并与另一个组织和整合两个 IT 基础结构或合并资源和帐户从 Windows 2000 或 Windows Server 2003 环境就地升级的域。|  
|[重新构造林内的广告 DS 域](https://go.microsoft.com/fwlink/?LinkId=82740))|不|是，如果你需要以减少域数，减少复制通信和量所需的用户和组管理或简化组策略管理。|是，如果你需要以减少域数，减少复制通信和量所需的用户和组管理或简化组策略管理。|  
  


