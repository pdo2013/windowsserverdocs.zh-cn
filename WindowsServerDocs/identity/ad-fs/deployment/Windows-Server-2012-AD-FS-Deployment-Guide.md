---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Windows Server 2012 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1597230fcfde4fe8a9767f0f14c634bc6fabdceb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408255"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 AD FS 部署指南


你可以使用 Active Directory®联合身份\(验证\)服务 AD FS 与 Windows Server®2012操作系统结合使用来构建扩展了分布式标识、身份验证和的联合身份管理解决方案针对跨组织和\-平台边界的基于 Web 的应用程序的授权服务。 通过部署 AD FS，你可以将组织现有的标识管理功能扩展到 Internet。  
  
部署 AD FS 可以：  
  
-   当你的员工或客户需要远程\-访问内部托管的\-网站或服务时，为你的员工或客户提供基于 Web 的单一\-登录\(SSO\)体验。  
  
-   当你的员工或客户从网络\-防火墙访问跨\-组织网站或服务时，为你的员工或客户提供基于 Web 的 SSO 体验。  
  
-   为你的员工或客户提供对 Internet 上\-任何联合身份验证伙伴组织中基于 Web 的资源的无缝访问权限，而无需员工或客户多次登录。  
  
-   无需使用其他登录\-提供程序\(Windows Live ID、\)自由联盟等，就能完全控制您的员工或客户标识。  
  
## <a name="about-this-guide"></a>关于本指南  
本指南旨在供系统管理员和系统工程师使用。 它提供有关部署由你或组织中的基础结构专家或系统架构师预先选择的 AD FS 设计的详细指南。  
  
如果尚未选择设计，我们建议你等待按照本指南中的说明进行操作，直到你在[Windows Server 2012 的 "AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)" 中查看了设计选项之后，为你的内部. 有关将本指南用于已选择的设计的详细信息，请参阅[实现 AD FS 设计计划](Implementing-Your-AD-FS-Design-Plan.md)。  
  
从设计指南中选择设计并收集有关声明、令牌类型、属性存储和其他项所需的信息后，可以使用本指南在生产环境中部署 AD FS 设计。 本指南提供用于部署以下任一主要 AD FS 设计的步骤：  
  
-   Web SSO  
  
-   联合 Web SSO  
  
使用[实现 AD FS 设计计划](Implementing-Your-AD-FS-Design-Plan.md)中的清单来确定如何最好地使用本指南中的说明部署特定设计。 有关部署 AD FS 的硬件和软件要求的信息，请参阅[附录 A：查看 AD FS 设计](https://technet.microsoft.com/library/ff678034.aspx)指南中的 AD FS 要求。  
  
### <a name="what-this-guide-does-not-provide"></a>本指南未提供的内容  
本指南未提供：  
  
-   有关何时以及在何处放置联合服务器、联合服务器代理或现有网络基础结构中的 Web 服务器的指南。 有关此信息，请参阅 AD FS 设计指南中的[规划联合服务器布局](https://technet.microsoft.com/library/dd807069.aspx)和[规划联合服务器代理的布局](https://technet.microsoft.com/library/dd807130.aspx)。  
  
-   使用证书颁发机构\(ca\)设置 AD FS 的指南  
  
-   有关设置或配置基于 Web\-的特定应用程序的指南  
  
-   特定于设置测试实验室环境的设置说明。  
  
-   有关如何自定义联合登录屏幕、web.config 文件或配置数据库的信息。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [规划部署 AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [实现 AD FS 设计规划](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [清单：实现 Web SSO 设计](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [清单：实现联合 Web SSO 设计](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [配置合作伙伴组织](Configuring-Partner-Organizations.md)  
  
-   [配置声明规则](Configuring-Claim-Rules.md)  
  
-   [部署联合服务器](Deploying-Federation-Servers.md)  
  
-   [部署联合服务器代理](Deploying-Federation-Server-Proxies.md)  
  
-   [与 AD FS 1.x 进行互操作](Interoperating-with-AD-FS-1.x.md)  
