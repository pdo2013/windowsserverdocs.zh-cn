---
title: 验证客户端计算机设置
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d628886186474d3f05d7961ca3d3b45b8bf12e73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834048"
---
# <a name="verify-client-computer-settings"></a>验证客户端计算机设置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于验证客户端计算机已正确配置 branchcache。  
  
> [!NOTE]  
> 此过程包括手动更新组策略和重新启动 BranchCache 服务的步骤。 不需要执行这些操作，如果重新启动计算机，因为它们将在此情况下会自动执行。  
  
您必须是属于**管理员**，或相当于执行此过程。  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>若要验证 BranchCache 客户端计算机设置  
  
1.  若要在你想要验证其 BranchCache 配置的客户端计算机上刷新组策略，以管理员身份运行 Windows PowerShell，键入以下命令，然后按 ENTER。  
  
    `gpupdate /force`  
  
2.  在托管的缓存模式下配置且配置的客户端计算机，以自动发现托管缓存服务器通过服务连接点，运行以下命令以停止并重新启动 BranchCache 服务。  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  通过运行以下命令来检查当前的 BranchCache 操作模式。  
  
    `Get-BCStatus`  
  
4.  在 Windows PowerShell 中，查看的输出**Get BCStatus**命令。  
  
    值**BranchCacheIsEnabled**应**True**。  
  
    在中**ClientSettings**，为值**CurrentClientMode**应**DistributedClient**或**HostedCacheClient**，具体取决于使用本指南配置的模式。  
  
    在中**ClientSettings**，如果配置托管的缓存模式和在配置期间，提供在托管的缓存服务器的名称或如果客户端已自动找到托管缓存服务器使用服务连接点**HostedCacheServerList**应具有与名称或托管的缓存服务器的名称是相同的值。 例如，如果托管的缓存服务器名为 HCS1 和你的域为 corp.contoso.com，值**HostedCacheServerList**是**HCS1.corp.contoso.com**。  
  
5.  如果任何 BranchCache 上面列出的设置没有正确的值，使用本指南中的步骤来验证组策略或本地计算机策略设置，以及防火墙例外情况，配置，并确保它们正确无误。 此外，重新启动计算机，或者按照以下过程刷新组策略并重新启动 BranchCache 服务中的步骤。  
  


