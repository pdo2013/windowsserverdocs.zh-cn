---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: 使用组织域林模型
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 876a15dcdd951e0323fb7ddb7be96317f5512f0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875908"
---
# <a name="using-the-organizational-domain-forest-model"></a>使用组织域林模型

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在组织域林模型中，几个自治组每个拥有某个域的林中。 每个组控制使他们能够自主管理服务管理的某些方面，而林所有者控制林级别的服务管理的域级服务管理。  

下图显示了组织域林模型。  

![使用组织域林模型](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>域级服务自治性

组织域林模型使授权的域级服务管理的委派。 下表列出了可以在域级别控制的服务管理的类型。  

|类型的服务管理|相关联的任务|  
|------------------------------|--------------------|  
|管理的域控制器操作|-创建和删除域控制器<br />-监视的域控制器正常运行<br />-管理域控制器运行的服务<br />-备份和还原目录|  
|全域性设置的配置|创建域和域用户帐户策略，例如密码、 Kerberos 和帐户锁定策略<br />-创建和应用的全域性组策略|  
|数据级别的管理委派|-创建组织单位 (Ou) 和委派管理<br />-修复 OU 所有者没有足够的访问权限，若要修复的 OU 结构中的问题|  
|外部信任的管理|-建立与林的外部域信任关系|  

其他类型的服务管理，例如架构或复制拓扑管理的林所有者负责。  

## <a name="domain-owner"></a>域所有者

在组织域林模型中，域所有者负责域级服务管理任务。 域所有者具有对整个域以及所有其他林中的域的访问权限的颁发机构。 出于此原因，域所有者必须是受信任的个人选择林所有者。  

域级别的服务将管理委派给域所有者，如果满足以下条件：  

- 参与该林的所有组都信任新的域所有者和新的域服务管理做法。  

- 新的域所有者信任林所有者和所有其他域所有者。  

- 在林中所有域所有者都同意新的域所有者具有服务管理员管理和选择策略和相同或比自己更严格的做法。  

- 在林中所有域所有者都同意的管理的新域中的新域所有者的域控制器都是以物理方式安全。  

请注意，是否林所有者委托域级别管理服务的域所有者，其他组可能选择不加入该林，是否它们不信任该域所有者。  

必须注意，如果这些条件的任何更改在将来，可能会将组织域移动到多个林部署所需所有域所有者。  

> [!NOTE]  
> Windows Server 2008 Active Directory 域的安全风险降到最低的另一个方法是使用管理员角色分隔，需要在 Active Directory 基础结构中的只读域控制器 (RODC) 部署。 RODC 是一种新型的 Windows Server 2008 操作系统承载的 Active Directory 数据库的只读分区中的域控制器。 在 Windows Server 2008 的发布之前, 的域控制器上任何服务器维护工作必须由域管理员执行。 在 Windows Server 2008 中，你可以而无需授予该用户的域或其他域控制器的任何管理权限委托给任何域用户的 RODC 的本地管理权限。 这允许受委派的用户登录到 RODC 并执行维护工作，例如升级驱动程序，在服务器上。 但是，此委托的用户无法登录到任何其他域控制器上或在域中执行任何其他管理任务。 这样一来，任何受信任的用户可以是委派的能力有效地管理 RODC 不会影响其他域的安全性。 有关 Rodc 的详细信息，请参阅[AD DS:只读域控制器](https://go.microsoft.com/fwlink/?LinkId=106616)。  
