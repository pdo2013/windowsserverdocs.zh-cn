---
title: 安装分支缓存功能和配置托管的缓存服务器服务连接点
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 854ff9f80a2221a857fab4e6ea7f7c8e6d51f1ef
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>安装分支缓存功能和配置托管的缓存服务器服务连接点

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在你托管的缓存服务器上，HCS1，安装分支缓存功能并配置服务器注册服务连接点，你可以使用此过程中 Active Directory 域服务 \(AD DS\) \(SCP\)。

与广告 DS 中 SCP 注册托管的缓存服务器时，SCP 使客户端正确配置为自动 SCP 查询广告 DS 发现托管的缓存服务器的计算机。 更高版本中本指南提供了如何将配置客户端计算机上的说明进行操作，若要执行此操作。

>[!IMPORTANT]
>执行此过程之前，你必须加入域的计算机并将计算机配置的静态 IP 地址。

若要执行此步骤，必须是管理员组中的成员。

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>若要安装分支缓存功能和配置托管的缓存服务器  

1. 以管理员身份运行 Windows PowerShell 服务器计算机。 键入以下命令，然后再次按 ENTER。

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  若要将计算机配置为托管的缓存服务器后安装分支缓存的功能，并在广告 DS 注册服务连接点，Windows PowerShell 中键入以下命令，然后按 ENTER。

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. 若要验证托管的缓存服务器配置，请键入以下命令，然后按 ENTER。

    ```  
    Get-BCStatus  
    ```  
  
    命令的结果中显示分支缓存安装的所有方面的状态。 以下是几个分支缓存设置和正确的值为每个项目：  
  
    -   BranchCacheIsEnabled： 如此

    -   HostedCacheServerIsEnabled： 如此

    -   HostedCacheScpRegistrationEnabled： 如此

4. 准备的步骤从你的内容服务器的数据包复制到托管的缓存服务器的这两识别现有的缓存托管的服务器上共享，或创建一个新文件夹和共享文件夹，以便它可以从你的内容服务器访问。 在你的内容服务器上创建你的数据程序包后，你将将数据包复制到此托管的缓存服务器上的共享文件夹中。
  
5. 如果你部署的多个托管的缓存服务器，请重复每个服务器上的此过程。

若要继续使用本指南，请参阅[移动和调整托管缓存 #40; 可选 & #41;](6-Bc-Move-Resize-Cache.md).
