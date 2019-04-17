---
title: 使用 Windows PowerShell 配置成员非域的客户端
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5415d9fa5a4af806f23a1af9c907b1f02e9627b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>使用 Windows PowerShell 配置成员非域的客户端

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程手动配置分支缓存客户端计算机分布式的缓存模式或托管的缓存模式。  
  
> [!NOTE]  
> 如果你已经将配置分支缓存客户端计算机使用组策略的组策略设置覆盖策略应用于客户端计算机任何手动配置。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>若要启用分支缓存分布式或托管的缓存模式  
  
1.  你想要配置分支缓存客户端计算机以 administrator 身份，运行 Windows PowerShell，然后执行以下一项。  
  
    -   若要将配置客户端计算机分支缓存分布式的缓存模式下，键入以下命令，，然后按 ENTER。  
  
        `Enable-BCDistributed`  
  
    -   若要将配置客户端计算机托管分支缓存缓存模式下的，键入以下命令，，然后按 ENTER。  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > 如果你想要指定可用托管的缓存服务器，使用`-ServerNames`具有逗号参数的分隔作为参数值托管的缓存服务器的列表。 例如，如果你有两个托管的缓存服务器命名 HCS1 和 HCS2，使用以下命令配置客户端计算机以托管的缓存模式。  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


