---
title: 验证客户端计算机设置
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d507f2097a9349cbefba520ad0dc143e0dd7452e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="verify-client-computer-settings"></a>验证客户端计算机设置

>适用于：Windows Server（半年通道），Windows Server 2016

可以使用此步骤验证已正确配置客户端计算机进行分支缓存。  
  
> [!NOTE]  
> 此过程将包括对手动更新组策略和重新启动分支缓存服务的步骤。 不需要执行这些操作，如果你重启计算机，因为它们将在此情况下自动进行。  
  
你必须成员**管理员**，或相当要执行此过程。  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>若要验证分支缓存客户端计算机设置  
  
1.  若要恢复你想要确认其分支缓存配置的客户端计算机上的组策略，以管理员身份运行 Windows PowerShell，键入以下命令，然后按 ENTER。  
  
    `gpupdate /force`  
  
2.  配置托管的缓存型且配置的客户端计算机，自动发现缓存服务器由托管服务连接点，运行以下命令以停止，并重新启动分支缓存服务。  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  通过运行以下命令，检查的当前分支缓存运行模式。  
  
    `Get-BCStatus`  
  
4.  在 Windows PowerShell 中检查的输出**获取 BCStatus**命令。  
  
    值**BranchCacheIsEnabled**应**如此**。  
  
    在**ClientSettings**的值**CurrentClientMode**应**DistributedClient**或**HostedCacheClient**，这取决于你可以使用本指南配置的模式。  
  
    在**ClientSettings**，如果你在配置托管的缓存模式和配置过程中，提供托管的缓存服务器的名称或如果客户端已自动找到托管使用的服务的连接点的缓存服务器**HostedCacheServerList**应已设置为相同的名称或托管的缓存服务器的名称。 例如，如果名为托管的缓存服务器 HCS1 和域是 corp.contoso.com 的值**HostedCacheServerList**是**HCS1.corp.contoso.com**。  
  
5.  如果任何设置上述分支缓存不具有正确的值，采用本指南中的步骤验证本地计算机策略或组策略设置，以及的防火墙异常，你将此配置，并确保其正确无误。 此外，请重启计算机，或按照此过程刷新组策略，然后重新启动分支缓存服务中的步骤。  
  


