---
title: 安装文件服务内容服务器
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c55f8df57ed98d13d6d0d6d2a281edfb55883bea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356564"
---
# <a name="install-file-services-content-servers"></a>安装文件服务内容服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

若要部署运行文件服务服务器角色的内容服务器，你必须安装 "文件服务" 服务器角色的 "网络文件 BranchCache" 角色服务。 此外，必须根据要求在文件共享上启用 BranchCache。  
  
在配置内容服务器的过程中，你可以为所有文件共享允许 BranchCache 发布内容，也可以选择要发布的部分文件共享。  
  
> [!NOTE]  
> 当你将启用了 BranchCache 的文件服务器或 Web 服务器部署为内容服务器时，内容信息现在会在 BranchCache 客户端请求文件之前脱机计算。 由于此改进，你无需为内容服务器配置哈希发布，这与之前版本的 BranchCache 中的操作相同。  
>   
> 此自动哈希生成提供更快的性能和更大的带宽节省，因为内容信息已为请求内容的第一个客户端做好准备，并且已执行计算。  
  
请参阅以下主题以部署内容服务器。  
  
-   [配置文件服务服务器角色](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [为文件服务器启用哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [在文件共享&#40;上启用 BranchCache （可选）&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


