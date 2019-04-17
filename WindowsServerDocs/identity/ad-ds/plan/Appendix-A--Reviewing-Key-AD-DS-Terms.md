---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: "附录 A-查看关键广告 DS 条款"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ce7c0b639ded498b15200dfd594b3f34d6492dce
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>附录 a： 查看关键广告 DS 条款

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

以下条款将与 Windows Server 2008 Active Directory 域服务 (广告 DS) 的部署过程相关。  
  
## <a name="active-directory-domain"></a>域的 Active Directory  
为方便起见管理，进行分组几个功能，包括以下的计算机网络中管理单元：  
  
-   **网络范围内用户身份**。 在域中，可以创建一次，然后引用已加入域所在森林中的任何计算机上用户的身份。 域控制器构成域安全地存储用户帐户和用户的凭据，例如密码或证书。  
  
-   **身份验证**。 域控制器提供用户身份验证的服务。 他们还提供其他授权数据，如用户组成员。 管理员可以使用这些服务来控制访问权限的网络上的资源。  
  
-   **信任关系**。 通过自动双向信任域覆盖自己森林中的其他域中的用户身份验证服务。 域还会在其他森林中的域中向用户的身份验证服务通过林信任或手动创建外部信任扩展。  
  
-   **策略管理**。 域是范围的管理策略，如密码复杂性和密码重复使用规则。  
  
-   **复制**。 域定义提供了足够提供所需的服务并且，域控制器之间复制的数据的目录树分区。 这种方式，所有域控制器同事在域中，并且不作为一个整体进行管理。  
  
## <a name="active-directory-forest"></a>Active Directory 森林  
一个或多个共享常见的逻辑结构、 目录架构网络配置，以及自动、 双向、 传递信任与您的关系的 Active Directory 域集合。 每个森林是一份目录，并定义安全边界。  
  
## <a name="active-directory-functional-level"></a>Active Directory 功能级别  
设置的广告 DS 使高级的域范围内使用或森林范围内的广告 DS 功能。  
  
## <a name="migration"></a>迁移  
同时保留或修改的对象以使其在新的域中访问特征对象从源域移动到目标域，则的过程。  
  
## <a name="domain-restructure"></a>域重构  
涉及更改森林域结构迁移过程。 域重构可能会涉及整合或添加的域中，并在森林林之间或，才能进行。  
  
## <a name="domain-consolidation"></a>域合并  
涉及消除通过合并的其他域的内容与其内容的广告 DS 域改组过程。  
  
## <a name="domain-upgrade"></a>域进行升级  
升级到较新版本的目录服务目录服务的域的进程。 这包括升级所有域控制器上的操作系统和 （如果适用） 提升广告 DS 功能级别。  
  
## <a name="in-place-domain-upgrade"></a>位置域进行升级  
例如升级给定的域中的所有域控制器的操作系统、 升级到 Windows Server 2008、 Windows Server 2003 和同时保持就地域对象，用户和组，例如如果适用，请提升功能级别的域中，进程。  
  
## <a name="forest-root-domain"></a>森林根域  
第一个域的 Active Directory 树林中创建。 此域自动被指森林根域。 它为奠定 Active Directory 森林基础结构。  
  
## <a name="regional-domain"></a>区域域  
地理区域优化复制交通在创建孩子域。  
  


