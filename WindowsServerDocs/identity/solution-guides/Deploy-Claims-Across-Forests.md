---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: "森林跨部署索赔"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-claims-across-forests"></a>森林跨部署索赔

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在 Windows Server 2012，声明类型是肯定关于，它有与之关联的对象。 每个林 Active Directory 中定义索赔类型。 有很多情况下，可能需要遍历信任边界访问信任的森林中的资源主体的安全。 在 Windows Server 2012 跨森林索赔转换使您能够转换进出遍历森林，以便索赔识别，并且接受信任和受信任的森林中的索赔。 一些的索赔转换真实的方案：  
  
-   信任林可用作索赔转换抵御特权提升通过筛选与特定值传入的索赔。  
  
    信任森林还可以颁发主体即将信任边界通过，如果不是受信任的森林支持或发出任何索赔提起的索赔。  
  
-   森林受信任可以使用索赔转换阻止某些索赔类型和使用某些值从要发布到信任林的索赔。  
  
-   你还可以使用转换到地图不同声称之间信任和受信任林类型的索赔。 这可用于一般化声明类型、该声明值，或两者都。 无需此，你需要标准化林之前，你可以使用索赔之间数据。 使通用信任和受信任的林之间的索赔减少 IT 成本。  
  
## <a name="claim-transformation-rules"></a>声称转换规则  
转换规则语言语法将分为两个主要部分的单个规则：一系列条件明细表和问题声明。 每个条件声明有两个子组件：索赔标识符和条件。 包含问题声明的关键字、分隔符和问题 expression。 （可选）开头索赔标识符变量，这表示此匹配输入的声明条件声明。 检查 expression 条件。 如果输入的声明不符合条件，转换引擎将忽略问题声明，并且在计算转换规则针对下一步输入的声明。 如果所有条件都匹配输入的索赔，该进程问题声明。  
  
索赔规则语言的详细信息，请参阅[索赔转换规则语言](Claims-Transformation-Rules-Language.md)。  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>森林链接索赔转换策略  
有两个组件索赔转换策略设置过程中涉及：声称转换策略对象和转换链接。 策略对象 live 配置命名林中上下文中，并且包含的索赔映射信息。 该链接指定的信任和森林受信任的地图适用于。  
  
请务必了解森林是否信任或受信任的森林，因为这很基础链接转换策略对象。 例如，受信任的森林是森林包含需要访问权限的用户帐户。 信任林是林包含你想要为用户的访问权限的资源。 主要的要求访问安全的方向相同旅行索赔。 例如，如果到 adatum.com 森林单向从 contoso.com 森林信任，索赔将从流 adatum.com 到 contoso.com，从而允许用户从 adatum.com 访问 contoso.com 中的资源。  
  
默认情况下，受信任的森林允许通过，所有传出索赔，并信任林丢弃接收到的所有传入的索赔。  
  
## <a name="in-this-scenario"></a>在此情况下  
以下指南也适用于这种情况：  
  
-   [森林 #40; 演示步骤 & #41; 跨部署索赔](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [索赔转换规则语言](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>负责并此方案中所含功能  
下表列出的角色和属于这种情况下的功能，并介绍了如何支持。  
  
|角色/功能|它如何支持此方案|  
|-----------------|---------------------------------|  
|Active Directory 域服务|在此情况下，你必须具有双向信任的两个 Active Directory 林设置。 您可以在两个林有索赔。 你还上信任林资源所在的位置设置中央访问策略。|  
|文件和存储服务的作用|在此情况下，数据分类应用到文件服务器上的资源。 中央访问策略应用到所需授予用户访问权限的文件夹。 转换后的索赔授予根据中心访问策略应用到文件服务器上的文件夹的用户访问。|  
  


