---
title: DirectAccess
description: 您可以使用本主题来了解 Windows Server 2016 中 DirectAccess 的简要概述。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4cc5ed275a6dc3cd235aa0ff1bfb0f0f46350a2b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394510"
---
# <a name="directaccess"></a>DirectAccess

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解 DirectAccess 的简要概述，其中包括支持 DirectAccess 的服务器和客户端操作系统，以及指向 Windows Server 2016 的其他 DirectAccess 文档的链接。  
  
> [!NOTE]  
> 除了本主题之外，还提供了以下 DirectAccess 文档。  
>   
> -   [Windows Server 中的 DirectAccess 部署路径](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [部署 DirectAccess 的先决条件](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [DirectAccess 不受支持的配置](DirectAccess-Unsupported-Configurations.md)  
> -   [DirectAccess 测试实验室指南](DirectAccess-Test-Lab-Guides.md)  
> -   [DirectAccess 的已知问题](DirectAccess-Known-Issues.md)  
> -   [DirectAccess 容量规划](DirectAccess-Capacity-Planning.md) 
> -   [DirectAccess 脱机加入域](DirectAccess-Offline-Domain-Join.md)  
> -   [DirectAccess 疑难解答](Troubleshooting-DirectAccess.md)  
> -   [使用入门向导部署单个 DirectAccess 服务器](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [使用高级设置部署单台 DirectAccess 服务器](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [将 DirectAccess 添加到现有的远程访问 (VPN) 部署](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
通过 DirectAccess，远程用户可以连接到组织网络资源，而无需进行传统的虚拟专用网络（VPN）连接。 通过 DirectAccess 连接，远程客户端计算机始终连接到你的组织-无需远程用户启动和停止连接，这在 VPN 连接中是必需的。 此外，你的 IT 管理员可以在运行和 Internet 连接时管理 DirectAccess 客户端计算机。

>[!IMPORTANT]
>请勿尝试在 Microsoft Azure 的虚拟机上部署远程访问 \(VM @ no__t-1。 不支持在 Microsoft Azure 中使用远程访问。 不能在 Azure VM 中使用远程访问在 Windows Server 2016 或更早版本的 Windows Server 中部署 VPN、DirectAccess 或任何其他远程访问功能。 有关详细信息，请参阅[Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。
  
DirectAccess 仅为加入域的客户端提供支持，其中包括对 DirectAccess 的操作系统支持。  
  
以下服务器操作系统支持 DirectAccess。  
  
-   你可以将所有版本的 Windows Server 2016 部署为 DirectAccess 客户端或 DirectAccess 服务器。  
  
-   你可以将所有版本的 Windows Server 2012 R2 作为 DirectAccess 客户端或 DirectAccess 服务器进行部署。  
  
-   你可以将所有版本的 Windows Server 2012 部署为 DirectAccess 客户端或 DirectAccess 服务器。  
  
-   你可以将所有版本的 Windows Server 2008 R2 作为 DirectAccess 客户端或 DirectAccess 服务器进行部署。  
  
以下客户端操作系统支持 DirectAccess。  
  
-   Windows 10 企业版  
  
-   Windows 10 企业版 2015 Long Term Servicing Branch （LTSB）  
  
-   Windows 8 和8.1 企业版  
  
-   Windows 7 旗舰版  
  
-   Windows 7 企业版
