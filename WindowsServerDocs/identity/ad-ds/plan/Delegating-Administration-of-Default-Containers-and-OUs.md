---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: "委派默认容器和华丽绚烂的管理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2504208210c03193451d19478f3bc8c98ec98f23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>委派默认容器和华丽绚烂的管理

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

每个 Active Directory 域包含一组标准容器和组织的 Active Directory 域服务 (广告 DS) 安装过程中创建的单位 （华丽绚烂）。 这些如下所示：  
  
-   作为到此目录结构根容器域容器  
  
-   内置容器，其中包含默认的服务管理员帐户  
  
-   在域中创建用户容器，这是默认位置为新的用户帐户和组  
  
-   计算机容器，这是默认位置为新的计算机帐户创建在域中  
  
-   域控制器，这是域控制器计算机帐户的计算机帐户的默认位置  
  
森林所有者控制这些默认容器和华丽绚烂。  
  
## <a name="domain-container"></a>域容器  
域容器是域分层根容器。 对策略或访问控制列表 (ACL) 这个容器更改可能会产生域范围内影响。 不委派此容器; 的控件必须受的服务管理员。  
  
## <a name="users-and-computers-containers"></a>用户和计算机容器  
当从 Windows Server 2003 执行就地域升级到 Windows Server 2008 时，用户和计算机的现有自动放入计算机容器和的用户。 如果你要创建新的 Active Directory 域，用户的计算机容器是默认位置为所有的新用户帐户和非域控制器计算机在域中的帐户。  
  
> [!IMPORTANT]  
> 如果你需要委托控制的用户或计算机时，不会修改的用户和计算机默认设置容器。 不过，创建新华丽绚烂 （如时才需要） 和移动从其默认容器拖到新华丽绚烂的用户和计算机的对象。 根据需要委派新华丽绚烂，控制。 我们建议你未修改谁控制的默认容器。  
  
此外，不能对默认用户和计算机应用组策略设置容器。 若要将组策略应用到用户和计算机，创建新华丽绚烂，并移动到这些华丽绚烂的用户和计算机的对象。 适用于新华丽绚烂的组策略设置。  
  
（可选），你可以重定向创建的放置在默认容器处于容器你选择的对象。  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>已知的用户和组以及内置的帐户  
默认情况下，几个已知的用户和组以及内置的帐户创建新的域。 我们建议管理这些帐户保持控制的服务管理员。 不将这些帐户的管理委派到不服务管理员的人员。 下表列出了已知的用户和组以及内置需要控制的服务管理员保留的帐户。  
  
|已知的用户和组|内置的帐户|  
|--------------------------------|----------------------|  
|证书发布者<br /><br />域控制器<br /><br />组策略 Creator 所有者<br /><br />KRBTGT<br /><br />域来宾<br /><br />管理员<br /><br />域管理员<br /><br />方案管理员 （仅限于域森林根）<br /><br />企业管理员 （仅限于域森林根）<br /><br />用户域|管理员<br /><br />访客<br /><br />来宾<br /><br />帐户运营商<br /><br />管理员<br /><br />备份运营商<br /><br />传入森林信任构建器<br /><br />打印运营商<br /><br />Windows 2000 兼容访问<br /><br />服务器运营商<br /><br />用户|  
  
## <a name="domain-controller-ou"></a>域控制器 OU  
时，域控制器添加到域，他们计算机对象将自动添加到域控制器 OU。 此 OU 有一默认组策略应用到它。 为了确保这些策略统一应用于所有域控制器，我们建议你无法移动利用此 OU 域控制器计算机对象。 若要将默认策略应用可能会导致无法正常工作的域控制器。  
  
默认情况下的服务管理员控制此 OU。 没有将此 OU 控制委派给以外的服务管理员个人。  
  


