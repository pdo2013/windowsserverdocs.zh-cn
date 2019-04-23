---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: 委派帐户 OU 和资源 OU 管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70900b44de85774e8e595f9691885d67682fa753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834258"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>委派帐户 OU 和资源 OU 管理

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

帐户组织单位 (Ou) 包含用户、 组和计算机对象。 资源 Ou 包含资源和负责管理这些资源的帐户。 林所有者是负责创建 OU 结构来管理这些对象和资源，并将该结构的控制权委派给 OU 所有者。  
  
## <a name="delegating-administration-of-account-ous"></a>委派帐户 Ou 管理  
向数据管理员委派帐户 OU 结构，如果他们需要创建和修改用户、 组和计算机对象。 帐户 OU 结构是必须单独控制每个帐户类型的 Ou 的子树。 例如，OU 所有者可以委派特定的控件到各种数据管理员通过子 Ou 中的帐户 OU 的用户、 计算机、 组和服务帐户。  
  
下图显示了一个示例中的帐户的 OU 结构。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
下表列出并描述的可能的子 Ou，您可以创建帐户的 OU 结构中。  
  
|OU|用途|  
|------|-----------|  
|用户|包含非管理人员的用户帐户。|  
|服务帐户|某些服务需要访问网络资源的用户帐户运行。 此 OU 中的用户 OU 包含的用户帐户从创建到单独的服务的用户帐户。 此外，将不同类型的用户帐户放在单独的 Ou，可根据其特定的管理要求对其进行管理。|  
|计算机|包含非域控制器的计算机帐户。|  
|组|包含除分开管理的管理组以外的所有类型的组。|  
|管理员|包含帐户的 OU 结构中数据管理员，以允许它们与普通用户单独管理用户和组帐户。 启用审核的此 OU，以便你可以跟踪对管理用户和组的更改。|  
  
下图显示了一个帐户 OU 结构的管理组设计的一个示例。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
管理子 Ou 的组授予仅对他们负责管理的对象的特定类的完全控制。  
  
使用 OU 结构中的控制委派的组类型的基于相对于 OU 结构，它是管理帐户的位置。 如果在单一域中存在的管理员用户帐户和 OU 结构，创建要用于委派的组必须是全局组。 如果你的组织有一个部门管理其自己的用户帐户，并存在于多个地理区域，您可能有的数据管理员负责管理帐户中多个域的 Ou 的组。 如果在单个域中存在的所有数据管理员的帐户，并且需要委派控制的多个域中具有 OU 结构，将这些管理帐户的全局组成员，并将在每个 OU 结构的控制委派向这些全局组的域。 如果数据管理员帐户的 OU 结构控制委派到来自多个域，则必须使用通用组。 通用组可以包含不同域中的用户，因此，使用它们来委派控制多个域中。  
  
## <a name="delegating-administration-of-resource-ous"></a>委派资源 Ou 管理  
资源 Ou 用于管理对资源的访问。 资源 OU 所有者可以创建加入到域包含资源，例如文件共享、 数据库和打印机的服务器的计算机帐户。 资源 OU 所有者还可以创建组以控制对这些资源的访问。  
  
下图显示资源 OU 的两个可能的位置。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
资源 OU 可以是位于域根下，也可以作为子 OU 管理层次结构中的相应帐户 OU 的 OU。 资源 Ou 不具有任何标准的子 Ou。 计算机和组直接置于资源 OU。  
  
资源 OU 所有者拥有在 OU 中的对象，但不拥有对 OU 容器本身。 资源 OU 所有者只能管理计算机和组对象;不能创建其他 OU 内的对象的类并不能创建子 Ou。  
  
> [!NOTE]  
> 创建者或所有者的对象具有而不考虑继承自父容器的权限对象上设置访问控制列表 (ACL) 的功能。 如果资源 OU 所有者可以重置 OU 上的 ACL，该所有者可以在 OU 中，包括用户创建对象的任何类。 出于此原因，不允许资源 OU 所有者创建的 Ou。  
  
对于每个资源域中的 OU 中，创建全局组来表示数据管理员负责管理 OU 的内容。 此组具有完全控制的 OU 中的组和计算机对象，但不是需要通过对 OU 容器本身。  
  
下图显示资源 OU 管理组设计。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
将计算机帐户放入资源 OU 允许对帐户对象的 OU 所有者控制，但不会使 OU 所有者的计算机的管理员。 在 Active Directory 域中，Domain Admins 组，默认情况下，位于中的所有计算机上的本地管理员组。 也就是说，服务管理员可以控制这些计算机。 如果资源 OU 所有者需要对其 Ou 中的计算机的管理控制，林所有者可以应用受限制组组策略以使该 OU 中的计算机上的资源 OU 所有者管理员组的成员。  
  


