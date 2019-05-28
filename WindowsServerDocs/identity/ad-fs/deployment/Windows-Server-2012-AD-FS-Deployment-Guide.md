---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Windows Server 2012 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6be56c25cc6f639f73842f57cdf48a6339dccf9c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191851"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 AD FS 部署指南


可以使用 Active Directory® 联合身份验证服务\(AD FS\)与 Windows Server® 2012年操作系统来构建一个联合的标识管理解决方案，扩展了分布式的标识、 身份验证，并授权服务添加到 Web\-基于跨越组织和平台边界的应用程序。 通过部署 AD FS，可将组织现有的标识管理功能扩展到 Internet。  
  
部署 AD FS 可以：  
  
-   你的员工或客户提供与 Web\-基于，单一\-符号\-上\(SSO\)体验时它们需要远程访问内部托管网站或服务。  
  
-   你的员工或客户提供与 Web\-基于，SSO 体验跨访问时\-组织的网站或服务从你的网络的防火墙内。  
  
-   向 Web 提供的无缝访问你的员工或客户\-基于在 Internet 上任何联合身份验证伙伴组织中的资源，而无需为员工或客户多次登录。  
  
-   不需使用其他登录保持对你的员工或客户身份的完全控制\-上提供程序\(Windows Live ID、 Liberty Alliance 和其他\)。  
  
## <a name="about-this-guide"></a>关于本指南  
本指南旨在供系统管理员和系统工程师使用。 它提供了用于部署由你或你的组织中的基础结构专家或系统架构师预先的 AD FS 设计的详细的指导。  
  
如果尚未选择设计，我们建议您等待要遵循本指南中的说明进行操作后已经查看了中的设计选项[Windows Server 2012 中 AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)和最多选择适合你的组织的设计。 本指南中使用的已选择的设计的详细信息，请参阅[实现 AD FS 设计规划](Implementing-Your-AD-FS-Design-Plan.md)。  
  
从设计指南中选择您的设计并收集有关声明、 令牌类型、 属性存储和其他项所需的信息后，可以使用本指南以在生产环境中部署 AD FS 设计。 本指南提供了用于部署以下主 AD FS 设计任一步骤：  
  
-   Web SSO  
  
-   联合 Web SSO  
  
使用中的清单[实现 AD FS 设计规划](Implementing-Your-AD-FS-Design-Plan.md)来确定如何最大程度地使用本指南中的说明部署特定设计。 有关部署 AD FS 的硬件和软件要求的信息，请参阅[附录 a:查看 AD FS 要求](https://technet.microsoft.com/library/ff678034.aspx)中 AD FS 设计指南。  
  
### <a name="what-this-guide-does-not-provide"></a>本指南未提供的内容  
本指南未提供：  
  
-   有关时间和位置将联合身份验证服务器、 联合服务器代理或 Web 服务器放在现有网络基础结构的指南。 此信息，请参阅[规划联合服务器的位置](https://technet.microsoft.com/library/dd807069.aspx)并[规划联合服务器代理位置](https://technet.microsoft.com/library/dd807130.aspx)AD FS 设计指南中。  
  
-   使用证书颁发机构的指导\(CAs\)来设置 AD FS  
  
-   设置或配置特定 Web 指导\-基于应用程序  
  
-   特定于设置测试实验室环境的设置说明。  
  
-   有关如何自定义联合登录屏幕、web.config 文件或配置数据库的信息。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [规划部署 AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [实现 AD FS 设计规划](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [清单：实现 Web SSO 设计](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [清单：实现联合 Web SSO 设计](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [配置伙伴组织](Configuring-Partner-Organizations.md)  
  
-   [配置声明规则](Configuring-Claim-Rules.md)  
  
-   [部署联合服务器](Deploying-Federation-Servers.md)  
  
-   [部署联合服务器代理](Deploying-Federation-Server-Proxies.md)  
  
-   [与 AD FS 1.x 进行互操作](Interoperating-with-AD-FS-1.x.md)  
