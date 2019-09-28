---
title: 通过服务连接点安装 BranchCache 功能并配置托管缓存服务器
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe2120310c6c410b410649aff1372f93e0ea5db7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356344"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>通过服务连接点安装 BranchCache 功能并配置托管缓存服务器

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

你可以使用此过程在托管缓存服务器 HCS1 上安装 BranchCache 功能，并将服务器配置为在 Active Directory 域服务 \(AD DS @ no__t-3 中注册服务连接点 \(SCP @ no__t。

在 AD DS 中使用 SCP 注册托管缓存服务器时，SCP 允许配置正确的客户端计算机通过查询 SCP AD DS 来自动发现托管缓存服务器。 本指南后面提供了有关如何将客户端计算机配置为执行此操作的说明。

>[!IMPORTANT]
>执行此过程之前，必须将计算机加入域，并使用静态 IP 地址来配置计算机。

若要执行该过程，你必须是 Administrators 组的成员。

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>安装 BranchCache 功能并配置托管缓存服务器  

1. 在服务器计算机上，以管理员身份运行 Windows PowerShell。 输入以下命令，然后按 ENTER。

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  若要在安装 BranchCache 功能后将计算机配置为托管缓存服务器，并在 AD DS 中注册服务连接点，请在 Windows PowerShell 中键入以下命令，然后按 ENTER。

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. 若要验证托管缓存服务器配置，请键入以下命令，然后按 ENTER。

    ```  
    Get-BCStatus  
    ```  
  
    命令的结果显示 BranchCache 安装的所有方面的状态。 下面是一些 BranchCache 设置，以及每个项的正确值：  
  
    -   BranchCacheIsEnabled:True

    -   HostedCacheServerIsEnabled:True

    -   HostedCacheScpRegistrationEnabled:True

4. 若要准备将数据包从内容服务器复制到托管缓存服务器的步骤，请标识托管缓存服务器上的现有共享，或创建新文件夹并共享该文件夹，以便可以从内容服务器访问该文件夹。 在内容服务器上创建数据包后，会将数据包复制到托管缓存服务器上的此共享文件夹中。
  
5. 如果要部署多个托管缓存服务器，请在每个服务器上重复此过程。

若要继续学习本指南，请参阅[移动托管缓存&#40;并调整&#41;其大小（可选](6-Bc-Move-Resize-Cache.md)）。
