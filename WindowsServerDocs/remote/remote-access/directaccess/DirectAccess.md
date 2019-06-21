---
title: DirectAccess
description: Windows Server 2016 中的 DirectAccess 的简短概述，可以使用本主题。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11c5aa093ddd5aa4777e88c536195bb70bd846db
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281924"
---
# <a name="directaccess"></a>DirectAccess

>适用于：Windows 服务器 （半年频道），Windows Server 2016

有关 DirectAccess，包括服务器和客户端操作系统支持 DirectAccess，简要概述和指向其他 DirectAccess 文档的 Windows Server 2016，你可以使用本主题。  
  
> [!NOTE]  
> 除了本主题提供了以下 DirectAccess 文档。  
>   
> -   [Windows Server 中的 DirectAccess 部署路径](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [部署 DirectAccess 的先决条件](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [DirectAccess 不受支持的配置](DirectAccess-Unsupported-Configurations.md)  
> -   [DirectAccess 测试实验室指南](DirectAccess-Test-Lab-Guides.md)  
> -   [DirectAccess 的已知问题](DirectAccess-Known-Issues.md)  
> -   [DirectAccess 容量规划](DirectAccess-Capacity-Planning.md) 
> -   [DirectAccess 脱机加入域](DirectAccess-Offline-Domain-Join.md)  
> -   [DirectAccess 疑难解答](Troubleshooting-DirectAccess.md)  
> -   [部署单个 DirectAccess 服务器使用开始的向导](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [使用高级设置部署单台 DirectAccess 服务器](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [将 DirectAccess 添加到现有的远程访问 (VPN) 部署](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
通过 DirectAccess，远程用户与组织网络资源，而无需对传统的虚拟专用网络 (VPN) 连接的连接。 使用 DirectAccess 连接，远程客户端计算机始终连接到你的组织-无需为远程用户可以启动和停止的连接，因为需要使用 VPN 连接。 此外，你的 IT 管理员可以管理 DirectAccess 客户端计算机，只要它们正在运行并连接到 Internet。

>[!IMPORTANT]
>请不要尝试在虚拟机上部署远程访问\(VM\)在 Microsoft Azure 中。 不支持在 Microsoft Azure 中使用远程访问。 您不能用于远程访问在 Azure VM 中部署 VPN、 DirectAccess、 或 Windows Server 2016 或 Windows Server 的早期版本中的任何其他远程访问功能。 有关详细信息，请参阅[对 Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。
  
DirectAccess 仅已加入域的客户端提供支持，包括 DirectAccess 的操作系统支持。  
  
以下服务器操作系统支持 DirectAccess。  
  
-   你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2016。  
  
-   你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2012 R2。  
  
-   你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2012。  
  
-   你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2008 R2。  
  
以下客户端操作系统支持 DirectAccess。  
  
-   Windows 10 企业版  
  
-   Windows 10 企业版 2015 Long Term Servicing Branch (LTSB)  
  
-   Windows 8 和 8.1 企业版  
  
-   Windows 7 旗舰版  
  
-   Windows 7 企业版
