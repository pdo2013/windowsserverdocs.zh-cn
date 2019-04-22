---
title: 部署托管的缓存服务器（可选）
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b19680e933e7a33871816578b63c5a141db0ce00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826208"
---
# <a name="deploy-hosted-cache-servers-optional"></a>部署托管的缓存服务器（可选）

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于安装和配置要部署 BranchCache 托管缓存模式的分支机构中的 BranchCache 托管缓存服务器。 使用 Windows Server 2016 中的 BranchCache，你可部署一个分支机构中的多个托管的缓存服务器。  
  
> [!IMPORTANT]  
> 此步骤是可选的因为分布式的缓存模式下不需要在分支机构中的托管的缓存服务器计算机。 如果您不打算部署托管缓存模式在所有分支机构中的，不需要部署托管的缓存服务器，并不需要执行此过程中的步骤。  
  
您必须是属于**管理员**，或相当于执行此过程。  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>若要安装和配置托管的缓存服务器  
  
1.  在你想要将配置为托管的缓存服务器的计算机，安装 BranchCache 功能的 Windows PowerShell 提示符处运行以下命令。  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  使用以下命令之一，将计算机配置为托管的缓存服务器：  
  
    -   若要将非域加入的计算机配置为托管的缓存服务器，在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
  
        `Enable-BCHostedServer`  
  
    -   若要加入的域的计算机配置为托管的缓存服务器，并注册服务连接点在 Active Directory 中自动托管的缓存服务器发现的客户端计算机，在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  若要验证托管的缓存服务器的正确配置，在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > 部分中运行此命令后**HostedCacheServerConfiguration**，为值**HostedCacheServerIsEnabled**是**True**。 如果在 Active Directory 中的值为注册服务连接点 (SCP) 域已加入托管的缓存服务器配置**HostedCacheScpRegistrationEnabled**是**True**。  
  

