---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: "通过使用 OU 对象委派管理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d0b4765304d8b302fc174c191af2c8e87a25304
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-by-using-ou-objects"></a>通过使用 OU 对象委派管理

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

可用于部门 （华丽绚烂） 委派的对象时，用户或计算机，如管理 OU 与指定的个人或一组中。 若要使用 OU 委派管理，位置的个人或入的组要向其中委派管理权限的组，位置的一套对象以进行控制插入 OU，然后为该组到 OU 委派管理任务。  
  
Active Directory 域服务 (广告 DS) 将使您可以控制可以在更细致委派的管理任务。 例如，你可以指定一组提供的所有对象完全控制眼。分配权利仅用于创建、 删除和管理眼。 中的用户帐户的另一组然后分配第三个分组右侧，仅重置用户帐户的密码。 你可以进行这些权限父系，以使它们适用于任何华丽绚烂放置在子原组织单位的树。  
  
默认华丽绚烂和容器 AD ds 安装过程中创建受服务管理员。 最好服务管理员继续控制这些容器。 如果你需要委派目录中的对象控制，创建其他华丽绚烂和放置在这些华丽绚烂的对象。 委派给相应的数据管理员这些华丽绚烂的控制。 这样，而无需更改与的服务管理员给定的默认控件委派控制目录中的对象。  
  
森林所有者确定委派给 OU 所有者的颁发机构级别。 这可以范围是从创建和处理对象到只可以控制某一类型的对象 OU 中的单个属性 OU 内的能力。 授予隐式在 OU 创建对象的功能的用户授予该用户的功能，可操作的用户创建的任何对象任何属性。 此外，如果创建对象容器，用户隐式能够创建和操作放置在容器任何物体。  
  
## <a name="in-this-section"></a>在此部分中  
  
-   [委派默认容器和华丽绚烂的管理](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [委派管理的帐户华丽绚烂和资源华丽绚烂](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


