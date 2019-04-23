---
title: 启用域成员文件服务器的哈希发布
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 174e83c950d2aff8afba4f05641a74861b9a7938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865458"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>启用域成员文件服务器的哈希发布

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在您使用 Active Directory 域服务 (AD DS) 时，可以使用域组策略启用 BranchCache 的哈希发布多个文件服务器。 若要执行此操作，必须创建组织单位 (OU)，将文件服务器添加到该 OU 中，创建 BranchCache 哈希发布组策略对象 (GPO)，以及如何配置 GPO。  
  
请参阅以下主题，以使多台文件服务器的哈希发布。  
  
-   [BranchCache 文件服务器组织单位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [将文件服务器移到 BranchCache 文件服务器组织单位](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [创建 BranchCache 哈希发布组策略对象](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [BranchCache 的哈希发布组策略对象配置](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


