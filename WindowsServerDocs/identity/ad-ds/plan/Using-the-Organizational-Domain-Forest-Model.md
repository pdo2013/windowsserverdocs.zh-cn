---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: "使用组织域林型号"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 22d871d9157622375619dd90336e597d4bfb3d68
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="using-the-organizational-domain-forest-model"></a>使用组织域林型号

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在组织域森林模型，几个自主组每个拥有林中域。 每个组控制域级别服务管理，从而使其时森林所有者控制林级别服务管理自主管理点点的某些方面。  
  
下图显示组织域森林模型。  
  
![使用组织域森林模型](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  
  
## <a name="domain-level-service-autonomy"></a>域级别服务自主  
组织域的森林模型使域级别服务管理颁发机构委派。 下表列出了可控制域级别的服务管理的类型。  
  
|服务管理类型|关联的任务|  
|------------------------------|--------------------|  
|管理域控制器操作|-创建和删除域控制器<br />-监视域控制器的运作情况<br />管理域控制器运行的服务<br />-备份和还原目录|  
|配置的域范围的设置|-创建域和域用户帐户的策略，例如密码、 Kerberos，以及帐户锁定策略<br />-创建和应用域范围组策略|  
|管理数据级别委派|-创建单位 （华丽绚烂） 和委派管理<br />-修复 OU 结构 OU 所有者没有足够的访问权限，若要修复的问题|  
|外部信任的管理|-建立信任与森林之外的域的关系|  
  
其他类型的服务管理，如方案或复制拓扑管理，负责森林所有者。  
  
## <a name="domain-owner"></a>域的所有者  
在组织域森林模型，域的所有者负责域级别服务管理任务。 域的所有者无权轻整个域以及访问森林中的所有其他域。 出于此原因，必须选中森林所有者的受信任的个人域的所有者。  
  
如果满足以下条件，委派给域的所有者域级别点点：  
  
-   参与森林中的所有组都信任新域的所有者和新的域的服务管理惯例。  
  
-   新域的所有者信任森林所有者和所有其他域的所有者。  
  
-   森林中的所有域所有者都同意新域的所有者具有管理员点点和所选内容策略等于或更严格比自己的做法。  
  
-   森林中的所有域所有者都同意域控制器由新域中新域的所有者都物理安全。  
  
请注意，是否森林所有者委托域级别服务管理到某个域的所有者其他组可以选择不加入该森林是否它们不信任该域的所有者。  
  
所有域的所有者必须都请注意，如果这些条件的任何更改将来时，它可能需要多的森林部署到移动组织域。  
  
> [!NOTE]  
> 最小化 Windows Server 2008 Active Directory 域安全风险的另一个方法是使用管理员角色分离，需要在你的 Active Directory 基础结构只读域控制器 (RODC) 的部署。 RODC 是一种新域控制器在 Windows Server 2008 操作系统承载只读分区的 Active Directory 数据库中。 之前 Windows Server 2008 发布时，域控制器上的任何服务器维护工作不得不由域管理员执行。 在 Windows Server 2008、 可以委派本地到任何域用户 RODC 管理权限而无需授予该用户的域或其他域控制器任何管理权限。 这样委派的用户在登录到 RODC 并执行维护工作，例如升级驱动程序，在服务器上。 但是，此委派的用户无法登录到任何其他域控制器上或在域中执行任何其他的管理任务。 这种方式，任何受信任的用户可以是委派有效管理 RODC，而不会影响的域的其余部分的安全功能。 有关 Rodc 的详细信息，请参阅广告 DS: Read-Only 域控制器 ([https://go.microsoft.com/fwlink/?LinkId=106616](https://go.microsoft.com/fwlink/?LinkId=106616))。  
  


