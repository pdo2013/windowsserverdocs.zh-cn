---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: 委派帐户 OU 和资源 OU 管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 63fda63d5a34404563bab44ee54ba2e22d852782
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402699"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>委派帐户 OU 和资源 OU 管理

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

帐户组织单位（Ou）包含用户、组和计算机对象。 资源 Ou 包含负责管理这些资源的资源和帐户。 林所有者负责创建 OU 结构来管理这些对象和资源，并将该结构的控制权委托给 OU 所有者。  
  
## <a name="delegating-administration-of-account-ous"></a>委派帐户 Ou 的管理  
如果需要创建和修改用户、组和计算机对象，请将帐户 OU 结构委派给数据管理员。 帐户 OU 结构是必须单独控制的每个帐户类型的 Ou 的子树。 例如，对于用户、计算机、组和服务帐户，OU 所有者可以通过帐户 OU 中的子 Ou 将特定控件委托给不同的数据管理员。  
  
下图显示了一个帐户 OU 结构的示例。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
下表列出并描述了可以在帐户 OU 结构中创建的可能的子 Ou。  
  
|OU|用途|  
|------|-----------|  
|用户|包含非管理员用户帐户。|  
|服务帐户|某些需要访问网络资源的服务以用户帐户的身份运行。 将创建此 OU，以便将用户 OU 中包含的用户帐户中的服务用户帐户分隔开来。 此外，将不同类型的用户帐户放在不同的 Ou 中，可以根据特定的管理要求对其进行管理。|  
|计算机|包含除域控制器之外的其他计算机的帐户。|  
|组|包含除管理组之外的所有类型的组，管理组是单独管理的。|  
|管理员|包含帐户 OU 结构中的数据管理员的用户和组帐户，以允许它们与普通用户分开管理。 为此 OU 启用审核，以便跟踪对管理用户和组所做的更改。|  
  
下图显示了帐户 OU 结构的管理组设计的一个示例。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
管理子 Ou 的组被授予对它们负责管理的特定对象类的 "完全控制" 权限。  
  
用于在 OU 结构内委托控件的组类型基于帐户相对于要管理的 OU 结构的位置。 如果管理员用户帐户和 OU 结构都存在于单个域中，则你创建用于委派的组必须是全局组。 如果你的组织有一个管理其自己的用户帐户且存在于多个地理区域的部门，则你可能有一组数据管理员负责管理多个域中的帐户 Ou。 如果数据管理员的帐户都存在于单个域中，并且你在需要委派控制的多个域中具有 OU 结构，请使这些管理帐户成为全局组的成员，并委派每个组中的 OU 结构的控制域到这些全局组。 如果委派 OU 结构控制的数据管理员帐户来自多个域，则必须使用通用组。 通用组可以包含来自不同域的用户，因此，它们可用于在多个域中委派控制。  
  
## <a name="delegating-administration-of-resource-ous"></a>委派资源 Ou 的管理  
资源 Ou 用于管理对资源的访问。 资源 OU 所有者为加入到域中的服务器创建计算机帐户，其中包括文件共享、数据库和打印机等资源。 资源 OU 所有者还会创建组来控制对这些资源的访问权限。  
  
下图显示了资源 OU 的两个可能的位置。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
资源 OU 可以位于域根下，也可以是 OU 管理层次结构中相应帐户 OU 的子 OU。 资源 Ou 没有任何标准的子 Ou。 计算机和组直接放置在资源 OU 中。  
  
资源 OU 所有者拥有 OU 中的对象，但不拥有 OU 容器本身。 资源 OU 所有者只管理计算机和组对象;它们不能在 OU 中创建其他类的对象，也不能创建子 Ou。  
  
> [!NOTE]  
> 对象的创建者或所有者能够在对象上设置访问控制列表（ACL），而无需考虑从父容器继承的权限。 如果资源 OU 所有者可以重置 OU 上的 ACL，则该所有者可以在 OU 中创建对象的任何类，包括用户。 出于此原因，不允许资源 OU 所有者创建 Ou。  
  
对于域中的每个资源 OU，请创建一个全局组来表示负责管理 OU 内容的数据管理员。 此组可以完全控制 OU 中的组和计算机对象，而不是 OU 容器本身。  
  
下图显示了资源 OU 的管理组设计。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
如果将计算机帐户放入资源 OU，则 OU 所有者可以控制帐户对象，但不会使 OU 所有者成为计算机的管理员。 在 Active Directory 域中，默认情况下，域管理员组位于所有计算机上的本地管理员组中。 也就是说，服务管理员可以控制这些计算机。 如果资源 OU 所有者要求对其 Ou 中的计算机进行管理控制，则林所有者可以应用受限制的组组策略以使资源 OU 所有者成为该 OU 中计算机上 Administrators 组的成员。  
  


