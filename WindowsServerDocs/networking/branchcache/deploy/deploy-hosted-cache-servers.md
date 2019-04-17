---
title: 部署托管的缓存服务器（可选）
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d7345b9acf5ef5003cc2a811569083d7c12894a1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-hosted-cache-servers-optional"></a>部署托管的缓存服务器（可选）

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程来安装和配置分支缓存托管缓存服务器位于你想要部署托管分支缓存缓存模式分支机构。 与 Windows Server 2016 中分支缓存，你可以将部署多个托管的缓存服务器，在一个分支机构。  
  
> [!IMPORTANT]  
> 这一步是可选的因为分布式的缓存模式不需要托管的缓存服务器计算机中分支机构。 如果你未计划部署托管中任何分支机构的缓存模式、不需要部署托管的缓存服务器，并不需要执行此过程中的步骤。  
  
你必须成员**管理员**，或相当要执行此过程。  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>若要安装和配置托管的缓存 server  
  
1.  你想要配置为服务器托管的缓存的计算机，在安装分支缓存功能的 Windows PowerShell 提示中运行以下命令。  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  将计算机配置为托管的缓存服务器中，使用以下命令之一：  
  
    -   若要配置为托管的缓存服务器非域连接的计算机，Windows PowerShell 提示时，键入以下命令，然后按 ENTER。  
  
        `Enable-BCHostedServer`  
  
    -   若要配置某个域连接的计算机托管的缓存服务器，并注册服务连接点的 Active Directory 中自动托管的缓存服务器发现的客户端计算机键入以下命令，在 Windows PowerShell 提示符下，，然后按 enter 键。  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  若要验证托管的缓存服务器的正确配置 Windows PowerShell 提示时，键入以下命令，然后按 ENTER。  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > 你的部分中运行此命令之后, **HostedCacheServerConfiguration**的值**HostedCacheServerIsEnabled**是**如此**。 如果你在配置 Active Directory 的值为中注册服务 (SCP) 连接点的域连接托管的缓存服务器**HostedCacheScpRegistrationEnabled**是**如此**。  
  

