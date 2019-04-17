---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: "森林设计型号"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b24eba7173a4878102ac7b7a84f7322425df8a52
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="forest-design-models"></a>森林设计型号

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

您可以在您的 Active Directory 环境应用以下三种森林设计型号之一：  
  
-   组织林型号  
  
-   资源森林模型  
  
-   限制的访问森林模型  
  
很可能需要使用这些模型组合以满足你的组织中的所有其他组的需求。  
  
## <a name="organizational-forest-model"></a>组织林型号  
在组织林模型，用户帐户和资源是树林中包含和分开管理。 可以使用组织林来提供服务的独立、 服务隔离或数据隔离如果森林配置为防止森林之外的所有用户访问。  
  
如果组织的森林中的用户需要访问其他森林 （或反向操作） 中的资源，可以在一个组织林和其他林之间建立信任关系。 这使管理员授予访问其他森林中的资源。 下图显示组织林型号。  
  
![森林设计型号](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
每个 Active Directory 设计包含在至少有一个组织林。  
  
## <a name="resource-forest-model"></a>资源森林模型  
在资源森林模型，单独森林用于管理资源。 森林资源不包含用户帐户之外所需的服务管理和所需来提供对该的树林中的资源备用访问组织森林中的用户帐户都不可用。 林信任建立，以便从森林其他用户可以访问资源森林中包含的资源。 下图显示资源森林模型。  
  
![森林设计型号](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
森林资源提供服务隔离用于保护的网络保持高可用性状态所需的区域。 例如，如果你的公司包括生产设备需要继续操作的网络的其他部分有问题时，你可以创建单独资源林制造组。  
  
## <a name="restricted-access-forest-model"></a>限制的访问森林模型  
在限制的访问森林模型，单独森林创建含有用户帐户和数据，必须从组织中的其余独立。 森林受限制的访问提供数据隔离严重损害项目数据后果在哪里的情况下。 下图显示的限制的访问森林模型。  
  
![森林设计型号](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
从森林其他用户无法访问限制数据授予因为没有信任存在。 在此模式中，用户访问保密数据限制的访问林中拥有以访问最常用的组织资源组织森林中的帐户和单独的用户帐户。 这些用户必须安装一个连接到组织林两个独立工作站和其他连接到限制的访问森林。 这可以防止的服务管理员个林能够访问工作站受限森林中的可能性。  
  
Extreme 的情况下，可能会限制的访问森林保留单独的物理网络上。 有时处理保密的政府项目的组织维护限制的访问森林单独的网络，以符合安全要求。  
  


