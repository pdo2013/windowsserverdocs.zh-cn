---
title: "在 Windows Server Essentials 远程访问 Web 连接疑难解答"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: af4725fd3b1861c847434e3ed62c3da030689fb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>在 Windows Server Essentials 远程访问 Web 连接疑难解答
 
>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 通常情况下，Windows Server Essentials 可以自动配置宽带路由器路由器是否 UPnP 已认证的设备，如果在路由器上启用了 UPnP 设置。  
  
## <a name="possible-issues"></a>可能出现的问题  
 使用远程访问 Web 连接，你可能会遇到的以下问题：  
  
-   你的路由器未处于打开状态，或者未连接到你的网络。  
  
-   你的路由器 UPnP 设置处于关闭状态。  
  
-   你的路由器可能不会完全支持的 UPnP 标准。 Microsoft 维护的适用于 Windows 操作系统的路由器的列表。 若要查看路由器（包括无线路由器）与 Windows Server Essentials 兼容的列表，请访问[Windows 兼容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。  
  
## <a name="possible-fixes"></a>可能修补程序  
 以下操作可以修复这些问题：  
  
-   验证你的路由器已打开并且正在正常运行。  
  
-   确保你服务器直接连接到你的路由器或它已连接到已连接到你的路由器切换。  
  
-   验证是否连接到你的 Internet 服务提供商 (ISP) 的宽带的设备已打开，正常工作，并且你的路由器已连接到宽带的设备。  
  
-   打开你的路由器 UPnP 设置。 连接到路由器打开 UPnP 设置配置网页。 有关如何登录到你的路由器以及如何打开 UPnP 设置的信息，请参阅路由器相关文档。 打开 UPnP 设置后，运行打开上远程 Web 访问向导再次来配置你的路由器。  
  
-   如果你的路由器完全支持的 UPnP 标准，它无法自动配置。 你必须手动配置你的路由器或购买的路由器支持 UPnP 标准。  
  
     若要手动配置你的路由器，完成以下任务：  
  
    -   创建适用于你的 Windows Server Essentials server 保留的 IP 地址。  
  
         你手动配置路由器将所需的端口转发给 Windows Server Essentials 之前，你必须为你运行的 Windows Server Essentials 在路由器的服务器设置动态主机配置协议 (DHCP) 预订。 此步骤保证您转发端口不会更改的 IP 地址。  
  
         有关如何手动设置为在你的路由器服务器 DHCP 预留的信息，请参阅路由器的制造商的文档。  
  
    -   在路由器上的以下端口配置端口转发：  
  
        |服务或协议|端口|  
        |-------------------------|----------|  
        |HTTP|TCP 80|  
        |HTTPS|TCP 443|  
  
     有关如何手动设置端口转发路由器上的信息，请参阅制造商的文档。  
  
     典型路由器配置网页包括类似于如下表。  
  
    > [!NOTE]
    >  在表中，运行的 Windows Server Essentials 计算机的 IP 地址是 192.168.0.100。 必须确定你的计算机的 IP 地址并替换表所示的 IP 地址的 IP 地址。  
  
    |IP 地址|协议 (TCP/UDP)|计划|归巢的筛选器|  
    |----------------|---------------------------|--------------|--------------------|  
    |192.168.0.100|TCP 80|始终|允许所有|  
    |192.168.0.100|TCP 443|始终|允许所有|  
  
     你手动配置你的路由器后，运行打开上远程 Web 访问向导中，确保你选择**跳过路由器设置**选项卡上**入门**页面。  
  
-   如果你的路由器完全不支持 UPnP 标准，购买新路由器。  
  
> [!TIP]
>  确保你的路由器已安装最新的 BIOS 固件。 在路由器配置网页来自路由器，你可以经常更新 BIOS 固件。 有关详细信息，请参阅路由器相关文档。 你的路由器已更新后，运行设置任何地方访问向导。  
  
## <a name="see-also"></a>请参阅  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Web 远程访问](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理任意位置的访问权限](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [支持 Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [支持 Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

