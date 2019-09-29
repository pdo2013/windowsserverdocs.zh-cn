---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: 方案使用分类了解你的数据
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cd6a6e9d3cb452a2cd0a48c6207aea181d85dc7b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357455"
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>场景：使用分类深入了解你的数据

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

对数据和存储资源的依赖在大多数组织的重要性方面持续增长。 IT 管理员面对着监督更大、更复杂存储基础结构的日渐增长所带来的挑战，而同时又担负着确保所有者总开支维持在合理水平的责任。 管理存储资源不仅仅是涉及数据的量和可用性；它还和公司政策的执行有关，并且和了解如何使用存储来启用高效利用与合规性以减轻风险有关。 文件分类基础结构通过自动化分类流程来让你深入了解数据，以便你可以更有效地管理数据。 下列分类方法可用于文件分类基础结构：手动、编程、自动。 本主题重点介绍自动文件分类方法。  
  
## <a name="BKMK_OVER"></a>方案描述  
文件分类基础结构使用分类规则来自动扫描文件，并且根据文件内容对它们进行分类。 在 Active Directory 中集中定义了分类属性，以便可以在组织中在文件服务器之间共享这些定义。 你可以创建分类规则，它可用于扫描文件以查找标准字符串或与模式匹配的字符串（正则表达式）。 当在文件中找到已配置的分类参数时，该文件将被归类为可在分类规则中进行配置的文件。 分类规则的一些示例包括：  
  
-   将包含字符串 "Contoso 机密" 的任何文件归类为具有高业务影响  
  
-   将包含至少 10 个身份证号的任何文件归类为具有个人身份信息。  
  
当对文件进行分类时，你可以使用文件管理任务对以特定方式进行分类的任何文件执行操作。 文件管理任务中的操作包括保护与文件相关联的权限、使文件过期以及运行自定义操作（例如将信息发布到 Web 服务）。  
  
可以在[自动文件分类规划](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)中找到关于配置自动文件分类的规划信息。  
  
可以在[部署自动文件分类&#40;演示步骤&#41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md)中找到有关如何自动分类文件的步骤。  
  
## <a name="in-this-scenario"></a>本方案内容  
本方案是动态访问控制方案的一部分。 有关动态访问控制的其他信息，请参阅：  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>实用应用程序  
Windows Server 2012 中的文件分类基础结构通过使业务数据所有者能够轻松地对数据进行分类和标记，有助于实现动态访问控制。 通过存储在中心访问策略中的分类信息，你可以为企业至关重要的数据类定义访问策略。  
  
## <a name="BKMK_NEW"></a>此方案中包含的功能  
下表列出了本方案的部分功能并说明了支持该方案的工作原理。  
  
|功能|如何支持本方案|  
|-----------|---------------------------------|  
|[文件服务器资源管理器概述](https://technet.microsoft.com/library/hh831701.aspx)|文件分类基础结构是包含在文件服务器资源管理器中的功能。|  
|[文件和存储服务概述](https://technet.microsoft.com/library/hh831487.aspx)|文件服务器资源管理器是包含在文件服务服务器角色中的功能。|  
  


