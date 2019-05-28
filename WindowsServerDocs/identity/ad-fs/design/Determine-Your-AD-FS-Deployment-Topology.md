---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: 确定 AD FS 部署拓扑
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 06cc4bd37905f6bb7afbc513ffce216104654aba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191483"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>确定 AD FS 部署拓扑

规划 Active Directory 联合身份验证服务的部署的第一步\(AD FS\)是确定正确的部署拓扑，以满足的单一登录\-上\(SSO\)需要的应用组织。 在本部分中的主题介绍可以与 AD FS 配合使用的各种部署拓扑。 这些主题还描述了与每个部署拓扑相关联的优势和限制，以便你可以为特定的业务需求选择最合适的拓扑。  
  
在阅读本部署拓扑主题之前，我们建议你先按照下表中显示的顺序完成任务。  
  
|建议的任务|描述|参考|  
|--------------------|---------------|-------------|  
|查看 AD FS 数据如何存储和复制到联合服务器场中其他联合身份验证服务器。|了解存储在 AD FS 配置数据库中的基础数据的用途以及针对其使用的复制方法。 本主题介绍了配置数据库的概念，并介绍了以下两种数据库类型：Windows 内部数据库\(WID\)和 Microsoft SQL Server。|[AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|选择将在你的组织中部署的 AD FS 配置数据库的类型。|查看与使用 WID 或 SQL Server 作为 AD FS 配置数据库相关联的各种优势和限制，以及它们所支持的各种应用程序方案。|[AD FS 部署拓扑注意事项](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> 若要实现基本冗余、 负载平衡，以及用于缩放联合身份验证服务的选项\(必要\)，我们建议你部署每个联合服务器场的所有生产环境中的至少两个联合服务器而不考虑你将使用的数据库的类型。  
  
当你查看了上一个表中的内容后，请继续执行本部分中的以下主题：  
  
-   [使用 WID 的独立联合服务器](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [使用 WID 的联合服务器场](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [使用 WID 和代理的联合服务器场](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [使用 SQL Server 的联合服务器场](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
选择 AD FS 部署拓扑后，我们建议你查看主题[AD FS 服务器容量规划](Planning-for-AD-FS-Server-Capacity.md)来确定将需要部署以支持此拓扑的服务器的建议的数目。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

