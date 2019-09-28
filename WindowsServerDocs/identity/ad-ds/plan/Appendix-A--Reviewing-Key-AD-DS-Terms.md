---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: 附录 A-查看关键 AD DS 术语
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 81beba874440f7a75c2d7932357fae70f046d996
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409013"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>附录 A：查看关键 AD DS 术语

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以下术语与 Windows Server 2008 Active Directory 域服务（AD DS）的部署过程相关。  
  
## <a name="active-directory-domain"></a>Active Directory 域  
计算机网络中的一个管理单元，为了便于管理，将多个功能组合在一起，其中包括：  
  
-   **网络范围的用户标识**。 在域中，可以创建一次用户标识，然后在加入域所在的林的任何计算机上引用用户标识。 安全地构成域存储用户帐户和用户凭据（如密码或证书）的域控制器。  
  
-   **身份验证**。 域控制器为用户提供身份验证服务。 它们还提供附加的授权数据，如用户组成员身份。 管理员可以使用这些服务来控制对网络上资源的访问。  
  
-   **信任关系**。 域通过自动双向信任，将身份验证服务扩展到其自身林中其他域中的用户。 域还通过林信任或手动创建外部信任，将身份验证服务扩展到其他林中域中的用户。  
  
-   **策略管理**。 域是管理策略的作用域，如密码复杂性和密码重复使用规则。  
  
-   **复制**。 域定义了目录树的分区，该分区提供的数据足以提供所需的服务并在域控制器之间进行复制。 通过这种方式，所有域控制器在域中都是对等的，它们作为一个单元进行管理。  
  
## <a name="active-directory-forest"></a>Active Directory 林  
一个或多个共享公共逻辑结构、目录架构和网络配置的 Active Directory 域的集合，以及自动的双向可传递信任关系。 每个林都是目录的一个实例，它定义一个安全边界。  
  
## <a name="active-directory-functional-level"></a>Active Directory 功能级别  
一种 AD DS 设置，它启用高级全域性或全林性 AD DS 功能。  
  
## <a name="migration"></a>迁移  
将对象从源域移动到目标域的过程，同时保留或修改对象的特征，使其在新域中可访问。  
  
## <a name="domain-restructure"></a>域重构  
涉及更改林的域结构的迁移过程。 域重构可能涉及到域合并或添加域，并且它可以在林之间或林中进行。  
  
## <a name="domain-consolidation"></a>域合并  
一个重构过程，其中包括通过将其内容与其他域的内容合并来消除 AD DS 域。  
  
## <a name="domain-upgrade"></a>域升级  
将域的目录服务升级到更高版本的目录服务的过程。 这包括升级所有域控制器上的操作系统，并在适用的情况下提升 AD DS 功能级别。  
  
## <a name="in-place-domain-upgrade"></a>就地域升级  
升级给定域中所有域控制器的操作系统的过程（例如，将 Windows Server 2003 升级到 Windows Server 2008），并在保留域对象（如用户）的同时提高域的功能级别。和组。  
  
## <a name="forest-root-domain"></a>目录林根级域  
在 Active Directory 林中创建的第一个域。 此域自动指定为目录林根级域。 它为 Active Directory 林基础结构奠定了基础。  
  
## <a name="regional-domain"></a>地区性域  
在地理区域中创建的子域，用于优化复制流量。  
  


