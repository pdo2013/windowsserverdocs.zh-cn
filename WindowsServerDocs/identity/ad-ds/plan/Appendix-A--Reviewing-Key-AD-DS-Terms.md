---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: 附录 A-查看关键的 AD DS 条款
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5434db4124c471c613f159dec28e27dee70e7086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852018"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>附录 A：查看关键的 AD DS 条款

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

以下术语都与 Windows Server 2008 Active Directory 域服务 (AD DS) 的部署过程相关。  
  
## <a name="active-directory-domain"></a>Active Directory 域  
为便于进行管理，组包括以下几个功能的计算机网络中管理单元：  
  
-   **整个网络的用户标识**。 在域中，可以创建一次，然后引用加入到域所在的林的任何计算机上的用户标识。 组成域的域控制器安全地存储用户帐户和用户凭据，如密码或证书。  
  
-   **身份验证**。 域控制器提供用户的身份验证服务。 它们还提供其他授权数据，例如用户组成员身份。 管理员可以使用这些服务来控制对网络上的资源的访问。  
  
-   **信任关系**。 域通过自动双向信任将身份验证服务扩展到其自己林中其他域中的用户。 域还会向其他林中的域中的用户的身份验证服务通过林信任或手动创建外部信任扩展。  
  
-   **策略管理**。 域是管理策略的作用域，如密码复杂性和密码重用规则。  
  
-   **复制**。 域定义的分区提供的数据足以提供所需的服务和域控制器之间的复制的目录树。 这样一来，所有域控制器都是在域中，对等节点和它们作为一个单元都进行管理。  
  
## <a name="active-directory-forest"></a>Active Directory 林  
一个或多个共享公用逻辑结构、 目录架构和网络配置，以及自动的双向可传递信任关系的 Active Directory 域的集合。 每个林的目录中，单个实例，它定义了安全边界。  
  
## <a name="active-directory-functional-level"></a>Active Directory 功能级别  
设置 AD DS 启用高级的全域性或全林性 AD DS 功能。  
  
## <a name="migration"></a>迁移  
将对象从源域移动到目标域，同时保留或修改要对其进行访问新的域中的对象的特征的过程。  
  
## <a name="domain-restructure"></a>域重组  
迁移过程，涉及更改林的域结构。 域重组可以包括合并或域中，添加，并且它可以是林之间或林内部的。  
  
## <a name="domain-consolidation"></a>域合并  
涉及合并其内容与其他域的内容通过消除 AD DS 域的重组过程。  
  
## <a name="domain-upgrade"></a>域升级  
升级到更高版本的目录服务的域的目录服务的过程。 这包括升级所有域控制器上的操作系统和引发的 AD DS 功能级别 （如果适用）。  
  
## <a name="in-place-domain-upgrade"></a>原位域升级  
例如升级操作系统的给定域中的所有域控制器、 将 Windows Server 2003 升级到 Windows Server 2008 和保留域对象，例如用户时才适用，增加在域中的功能级别的过程和组，在位置。  
  
## <a name="forest-root-domain"></a>目录林根域  
在 Active Directory 林中创建第一个域。 此域自动指定为目录林根域。 为 Active Directory 林基础结构，它提供了基础。  
  
## <a name="regional-domain"></a>地区域  
在要优化复制流量的地理区域中创建的子域。  
  


