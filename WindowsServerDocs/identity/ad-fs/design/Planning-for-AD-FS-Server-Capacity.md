---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: "规划广告 FS 服务器容量"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-ad-fs-server-capacity"></a>规划广告 FS 服务器容量

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

  
> [!NOTE]  
> 在本主题提供的内容不会反映实际测试服务器运行 Windows Server 2012 上所执行。 执行了所需的测试后，将更新此主题。  
  
容量 Active Directory 联合身份验证服务规划 \(AD FS\) 预测峰值使用时段的联合身份验证服务并规划的过程，或者 scaling\ 向上广告 FS 服务器部署以满足这些加载要求。  
  
此部分中介绍了联合身份验证的服务器和联合身份验证的服务器代理角色部署指南和所采用的实验测试所执行的广告 FS 产品团队在 Microsoft。 此内容旨在帮助你：  
  
-   密切估计的硬件需要针对你的组织的特定广告 FS 部署，如广告 FS 服务器数。  
  
-   准确投影 sign\ 输入请求，增长，套餐的预期的峰值使用，并确保广告 FS 部署预期峰值使用的处理的支持。  
  
在继续进行阅读此容量规划内容之前，我们建议你首次完成的任务以下两个表格中显示的顺序。 第一个表格，我们提供了指向推荐有助于的任务提供相关的上下文此容量规划讨论。  
  
|推荐的任务|描述|参考|  
|--------------------|---------------|-------------|  
|了解用于部署广告 FS 联盟服务器和联合身份验证的服务器代理要求|检查重要的硬件和软件要求部署联合身份验证的服务器和联合身份验证的服务器代理所需的。|[附录 a：检查广告 FS 要求](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|选择你将在你的组织中部署的广告 FS 配置数据库类型|你可以在此部分中使用容量计划数据开始之前，你首先需要确定广告 FS 配置数据库类型将部署 Windows 内部数据库 \(WID\) 或结构化查询语言 \(SQL\) 数据库。|[广告 FS 配置数据库中的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[广告 FS 部署拓扑注意事项](AD-FS-Deployment-Topology-Considerations.md)|  
|确定拓扑布局，若要使用新的广告 FS 配置数据库选择类型|后，你已决定在的广告 FS 配置数据库类型部署中使用，你将需要考虑哪种部署拓扑最匹配在你将需要位置联合身份验证的服务器和联合身份验证的代理服务器生产环境中。|[确定您的广告 FS 部署拓扑](Determine-Your-AD-FS-Deployment-Topology.md)|  
|了解的重要广告相关 FS – 容量规划条款|查看的定义的常见容量规划用于广告 FS 容量计划讨论整个的条款。|请参见一节[广告 FS 容量规划条款](Planning-for-AD-FS-Server-Capacity.md#bk_terms)本主题中|  
  
一旦检查上一个表格中的内容后，你现在可以完成先决条件下表中的任务。  
  
|先决条件任务|描述|参考|  
|---------------------|---------------|-------------|  
|下载广告 FS 容量计划大小电子表格|广告 FS 容量规划大小电子表格可以帮助你确定联盟所需的服务器广告 FS 联盟服务器电场的日落部署数。 有关如何使用此电子表格说明都可在下面的下一个任务提供的链接。|[广告 FS 容量规划电子表格](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|收集有关将需要一个 sign\ 上的 \(SSO\) 访问目标 claims\ 感知应用程序和相关此访问预期的峰值使用时段的用户的数量的数据|你将收集此用户数据将用于输入所需的广告 FS 容量规划大小电子表格上下文中的值。|[估计数联合针对你的组织的服务器](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|广告 FS 容量规划电子表格以进行 Windows Server 2016|对于 Windows Server 2016 的更新的规划工作表|[广告 FS Windows Server 2016 容量计划](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>广告 FS 容量规划条款  
下表介绍了本容量规划部分广告 FS 设计指南中经常使用的重要条款。 广告 FS 条款更完整列表，请参阅[了解关键广告 FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
|术语|定义|  
|--------|--------------|  
|并行用户|预计希望提交内一给定的一段时间，通常高峰活动时段的服务请求的用户数。|  
|活动用户|大致平均数的一种系统，但不是一定提交期间指定的一段时间的请求，处于活动状态的用户。|  
|定义的用户|最大的用户理论计数，通常根据已系统中定义帐户的用户的数量。|  
|每秒请求|客户端任一提交请求数 \（当 system\ 上讨论加载）或服务器处理 \（当讨论服务器 throughput\）在第二个。 在规划 server 处理器和的内存容量使用此指标。|  
|目标服务器响应能力和利用率|成功绑定接受服务器性能范围的指标。 一般情况下，如果响应速度低于或利用率相应上方目标，系统将视为来重载和，则需要更多的容量。|  
|Windows 内部数据库 \(WID\)|可用作在某些广告 FS 部署 SQL Server 替代默认广告 FS 配置数据库。|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>广告 FS 测试过程中使用的配置环境  
本部分介绍配置环境广告 FS 产品团队用来执行测试。 团队使用下面的计算机硬件、软件和网络配置收集性能和中联盟服务器的测试可扩展性数据：  
  
-   双四核 2.27 千兆赫 \(GHz\) \(8 cores\)  
  
-   16\ GB RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   千兆位网络  
  
> [!NOTE]  
> 尽管测试过程中联合身份验证的服务器上使用的 16 GB 的 RAM，中等更多内存的大小，如 4 GB 的每个联合身份验证的服务器 RAM 可用大多数广告 FS 部署。 此广告 FS 容量规划内容，以及所提供的广告 FS 容量规划电子表格结果基于每个联合身份验证的服务器将大约使用的假设界面还设有建议 4 GB RAM 的对于大多数广告 FS 生产环境。  
  
产品团队采用以下配置的联合身份验证的服务器代理测试收集性能和可扩展性数据：  
  
-   双四核 2.24 GHz \(4 cores\)  
  
-   4\ GB RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   千兆位网络  
  
> [!NOTE]  
> 广告 FS 服务器的容量建议可以相当，有所不同，具体取决于你在给定的环境中使用的硬件和网络配置为选择规范。 为参考的点，这些内容中提供的调整大小指南基于利用率目标 80%前面提到的计算机上。  
  
## <a name="measure-ad-fs-server-capacity"></a>测量广告 FS 服务器容量  
通常，影响服务器的性能和可缩放的硬件组件是 CPU、内存和磁盘，网络适配器。 幸运的是，每个组件广告 FS 需要对内存和磁盘空间很低需求。 网络连接都明显的要求。 因此，所执行的联合身份验证的服务器和联盟服务器代理加载测试着重用于测量服务器容量两个主要区域：  
  
-   **峰值每秒广告 FS 请求：**联合身份验证的服务器上每秒进行处理 sign\ 在请求数。 此度量可以帮助你确定多少个用户都将可以登录到给定的服务器。 您可以使用此度量结合使用的 CPU 消耗测量值来了解此度量会影响性能。  
  
-   **CPU 用量：**百分比的 cpu 测量容量。 此度量可以帮助你确定发生整体 CPU 加载基于每秒传入 sign\ 在请求数。  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>继续阅读有关广告 FS 容量计划的详细信息  
你已完成先决条件任务，并具有熟悉相关的条款和硬件要求后，可以使用以下其他容量计划内容来帮助你确定你部署所需的广告 FS 服务器的推荐的数量：  
  
-   [规划联合身份验证的服务器容量](Planning-for-Federation-Server-Capacity.md)  
  
-   [规划联合身份验证的服务器代理容量](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
