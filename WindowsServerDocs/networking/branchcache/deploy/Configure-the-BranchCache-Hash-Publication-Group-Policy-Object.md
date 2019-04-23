---
title: 配置 BranchCache 哈希发布组策略对象
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e9a338dfebfb1dfadb258bcbdfcc8d75bd3ea86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862958"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>配置 BranchCache 哈希发布组策略对象

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于配置 BranchCache 的哈希发布组策略对象 (GPO)，以便添加到您的 OU 的所有文件服务器都具有相同的哈希发布策略设置应用于它们。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 之前执行此过程，必须创建 BranchCache 文件服务器组织单位，将文件服务器移到 OU，并创建 BranchCache 的哈希发布 GPO。 有关详细信息，请参阅[启用域成员的文件服务器的哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>若要配置 BranchCache 的哈希发布组策略对象  
  
1.  以管理员身份，类型运行 Windows PowerShell **mmc**，然后按 ENTER。 Microsoft 管理控制台 (MMC) 会打开。  
  
2.  在 MMC 中，在**文件**菜单上，单击**添加/删除管理单元中**。 **添加或删除管理单元**对话框随即打开。  
  
3.  在中**添加或删除管理单元**，在**可用的管理单元**，双击**组策略管理**，然后单击**确定**。  
  
4.  在组策略管理 MMC 中，展开 BranchCache 的哈希发布之前创建的 GPO 的路径。 例如，如果你的林名为 example.com，域名为 example1.com，并且名为 GPO **BranchCache 的哈希发布**，展开以下路径：**组策略管理**，**林： example.com**，**域**， **example1.com**，**组策略对象**， **BranchCache 的哈希发布**。  
  
5.  右键单击**BranchCache 的哈希发布**GPO，然后单击**编辑**。 此时将打开组策略管理编辑器控制台。  
  
6.  在组策略管理编辑器控制台中，展开以下路径：**计算机配置**，**策略**，**管理模板**，**网络**， **Lanman 服务器**。  
  
7.  在组策略管理编辑器控制台中，单击**Lanman 服务器**。 在详细信息窗格中，双击**BranchCache 的哈希发布**。 **BranchCache 的哈希发布**对话框随即打开。  
  
8.  在中**BranchCache 的哈希发布**对话框中，单击**已启用**。  
  
9. 在中**选项**，单击**允许所有共享文件夹的哈希发布**，然后单击以下项之一：  
  
    1.  若要启用的所有共享文件夹哈希发布的所有文件添加到该 OU 的服务器，请单击**允许所有共享文件夹的哈希发布**。  
  
    2.  若要启用仅为共享文件夹启用了 BranchCache 的哈希发布，单击**允许在其启用 BranchCache 的共享文件夹哈希发布**。  
  
    3.  若要对所有共享的计算机上的文件夹，即使在文件共享上启用 BranchCache，不允许的哈希发布，单击**禁止所有共享文件夹上的哈希发布**。  
  
10. 单击 **“确定”**。  
  
> [!NOTE]  
> 在大多数情况下，必须保存 MMC 控制台，并刷新视图以显示所做的配置更改。  
  


