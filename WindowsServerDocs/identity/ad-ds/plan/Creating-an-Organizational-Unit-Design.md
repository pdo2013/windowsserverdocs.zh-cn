---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: 创建组织单位设计
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a699a30cce3b330c434fdb3784214de3a2daa403
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408953"
---
# <a name="creating-an-organizational-unit-design"></a>创建组织单位设计

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

林所有者负责为其域创建组织单位（OU）设计。 创建 OU 设计涉及到设计 OU 结构、分配 OU 所有者角色以及创建帐户和资源 Ou。  
  
最初，设计 OU 结构以启用管理委派。 OU 设计完成后，您可以为应用程序组策略的应用程序创建更多 OU 结构，并限制对象的可见性。 有关详细信息，请参阅设计组策略基础结构（[@no__t](https://go.microsoft.com/fwlink/?LinkId=106655)）。  
  
## <a name="ou-owner-role"></a>OU 所有者角色  
林所有者为你为域设计的每个 OU 指定一个 OU 所有者。 OU 所有者是在 Active Directory 域服务（AD DS）中控制对象子树的数据管理器。 OU 所有者可以控制委派管理的方式，以及如何将策略应用到其 OU 中的对象。 他们还可以创建新的子树并委派这些子树内 Ou 的管理。  
  
由于 OU 所有者不拥有或控制目录服务的操作，因此你可以将目录服务的所有权和管理与对象的所有权和管理分开，从而减少具有高级别的服务管理员的数量的访问权限。  
  
Ou 提供管理自治和控制目录中对象的可见性的方法。 Ou 提供与其他数据管理员的隔离，但不提供与服务管理员的隔离。 虽然 OU 所有者可以控制对象的子树，但林所有者仍保留对所有子树的完全控制。 这样，林所有者就可以更正错误，如访问控制列表（ACL）中的错误，以及在终止数据管理员时回收委派的子树。  
  
## <a name="account-ous-and-resource-ous"></a>帐户 Ou 和资源 Ou  
帐户 Ou 包含用户、组和计算机对象。 林所有者必须创建 OU 结构来管理这些对象，然后将结构控制委派给 OU 所有者。 如果要部署新的 AD DS 域，请为该域创建帐户 OU，以便可以委派域中帐户的控制。  
  
资源 Ou 包含负责管理这些资源的资源和帐户。 林所有者还负责创建 OU 结构来管理这些资源并将该结构的控制权委托给 OU 所有者。 根据组织中每个组的要求，根据组织中的每个组的要求创建资源 Ou，以便管理数据和设备。  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>记录每个域的 OU 设计  
汇编团队，设计用于委派对林中资源的控制的 OU 结构。 林所有者可能涉及设计过程，并且必须批准 OU 设计。 你可能还至少涉及一个服务管理员，以确保此设计有效。 其他设计团队参与者可能会将负责处理 Ou 和 OU 所有者的数据管理员包括在内。  
  
务必记录 OU 设计。 列出你计划创建的 Ou 的名称。 对于每个 OU，记录 OU 类型、OU 所有者、父 OU （如果适用）以及该 OU 的来源。  
  
对于用于帮助您记录 OU 设计的工作表，请从 Windows Server 2003 部署工具包的作业助手（[https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)）下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "标识每个域的 ou"（DSSLOGI_9）。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [查看 OU 设计概念](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [通过使用 OU 对象委派管理](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


