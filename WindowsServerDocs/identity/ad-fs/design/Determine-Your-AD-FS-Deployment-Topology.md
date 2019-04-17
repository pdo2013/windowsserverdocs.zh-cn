---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: "确定您的广告 FS 部署拓扑"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="determine-your-ad-fs-deployment-topology"></a>确定您的广告 FS 部署拓扑

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在规划的 Active Directory 联合身份验证服务 \(AD FS\) 部署的第一步是组织的以确定右部署拓扑以满足单个 sign\ 上 \(SSO\) 需要你。 在此部分中的主题介绍可用于广告 FS 各种部署拓扑。 他们还描述好处和限制，以便你可以选择最适合拓扑针对特定的企业需要与每个部署拓扑相关联。  
  
阅读本部署拓扑主题之前，我们建议你首次完成的任务，如下表中所显示的顺序。  
  
|推荐的任务|描述|参考|  
|--------------------|---------------|-------------|  
|查看如何存储和复制到其他联盟服务器联合身份验证的服务器场中广告 FS 数据。|了解的用途以及可以用于存储在数据库中广告 FS 配置的基本数据复制方法。 本主题介绍的概念配置数据库并介绍了两个数据库类型：Windows 内部数据库 \(WID\) 和 Microsoft SQL Server。|[广告 FS 配置数据库的作用](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|选择你将在你的组织中部署的广告 FS 配置数据库类型。|查看的各种优缺点与广告 FS 配置数据库中，以及他们所支持的各种应用程序方案为使用 WID 或 SQL Server 相关联。|[广告 FS 部署拓扑注意事项](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> 若要实现基本冗余、负载平衡和缩放联合身份验证服务 \(if required\) 的选项，我们建议你部署至少两个联盟服务器每联合身份验证的服务器场所有生产环境，无论哪种类型的数据库，您将使用。  
  
当你检查上一个表格中的内容后时，请继续执行此部分中的以下主题：  
  
-   [使用 WID 独立联盟服务器](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [使用 WID 联合身份验证的服务器场](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [联合 Server 场使用 WID 和代理服务器](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [联合 Server 场使用 SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
选择您的广告 FS 部署拓扑完后，我们建议您审查主题[规划广告 FS 服务器容量](Planning-for-AD-FS-Server-Capacity.md)以确定建议你将需要部署支持此拓扑的服务器的数量。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

