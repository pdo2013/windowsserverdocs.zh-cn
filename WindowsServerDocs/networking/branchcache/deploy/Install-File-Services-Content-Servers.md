---
title: 安装文件服务内容服务器
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496c0c1408c64216f29a31d5b22d3d9b48d4f44c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855058"
---
# <a name="install-file-services-content-servers"></a>安装文件服务内容服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

若要部署运行文件服务服务器角色的内容服务器，必须安装 BranchCache 的文件服务服务器角色的网络文件角色服务。 此外，必须根据您的要求在文件共享上启用 BranchCache。  
  
在内容服务器的配置，你可以允许 BranchCache 发布的所有文件共享的内容或者可以选择要发布的文件共享的子集。  
  
> [!NOTE]  
> 在部署 BranchCache 启用的文件服务器或内容服务器的 Web 服务器时，内容信息是现在脱机，计算，BranchCache 客户端请求文件之前。 由于此项改进，您不必配置哈希发布内容服务器，就像在以前版本的 BranchCache 中。  
>   
> 此自动哈希生成提供更快的性能并节约更多的带宽，因为请求的内容，第一个客户端准备好内容信息和已执行计算。  
  
请参阅以下主题以部署内容服务器。  
  
-   [配置文件服务服务器角色](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [启用文件服务器的哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [启用 BranchCache 文件共享上的&#40;可选&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


