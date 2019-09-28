---
title: 创建 BranchCache 哈希发布组策略对象
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 10230b57075943a5d92dce7155e794293157cba4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356644"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>创建 BranchCache 哈希发布组策略对象

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此过程来创建 BranchCache 哈希发布组策略对象（GPO）。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 在执行此过程之前，必须创建 BranchCache 文件服务器组织单位，并将文件服务器移到 OU 中。 有关详细信息，请参阅[为域成员文件服务器启用哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>组策略对象创建 BranchCache 哈希发布  
  
1.  打开 Windows PowerShell、键入 **mmc**，然后按 ENTER。 Microsoft 管理控制台 (MMC) 会打开。  
  
2.  在 MMC 的 "**文件**" 菜单上，单击 "**添加/删除管理单元**"。 "**添加或删除管理单元**" 对话框将打开。  
  
3.  在 "**添加或删除管理单元**" 的 "**可用管理单元**" 中，双击 "**组策略管理**"，然后单击 **"确定"** 。  
  
4.  在组策略管理 MMC 中，展开之前创建的 BranchCache 文件服务器 OU 的路径。 例如，如果你的林名为 example.com，你的域命名为 example1.com，并且你的 OU 命名为 BranchCache file servers，则展开以下路径：**组策略管理**，**林： example.com**、**域**、 **example1.com**、 **BranchCache 文件服务器**。  
  
5.  右键单击 " **BranchCache 文件服务器**"，然后单击 "**在此域中创建 GPO 并在此处链接"** 。 此时将打开 "**新建 GPO** " 对话框。 在 "**名称**" 中，键入新 GPO 的名称。 例如，如果想要为对象 BranchCache 哈希发布命名，请键入**BranchCache 哈希发布**。 单击 **“确定”** 。  
  


