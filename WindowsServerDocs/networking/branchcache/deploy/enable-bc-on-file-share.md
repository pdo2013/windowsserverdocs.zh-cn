---
title: 在文件共享上启用 BranchCache（可选）
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd1757f6da011c2f774d8f97f628e5f0e87d3bf7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284026"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>在文件共享上启用 BranchCache（可选）

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于在文件共享上启用 BranchCache。  
  
> [!IMPORTANT]  
> 不需要执行此过程，如果将哈希发布设置配置值**允许所有共享文件夹的哈希发布**。  
  
中的成员身份**管理员**，或等效身份是执行此过程所需的最低。  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>若要在文件共享上启用 BranchCache  
  
1.  打开 Windows PowerShell、键入 **mmc**，然后按 ENTER。 Microsoft 管理控制台 (MMC) 会打开。  
  
2.  在 MMC 中，在**文件**菜单上，单击**添加/删除管理单元中**。 **添加或删除管理单元**对话框随即打开。  
  
3.  在中**添加或删除管理单元**，在**可用的管理单元**，双击**共享文件夹**。 共享文件夹向导将打开与选择的本地计算机对象。 配置的视图的需要，单击**完成**，然后单击**确定**。  
  
4.  双击**共享文件夹 （本地）** ，然后单击**共享**。  
  
5.  在详细信息窗格中，右键单击共享，然后依次**属性**。 共享的**属性**对话框随即打开。  
  
6.  在中**属性**对话框中，在**常规**选项卡上，单击**脱机设置**。 **脱机设置**对话框随即打开。  
  
7.  絋粄**仅的文件和用户指定的程序可以脱机**已选中，然后单击**启用 BranchCache**。  
  
8.  单击“确定”两次  。  
  

