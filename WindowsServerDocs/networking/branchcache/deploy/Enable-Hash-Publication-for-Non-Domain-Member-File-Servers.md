---
title: 启用非域成员文件服务器的哈希发布
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00be97abbd583e4c5e762775ea563ba0720d5142
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814948"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>启用非域成员文件服务器的哈希发布

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用此过程来配置运行 Windows Server 2016 的文件服务器上使用本地计算机组策略的 BranchCache 的哈希发布**网络文件 BranchCache**文件服务角色服务安装服务器角色。  
  
此过程适用于非域成员的文件服务器上使用。 如果域成员文件服务器上执行此过程，并且还配置 BranchCache 使用域组策略，域组策略设置将替代本地组策略设置。  
  
中的成员身份**管理员**，或等效身份是执行此过程所需的最低。  
  
> [!NOTE]  
> 如果有一个或多个域成员文件服务器，您可以将它们添加到 Active Directory 域服务中的组织单位 (OU)，然后使用组策略配置哈希发布的所有文件服务器，一次而不是单独配置每个文件服务器。 有关详细信息，请参阅[启用域成员的文件服务器的哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>若要启用哈希发布一个文件服务器  
  
1.  打开 Windows PowerShell、键入 **mmc**，然后按 ENTER。 Microsoft 管理控制台 (MMC) 会打开。  
  
2.  在 MMC 中，在**文件**菜单上，单击**添加/删除管理单元中**。 **添加或删除管理单元**对话框随即打开。  
  
3.  在中**添加或删除管理单元**，在**可用的管理单元**，双击**组策略对象编辑器**。 组策略向导将打开与选择的本地计算机对象。 单击“完成” ，然后单击“确定” 。  
  
4.  在本地组策略编辑器 MMC 中，展开以下路径：**本地计算机策略**，**计算机配置**，**管理模板**，**网络**， **Lanman 服务器**。 单击**Lanman 服务器**。  
  
5.  在详细信息窗格中，双击**BranchCache 的哈希发布**。 **BranchCache 的哈希发布**对话框随即打开。  
  
6.  在中**BranchCache 的哈希发布**对话框中，单击**已启用**。  
  
7.  在中**选项**，单击**允许所有共享文件夹的哈希发布**，然后单击以下项之一：  
  
    1.  若要启用此计算机上的所有共享文件夹的哈希发布，请单击**允许所有共享文件夹的哈希发布**。  
  
    2.  若要启用仅为共享文件夹启用了 BranchCache 的哈希发布，单击**允许在其启用 BranchCache 的共享文件夹哈希发布**。  
  
    3.  若要对所有共享的计算机上的文件夹，即使在文件共享上启用 BranchCache，不允许的哈希发布，单击**禁止所有共享文件夹上的哈希发布**。  
  
8.  单击 **“确定”**。  
  


