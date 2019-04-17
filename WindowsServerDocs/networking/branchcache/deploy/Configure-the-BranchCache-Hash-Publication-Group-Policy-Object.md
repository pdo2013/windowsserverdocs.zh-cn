---
title: 配置分支缓存哈希发布组策略对象
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f528c9f0a8a39b286ad7ac4fa728d93c311f779
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>配置分支缓存哈希发布组策略对象

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程配置分支缓存哈希发布组策略对象 (GPO)，以便你添加到你 OU 的所有文件服务器都具有相同的希发布策略设置应用到他们。  
  
在会员**域管理员**，或等效的最低要求执行此过程。  
  
> [!NOTE]  
> 执行此过程之前, 到 OU，你必须创建分支缓存文件服务器单位，移动文件服务器，并创建分支缓存哈希发布 GPO。 有关详细信息，请参阅[域成员文件服务器的启用哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>若要配置分支缓存哈希发布组策略对象  
  
1.  以 administrator 身份，类型运行 Windows PowerShell **mmc**，然后按 ENTER。 打开 Microsoft 管理控制台 (MMC)。  
  
2.  在 MMC，在**文件**菜单上，单击**添加/删除管理单元在**。 **添加或删除管理单元**对话框中打开。  
  
3.  在**添加或删除管理单元**中**可用管理单元**，双击**组策略管理**，然后单击**确定**。  
  
4.  在组策略管理 MMC 中，展开对分支缓存哈希发布你之前创建的 GPO 的路径。 例如，如果你森林名为 example.com，您的域命名为 example1.com，，名为你 GPO**分支缓存哈希发布**，展开以下路径：**组策略管理**，**森林： example.com**，**域**， **example1.com**，**组策略对象**，**分支缓存哈希发布**。  
  
5.  右键单击**分支缓存哈希发布**GPO，然后单击**编辑**。 打开组策略编辑器中管理主机。  
  
6.  在组策略编辑器中管理控制台中，展开以下路径：**计算机配置**，**策略**，**管理模板**，**网络**， **Lanman 服务器**。  
  
7.  在组策略编辑器中管理控制台中，单击**Lanman 服务器**。 在详细信息窗格中，双击**哈希发布有关分支缓存**。 **哈希发布有关分支缓存**对话框中打开。  
  
8.  在**哈希发布有关分支缓存**对话框中，单击**启用**。  
  
9. 在**选项**，单击**允许所有共享的文件夹哈希发布**，然后单击下列情况之一：  
  
    1.  若要的所有文件服务器，在你添加到 OU 启用哈希发布的所有共享的文件夹，请单击**允许所有共享的文件夹哈希发布**。  
  
    2.  若要启用哈希发布，仅用于为其启用分支缓存的共享文件夹，请单击**允许哈希发布，仅用于启用分支缓存的共享文件夹**。  
  
    3.  若要禁止哈希发布，对所有共享计算机上的文件夹，即使分支缓存启用上共享的文件，请单击**禁止哈希发布到所有共享文件夹**。  
  
10. 单击**确定**。  
  
> [!NOTE]  
> 在大多数情况下，你必须保存 MMC 主机并刷新显示你所做的配置更改视图。  
  


