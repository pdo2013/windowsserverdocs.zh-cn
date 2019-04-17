---
title: 移动到分支缓存的文件服务器部门文件服务器
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09afb4936545cb1f5bb14573261008ff18badd4d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>移动到分支缓存的文件服务器部门文件服务器

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程将添加到部门 (OU) 分支缓存的文件服务器 Active Directory 域服务 (广告 DS) 中。  
  
在会员**域管理员**，或等效的最低要求执行此过程。  
  
> [!NOTE]  
> 你必须先将计算机帐户添加到此过程与 OU Active Directory 用户和计算机控制台中创建分支缓存文件服务器。 有关详细信息，请参阅[创建分支缓存文件服务器部门](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)。  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>若要移动到分支缓存的文件服务器文件服务器部门  
  
1.  在安装广告 DS、服务器管理器中的计算机上单击**工具**，然后单击**Active Directory 用户和计算机**。 打开 Active Directory 用户和计算机主机。  
  
2.  在 Active Directory 用户和计算机控制台中，查找分支缓存的文件服务器、左键单击以选择帐户，然后拖放分支缓存文件服务器，在你之前创建的计算机帐户计算机帐户。 例如，如果你之前创建了一个名为 OU**分支缓存的文件服务器**并拖放在计算机帐户**分支缓存的文件服务器**OU。  
  
3.  为你想要移动到 OU 域中每个分支缓存文件服务器，重复上述步骤。  
  


