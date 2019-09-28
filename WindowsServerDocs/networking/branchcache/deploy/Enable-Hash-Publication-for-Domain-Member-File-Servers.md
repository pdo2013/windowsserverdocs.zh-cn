---
title: 启用域成员文件服务器的哈希发布
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e450b9a2282cb4820b8802aa6d36e822f56ca12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356589"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>启用域成员文件服务器的哈希发布

>适用于：Windows Server（半年频道）、Windows Server 2016

使用 Active Directory 域服务（AD DS）时，可以使用域组策略为多个文件服务器启用 BranchCache 哈希发布。 为此，你必须创建一个组织单位（OU），将文件服务器添加到 OU，创建 BranchCache 哈希发布组策略对象（GPO），然后配置 GPO。  
  
请参阅以下主题，为多个文件服务器启用哈希发布。  
  
-   [创建 BranchCache 文件服务器组织单位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [将文件服务器移到 BranchCache 文件服务器组织单位](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [组策略对象创建 BranchCache 哈希发布](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [配置 BranchCache 哈希发布组策略对象](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


