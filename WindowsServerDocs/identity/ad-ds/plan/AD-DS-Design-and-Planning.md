---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: AD DS 设计和规划
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5267561e4a3d19514d9105f21122db73f85bffb9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409029"
---
# <a name="ad-ds-design-and-planning"></a>AD DS 设计和规划

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

通过在你的环境中部署 Windows Server Active Directory 域服务（AD DS），你可以利用 AD DS 提供的集中委派管理模型和单一登录（SSO）功能。 确定组织的部署任务和当前环境后，可以创建满足组织需求的 AD DS 部署策略。  
  
## <a name="about-this-guide"></a>关于本指南

本指南提供了一些建议，以帮助你基于你的组织的需求和你想要创建的特定设计来开发 AD DS 部署策略。 本指南旨在供基础结构专家或系统架构师使用。 阅读本指南之前，应充分了解 AD DS 在功能级别上的工作方式。 你还应充分了解将反映在你的 AD DS 部署策略中的组织要求。  
  
本指南介绍了 Windows Server 2008 AD DS 部署的几个可能的起点的任务集。 本指南可帮助你确定最适合你的环境的部署策略。  
  
尽管本指南中所述的策略适用于几乎所有服务器操作系统部署，但它们已专门针对包含少于100000个用户和少于1000个站点的环境进行了测试和验证，并提供了最小每秒28.8 千比特（Kbps）的网络连接。 如果你的环境不满足这些条件，请考虑使用在更复杂的环境中部署 AD DS 的咨询公司。  
  
有关测试 AD DS 部署过程的详细信息，请参阅[测试和验证部署过程](https://go.microsoft.com/fwlink/?LinkId=100206)一文。  
  
## <a name="in-this-guide"></a>本指南包含的内容

[了解 AD DS 设计](Understanding-AD-DS-Design.md)  
  
[识别 AD DS 设计和部署要求](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)  
  
[将要求映射到 AD DS 部署策略](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)  
  
[设计 Windows Server 2008 的逻辑结构 AD DS](Designing-the-Logical-Structure.md)  
  
[为 Windows Server 2008 设计站点拓扑 AD DS](Designing-the-Site-Topology.md)  
  
[为 AD DS 启用高级功能](Enabling-Advanced-Features-for-AD-DS.md)  
  
[评估 AD DS 部署策略示例](Evaluating-AD-DS-Deployment-Strategy-Examples.md)  
  
[附录 A：查看关键的 AD DS 条款](Appendix-A--Reviewing-Key-AD-DS-Terms.md)  
