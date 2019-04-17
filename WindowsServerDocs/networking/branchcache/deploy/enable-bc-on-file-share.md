---
title: 启用分支文件共享（可选）上的缓存
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33ed40ef91d9389bb7940dcf928cba43f0c9dbd2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>启用分支文件共享（可选）上的缓存

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程文件共享上启用分支缓存。  
  
> [!IMPORTANT]  
> 不需要执行此过程，如果你使用值配置哈希发布设置**允许所有共享的文件夹哈希发布**。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>若要启用分支缓存的文件共享  
  
1.  打开 Windows PowerShell、 键入**mmc**，然后按 ENTER。 打开 Microsoft 管理控制台 (MMC)。  
  
2.  在 MMC，在**文件**菜单上，单击**添加/删除管理单元在**。 **添加或删除管理单元**对话框中打开。  
  
3.  在**添加或删除管理单元**中**可用管理单元**，双击**共享文件夹**。 打开本地计算机对象选择共享文件夹向导。 将配置你愿意，视图单击**完成**，然后单击**确定**。  
  
4.  双击**共享文件夹（本地）**，然后单击**共享**。  
  
5.  在详细信息窗格中，右键单击一个共享，，然后单击**属性**。 共享**属性**对话框中打开。  
  
6.  在**属性**对话框中，在**常规**选项卡上，单击**脱机设置**。 **脱机设置**对话框中打开。  
  
7.  确保**仅用户指定的程序可以访问文件和脱机**选中，则，然后单击**启用分支缓存**。  
  
8.  单击**确定**两次。  
  

