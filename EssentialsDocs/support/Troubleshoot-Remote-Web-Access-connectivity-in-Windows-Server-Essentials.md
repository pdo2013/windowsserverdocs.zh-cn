---
title: Windows Server Essentials 远程 Web 访问连接疑难解答
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: fda0b5a227fe25b4e8780915089e97ee48620383
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432435"
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Windows Server Essentials 远程 Web 访问连接疑难解答
 
>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 通常情况下，如果路由器是经 UPnP 认证的设备并且路由器上的 UPnP 设置已启用，则 Windows Server Essentials 可以自动配置宽带路由器。  
  
## <a name="possible-issues"></a>可能的问题  
 你可能会遇到以下远程 Web 访问连接问题：  
  
-   路由器未打开或未连接到网络。  
  
-   路由器的 UPnP 设置已关闭。  
  
-   路由器可能不完全支持 UPnP 标准。 Microsoft 保留了可与 Windows 操作系统协同工作的路由器的列表。 若要查看与 Windows Server Essentials 兼容的路由器的列表，请访问 [Windows 兼容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。  
  
## <a name="possible-fixes"></a>可能的修复方法  
 以下操作可能可以修复这些问题：  
  
- 确认路由器已打开且正常运行。  
  
- 确保服务器已直接连接到路由器或已连接到与路由器相连的交换机。  
  
- 验证连接到你的 Internet 服务提供商的宽带设备是否启用、是否正常工作，并验证你的路由器是否连接到该宽带设备。  
  
- 启用路由器的 UPnP 设置。 连接到路由器的配置网页以启用 UPnP 设置。 有关如何登录路由器以及如何启用 UPnP 设置的信息，请参阅路由器文档。 启用 UPnP 设置后，将在远程 Web 访问再次运行向导以配置你的路由器。  
  
- 如果你的路由器不完全支持 UPnP 标准，则无法自动配置。 你必须手动配置路由器，或购买一台支持 UPnP 标准的路由器。  
  
   若要手动配置路由器，请完成以下任务：  
  
  - 为 Windows Server Essentials 服务器创建 IP 地址保留。  
  
     在手动将路由器配置为将所需端口转发到 Windows Server Essentials 之前，必须在路由器上为正在运行 Windows Server Essentials 的服务器设置动态主机配置协议 (DHCP) 保留。 此步骤将保证端口所转发到的 IP 地址不会发生变化。  
  
     有关如何手动设置你的服务器在路由器上的 DHCP 保留的信息，请参阅路由器制造商的文档。  
  
  - 在路由器上为以下端口配置端口转发：  
  
    |服务或协议|Port|  
    |-------------------------|----------|  
    |HTTP|TCP 80|  
    |HTTPS|TCP 443|  
  
    有关如何手动设置端口转发在路由器上的信息，请参阅制造商的文档。  
  
    典型的路由器配置页面包括一张与下表类似的表。  
  
  > [!NOTE]
  >  在此表中，运行 Windows Server Essentials 的计算机的 IP 地址为 192.168.0.100。 你必须确定计算机的 IP 地址，并将该 IP 地址替换为表中所示 IP 地址。  
  
  |IP 地址|协议 (TCP/UDP)|计划|入站筛选器|  
  |----------------|---------------------------|--------------|--------------------|  
  |192.168.0.100|TCP 80|始终|全部允许|  
  |192.168.0.100|TCP 443|始终|全部允许|  
  
   手动配置路由器之后，运行将在远程 Web 访问向导，确保你选择**跳过路由器设置**选项卡上**入门**页。  
  
- 如果你的路由器不完全支持 UPnP 标准，请购买新的路由器。  
  
> [!TIP]
>  请确保你的路由器安装了最新的 BIOS 固件。 通常，你可以从路由器配置网页为路由器更新 BIOS 固件。 有关详细信息，请参阅你的路由器文档。 更新路由器后，请运行“设置随处访问”向导。  
  
## <a name="see-also"></a>请参阅  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理远程 Web 访问](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理随处访问](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [支持 Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [支持 Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

