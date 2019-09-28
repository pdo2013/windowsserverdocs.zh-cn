---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: 使用组织域林模型
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af5fd2da396ecc27db68d3be8d1c0eda82314d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402444"
---
# <a name="using-the-organizational-domain-forest-model"></a>使用组织域林模型

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在组织域林模型中，多个自治组都在林中拥有一个域。 每个组控制域级别服务管理，使其能够在林所有者控制林级服务管理的同时管理服务管理的某些方面。  

下图显示了组织域林模型。  

![使用组织域林模型](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>域级服务自治

组织域林模型为域级服务管理启用了授权委派。 下表列出了可在域级别控制的服务管理的类型。  

|服务管理的类型|关联的任务|  
|------------------------------|--------------------|  
|域控制器操作的管理|-创建和删除域控制器<br />-监视域控制器的功能<br />-管理域控制器上运行的服务<br />-备份和还原目录|  
|域范围设置的配置|-创建域和域用户帐户策略，例如密码、Kerberos 和帐户锁定策略<br />-创建和应用全域性组策略|  
|数据级别管理的委派|-创建组织单位（Ou）并委派管理<br />-修复 ou 结构中 OU 所有者没有足够的访问权限来进行修复的问题|  
|外部信任的管理|-建立与林外域的信任关系|  

其他类型的服务管理（如架构或复制拓扑管理）由林所有者负责。  

## <a name="domain-owner"></a>域所有者

在组织域林模型中，域所有者负责域级服务管理任务。 域所有者拥有整个域的权限，以及对林中所有其他域的访问权限。 出于此原因，域所有者必须是林所有者选择的受信任人员。  

如果满足以下条件，则将域级服务管理委托给域所有者：  

- 参与林中的所有组都信任新域所有者和新域的服务管理做法。  

- 新域所有者信任林所有者和所有其他域所有者。  

- 林中的所有域所有者都同意新的域所有者具有与他们自己的服务管理员管理和选择策略和做法。  

- 林中的所有域所有者都同意新域中由新域所有者管理的域控制器在物理上是安全的。  

请注意，如果林所有者将域级服务管理委托给域所有者，则当用户不信任该域所有者时，其他组可能会选择不加入该林。  

所有域所有者都必须知道，如果将来有任何情况发生变化，则可能需要将组织域移到多个林部署中。  

> [!NOTE]  
> 将 Windows Server 2008 Active Directory 域的安全风险降至最低的另一种方法是使用管理员角色分离，这需要在 Active Directory 基础结构中部署只读域控制器（RODC）。 RODC 是 Windows Server 2008 操作系统中的一种新类型的域控制器，它承载 Active Directory 数据库的只读分区。 在发布 Windows Server 2008 之前，域控制器上的任何服务器维护工作都必须由域管理员执行。 在 Windows Server 2008 中，你可以将 RODC 的本地管理权限委派给任何域用户，而无需授予该用户对域或其他域控制器的任何管理权限。 这允许委派的用户登录到 RODC，并在服务器上执行维护工作（如升级驱动程序）。 但是，此委派的用户无法登录到任何其他域控制器或执行域中的任何其他管理任务。 这样，任何受信任的用户都可以被委派有效地管理 RODC 的能力，而不会影响域的其余部分的安全性。 有关 Rodc 的详细信息，请参阅 [AD DS：只读域控制器 @ no__t。  
