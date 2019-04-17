---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: "查看 OU 设计概念"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e832d068a4d03316853d8b59e3f2ac4a6ebc816
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-ou-design-concepts"></a>查看 OU 设计概念

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

对于域部门 (OU) 结构包括：  
  
-   OU 分层的图示  
  
-   华丽绚烂的列表  
  
-   为每个 OU:  
  
    -   OU 的用途  
  
    -   用户或组中 OU 具有控制 OU 或对象的列表  
  
    -   类型的用户和组有通过在 OU 对象的控件  
  
OU 层次不需要，以反映部门分层组织或组。 华丽绚烂创建特定用途，例如委派管理，应用程序的组策略或限制可见性物体。  
  
您可以设计 OU 结构委派给个人或你的组织中的组需要自主管理自己资源和数据的管理。 华丽绚烂表示管理限制，并使您可以控制数据管理员颁发机构的范围。  
  
例如，你可以创建一个名为 ResourceOU OU 并使用它存储到的文件和打印服务器由一组属于所有计算机帐户。 然后，你可以在 OU 配置安全，以便仅组中的数据管理员有权访问 OU。 这可以防止数据管理员，其他组中的文件和打印服务器帐户被篡改。  
  
您可以通过创建华丽绚烂的子树上的特定用途，例如的应用程序的组策略或限制的受保护的对象可见性，以便仅某些用户可以欣赏进一步改进 OU 结构。 例如，如果你需要适用于一组精选的用户或资源的组策略，你可以将这些用户或资源添加到 OU，并将组策略应用到该 OU。 你还可以使用 OU 分层启用进一步委派管理控件。  
  
虽然中 OU 结构级别数目没有技术限制的可管理性我们建议你限制你 OU 结构延伸不超过 10 级别。 没有在每个级别的华丽绚烂的数量 technical 限制。 注意该 Active Directory 域服务 (广告 DS)-启用应用程序可能会对数量的字符识别名称（即完整轻型目录访问协议 (LDAP) 路径目录中对象）中使用的限制或内层次 OU 深度上。  
  
广告 DS OU 结构不是最终用户可见。 OU 结构管理工具的服务管理员和数据管理员，并且很容易就可以更改。 若要查看和更新您 OU 结构设计，以反映你管理结构中的更改，并支持策略基于管理将继续。  
  


