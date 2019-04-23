---
title: 附加其他工作站到 MultiPoint server
description: 将多个工作站添加到你的 MultiPoint Services 部署
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 57fc8ed6774c3266298ecd98e8f609ec01f63ef6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889258"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>附加其他工作站到 MultiPoint 服务
在 MultiPoint Services 环境中，你的用户使用的工作站连接到 MultiPoint 服务并完成其工作。 工作站是用于连接到运行 Multipoint 服务的计算机的用户终结点。  
  
MultiPoint 服务支持三种类型的工作站：  
  
-   直接视频连接工作站  
  
-   USB 零客户端连接工作站 （和通过以太网零客户端连接工作站的 USB）  
  
-   RDP over LAN 连接工作站  
  
分类基于工作站的硬件和连接，使用它的类型。 可以混合和匹配您工作站的连接类型。 唯一要求是主站 （它之前安装） 必须直接视频连接的工作站。 有关工作站设置的详细信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。  
  
介绍如何设置每种类型的工作站的说明，请参阅以下资源：  
  
-   [设置直接视频连接工作站](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [设置 USB 零客户端连接工作站](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [设置 RDP over LAN 连接工作站](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
工作站类型的详细比较，请参阅[工作站类型比较](multipoint-services-stations.md#BKMK_StationTypeComparison)。  
  
> [!NOTE]  
> -   用于附加工作站的过程不介绍如何设置中间中心或下游集线器。 有关在何处安装这些中心的信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。  
> -   在某些情况下，可能需要创建虚拟机中运行的虚拟桌面的工作站。 例如，你使用不能在 Windows Server 上安装的应用程序或应用程序将在同一台主机计算机上运行多个实例。 有关详细信息，请参阅[创建 Windows 10 企业版虚拟桌面工作站](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md)。  
  
> [!TIP]  
> 它可用于创建按其物理位置顺序在工作站，以便按顺序标识 MultiPoint Server 中。 如果以后想要更改的工作站名称，可以执行的操作在 MultiPoint 管理器中。 有关详细信息，请参阅重映射 MultiPoint Server 帮助和支持全部站配合。