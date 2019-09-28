---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: 委派默认容器和 OU 管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 15c6688e32a7ebefbb2dd0fa1e53a4d72baef267
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408932"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>委派默认容器和 OU 管理

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

每个 Active Directory 域包含一组标准容器和组织单位（Ou），在安装 Active Directory 域服务（AD DS）的过程中创建。 例如：  
  
-   域容器，用作层次结构的根容器  
  
-   内置容器，用于保存默认服务管理员帐户  
  
-   用户容器，这是在域中创建的新用户帐户和组的默认位置  
  
-   "计算机" 容器，这是在域中创建的新计算机帐户的默认位置  
  
-   域控制器 OU，这是域控制器计算机帐户的计算机帐户的默认位置  
  
林所有者控制这些默认容器和 Ou。  
  
## <a name="domain-container"></a>域容器  
域容器是域的层次结构的根容器。 对此容器上的策略或访问控制列表（ACL）所做的更改可能会影响到域范围。 不要委托此容器的控制;它必须由服务管理员控制。  
  
## <a name="users-and-computers-containers"></a>用户和计算机容器  
当你执行就地域从 Windows Server 2003 到 Windows Server 2008 的升级时，现有的用户和计算机会自动放入 "用户" 和 "计算机" 容器。 如果要创建新的 Active Directory 域，则 "用户和计算机" 容器是域中所有新用户帐户和非域控制器计算机帐户的默认位置。  
  
> [!IMPORTANT]  
> 如果需要对用户或计算机委派控制，请不要在 "用户和计算机" 容器上修改默认设置。 相反，请创建新 Ou （根据需要），并将用户和计算机对象从其默认容器移动到新 Ou。 根据需要委托对新 Ou 的控制。 建议您不要修改控制默认容器的用户。  
  
此外，不能将组策略设置应用于默认的 "用户和计算机" 容器。 若要将组策略应用于用户和计算机，请创建新的 Ou，并将用户和计算机对象移到这些 Ou 中。 将组策略设置应用到新 Ou。  
  
或者，您可以重定向要放置在所选容器中的对象的创建。  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>众所周知的用户和组以及内置帐户  
默认情况下，会在新域中创建多个已知的用户和组以及内置帐户。 建议对这些帐户进行管理，以控制服务管理员。 不要将这些帐户的管理委托给不是服务管理员的个人。 下表列出了众所周知的用户和组以及需要保持对服务管理员控制的内置帐户。  
  
|众所周知的用户和组|内置帐户|  
|--------------------------------|----------------------|  
|Cert Publishers<br /><br />域控制器<br /><br />Group Policy Creator Owners<br /><br />KRBTGT<br /><br />域来宾<br /><br />管理员<br /><br />Domain Admins<br /><br />架构管理员（仅限林根域）<br /><br />Enterprise Admins （仅限林根域）<br /><br />域用户|管理员<br /><br />Guest<br /><br />Guests<br /><br />Account Operators<br /><br />Administrators<br /><br />Backup Operators<br /><br />传入林信任生成器<br /><br />打印操作员<br /><br />Windows 之前2000兼容访问<br /><br />Server Operators<br /><br />用户|  
  
## <a name="domain-controller-ou"></a>域控制器 OU  
向域中添加域控制器时，会自动将其计算机对象添加到域控制器 OU。 此 OU 应用了一组默认策略。 为了确保这些策略统一应用于所有域控制器，我们建议你不要将域控制器的计算机对象移出此 OU。 如果无法应用默认策略，可能会导致域控制器无法正常工作。  
  
默认情况下，服务管理员控制此 OU。 不要将此 OU 的控制权委派给除服务管理员之外的其他人员。  
  


