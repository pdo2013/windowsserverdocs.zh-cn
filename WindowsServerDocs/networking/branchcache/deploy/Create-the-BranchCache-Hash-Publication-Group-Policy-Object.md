---
title: 创建分支缓存哈希发布组策略对象
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4510d13c806adc2db46dfccce02a5a1b2814a2c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>创建分支缓存哈希发布组策略对象

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此步骤以创建该分支缓存哈希发布组策略对象 (GPO)。  
  
在会员**域管理员**，或等效的最低要求执行此过程。  
  
> [!NOTE]  
> 执行此过程前, 必须创建分支缓存文件服务器部门并移动到 OU 文件服务器。 有关详细信息，请参阅[域成员文件服务器的启用哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>若要创建分支缓存哈希发布组策略对象  
  
1.  打开 Windows PowerShell、 键入**mmc**，然后按 ENTER。 打开 Microsoft 管理控制台 (MMC)。  
  
2.  在 MMC，在**文件**菜单上，单击**添加/删除管理单元在**。 **添加或删除管理单元**对话框中打开。  
  
3.  在**添加或删除管理单元**中**可用管理单元**，双击**组策略管理**，然后单击**确定**。  
  
4.  在组策略管理 MMC 中，展开到分支缓存文件服务器，在你之前创建的路径。 例如，如果你森林名为 example.com，名为你的域 example1.com，并且你 OU 名为分支缓存的文件服务器，展开以下路径：**组策略管理**，**森林： example.com**，**域**， **example1.com**，**分支缓存的文件服务器**。  
  
5.  右键单击**分支缓存的文件服务器**，然后单击**在域中，创建一个 GPO 并将其链接此处**。 **新 GPO**对话框中打开。 在**名称**，键入新 GPO 的名称。 例如，如果你想要名称的对象分支缓存哈希发布，键入**分支缓存哈希发布**。 单击**确定**。  
  


