---
title: 启用哈希发布非域成员文件服务器的
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4cb3d217115cff3a9b30ee11acb7ba0de6672b24
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>启用哈希发布非域成员文件服务器的

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程配置为使用本地计算机组策略，在运行 Windows Server 2016 的文件服务器分支缓存哈希发布**网络文件的分支缓存**安装文件服务 server 角色的角色服务。  
  
此过程适用于使用非域成员文件服务器上。 如果你执行此过程域成员文件服务器上，并且还配置分支缓存使用组策略域，则域组策略设置覆盖本地组策略设置。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
> [!NOTE]  
> 如果你有一个或多个域成员文件服务器，你可以将其添加到在 Active Directory 域服务部门 (OU)，然后使用组策略配置而不是单独配置每个文件服务器哈希发布的所有文件服务器一次。 有关详细信息，请参阅[域成员文件服务器的启用哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>若要启用有一个文件服务器哈希发布  
  
1.  打开 Windows PowerShell、 键入**mmc**，然后按 ENTER。 打开 Microsoft 管理控制台 (MMC)。  
  
2.  在 MMC，在**文件**菜单上，单击**添加/删除管理单元在**。 **添加或删除管理单元**对话框中打开。  
  
3.  在**添加或删除管理单元**中**可用管理单元**，双击**组策略对象编辑器**。 打开本地计算机对象选中组策略向导。 单击**完成**，然后单击**确定**。  
  
4.  在本地组策略编辑器 MMC，展开以下路径：**本地计算机策略**，**计算机配置**，**管理模板**，**网络**，**Lanman 服务器**。 单击**Lanman 服务器**。  
  
5.  在详细信息窗格中，双击**哈希发布有关分支缓存**。 **哈希发布有关分支缓存**对话框中打开。  
  
6.  在**哈希发布有关分支缓存**对话框中，单击**启用**。  
  
7.  在**选项**，单击**允许所有共享的文件夹哈希发布**，然后单击下列情况之一：  
  
    1.  若要启用哈希发布此计算机上的所有共享文件夹，请单击**允许所有共享的文件夹哈希发布**。  
  
    2.  若要启用哈希发布，仅用于为其启用分支缓存的共享文件夹，请单击**允许哈希发布，仅用于启用分支缓存的共享文件夹**。  
  
    3.  若要禁止哈希发布，对所有共享计算机上的文件夹，即使分支缓存启用上共享的文件，请单击**禁止哈希发布到所有共享文件夹**。  
  
8.  单击**确定**。  
  


