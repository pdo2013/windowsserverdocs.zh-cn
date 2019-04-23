---
title: 通过服务连接点安装 BranchCache 功能并配置托管缓存服务器
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6619b09df0d4c161148d22091337a5039c7ea3af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849648"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>通过服务连接点安装 BranchCache 功能并配置托管缓存服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

您可以使用此过程，在托管的缓存服务器，HCS1 上, 安装 BranchCache 功能并配置服务器注册服务连接点\(SCP\) Active Directory 域服务中\(AD DS\).

与 AD DS 中 SCP 注册托管的缓存服务器时，SCP 会允许客户端正确配置为自动发现通过查询 SCP AD DS 托管的缓存服务器的计算机。 在本指南后面提供了有关如何配置客户端计算机的说明执行此操作。

>[!IMPORTANT]
>在执行此过程之前，必须将计算机加入到域，并将计算机配置具有静态 IP 地址。

若要执行该过程，你必须是 Administrators 组的成员。

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>若要安装 BranchCache 功能和配置托管的缓存服务器  

1. 服务器计算机上以管理员身份运行 Windows PowerShell。 输入以下命令，然后按 ENTER。

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  若要将计算机配置为托管的缓存服务器之后安装 BranchCache 功能，并在 AD DS 中注册服务连接点，在 Windows PowerShell 中，键入以下命令，然后按 ENTER。

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. 若要验证托管的缓存服务器配置，请键入以下命令并按 ENTER。

    ```  
    Get-BCStatus  
    ```  
  
    命令的结果显示 BranchCache 安装的所有方面的状态。 以下是几个 BranchCache 设置和正确的值为每个项：  
  
    -   BranchCacheIsEnabled:True

    -   HostedCacheServerIsEnabled:True

    -   HostedCacheScpRegistrationEnabled:True

4. 若要准备将您的数据的包从内容服务器复制到托管的缓存服务器的步骤，这两标识托管的缓存服务器上现有的共享，或创建一个新文件夹和共享文件夹，以便对其进行访问内容服务器中。 在内容服务器上创建您的数据的包后，你会将数据包复制到托管的缓存服务器上此共享文件夹。
  
5. 如果要部署多个托管的缓存服务器，重复此过程在每个服务器上。

若要继续使用本指南，请参阅[移动和调整托管缓存&#40;可选&#41;](6-Bc-Move-Resize-Cache.md)。
