---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: 林设计模型
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aca59a31f75628b311a92bd842d93ee91408dc4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819788"
---
# <a name="forest-design-models"></a>林设计模型

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以在 Active Directory 环境中应用以下三个林设计模型之一：  
  
-   组织林模型  
  
-   资源林模型  
  
-   受限制的访问林模型  
  
很可能需要使用这些模型的组合来满足你的组织中的所有不同组的需求。  
  
## <a name="organizational-forest-model"></a>组织林模型  
在组织林模型中，用户帐户和资源包含在林中并单独管理。 可以使用组织的林提供了服务自治性、 服务隔离或数据隔离，如果林配置为阻止访问林之外的任何对象。  
  
如果组织林中的用户需要访问其他林 （或反之） 中的资源，可以组织林和其他林之间建立信任关系。 这使管理员能够授予对在其他林中的资源的访问。 下图显示组织林模型。  
  
![林设计模型](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
每个 Active Directory 的设计包括至少一个组织的林。  
  
## <a name="resource-forest-model"></a>资源林模型  
在资源林模型中，单独的林中用于管理资源。 资源林不包含不是所需的服务管理和所需以提供对该林中的资源的备用访问组织的林中的用户帐户都不可用的用户帐户。 建立林信任，以便其他林中的用户可以访问包含在资源林中的资源。 下图显示资源林模型。  
  
![林设计模型](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
资源林可以提供用于保护需要维护状态的高可用性的网络区域的服务隔离。 例如，如果你的公司包括制造设施需要继续运行在网络中的其他问题时，可以创建用于制造组的单独的资源林。  
  
## <a name="restricted-access-forest-model"></a>受限制的访问林模型  
在受限制的访问林模型中，将创建单独的林以包含用户帐户和数据，以便为组织的其余部分隔离。 受限制的访问林可以提供影响项目数据的后果严重的情况中的数据隔离。 下图显示了一个受限制的访问林模型。  
  
![林设计模型](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
其他林中的用户不能对受限制的数据的访问授予因为不存在信任。 在此模型中，用户可以对已分类数据的访问受限的访问林中中用于常规的组织资源的访问权限的组织林的帐户和单独的用户帐户。 这些用户必须具有两个单独的工作站，一个连接到组织的林和其他连接到受限的访问林。 这可避免从林的服务管理员能够访问受限制的林中的工作站的可能性。  
  
在极端情况下，受限制的访问林可能保留在单独的物理网络。 有时会处理已分类的政府项目的组织继续保留在单独的网络来满足安全要求的受限的访问林。  
  


