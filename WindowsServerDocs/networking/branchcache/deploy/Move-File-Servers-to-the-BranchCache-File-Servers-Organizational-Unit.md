---
title: 将文件服务器移到 BranchCache 文件服务器组织单位
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 037b354bb6725ac7f91fc323b81bbdf15d03ac15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885898"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>将文件服务器移到 BranchCache 文件服务器组织单位

>适用于：Windows 服务器 （半年频道），Windows Server 2016

您可以使用此过程将 BranchCache 文件服务器添加到组织单位 (OU) 在 Active Directory 域服务 (AD DS) 中。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 将计算机帐户添加到 OU，并使此过程之前，必须在 Active Directory 用户和计算机控制台中创建 BranchCache 文件服务器 OU。 有关详细信息，请参阅[BranchCache 文件服务器组织单位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)。  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>若要将移动到 BranchCache 的文件服务器文件服务器组织单位  
  
1.  在其中安装 AD DS，服务器管理器中的计算机上单击**工具**，然后单击**Active Directory 用户和计算机**。 此时将打开 Active Directory 用户和计算机控制台。  
  
2.  在 Active Directory 用户和计算机控制台中，找到 BranchCache 的文件服务器，再左键单击来选择帐户，然后拖放上 BranchCache 文件服务器之前创建的 OU 的计算机帐户的计算机帐户。 例如，如果你之前创建名为 OU **BranchCache 文件服务器**中，拖放的计算机帐户上**BranchCache 文件服务器**OU。  
  
3.  为你想要将移动到 OU 的域中每个 BranchCache 文件服务器重复上一步。  
  


