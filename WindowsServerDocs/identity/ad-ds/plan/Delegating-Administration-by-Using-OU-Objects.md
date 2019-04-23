---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: 通过使用 OU 对象委派管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39c4278270e4ab4fba9ff1062d2aa043d203a74b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886938"
---
# <a name="delegating-administration-by-using-ou-objects"></a>通过使用 OU 对象委派管理

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可用于组织单位 (Ou) 的对象，例如用户或计算机，管理委托给指定的个人或组的 OU 中。 若要通过使用 OU 委派管理，放置的个人或组到要管理权限委派到的组、 将对象以控制到一个 OU 的组，然后向该组 ou 委派管理任务。  
  
Active Directory 域服务 (AD DS)，可控制在非常详细级别，可以将委派的管理任务。 例如，将分配一个组具有完全控制权限的所有对象的 OU; 中分配权限仅以创建、 删除和管理用户帐户的 OU; 中的另一个组然后分配第三个分组仅重置用户帐户密码的权限。 您可以将这些权限可继承，这样它们适用于位于子树中的原始 OU 中的任何 Ou。  
  
默认 Ou 和容器在 AD ds 安装过程中创建，由服务管理员进行控制。 最好服务管理员继续控制这些容器。 如果您需要委派控制权限的目录中的对象，创建其他 Ou 和对象放置在这些 Ou。 将对这些 Ou 控制委托给相应的数据管理员。 这样，可以委派控制权限的目录中的对象，而无需更改提供给服务管理员的默认控件。  
  
林所有者确定委派给 OU 所有者的颁发机构的级别。 这可以包括能够创建和操作中的 OU 与只被允许控制单一类型的 OU 中的对象的单个属性的对象。 向用户授予在 OU 中隐式创建一个对象的功能授予该用户对用户创建任何对象的任何属性进行操作的功能。 此外，如果创建的对象是容器，用户将隐式具有能够创建和操作在容器中放置任何对象。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [委派默认容器和 Ou 的管理](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [委派帐户 Ou 和资源 Ou 的管理](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


