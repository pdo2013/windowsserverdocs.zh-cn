---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: 了解 Active Directory 逻辑模型
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e909d4e9c1fb26aa0f7cb97a7dc06192db5cc21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834268"
---
# <a name="understanding-the-active-directory-logical-model"></a>了解 Active Directory 逻辑模型

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

为 Active Directory 域服务 (AD DS) 设计逻辑结构包括定义你的目录中的容器之间的关系。 这些关系可能会基于管理要求，如授权的委派，或者它们可能定义的操作要求，例如需要控制复制。  
  
设计 Active Directory 逻辑结构之前，务必了解 Active Directory 逻辑模型。 AD DS 是一个分布式的数据库，将存储和管理网络资源，以及特定于应用程序的数据有关的信息已启用目录的应用程序中。 AD DS 允许管理员 （如用户、 计算机和设备） 的网络元素组织到层次内嵌结构。 顶级容器是林。 在林的域，且在域中组织单位 (Ou)。 因为它是独立于部署，例如每个域和网络拓扑中所需的域控制器的数量的物理方面，这称为逻辑模型。  
  
## <a name="active-directory-forest"></a>Active Directory 林  
林是共享公用的逻辑结构的一个或多个 Active Directory 域的集合目录架构 （类和属性定义）、 目录配置 （站点和复制信息） 和全局编录 （全林性搜索功能）。 相同林中的域具有双向可传递信任关系自动链接。  
  
## <a name="active-directory-domain"></a>Active Directory 域  
域是在 Active Directory 林中的分区。 对数据进行分区使组织仅对需要它的位置复制数据。 这种方式，该目录即可全局缩放，通过网络可用带宽有限。 此外，域支持的其他核心数与管理相关的函数包括：  
  
-   整个网络的用户标识。 域可让用户标识来创建一次并将其加入到域所在的林的任何计算机上的引用。 组成域的域控制器用于安全地存储用户帐户和用户凭据 （如密码或证书）。  
  
-   身份验证。 域控制器提供用户的身份验证服务，并提供其他授权数据，例如用户组成员身份，这可用于控制对网络上的资源的访问。  
  
-   信任关系。 域信任关系通过，实现将身份验证服务扩展到超出其自己林中的域中的用户。  
  
-   复制。 域定义包含足够的数据来提供域服务，然后将其复制域控制器之间的目录分区。 这样一来，所有域控制器是域中的对等的并作为一个单元进行管理。  
  
## <a name="active-directory-organizational-units"></a>Active Directory 组织单位  
Ou 可以用于在一个域中形成的层次结构的容器。 Ou 出于管理目的，如组策略的应用程序或授权的委派用于到组对象。 （对一个 OU 和其中的对象） 控制由在 OU 中和在 OU 中对象的访问控制列表 (Acl)。 为了便于管理大量对象，AD DS 支持授权的委派的概念。 通过委派，所有者可将对对象的完全或有限管理控制传输到其他用户或组。 委派非常重要，因为它有助于跨多个被信任执行管理任务的人员分配的管理大量对象。  
  


