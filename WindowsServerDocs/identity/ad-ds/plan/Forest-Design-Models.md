---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: 林设计模型
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 436bbc7038e9797d194f4b4a6ea65d88c0b8279a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402566"
---
# <a name="forest-design-models"></a>林设计模型

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可以在 Active Directory 环境中应用以下三种林设计模型之一：  
  
-   组织林模型  
  
-   资源林模型  
  
-   受限访问林模型  
  
您可能需要结合使用这些模型，以满足您的组织中所有不同组的需求。  
  
## <a name="organizational-forest-model"></a>组织林模型  
在组织林模型中，用户帐户和资源包含在林中，并单独进行管理。 如果将林配置为阻止访问林外部的任何人，则可以使用组织林来提供服务自治、服务隔离或数据隔离。  
  
如果组织林中的用户需要访问其他林中的资源（或相反），则可以在一个组织林与其他林之间建立信任关系。 这样，管理员就可以授予对其他林中资源的访问权限。 下图显示了组织林模型。  
  
![林设计模型](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
每个 Active Directory 设计至少包含一个组织林。  
  
## <a name="resource-forest-model"></a>资源林模型  
在资源林中，单独的林用于管理资源。 如果组织林中的用户帐户不可用，则资源林不包含服务管理所需的用户帐户，以及提供对该林中资源的备用访问权限所需的用户帐户。 已建立林信任，以便其他林中的用户可以访问资源林中包含的资源。 下图显示了资源林模型。  
  
![林设计模型](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
资源林提供服务隔离，用于保护需要维护高可用性状态的网络区域。 例如，如果您的公司包含的制造设施需要在网络的其余部分出现问题时继续运行，则可以为该生产组创建单独的资源林。  
  
## <a name="restricted-access-forest-model"></a>受限访问林模型  
在受限制的访问林模型中，将创建一个单独的林，以包含必须与组织的其余部分隔离的用户帐户和数据。 受限制的访问林在威胁项目数据的后果严重的情况下提供数据隔离。 下图显示了受限制的访问林模型。  
  
![林设计模型](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
由于不存在信任，无法向其他林中的用户授予对受限制数据的访问权限。 在此模型中，用户在组织林中具有一个帐户，用于访问常规组织资源，以及访问受限制访问林中的单独用户帐户以访问已分类数据。 这些用户必须有两个单独的工作站，一个工作站连接到组织林，另一个连接到受限制的访问林。 这可以防止一个林的服务管理员获得对受限林中工作站的访问权限。  
  
在极个别的情况下，受限的访问林可能会保留在单独的物理网络上。 使用分类政府项目的组织有时会在不同的网络上维护受限制的访问林，以满足安全要求。  
  


