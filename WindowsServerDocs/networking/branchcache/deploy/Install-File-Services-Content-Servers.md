---
title: 安装文件服务内容服务器
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7036323e7cc31bd14be8025b6064a806fb45ef19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-file-services-content-servers"></a>安装文件服务内容服务器

>适用于：Windows Server（半年通道），Windows Server 2016

部署内容文件服务 server 角色运行的服务器，必须安装分支缓存的文件服务 server 角色网络文件角色服务。 此外，必须启用分支缓存的文件共享根据你的要求。  
  
配置期间内容服务器，您可以允许分支缓存发布的所有文件共享的内容，或你可以选择文件共享发布的一部分。  
  
> [!NOTE]  
> 部署分支缓存启用的文件服务器或 Web 服务器作为内容服务器时，内容信息现在离线，也之前计算分支缓存客户端请求文件。 由于此项改进，你不必配置哈希发布的内容服务器，就像在以前版本的分支缓存中。  
>   
> 此自动哈希代提供更快的性能和更多节省带宽，因为内容信息已准备好进行第一个请求内容的客户，已经执行计算。  
  
请参阅以下主题部署内容的服务器。  
  
-   [配置文件服务 server 角色](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [启用文件服务器哈希发布](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [启用分支文件共享 & #40; 上的缓存可选 & #41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


