---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: 查看 OU 设计概念
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f05104466c1cedcfbc8d94060ffa8fbfd9d18033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832168"
---
# <a name="reviewing-ou-design-concepts"></a>查看 OU 设计概念

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

域组织单位 (OU) 结构包括以下组件：  
  
-   OU 层次结构的关系图  
  
-   Ou 列表  
  
-   为每个 OU:  
  
    -   OU 的用途  
  
    -   用户或组的 OU 中可以控制的 OU 或对象的列表  
  
    -   用户和组具有对对象的 OU 中的控件的类型  
  
OU 层次结构不需要以反映组织或组的部门层次结构。 Ou 是为特定目的，例如委派管理，创建应用程序的组策略，或限制可见性的对象。  
  
您可以设计您的 OU 结构到个人或组在组织中需要自主权来管理其自己的资源和数据的委派管理。 Ou 代表管理边界，使您可以控制的数据管理员的授权范围。  
  
例如，可以创建名为 ResourceOU OU，并使用它来存储所有属于的文件和打印服务器由一组管理的计算机帐户。 然后，可以在 OU 中配置安全性，以便只有数据管理员组中的有权访问该 OU。 这可以防止在其他组中的数据管理员篡改的文件和打印服务器帐户。  
  
通过为特定目的，例如应用程序的组策略，或若要限制的受保护的对象可见性，以便只有特定用户可以看到它们创建的 Ou 的子树，可以进一步完善您的 OU 结构。 例如，如果需要将组策略应用到选择的用户或资源组，您可以将这些用户或资源添加到某个 OU，，然后将组策略应用到该 OU。 此外可以使用 OU 层次结构以进行进一步的管理控制委派。  
  
虽然您的 OU 结构中的级别数没有技术限制，可管理性建议限制为不超过 10 个级别的深度 OU 结构。 为 Ou 上每个级别数没有技术限制。 请注意该 Active Directory 域服务 (AD DS) 的启用的应用程序可能对可分辨名称 （即，到目录中对象的完整轻型目录访问协议 (LDAP) 路径） 中使用或上的字符数限制在层次结构中的 OU 深度。  
  
在 AD DS 中的 OU 结构不是向最终用户可见。 OU 结构是一种管理工具为服务管理员和数据管理员，并且可以轻松地更改。 继续查看和更新您的 OU 结构设计以反映您的管理结构中的更改并支持基于策略的管理。  
  


