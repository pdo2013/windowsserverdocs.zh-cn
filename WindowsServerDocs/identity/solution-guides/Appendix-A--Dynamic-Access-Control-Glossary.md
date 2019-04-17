---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: "附录 A 动态访问控制词汇表"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>附录 a：动态访问控制词汇表

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

以下是列表中的条款和动态访问控制方案中包含的定义。  
  
|术语|定义|  
|--------|--------------|  
|自动分类|发生的分类基于由管理员配置分类规则的分类属性。|  
|CAPID|中央访问策略 id。 此 ID 引用特定中心访问策略，，并使用它来从的文件和文件夹的安全描述符引用的策略。|  
|中央使用规则|包含一个条件和访问 expression 规则。|  
|中央访问策略|编写、 Active Directory 中托管的策略。|  
|基于索赔访问控制|利用索赔，以使访问到资源的控制决策一模式。|  
|分类|确定资源的分类属性和这些属性分配关联的资源的元数据的过程。 请参阅引用 AutomaticClassification \h？ * 参阅自动分类、 引用 InheritedClassification \h \\\ * 参阅继承分类和引用 ManualClassification \h \\\ * 参阅手册分类。|  
|设备声明|与系统关联的索赔。  与用户索赔，它将包括在用户尝试访问资源的标记。|  
|自由访问控制列表 (DACL)|标识受被允许使用或拒绝访问安全资源信者访问控制列表。 它可以资源所有者自行进行修改。|  
|资源属性|描述某个文件，并使用分类自动或手动分类分配给文件属性 （如标签）。 示例包括： 期间敏感度，项目中，并保留。|  
|文件服务器资源管理器|提供的文件夹配额、 文件屏蔽、 存储报告、 文件分类和文件服务器上的文件管理作业管理 Windows Server 操作系统中的新功能。|  
|文件夹属性和标签|属性和描述某个文件夹，并通过管理员和文件夹所有者手动分配标签。 这些属性分配到这些文件夹，例如，保密或部门内文件默认属性值。|  
|组策略|控制的用户和 Active Directory 环境中的计算机的工作环境规则和策略的一组。|  
|附近实时分类|创建或修改自动分类执行结束不久后某个文件时。|  
|附近实时的文件管理任务|文件执行不久之后的管理任务 （创建的文件或修改。 通过近实时分类触发这些任务。|  
|部门 (OU)|代表在组织内的层次逻辑结构 Active Directory 容器。 它是对的组策略设置应用最小范围。|  
|安全属性|可以信任的授权运行时是关于在特定的时间点资源有效肯定分类属性。 索赔基于访问控件中的分配给某个资源的安全属性视为资源索赔。|  
|安全描述符|包含安全资源，如控件的访问列表关联的安全信息的数据结构。|  
|安全描述符清晰度语言|介绍作为文本字符串安全描述符规范。|  
|临时策略|尚不起作用中心访问策略。|  
|系统访问控制列表 (SACL)|指定的访问权限类型访问控制列表尝试通过受审核记录需要生成的特定信者。|  
|用户声明|用户的用户安全令牌内提供的属性。 示例包括： 商业部、 公司、 项目和安全许可。  从系统之前 Windows Server 2012 安全组的用户已属于，如用户令牌中的信息还可以将视为用户索赔。 通过 Active Directory 提供了一些用户索赔和其他人计算动态，例如，用户是否已使用智能卡。|  
|用户标记|标识用户的用户索赔和与该用户的设备索赔数据对象。 它用于授权资源为用户的访问。|  
  
## <a name="see-also"></a>请参阅  
[动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  


