---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: "Windows Server 2012 广告 FS 部署指导"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 广告 FS 部署指导

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以使用 Active Directory® 与 Windows Server 的联合身份验证服务 \(AD FS\)® 2012年操作系统版本组织和平台跨扩展到 Web\ 基于应用程序分布式的识别，身份验证和授权的服务联合的身份管理解决方案。 通过部署广告 FS，可以扩展你的组织的现有身份管理能力到 Internet。  
  
你可以将部署到广告 FS:  
  
-   为你的员工或客户提供 Web\ 基于、 single\-上 sign\ \(SSO\) 体验时，他们需要远程内部托管网站或服务访问。  
  
-   为你的员工或客户提供基于 Web\ SSO 体验时他们访问 cross\ 组织网站或在你的网络的防火墙服务。  
  
-   为你的员工或客户而无需员工或客户多次登录无缝访问 Web\ 基于资源，在 Internet 上的任何联盟合作伙伴公司提供。  
  
-   保留完全控制你的员工或客户身份，而无需使用其他 sign\ 上提供 \ （Windows Live ID，自由联盟 others\）。  
  
## <a name="about-this-guide"></a>有关此指南  
本指南介绍由系统管理员和系统工程师供使用。 它提供了详细的指导用于部署已由你或你的组织基础结构专员或系统设计师已事先选择广告 FS 设计。  
  
如果设计未被选中，我们建议你等到遵循直到本指南中的说明进行操作后，你有审查设计选项中的[广告 FS 设计指南 Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx)并已针对你的组织中选择最适合的设计。 使用本指南已选择的设计的详细信息，请参阅[实现广告 FS 设计计划](Implementing-Your-AD-FS-Design-Plan.md)。  
  
从设计指南中选择你的设计，并收集有关索赔、 标记类型、 特性存储和其他项目的所需的信息后，你可以使用本指南生产环境中部署广告 FS 设计。 本指南提供用于部署下面的主要广告 FS 设计任一步骤：  
  
-   Web SSO  
  
-   联盟的 Web SSO  
  
使用中的清单[实现广告 FS 设计计划](Implementing-Your-AD-FS-Design-Plan.md)来确定如何最佳采用本指南中的说明进行操作部署你特定设计。 有关部署广告 FS 硬件和软件要求的信息，请参阅[附录 a： 查看广告 FS 要求](https://technet.microsoft.com/library/ff678034.aspx)广告 FS 设计指南中。  
  
### <a name="what-this-guide-does-not-provide"></a>本指南不提供的内容  
不提供此指南：  
  
-   有关何时以及在你现有的网络基础结构放置联合身份验证的服务器、 联合身份验证的服务器代理或 Web 服务器的位置的指南。 此信息，请参阅[规划联合身份验证的服务器放置](https://technet.microsoft.com/library/dd807069.aspx)和[规划联盟服务器代理放置](https://technet.microsoft.com/library/dd807130.aspx)广告 FS 设计指南中。  
  
-   用于使用证书颁发机构 \(CAs\) 广告 FS 设置指南  
  
-   设置，或配置 Web\ 基于特定应用程序的指南  
  
-   设置适用于设置测试实验环境的说明进行操作。  
  
-   了解如何自定义联盟的登录屏幕、 web.config 文件或配置数据库。  
  
## <a name="in-this-guide"></a>本指南中  
  
-   [计划部署广告 FS](Planning-to-Deploy-AD-FS.md)  
  
-   [实现广告 FS 设计套餐](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [清单：实施 Web SSO 设计](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [清单：实施联盟的 Web SSO 设计](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [配置合作伙伴公司](Configuring-Partner-Organizations.md)  
  
-   [配置索赔规则](Configuring-Claim-Rules.md)  
  
-   [部署联盟服务器](Deploying-Federation-Servers.md)  
  
-   [部署联合身份验证的服务器代理服务器](Deploying-Federation-Server-Proxies.md)  
  
-   [可以与广告 FS 互操作 1.x](Interoperating-with-AD-FS-1.x.md)  
