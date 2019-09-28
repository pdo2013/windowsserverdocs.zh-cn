---
title: 部署托管的缓存服务器（可选）
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69dc525a093c86d57b665e26ff5acaf2679c81a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356443"
---
# <a name="deploy-hosted-cache-servers-optional"></a>部署托管的缓存服务器（可选）

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用此过程来安装和配置 BranchCache 托管缓存服务器，这些服务器位于要在其中部署 BranchCache 托管缓存模式的分支机构。 借助 Windows Server 2016 中的 BranchCache，你可以在一个分支机构中部署多个托管缓存服务器。  
  
> [!IMPORTANT]  
> 此步骤是可选的，因为分布式缓存模式不需要在分支机构中托管缓存服务器计算机。 如果不打算在任何分支机构中部署托管缓存模式，则无需部署托管缓存服务器，并且无需执行此过程中的步骤。  
  
你必须是**管理员**的成员或等效于执行此过程。  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>安装和配置托管缓存服务器  
  
1.  在要配置为托管缓存服务器的计算机上，在 Windows PowerShell 提示符下运行以下命令以安装 BranchCache 功能。  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  使用以下命令之一将计算机配置为托管缓存服务器：  
  
    -   若要将未加入域的计算机配置为托管缓存服务器，请在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
  
        `Enable-BCHostedServer`  
  
    -   若要将已加入域的计算机配置为托管缓存服务器，并在 Active Directory 中注册客户端计算机自动托管缓存服务器发现的服务连接点，请在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  若要验证托管缓存服务器的配置是否正确，请在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > 运行此命令后，在**HostedCacheServerConfiguration**节中， **HostedCacheServerIsEnabled**的值为**True**。 如果在 Active Directory 中配置了已加入域的托管缓存服务器来注册服务连接点（SCP），则**HostedCacheScpRegistrationEnabled**的值为**True**。  
  

