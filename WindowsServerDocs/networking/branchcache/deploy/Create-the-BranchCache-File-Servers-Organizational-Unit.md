---
title: 创建分支缓存的文件服务器部门
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a92cb3110e4aecb1ef09a45ed14173305722c655
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>创建分支缓存的文件服务器部门

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程创建部门 (OU) 在对于分支缓存的文件服务器 Active Directory 域服务 (广告 DS)。  
  
在会员**域管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>若要创建分支缓存文件服务器部门  
  
1.  在安装广告 DS、服务器管理器中的计算机上单击**工具**，然后单击**Active Directory 用户和计算机**。 打开 Active Directory 用户和计算机主机。  
  
2.  在 Active Directory 用户和计算机控制台中，右键单击你想要添加 OU 域。 例如，如果你的域命名 example.com，右键单击**example.com**。指向**新建**，然后单击**单位**。 **新对象-单位**对话框中打开。  
  
3.  在**新对象-单位**对话框中，在**名称**，键入新的 OU 的名称。 例如，如果你想要名称 OU 分支缓存的文件服务器，键入**分支缓存的文件服务器**，然后单击**确定**。  
  


