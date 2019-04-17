---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: "委派管理的帐户华丽绚烂和资源华丽绚烂"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56e69bffdf8ecc0f37f9f5159ae6bce7fcdc49f8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>委派管理的帐户华丽绚烂和资源华丽绚烂

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

帐户组织单位 （华丽绚烂） 中包含用户、 组和计算机的对象。 资源华丽绚烂包含资源和负责管理这些资源的帐户。 森林所有者是负责创建一个 OU 结构用于管理这些对象和资源，以及的 OU 所有者分配，结构的控件。  
  
## <a name="delegating-administration-of-account-ous"></a>委派管理的帐户华丽绚烂  
将帐户 OU 结构委派给管理员数据，如果他们需要创建和修改用户、 组和计算机的对象。 帐户 OU 结构是必须独立地控制每个帐户类型华丽绚烂的子树。 例如，OU 所有者可以委派特定控制数据的各种管理员帐户的用户、 的计算机、 组 OU 和服务帐户中的孩子华丽绚烂。  
  
下图显示帐户 OU 结构一个的示例。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
下表列出，并描述你可以创建 OU 结构帐户中的孩子可以华丽绚烂。  
  
|OU|目的|  
|------|-----------|  
|用户|包含人员非管理员用户帐户。|  
|服务帐户|需要访问网络资源的某些服务运行为用户帐户。 此 OU 用户 OU 中包含的用户帐户创建单独的服务用户帐户。 此外，放在单独的华丽绚烂的不同类型的用户帐户使您能够对它们进行管理根据其特定管理需求。|  
|计算机|包含计算机域控制器以外的帐户。|  
|组|包含除外管理这些组单独管理所有类型的组。|  
|管理员|包含的数据管理员帐户 OU 结构允许他们从常规用户单独管理的用户和组帐户。 启用审核此 OU，以便你可以更改跟踪到管理用户和组。|  
  
下图显示帐户 OU 结构管理组设计的一个的示例。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
管理孩子华丽绚烂的组授予只对特定的对象它们负责管理类完全控制。  
  
使用委派内 OU 结构控件的组类型的基于相对于 OU 结构要管理的帐户的位置。 管理用户帐户和 OU 结构存在一个域中，如果你创建用于委派组必须全局组。 如果你的组织有部门，它管理其自己的用户帐户和存在多个地理区域中，你可能已数据管理员负责管理的帐户中的多个域华丽绚烂的组。 如果你需要委派控制的多个域中有 OU 结构的所有数据管理员帐户存在一个域中，使这些管理帐户组成员的全球和委派 OU 结构每个域向全球的这些组中的控制。 如果您向其中委派 OU 结构控制数据管理员帐户来自多个域中，你必须使用通用组。 通用组可能包含来自不同域的用户，因此，使用它们委派多个域中的控制。  
  
## <a name="delegating-administration-of-resource-ous"></a>委派资源华丽绚烂的管理  
资源华丽绚烂用于管理访问资源。 资源 OU 所有者创建计算机帐户连接到域包含文件共享、 数据库和打印机等资源的服务器。 资源 OU 所有者还可创建组来控制访问这些资源。  
  
下图显示的资源 OU 两个可能的位置。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
资源 OU 可以域根下或儿童 OU 管理分层中的相应帐户 OU OU 位于。 资源华丽绚烂没有任何标准孩子华丽绚烂。 计算机和组直接置于 OU 的资源。  
  
资源 OU 所有者拥有 OU 内的对象，但并不属于 OU 容器本身。 资源 OU 所有者管理仅计算机和组对象;它们不能创建其他类别的对象 OU 内,，并且不能创建孩子华丽绚烂。  
  
> [!NOTE]  
> 创建者或对象所有者无论从家长容器权限的对象具有设置访问控制列表 (ACL) 的能力。 资源 OU 所有者可以重置 ou ACL，如果该的所有者可以创建在 OU，包括用户的任何类别的对象。 出于此原因，不能创建华丽绚烂资源 OU 所有者。  
  
对于域中组织单位每个资源，创建全局组代表数据管理员负责管理 OU 的内容。 此组具有轻 OU 中的组和计算机对象又不会自行 OU 容器完全控制。  
  
下图显示的管理组设计为 OU 资源。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
计算机帐户投入资源 OU 使 OU 所有者帐户对象的控制，但不会使 OU 所有者的计算机管理员。 在 Active Directory 域中，域管理员放置组时，默认情况下，在所有计算机上本地。 即服务管理员可以在这些计算机上的控制。 如果资源 OU 所有者需要管理控制中其华丽绚烂的计算机，请森林所有者可以应用受限组组策略以使该 OU 在计算机上的资源 OU 所有者管理员组中的成员。  
  


