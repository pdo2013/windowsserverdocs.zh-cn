---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: "方案获取深入地了解你使用分类的数据"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>通过使用分类的方案：获得深入地了解你的数据

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

依赖于数据和存储资源一直日益大多数组织的重要。 IT 管理员面临监督而且越来越复杂存储基础结构的日益挑战，同时还要使用负责确保时合理级别保留该总体--拥有成本。 管理存储资源是不仅的音量或数据; 可用性它也是有关履行公司策略，并了解如何使用存储启用高效利用率和合规性降低风险。 通过自动分类进程，以便你可以更有效地管理你的数据，文件分类基础结构提供深入地了解你的数据。 以下分类方法是使用文件分类基础结构提供：手动、编程，并自动。 本主题着重于自动文件分类方法。  
  
## <a name="BKMK_OVER"></a>方案说明  
文件分类基础结构使用分类规则自动扫描的文件和对它们进行分类根据该文件的内容。 分类属性 Active Directory 中以便在组织中，可以在文件服务器之间共享这些定义集中定义。 你可以创建扫描文件标准字符串或匹配模式 (常规 expression) 的字符串的分类规则。 文件中找到配置的分类参数后，该文件被归类，为配置中分类规则。 分类规则的一些示例包括：  
  
-   划分为高与众不同之处时遇到的任何文件，其中包含"Contoso 机密"字符串  
  
-   分类包含至少 10 身份证号作为时遇到的个人身份信息的任何文件  
  
当归文件时，你可以使用某个文件上的任何文件采取措施的管理任务将其划分特定的方式。 文件管理任务操作包括保护文件，过期该文件，并运行自定义操作（如发布到 web 服务的信息）相关联的权限。  
  
你可以找到规划配置自动文件分类以信息[自动文件分类的计划](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)。  
  
你可以找到如何自动分类文件中的步骤[部署自动文件分类 #40; 演示步骤 & #41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>在此情况下  
此项 scenario 是动态访问控制方案的一部分。 有关动态访问控制其他信息，请参阅：  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>实际应用  
在 Windows Server 2012 文件分类基础结构是会影响动态访问控制通过启用数据商家能够轻松地进行分类和标出数据。 存储在中心访问策略分类信息允许你定义访问策略为企业至关重要的数据类。  
  
## <a name="BKMK_NEW"></a>此项 scenario 中所含功能  
下表列出了这种情况的一部分的功能，并介绍了如何支持。  
  
|功能|它如何支持此方案|  
|-----------|---------------------------------|  
|[文件服务器资源管理器概述](https://technet.microsoft.com/library/hh831701.aspx)|文件分类基础结构是文件服务器资源管理器中包含的功能。|  
|[文件和存储服务概述](https://technet.microsoft.com/library/hh831487.aspx)|文件服务器资源管理器是一项功能，包含在服务文件服务器角色。|  
  


