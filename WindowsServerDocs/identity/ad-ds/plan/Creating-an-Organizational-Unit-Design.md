---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: 创建组织单位设计
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9ecd0228b50a4e597c3fa1dbd3fdaf1f84ca1d3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830398"
---
# <a name="creating-an-organizational-unit-design"></a>创建组织单位设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

林所有者负责创建组织单位 (OU) 设计为他们的域。 创建 OU 设计涉及设计 OU 结构，将分配的 OU 所有者角色，并创建帐户和资源 Ou。  
  
最初，设计 OU 结构启用的管理委派。 OU 设计完成后，可以创建应用程序的组策略为用户和计算机，将限制对象的可见性的其他 OU 结构。 有关详细信息，请参阅设计组策略基础结构 ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655))。  
  
## <a name="ou-owner-role"></a>OU 所有者角色  
林所有者将指定的域设计每个 OU 的 OU 所有者。 OU 所有者是控制 Active Directory 域服务 (AD DS) 中的对象的子树的数据管理器。 OU 所有者可以控制如何委派管理以及如何将策略应用到在其 OU 内的对象。 它们还可以创建新的子树，并委派管理的 Ou 内这些子树。  
  
由于 OU 所有者不拥有或控制的目录服务操作，您可以分开目录服务的所有权和管理从所有权和管理的对象，减少的较高的服务管理员访问权限。  
  
Ou 提供管理自治和控制的目录中的对象的可见性的方法。 Ou 提供隔离来自其他数据管理员，但它们不从服务管理员提供隔离。 尽管 OU 所有者可以控制对象的子树，林所有者将保留完全控制所有子树。 这样，林所有者来更正错误，如访问控制列表 (ACL) 中的错误和回收时都将终止数据管理员委派子树。  
  
## <a name="account-ous-and-resource-ous"></a>帐户 Ou 和资源 Ou  
帐户 Ou 包含用户、 组和计算机对象。 林所有者必须创建一个 OU 结构，以管理这些对象，然后结构的控制权委派给 OU 所有者的。 如果要部署新的 AD DS 域，创建的帐户 OU 的域，以便可以委派的域中的帐户控制。  
  
资源 Ou 包含资源和负责管理这些资源的帐户。 林所有者也是负责创建 OU 结构来管理这些资源，并将该结构的控制权委派给 OU 所有者。 资源 Ou 根据需要创建基于组织中管理自治数据和设备的每个组的要求。  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>记录为每个域 OU 设计  
组建团队设计 OU 结构，用于委派对林中的资源的控制。 林所有者可能会涉及到在设计过程中，并必须批准 OU 设计。 此外可能涉及到至少一个服务管理员，以确保设计有效。 设计团队的其他参与者可能包括将适用于的 Ou 和 OU 将负责管理它们的所有者的数据管理员。  
  
请务必记录您的 OU 设计。 列出你打算创建 Ou 的名称。 而且，对于每个 OU 要记录的 OU、 OU 所有者、 父 OU （如果适用） 和该 OU 的源类型。  
  
有关可帮助您记录您的 OU 设计的工作表，作业辅助工具为 Windows Server 2003 部署工具包中下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，然后打开"确定每个域的 Ou"(DSSLOGI_9.doc)。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [查看 OU 设计概念](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [通过使用 OU 对象委派管理](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


