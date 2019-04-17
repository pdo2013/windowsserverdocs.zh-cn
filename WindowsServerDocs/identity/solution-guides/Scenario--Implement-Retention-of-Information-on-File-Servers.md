---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: "文件服务器上的信息方案实现保留"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>文件服务器上的方案：实现保留的信息

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

保留期是的文档应该的时间保持之前它已过期。 根据组织机构保留期可能会有所不同。 你可以分类遇到短、中等或长期保留期间，将某个文件夹中的文件，然后每个期间指定的时间范围。 你可能希望来将其法律按无限期保留文件。  
  
## <a name="BKMK_OVER"></a>方案说明  
文件分类基础结构和文件 Server 资源管理器中使用文件管理任务和文件分类将应用保留期间的一组文件。 你可以将保留期间分配文件夹上，并将文件管理任务配置多长时间保留分配的时间是到最后一个。 在该文件夹中的文件即将到期时，该文件的所有者接收通知电子邮件。 你还可以将文件视为在法律按以便文件管理任务将不会过期该文件。  
  
你可以找到规划信息来配置中的保留[文件服务器上的信息保留计划](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)。  
  
你可以找到分类法律按的文件和配置中的保留期间步骤[部署实现保留的信息文件服务器 #40; 演示步骤 & #41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> 这种情况下仅讨论了如何手动分类的法律按某个文档。 但是，则可能在 Windows Server 2012 自动分类法律按的文档。 若要执行此操作的一种方法是创建 Windows PowerShell 分类比较文件到列表的法律按用户帐户的所有者。 如果文件所有者属于用户帐户列表，该文件的法律按归。  
  
## <a name="in-this-scenario"></a>在此情况下  
此项 scenario 是动态访问控制方案的一部分。 有关动态访问控制其他信息，请参阅：  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>此项 scenario 中所含功能  
下表列出了这种情况的一部分的功能，并介绍了如何支持。  
  
|功能|它如何支持此方案|  
|-----------|---------------------------------|  
|[文件服务器资源管理器概述](https://technet.microsoft.com/library/hh831701.aspx)|文件分类基础结构是文件服务器资源管理器中包含的功能。|  
|[文件和存储服务概述](https://technet.microsoft.com/library/hh831487.aspx)|文件服务器资源管理器是一项功能，包含在服务文件服务器角色。|  
  
  


