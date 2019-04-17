---
title: "Windows Server Essentials 安装之前"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e7eb1b7bed780b41f1a87589add4ab015f41624a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="before-you-install-windows-server-essentials"></a>Windows Server Essentials 安装之前

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a>你安装的 Windows Server Essentials 之前，请执行以下任务：  

-   **确保你的计算机符合最低硬件要求**。 这包括确定是否你需要额外的硬件和验证你的硬件的驱动程序将受 Windows Server Essentials。 有关详细信息，请参阅[Windows Server Essentials 的系统要求](../get-started/system-requirements.md)。   

  
    > [!IMPORTANT]
    >  已有计算机上安装了 Windows Server Essentials 之前，我们建议你完全格式，然后再重新已有计算机的硬盘分区。 通过格式化并重新硬盘分区，你可以删除隐藏的分区保留在硬盘的可能性。  
  
-   **准备你的网络**准备你的网络，若要安装 Windows Server Essentials，请执行以下操作：  
    
  
    -   **升级你的客户端计算机上的操作系统**Windows Server Essentials 支持以下操作系统： Windows 8、 Windows 7、 Windows 10 和 Macintosh 操作系统 X 明或更高版本。 以下操作系统提供必要的安全功能、 可靠性、 性能和本地网络的功能。  
  
    -   **将配置你的路由器**验证你的路由器已配置，如下所示：  
  
        -   你的路由器上启用了 UPnP 框架。  
  
        -   可以启用或禁用 lan 之外的动态主机配置协议 (DHCP) 服务器服务。  Windows Server Essentials 确保，DHCP 服务器和路由器上未运行？ 在路由器上已启用 DHCP，在安装过程中系统是不在服务器上启用 DHCP。  
  
        -   你已为你的路由器，由你的 Internet 服务提供商 (ISP) 的外部接口 IP 地址。 可以通过在您的 ISP DHCP 服务器的服务动态分配的 IP 地址，或者你必须手动配置使用路由器管理控制台的静态 IP 地址。  
  
        -   如果你的 Internet 连接要求用户名和密码，还以太 (PPPoE)，调用点到点协议即使设备支持 UPnP 框架在你的路由器配置这些设置。  
  
        -   路由器到 lan 之外和 Internet 连接，它已打开，正常。  
  
     如果你的路由器中不支持 UPnP 框架，或在安装过程中无法配置你的路由器，你必须手动配置它设置为你的网络。 确保以下端口处于打开状态，并将定向到目的地服务器的 IP 地址：  
  
    |端口号|应用程序|  
    |-----------------|-----------------|  
    |端口 80|HTTP Web 通信|  
    |端口 443|HTTPS Web 通信|  
  

-   **Windows Server Essentials 发布文档的阅读**。 发布文档包含可能是关键到正确安装和配置 Windows Server Essentials 的最新信息。 若要查看或打印发布的文档，请参阅[发布文档为 Windows Server Essentials](../get-started/release-notes.md)。  
  
## <a name="see-also"></a>请参阅  
  
-   [安装 Windows Server Essentials](Install-Windows-Server-Essentials.md)

