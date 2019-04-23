---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: 委派默认容器和 OU 管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d482854fd82b4bf0d0e61315d36e6222470ca55
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830888"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>委派默认容器和 OU 管理

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

每个 Active Directory 域包含一组标准的容器和在 Active Directory 域服务 (AD DS) 的安装过程中创建组织单位 (Ou)。 例如：  
  
-   域容器，用作到层次结构的根容器  
  
-   保存服务管理员帐户的默认值的内置容器  
  
-   在域中创建的用户容器，这是新用户帐户和组的默认位置  
  
-   在域中创建的是新的计算机帐户的默认位置的计算机容器  
  
-   是域控制器计算机帐户的计算机帐户的默认位置的域控制器 OU  
  
林所有者控制这些默认容器和 Ou。  
  
## <a name="domain-container"></a>域容器  
域容器为根容器的层次结构的域。 对策略或访问控制列表 (ACL) 此容器上的更改可能会产生全域性影响。 未委派此容器; 的控制它必须由服务管理员控制。  
  
## <a name="users-and-computers-containers"></a>用户和计算机容器  
当你从 Windows Server 2003 中执行原位域升级到 Windows Server 2008 时，现有用户和计算机自动放入用户和计算机容器。 如果要创建新的 Active Directory 域、 用户和计算机容器是所有新用户帐户和域中的非域控制器计算机帐户的默认位置。  
  
> [!IMPORTANT]  
> 如果您需要委派控制权限的用户或计算机，不会修改的用户和计算机上的默认设置容器。 相反，创建新的 Ou （根据需要），移动用户和计算机对象从其默认容器拖到新 Ou。 根据需要则委派控制权限的新 Ou。 我们建议你不修改由谁来控制默认容器。  
  
此外，不能将组策略设置应用于的默认用户和计算机容器。 若要将组策略应用到用户和计算机，创建新的 Ou 并将用户和计算机对象移到这些 Ou。 将组策略设置应用于新 Ou。  
  
（可选） 可以重定向对象，并放置在要放置在所选的容器中的默认容器中的创建。  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>熟知的用户和组以及内置帐户  
默认情况下，多个已知的用户和组以及内置帐户创建新域中。 我们建议这些帐户的管理仍受控制的服务管理员。 不将这些帐户的管理委托给不是服务管理员的人员。 下表列出了已知的用户和组以及需要的服务管理员控制的内置帐户。  
  
|熟知的用户和组|内置帐户|  
|--------------------------------|----------------------|  
|Cert Publishers<br /><br />域控制器<br /><br />Group Policy Creator Owners<br /><br />KRBTGT<br /><br />域来宾<br /><br />管理员<br /><br />Domain Admins<br /><br />架构管理员 （仅限目录林根域）<br /><br />企业管理员 （仅限目录林根域）<br /><br />域用户|管理员<br /><br />Guest<br /><br />Guests<br /><br />Account Operators<br /><br />Administrators<br /><br />Backup Operators<br /><br />传入林信任构建者<br /><br />打印操作员<br /><br />Pre-Windows 2000 Compatible Access<br /><br />Server Operators<br /><br />用户|  
  
## <a name="domain-controller-ou"></a>域控制器 OU  
当域控制器添加到域时，其计算机对象是自动添加到域控制器 OU 中。 此 OU 具有一组默认的策略应用于它。 若要确保这些策略统一应用于所有域控制器，我们建议你不移出此 OU 的域控制器的计算机对象。 若要应用默认策略可能导致域控制器无法正常工作。  
  
默认情况下，服务管理员控制此 OU。 不将控制此 OU 的委派给非服务管理员的人员。  
  


