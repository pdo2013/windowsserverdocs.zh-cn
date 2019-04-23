---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: 附录 A 动态访问控制术语表
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856698"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>附录 A：动态访问控制术语表

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

以下是术语和定义包括在动态访问控制方案的列表。  
  
|术语|定义|  
|--------|--------------|  
|自动分类|基于由由管理员配置分类规则的分类属性时发生的分类。|  
|CAPID|中心访问策略 id。 此 ID 引用特定中心访问策略，但它用于从文件和文件夹的安全描述符引用策略。|  
|中心访问规则|包括一个条件以及访问表达式的规则。|  
|中心访问策略|创作和承载 Active Directory 中的策略。|  
|基于声明的访问控制|一个范例，它使用声明来生成对资源的访问控制决策。|  
|分类|确定资源的分类属性并将这些属性分配给与资源相关联的元数据的过程。 另请参阅 REF AutomaticClassification \h \\* 参阅自动分类、 REF InheritedClassification \h \\ \*参阅继承分类和 REF ManualClassification \h \\ \*参阅手动分类。|  
|设备声明|与系统相关联的声明。  使用用户声明时，它包含在用户尝试访问资源的令牌。|  
|自由访问控制列表 (DACL)|访问控制列表，用于标识受信者允许或拒绝对安全资源的访问。 可以自行决定资源所有者对其进行修改。|  
|资源属性|（例如标签） 的属性，用于描述一个文件并使用自动分类或手动分类分配给文件。 示例包括：敏感度、 项目和保留期。|  
|文件服务器资源管理器|在 Windows Server 操作系统提供的文件夹配额、 文件屏蔽、 存储报告，文件分类和文件服务器上的文件管理作业的管理功能。|  
|文件夹属性和标签|属性，用于描述一个文件夹，并由管理员和文件夹所有者手动分配的标签。 这些属性分配到这些文件夹，例如，保密或部门中的文件默认属性值。|  
|组策略|一组规则和策略控制用户和计算机在 Active Directory 环境中的工作环境。|  
|准实时分类|创建或修改文件后，立即执行的自动分类。|  
|准实时文件管理任务|文件管理任务，不久之后执行 （创建或修改文件。 通过近乎实时的分类，将触发这些任务。|  
|组织单位 (OU)|Active Directory 容器，表示组织中的层次结构、 逻辑结构。 它是最小范围设置将应用到哪些组策略。|  
|安全属性|一个分类属性，可以信任授权运行时是有效的断言有关特定的时间点处的资源。 在基于声明的访问控制分配给某个资源的安全属性视为资源声明。|  
|安全描述符|数据结构，它包含与安全对象的资源，例如访问控制列表相关联的安全信息。|  
|安全描述符定义语言|一种规范描述文本字符串形式的安全描述符中的信息。|  
|过渡策略|但实际上不是一个中央访问策略。|  
|系统访问控制列表 (SACL)|访问控制列表用于指定由需要生成审核记录的特定受信者的访问尝试的类型。|  
|用户声明|用户安全令牌中提供的用户的属性。 示例包括：部门、 公司、 项目和安全许可。  Windows Server 2012 之前的系统，例如，属于用户的安全组的用户令牌中的信息还可被视为用户声明。 通过 Active Directory 提供了一些用户声明和其他人动态计算，如用户的登录是否使用智能卡。|  
|用户令牌|一个标识用户的用户声明和与该用户相关联的设备声明的数据对象。 它用于授予对资源的用户的访问权限。|  
  
## <a name="see-also"></a>请参阅  
[动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  


