---
title: 配置 BranchCache 哈希发布组策略对象
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bae0d3dff22205f3311020e233a26835527186f5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406500"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>配置 BranchCache 哈希发布组策略对象

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此过程来配置 BranchCache 哈希发布组策略对象（GPO），以便添加到 OU 的所有文件服务器都具有相同的哈希发布策略设置。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 在执行此过程之前，你必须创建 BranchCache 文件服务器组织单位，将文件服务器移入 OU，然后创建 BranchCache 哈希发布 GPO。 有关详细信息，请参阅[为域成员文件服务器启用哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>配置 BranchCache 哈希发布组策略对象  
  
1.  以管理员身份运行 Windows PowerShell，键入**mmc**，然后按 enter。 Microsoft 管理控制台 (MMC) 会打开。  
  
2.  在 MMC 的 "**文件**" 菜单上，单击 "**添加/删除管理单元**"。 "**添加或删除管理单元**" 对话框将打开。  
  
3.  在 "**添加或删除管理单元**" 的 "**可用管理单元**" 中，双击 "**组策略管理**"，然后单击 **"确定"** 。  
  
4.  在组策略管理 MMC 中，展开之前创建的 BranchCache 哈希发布 GPO 的路径。 例如，如果你的林名为 example.com，你的域命名为 example1.com，并且你的 GPO 名为**BranchCache 哈希发布**，则展开以下路径：**组策略管理**，**林： example.com**、**域**、 **Example1.com**、**组策略对象**、 **BranchCache 哈希发布**。  
  
5.  右键单击**BranchCache 哈希发布**GPO，然后单击 "**编辑**"。 此时将打开组策略管理编辑器控制台。  
  
6.  在组策略管理编辑器控制台中，展开以下路径：**计算机配置**，**策略**，**管理模板**，**网络**， **Lanman 服务器**。  
  
7.  在组策略管理编辑器控制台中，单击 " **Lanman 服务器**"。 在详细信息窗格中，双击 " **BranchCache 的哈希发布**"。 " **BranchCache 的哈希发布**" 对话框将打开。  
  
8.  在 " **BranchCache 的哈希发布**" 对话框中，单击 "**启用**"。  
  
9. 在 "选项" 中，单击 "**允许对所有共享文件夹进行哈希发布**"，然后单击以下**选项**之一：  
  
    1.  若要为添加到 OU 的所有文件服务器上的所有共享文件夹启用哈希发布，请单击 "**允许对所有共享文件夹进行哈希发布**"。  
  
    2.  若要仅为启用了 BranchCache 的共享文件夹启用哈希发布，请**针对启用了 branchcache 的共享文件夹单击 "仅允许哈希发布**"。  
  
    3.  若要禁止对计算机上的所有共享文件夹进行哈希发布，即使已在文件共享上启用 BranchCache，请单击 "**禁止对所有共享文件夹进行哈希发布**"。  
  
10. 单击 **“确定”** 。  
  
> [!NOTE]  
> 在大多数情况下，您必须保存 MMC 控制台并刷新视图以显示您所做的配置更改。  
  


