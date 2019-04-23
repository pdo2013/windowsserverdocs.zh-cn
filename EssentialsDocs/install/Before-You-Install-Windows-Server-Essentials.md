---
title: 安装 Windows Server Essentials 之前的准备事项
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828718"
---
# <a name="before-you-install-windows-server-essentials"></a>安装 Windows Server Essentials 之前的准备事项

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a> 在开始您的 Windows Server Essentials 的安装之前，请执行以下任务：  

-   **请确保你的计算机符合最低硬件要求**。 这包括确定是否需要额外的硬件和验证，通过 Windows Server Essentials 支持您的硬件的驱动程序。 有关详细信息，请参阅[Windows Server Essentials 的系统要求](../get-started/system-requirements.md)。   

  
    > [!IMPORTANT]
    >  在预先存在的计算机上安装 Windows Server Essentials 之前，我们建议您完全格式化，然后进行预先存在的计算机的硬盘重新分区。 通过对硬盘进行格式化和重新分区，你可以消除硬盘存在隐藏分区的可能性。  
  
-   **准备网络**若要准备网络以安装 Windows Server Essentials，请执行以下操作：  
    
  
    -   **客户端计算机上的操作系统升级**Windows Server Essentials 支持以下操作系统：Windows 8、 Windows 7、 Windows 10 和 Macintosh OS X Lion 或更高版本。 这些操作系统为本地网络提供了必要的安全功能、可靠性、性能和功能。  
  
    -   **配置路由器** 检查路由器的配置是否符合下述要求：  
  
        -   路由器上已启用 UPnP 框架。  
  
        -   可启用或禁用 LAN 的动态主机配置协议 (DHCP) 服务器服务。  Windows Server Essentials 可确保 DHCP 服务器和路由器上均不运行？ 在路由器上启用 DHCP，则在安装过程中系统是未在服务器上启用 DHCP。  
  
        -   路由器有一个外部接口 IP 地址，该地址由 Internet 服务提供商 (ISP) 提供。 IP 地址可由 ISP 的 DHCP 服务器服务动态分配，或者你必须使用路由器管理控制台手动配置静态 IP 地址。  
  
        -   如果你的 Internet 连接需要用户名和密码（也称为点对点以太网协议 (PPPoE)），则即使设备支持 UPnP 框架，也要在路由器上配置这些设置。  
  
        -   路由器连接到 LAN 和 Internet 并打开，而且可以正常工作。  
  
     如果路由器不支持 UPnP 框架，或者如果在安装过程中无法配置路由器，则必须使用网络设置来手动配置路由器。 确保以下端口已打开并且定向到目标服务器的 IP 地址：  
  
    |端口号|应用程序|  
    |-----------------|-----------------|  
    |端口 80|HTTP Web 流量|  
    |端口 443|HTTPS Web 流量|  
  

-   **阅读 Windows Server Essentials 发行文档**。 发行文档包含重要的正确安装和配置 Windows Server Essentials 的最新信息。 若要查看或打印发行文档，请参阅[Release Documentation for Windows Server Essentials](../get-started/release-notes.md)。  
  
## <a name="see-also"></a>请参阅  
  
-   [安装 Windows Server Essentials](Install-Windows-Server-Essentials.md)

