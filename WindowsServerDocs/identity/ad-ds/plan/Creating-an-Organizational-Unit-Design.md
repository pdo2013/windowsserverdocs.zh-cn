---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: "创建单位设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3a74770a4558c79d1f9250f37181562e1d235d34
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="creating-an-organizational-unit-design"></a>创建单位设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

森林所有者负责创建部门 (OU) 设计为他们的域。 OU 设计涉及设计 OU 结构，分配 OU 负责人的角色和创建帐户和华丽绚烂的资源。  
  
最初，设计启用管理委派你 OU 结构。 OU 设计完成后，可以创建其他 OU 结构组策略应用程序用户和计算机，并限制的可见性物体。 有关详细信息，请参阅设计组策略基础结构 ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655))。  
  
## <a name="ou-owner-role"></a>OU 负责人的角色  
森林所有者指定为域设计每个 OU OU 所有者。 所有者 OU 是数据经理控制子树中 Active Directory 域服务 (广告 DS) 的对象。 OU 的所有者可以控制如何进行管理委派和策略如何应用到他们 OU 内对象。 它们还可创建新的子树和委派内这些子树华丽绚烂的管理。  
  
因为 OU 所有者不拥有或控制目录服务的操作，你可以分开目录服务的所有权和管理所有权和管理的对象，减少了具有高级别的访问权限的服务管理员。  
  
华丽绚烂提供管理自主和来控制可见性物体目录中的方式。 华丽绚烂提供隔离从其他数据管理员，但它们不提供的服务管理员隔离。 尽管 OU 的所有者可以控制的对象子树，森林所有者将保留所有子树的完整控制。 这可使森林所有者更正错误，如访问控制 (ACL，) 列表中的错误和管理员数据终止时，释放委派子树。  
  
## <a name="account-ous-and-resource-ous"></a>帐户华丽绚烂和资源华丽绚烂  
帐户华丽绚烂包含用户、组和计算机的对象。 森林所有者必须创建一个 OU 结构用于管理这些对象，然后到 OU 所有者委派的结构控制。 如果你正在部署新的广告 DS 域，创建 OU 域帐户，以便你可以委派域中的帐户的控制。  
  
资源华丽绚烂包含资源和负责管理这些资源的帐户。 森林所有者是还负责创建一个 OU 结构用于管理这些资源以及的 OU 所有者分配，结构的控件。 资源华丽绚烂根据需要创建基于每个组中的数据以及设备组织进行管理自主要求。  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>记录 OU 设计为每个域  
组装设计用于委派林中资源的控制 OU 结构的团队。 森林所有者可能会涉及到在设计过程中，并且必须批准 OU 设计。 你可能还涉及到在至少有一个的服务管理员，确保设计有效。 其他设计团队参与者可能包含的数据管理员适用于华丽绚烂，而且将负责管理它们的所有者。  
  
务必记录 OU 设计。 列表中的计划来创建华丽绚烂的名称。 为每个 OU，文档类型 OU、OU 所有者、家长 OU（如果适用），以及该 OU 的来源。  
  
对于以帮助你记录 OU 设计工作表，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip)。  
  
## <a name="in-this-section"></a>在此部分中  
  
-   [查看 OU 设计概念](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [通过使用 OU 对象委派管理](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


