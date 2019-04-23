---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: 跨林部署声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890648"
---
# <a name="deploy-claims-across-forests"></a>跨林部署声明

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在 Windows Server 2012 中，声明类型是与它关联的对象有关的断言。 声明类型在 Active Directory 中根据林来定义。 在许多方案中，安全主体可能需要遍历信任边界以访问受信任林中的资源。 Windows Server 2012 中的跨林声明转换使你转换遍历林，以便声明是能够识别并接受在相互信任和受信任林中的出口和入口声明。 声明转换的一些实际应用场景包括：  
  
-   信任林可以使用声明转换，通过筛选具有特定值的传入声明来防止权限提升。  
  
    如果受信任林不支持或发出任何声明，信任林还可以为越过信任边界的主体发出声明。  
  
-   受信任林可以使用声明转换来防止特定声明类型和具有特定值的声明流入信任林。  
  
-   此外，还可以使用声明转换在信任林和受信任林之间映射不同的声明类型。 声明转换可用于对声明类型和/或声明值执行通用化。 如果没有声明转换，你需要先对林之间的数据执行标准化，然后才能使用声明。 对信任林和受信任林之间的声明执行通用化可以减少 IT 成本。  
  
## <a name="claim-transformation-rules"></a>声明转换规则  
转换规则语言语法将一项规则分为两个主要部分：一系列条件语句和一个发出语句。 每个条件语句都有两个子组件：声明标识符和条件。 发出语句包含关键字、分隔符和发出表达式。 条件语句可以声明标识符变量开头，表示匹配的输入声明。 条件用于检查表达式。 如果输入声明与条件不匹配，转换引擎将忽略发出语句并根据转换规则评估下一个输入声明。 如果所有条件都与输入声明匹配，它将处理发出语句。  
  
有关声明规则语言的详细信息，请参阅 [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md)。  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>将声明转换策略链接到林  
设置声明转换策略时需用到两个组件：声明转换策略对象和转换链接。 策略对象位于林中的配置命名上下文中，并包含声明的映射信息。 链接指定映射应用到的信任林和受信任林。  
  
请务必了解林是信任林还是受信任林，因为这是链接转换策略对象的基础。 例如，受信任林是包含需要访问权限的用户帐户的林。 信任林是包含想要允许用户访问的资源的林。 声明的传输方向与需要访问权限的安全主体的方向相同。 例如，如果从 contoso.com 林到 adatum.com 林存在单向信任，声明将从 adatum.com contoso.com 流向 contoso.com，从而允许用户从 adatum.com 访问 contoso.com 中的资源。  
  
默认情况下，受信任林允许所有传出声明通过，而信任林将删除它收到的所有传入声明。  
  
## <a name="in-this-scenario"></a>本方案内容  
本方案可使用以下指南：  
  
-   [跨林部署声明&#40;演示步骤&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [声明转换规则语言](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>在此方案中包括角色和功能  
下表列出了作为本方案组成部分的角色和功能，并描述了它们如何为本方案提供支持。  
  
|角色/功能|如何支持本方案|  
|-----------------|---------------------------------|  
|Active Directory 域服务|在本方案中，你需要设置具有双向信任的两个 Active Directory 林。 两个林中都有声明。 另外在资源所在的信任林上设置中心访问策略。|  
|“文件和存储服务”角色|在本方案中，对文件服务器上的资源应用数据分类。 对想要允许用户访问的文件夹应用中心访问策略。 在转换后，声明基于应用于文件服务器上文件夹的中心访问策略向用户授予对资源的访问权限。|  
  


