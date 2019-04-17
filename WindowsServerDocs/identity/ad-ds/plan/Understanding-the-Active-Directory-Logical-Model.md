---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: "了解 Active Directory 逻辑模型"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f8cdc4789d1b3008f3b53104e5517d4ef1e65b9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="understanding-the-active-directory-logical-model"></a>了解 Active Directory 逻辑模型

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

为 Active Directory 域服务 (广告 DS) 设计你逻辑结构涉及定义之间目录中容器关系。 这些关系可能会根据管理的要求，如委派颁发机构，或运行的要求，如需控制复制可能定义。  
  
设计你 Active Directory 逻辑结构之前，请务必了解 Active Directory 逻辑模型。 广告 DS 是存储，并管理目录启用应用程序从网络资源，以及应用程序相关数据信息分布式的数据库。 广告 DS 到分层包容结构允许管理员整理元素网络（如用户、计算机和设备）。 顶级容器是森林。 森林在域中，以及在域中部门（华丽绚烂）。 这称为逻辑模型因为它不依赖于物理方面的部署，如需在每个域和网络拓扑域控制器的数量。  
  
## <a name="active-directory-forest"></a>Active Directory 森林  
森林是一个或多个共享常见的逻辑结构的 Active Directory 域的集合目录架构（类和属性定义）、目录配置（站点并复制信息）和全球目录（树林搜索功能）。 自动与双向可传递信任关系链接相同的树林中的域。  
  
## <a name="active-directory-domain"></a>域的 Active Directory  
域是 Active Directory 森林中的分区。 分区数据使组织复制仅向需要它的数据。 这种方式，directory 可以通过提供带宽有限网络全球缩放。 此外，域支持数量的其他核心功能与管理，包括：  
  
-   整个网络的用户的身份。 域允许用户身份要创建一次和引用连接到域所在的林任何计算机上。 域组成的域控制器用于安全地存储用户帐户和用户凭据（如密码或证书）。  
  
-   身份验证。 域控制器提供的用户身份验证服务，并提供其他授权数据，如用户组成员资格，它可以用来控制访问权限的网络上的资源。  
  
-   信任与您的关系。 域可通过信任可以延伸到域自己森林之外的用户身份验证服务。  
  
-   复制。 域定义分区包含足够的数据来提供服务域，然后将它复制域控制器之间的目录。 这种方式，所有的域控制器同级某个域中，并作为一个整体进行管理。  
  
## <a name="active-directory-organizational-units"></a>Active Directory 单位  
华丽绚烂可用于在某个域中形成容器的层次。 用于管理用途，例如的组策略应用程序或授权委派对象组合使用华丽绚烂。 （OU 和控制其中对象）由和 OU 中对象 OU 上访问控制列表 (Acl)。 为了便于大量对象管理，广告 DS 支持授权委派的概念。 委派，通过所有者可以将全部或有限管理控制对象转移到其他用户或组。 委派很重要，因为它有助于分布受信任的执行管理任务人数大量对象的管理。  
  


