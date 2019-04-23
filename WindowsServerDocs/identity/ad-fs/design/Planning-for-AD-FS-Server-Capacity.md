---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: AD FS 服务器容量规划
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847388"
---
# <a name="planning-for-ad-fs-server-capacity"></a>AD FS 服务器容量规划

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

  
> [!NOTE]  
> 本主题中提供的内容不反映在运行 Windows Server 2012 的服务器上执行的实际测试。 本主题将在所需的测试已执行后立即进行更新。  
  
Active Directory 联合身份验证服务的容量规划\(AD FS\)是为你的联合服务预测高峰使用周期以及规划或缩放过程\-AD FS 服务器部署以满足这些负载系统要求。  
  
本部分介绍联合服务器和联合服务器代理角色的部署指南，并根据 Microsoft 的 AD FS 产品团队已执行实验测试。 此内容的目的是帮助你：  
  
-   严密估计你组织的特定 AD FS 部署，如 AD FS 服务器数量的硬件需求。  
  
-   准确地项目符号的预期的高峰使用情况\-在请求中，规划增长，并确保你的 AD FS 部署能够处理该预期使用率峰值。  
  
在继续阅读此容量规划内容之前，我们建议你先按照以下两个表中所示的顺序完成任务。 在第一个表中，我们提供了建议任务的链接，这些任务将有助于为此容量规划讨论提供相关的上下文。  
  
|建议的任务|描述|参考|  
|--------------------|---------------|-------------|  
|了解有关部署 AD FS 联合身份验证服务器和联合服务器代理的要求|查看部署联合服务器和联合服务器代理所需的重要硬件和软件要求。|[附录 a:查看 AD FS 要求](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|选择将在你的组织中部署的 AD FS 配置数据库的类型|在可以开始使用本部分中的容量规划数据之前，首先需要确定哪些 AD FS 配置数据库的类型将部署这两个 Windows 内部数据库\(WID\)或结构化查询语言\(SQL\)数据库。|[The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)；<br /><br />[AD FS 部署拓扑注意事项](AD-FS-Deployment-Topology-Considerations.md)|  
|确定可用于新的 AD FS 配置数据库选择的拓扑布局的类型。|一旦你已决定要在部署中使用的 AD FS 配置数据库的类型，你将需要考虑在生产环境中哪种部署拓扑最匹配将需要放置联合服务器和联合服务器代理的位置。|[确定 AD FS 部署拓扑](Determine-Your-AD-FS-Deployment-Topology.md)|  
|了解关键 AD FS 相关容量规划术语|查看常见容量规划 AD FS 容量规划讨论中使用的术语的定义。|请参阅本主题中的 [AD FS capacity planning terms](Planning-for-AD-FS-Server-Capacity.md#bk_terms) 部分|  
  
在查看完上一个表中的内容后，现在就可以完成下一个表中的必备任务。  
  
|必备任务|描述|参考|  
|---------------------|---------------|-------------|  
|下载 AD FS 容量规划大小调整电子表格|AD FS 容量规划大小调整电子表格可以帮助您确定所需的 AD FS 联合服务器场部署的联合身份验证服务器的数目。 下面为下一项任务提供的链接中提供了有关如何使用该电子表格的说明。|[AD FS 容量规划电子表格](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|收集有关需要单一登录的用户数的数据\-上\(SSO\)对目标声明的访问\-感知应用程序和预期的高峰使用期间与此访问权限|所收集的此用户数据将用于 AD FS 容量规划大小调整电子表格的上下文中所需的输入值。|[估计数为你的组织的联合身份验证服务器](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|AD FS 容量规划电子表格适用于 Windows Server 2016|Windows Server 2016 的更新的规划工作表|[AD FS Windows Server 2016 容量规划](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>AD FS 容量规划术语  
下表描述了容量规划部分中的 AD FS 设计指南中经常使用的重要术语。 有关更完整的 AD FS 术语列表，请参阅[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
|术语|定义|  
|--------|--------------|  
|并发用户|预期将在给定时间段内（通常为高峰活动期间）向服务提交请求的用户的估计数目。|  
|活动用户|在系统上处于活动状态（但不一定在给定时间段内提交请求）的用户的大致平均数。|  
|已定义的用户|理论上的最大用户数，通常基于已经在系统中定义帐户的用户数。|  
|每秒的请求|或者客户端提交的请求数\(时的系统上的负载的相关通信\)或由服务器处理\(当谈到服务器的吞吐量时\)一秒内。 此指标用于规划服务器处理器和内存容量。|  
|目标服务器的响应速度和使用率|绑定可接受的服务器性能范围的成功度量标准。 通常情况下，如果响应速度低于目标或利用率超过目标，则系统将被视为过载，并需要更多的容量。|  
|Windows 内部数据库\(WID\)|可以作为 SQL Server 中某些 AD FS 部署的替代方法使用默认的 AD FS 配置数据库。|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>AD FS 测试过程中使用的配置环境  
本部分描述了 AD FS 产品团队用于执行其测试的配置环境。 在联合服务器的测试中，该团队使用了以下计算机硬件、软件和网络配置来收集性能和可伸缩性数据：  
  
-   双四核 2.27 千兆赫\(GHz\) \(8 个核心\)  
  
-   16\-GB RAM  
  
-   Windows Server 2008 R2（企业版）  
  
-   千兆位网络  
  
> [!NOTE]  
> 尽管 16 GB 的 RAM 了联合身份验证服务器上测试期间使用的但更适中的内存大小，如 4 GB 的每个联合身份验证服务器的 RAM 可用于大多数 AD FS 部署。 在此 AD FS 容量规划以及所提供的 AD FS 容量规划电子表格结果的内容都基于假设每个联合身份验证服务器将使用大约中提供的建议 4 GB 的 RAM 的大多数 AD FS 生产环境环境。  
  
产品团队使用了以下配置为联合服务器代理测试收集性能和可伸缩性数据：  
  
-   双四核 2.24 \(4 个核心\)  
  
-   4\-GB RAM  
  
-   Windows Server 2008 R2（企业版）  
  
-   千兆位网络  
  
> [!NOTE]  
> AD FS 服务器的容量建议可能相差迥异，具体取决于选择的要在给定环境中使用的硬件和网络配置的规范。 作为参考，此内容中提供的大小调整指南将基于前面所述的计算机上 80% 的利用率目标。  
  
## <a name="measure-ad-fs-server-capacity"></a>测量 AD FS 服务器容量  
通常，可影响服务器的性能和可伸缩性的硬件组件包括 CPU、内存、磁盘和网络适配器。 幸运的是，每个 AD FS 组件需要很少内存和磁盘空间需求。 网络连接是一个明显的要求。 因此，在联合服务器和联合服务器代理上执行的负载测试将集中用于测量服务器容量的两个主要方面：  
  
-   **每秒的峰值 AD FS 请求：** 登录数\-中联合身份验证服务器上每秒处理的请求。 此测量值可帮助你确定有多少个用户可以同时登录到给定的服务器。 你可以将此测量值与 CPU 消耗测量值结合使用以了解此测量值对性能的影响。  
  
-   **CPU 消耗：** 测量的 CPU 容量百分比。 此测量值可帮助你确定发生的总体 CPU 负载基于传入的登录数\-中每秒请求数。  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>继续阅读有关 AD FS 容量规划的详细信息  
已经完成必备的任务，并已熟悉相关的术语和硬件要求之后，可以使用下列其他容量规划内容来帮助您确定所需的 AD FS 服务器的建议的数目应用部署：  
  
-   [联合身份验证服务器容量规划](Planning-for-Federation-Server-Capacity.md)  
  
-   [联合身份验证服务器代理容量规划](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
