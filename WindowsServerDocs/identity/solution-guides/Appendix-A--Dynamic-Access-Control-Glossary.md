---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: 附录 A 动态访问控制术语表
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5508c3397039a1a70c07f1dc5f29e06bd02234a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357616"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>附录 A：动态访问控制术语表

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

下面是动态访问控制方案中包含的术语和定义的列表。  
  
|术语|定义|  
|--------|--------------|  
|自动分类|基于由管理员配置的分类规则确定的分类属性发生的分类。|  
|CAPID|中心访问策略 ID。 此 ID 引用特定的中心访问策略，并用于引用文件和文件夹的安全描述符中的策略。|  
|中心访问规则|一个包含条件和访问表达式的规则。|  
|中心访问策略|在 Active Directory 中创作和承载的策略。|  
|基于声明的访问控制|使用声明对资源做出访问控制决策的范例。|  
|分类|确定资源的分类属性并将这些属性分配给与资源关联的元数据的过程。 另请参阅 REF AutomaticClassification \h \\ * MERGEFORMAT 自动分类、REF InheritedClassification \h \\ @ no__t MERGEFORMAT 继承的分类和 REF ManualClassification \h \\ @ no__t MERGEFORMAT Manual分类.|  
|设备声明|与系统关联的声明。  使用用户声明时，它包含在尝试访问资源的用户的令牌中。|  
|随机访问控制列表（DACL）|一个访问控制列表，该列表标识允许或拒绝其访问安全资源的受信者。 它可以由资源所有者的判断来修改。|  
|资源属性|描述文件并使用自动分类或手动分类分配给文件的属性（例如标签）。 示例包括：敏感度、项目和保持期。|  
|文件服务器资源管理器|Windows Server 操作系统中的一项功能，它提供文件服务器上的文件夹配额、文件屏蔽、存储报告、文件分类和文件管理作业的管理。|  
|文件夹属性和标签|描述文件夹并由管理员和文件夹所有者手动分配的属性和标签。 这些属性将默认属性值分配给这些文件夹内的文件，例如，保密或部门。|  
|组策略|在 Active Directory 环境中控制用户和计算机的工作环境的一组规则和策略。|  
|近乎实时的分类|创建或修改文件后不久执行的自动分类。|  
|近乎实时的文件管理任务|不久后（创建或修改文件）执行的文件管理任务。 这些任务由近乎实时的分类触发。|  
|组织单位 (OU)|表示组织中的层次结构、逻辑结构的 Active Directory 容器。 这是应用组策略设置的最小作用域。|  
|安全属性|一个分类属性，授权运行时可以信任该属性，以便在某个时间点成为有关资源的有效断言。 在基于声明的访问控制中，分配给资源的安全属性被视为资源声明。|  
|安全描述符|包含与安全对象关联的安全信息的数据结构，例如访问控制列表。|  
|安全描述符定义语言|一种规范，它将安全描述符中的信息描述为文本字符串。|  
|过渡策略|尚未生效的中心访问策略。|  
|系统访问控制列表（SACL）|一种访问控制列表，该列表指定特定信者在需要为其生成审核记录的访问尝试的类型。|  
|用户声明|用户安全令牌中提供的用户的属性。 示例包括：部门、公司、项目和安全许可。  Windows Server 2012 之前的系统中的用户令牌中的信息（例如用户所属的安全组）也可以被视为用户声明。 某些用户声明通过 Active Directory 提供，其他则以动态方式进行计算，例如用户是否使用智能卡登录。|  
|用户令牌|一个数据对象，该对象标识用户以及与该用户相关联的用户声明和设备声明。 它用于授权用户访问资源。|  
  
## <a name="see-also"></a>请参阅  
[动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  


