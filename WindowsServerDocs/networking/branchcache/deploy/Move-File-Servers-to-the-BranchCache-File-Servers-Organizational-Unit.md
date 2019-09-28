---
title: 将文件服务器移到 BranchCache 文件服务器组织单位
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ad297e25f258140fce4af3f825e362f62748c77d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356532"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>将文件服务器移到 BranchCache 文件服务器组织单位

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此过程将 BranchCache 文件服务器添加到 Active Directory 域服务（AD DS）中的组织单位（OU）。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 使用此过程将计算机帐户添加到 OU 之前，必须在 "Active Directory 用户和计算机" 控制台中创建 BranchCache 文件服务器 OU。 有关详细信息，请参阅[创建 BranchCache 文件服务器组织单位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)。  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>将文件服务器移动到 BranchCache 文件服务器组织单位  
  
1.  在安装了 AD DS 的计算机上的服务器管理器中，单击 "**工具**"，然后单击 " **Active Directory 用户和计算机**"。 此时将打开 Active Directory 用户和计算机 "控制台。  
  
2.  在 "Active Directory 用户和计算机" 控制台中，找到 BranchCache 文件服务器的计算机帐户，左键单击以选择该帐户，然后将计算机帐户拖放到前面创建的 BranchCache 文件服务器 OU 上。 例如，如果你以前创建了一个名为**BranchCache 文件服务器**的 OU，请将该计算机帐户拖放到**branchcache 文件服务器**OU 上。  
  
3.  为要移动到 OU 的域中的每个 BranchCache 文件服务器重复上一步。  
  


