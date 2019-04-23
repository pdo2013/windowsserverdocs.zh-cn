---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: 将要求映射到 AD DS 部署策略
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a61e5e6d429acd92e48a353bc829cb620dd66054
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881828"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>将要求映射到 AD DS 部署策略

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

完成检查并确定 Active Directory 域服务 (AD DS) 设计和部署要求，确定其中哪些相关的特定部署后，可以将这些要求映射到特定的 AD DS 部署策略。  
  
使用下表来确定哪些 AD DS 部署策略映射到适当的 AD DS 设计和部署的组合为你的组织的要求。 （"是"表示特定的要求是您的部署策略; 的必要条件"否"表示特定的要求不是必需的部署策略。）  
  
此表仅指三个主要的 AD DS 部署策略在本指南中所述：  
  
-   [新的组织中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Windows Server 2003 组织中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Windows 2000 组织中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
但是，您可以使用 AD DS 设计和部署要求的任意组合以满足你的组织的需求创建混合或自定义 AD DS 部署策略。  
  
|AD DS 设计和部署要求|在新组织中部署 AD DS|在 Windows Server 2003 组织中部署 AD DS|在 Windows Server 2000 组织中部署 AD DS|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Windows Server 2008 AD ds 设计逻辑结构](https://technet.microsoft.com/library/cc770806.aspx)|是|是|是|  
|[Windows Server 2008 AD ds 设计站点拓扑](Designing-the-Site-Topology.md)|是|是|是|  
|规划域控制器容量|是|是|是|  
|[部署 Windows Server 2008 目录林根域](https://technet.microsoft.com/library/cc731174.aspx)|是|否|否|  
|[部署 Windows Server 2008 地区域](https://technet.microsoft.com/library/cc755118.aspx)|是|是|是|  
|[适用于 AD DS 启用高级的功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|是|是的但在环境中的所有域控制器必须运行 Windows Server 2008，然后将域或林功能级别设置为 Windows Server 2008。|是的但在环境中的所有域控制器必须运行 Windows Server 2008，然后将域或林功能级别设置为 Windows Server 2008。|  
|[将 Active Directory 域升级到 Windows Server 2008 和 Windows Server 2008 R2 AD DS 域](https://technet.microsoft.com/library/cc731188.aspx)|否|是|是|  
|[重组林之间的 AD DS 域](https://go.microsoft.com/fwlink/?LinkId=93678)|如果你想要将试验域迁移到生产环境，是的与其他组织合并和合并两个信息技术 (IT) 基础结构，或合并来自 Windows 就地升级的资源和帐户域2000 或 Windows Server 2003 环境。|是的如果你想要与另一个组织合并和合并两个 IT 基础结构或合并来自 Windows 2000 或 Windows Server 2003 环境就地升级的资源和帐户域。|是的如果你想要与另一个组织合并和合并两个 IT 基础结构或合并来自 Windows 2000 或 Windows Server 2003 环境就地升级的资源和帐户域。|  
|[重新构建 AD DS 域中林](https://go.microsoft.com/fwlink/?LinkId=82740))|否|是的如果需要减少域的数量，减少复制流量和所需的用户和组管理的数量或简化的组策略管理。|是的如果需要减少域的数量，减少复制流量和所需的用户和组管理的数量或简化的组策略管理。|  
  


