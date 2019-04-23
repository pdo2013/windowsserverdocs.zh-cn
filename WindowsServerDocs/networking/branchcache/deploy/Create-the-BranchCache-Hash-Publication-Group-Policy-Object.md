---
title: 创建 BranchCache 哈希发布组策略对象
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15e89b961d20b6f14065392553e413374358062d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865428"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>创建 BranchCache 哈希发布组策略对象

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于创建 BranchCache 的哈希发布组策略对象 (GPO)。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 然后再执行此过程，必须创建 BranchCache 文件服务器组织单位并将文件服务器移到 OU。 有关详细信息，请参阅[启用域成员的文件服务器的哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>若要创建的 BranchCache 的哈希发布组策略对象  
  
1.  打开 Windows PowerShell、键入 **mmc**，然后按 ENTER。 Microsoft 管理控制台 (MMC) 会打开。  
  
2.  在 MMC 中，在**文件**菜单上，单击**添加/删除管理单元中**。 **添加或删除管理单元**对话框随即打开。  
  
3.  在中**添加或删除管理单元**，在**可用的管理单元**，双击**组策略管理**，然后单击**确定**。  
  
4.  在组策略管理 MMC 中，展开到 BranchCache 文件服务器之前创建的 OU 的路径。 例如，如果你的林名为 example.com，域名为 example1.com，并且您 OU 名为 BranchCache 文件服务器，展开以下路径：**组策略管理**，**林： example.com**，**域**， **example1.com**， **BranchCache 文件服务器**。  
  
5.  右键单击**BranchCache 文件服务器**，然后单击**在此域中创建 GPO 并在此处链接**。 **新的 GPO**对话框随即打开。 在中**名称**，键入新 GPO 的名称。 例如，如果你想要对象 BranchCache 的哈希发布命名，键入**BranchCache 的哈希发布**。 单击 **“确定”**。  
  


