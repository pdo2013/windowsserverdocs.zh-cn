---
title: 将其他工作站附加到 MultiPoint 服务器
description: 将更多工作站添加到 MultiPoint 服务部署
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
ms.openlocfilehash: 70609d491f5eb60daf89df219c06c8b9d4c3cd3e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871410"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>将其他工作站附加到 MultiPoint 服务
在 MultiPoint 服务环境中，用户使用工作站连接到 MultiPoint 服务并完成其工作。 工作站是用于连接到运行 Multipoint 服务的计算机的用户终结点。  
  
MultiPoint 服务支持三种类型的工作站：  
  
-   直接连接视频的工作站  
  
-   USB 零客户端连接的工作站（和通过以太网的零客户端连接的工作站）  
  
-   通过局域网的 RDP 连接工作站  
  
分类基于工作站的硬件及其使用的连接类型。 可以混合和匹配工作站的连接类型。 唯一的要求是，主工作站（你之前安装的工作站）必须是直接连接视频的工作站。 有关工作站设置的详细信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。  
  
有关说明如何设置每种类型的工作站的说明，请参阅以下内容：  
  
-   [设置直接视频连接的工作站](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [设置 USB 零客户端连接的工作站](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [设置 RDP-over-LAN 连接的工作站](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
有关工作站类型的详细比较，请参阅[工作站类型比较](multipoint-services-stations.md#BKMK_StationTypeComparison)。  
  
> [!NOTE]  
> -   附加工作站的过程不介绍如何设置中间集线器或下游集线器。 有关安装这些中心的位置的信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。  
> -   在某些情况下，可能需要创建在虚拟机中运行的工作站虚拟桌面。 例如，使用无法在 Windows Server 上安装的应用程序，也不能在同一台主机上运行多个实例的应用程序。 有关详细信息，请参阅为[工作站创建 Windows 10 企业虚拟桌面](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md)。  
  
> [!TIP]  
> 按照物理位置的顺序创建工作站是非常有用的，以便在 MultiPoint Server 中按顺序标识它们。 如果以后要更改工作站的名称，可在 MultiPoint 管理器中执行此操作。 有关详细信息，请参阅在 MultiPoint Server 帮助和支持中重新映射所有工作站。