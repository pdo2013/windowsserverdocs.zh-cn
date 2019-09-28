---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: 通过使用 OU 对象委派管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 31b8ef30cb12903936d00a8ab8fe56de77f8025a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408948"
---
# <a name="delegating-administration-by-using-ou-objects"></a>通过使用 OU 对象委派管理

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

你可以使用组织单位（Ou）将 OU 中的对象（如用户或计算机）的管理委托给指定的个人或组。 若要使用 OU 委派管理，请将管理权限委派给组，将要控制的一组对象置于 OU，然后将 OU 的管理任务委派给该组。  
  
Active Directory 域服务（AD DS）允许您控制可以在非常详细的级别委派的管理任务。 例如，可以分配一个组以对 OU 中的所有对象拥有完全控制权;为另一个组分配权限，只授予在 OU 中创建、删除和管理用户帐户的权限;然后，将第三组权限仅分配给重置用户帐户密码。 您可以通过继承这些权限，使其应用于放置在原始 OU 的子树中的任何 Ou。  
  
默认 Ou 和容器是在 AD DS 安装期间创建的，由服务管理员控制。 如果服务管理员继续控制这些容器，则最好这样做。 如果需要委派对目录中对象的控制，请创建更多 Ou，并将对象放置在这些 Ou 中。 将这些 Ou 的控制权委托给适当的数据管理员。 这样就可以在不更改为服务管理员提供的默认控件的情况下，委派对目录中对象的控制。  
  
林所有者确定委托给 OU 所有者的授权级别。 这可能包括创建和操作 OU 内的对象，只允许在 OU 中控制单个类型的对象的单个属性。 为用户授予在 OU 中创建对象的权限，可以隐式授予该用户操作用户创建的任何对象的任何属性的能力。 此外，如果创建的对象是一个容器，则用户可以隐式地创建和操作放置在容器中的所有对象。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [委派默认容器和 OU 管理](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [委派帐户 OU 和资源 OU 管理](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


