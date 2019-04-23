---
title: 收集安装所需的硬件和设备驱动程序
description: 有关驱动程序需要安装适用于 MultiPoint 服务的信息
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: a9d902e2599cdcd69e156d1fabec87a067b1d8ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833418"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>收集安装所需的硬件和设备驱动程序
在开始部署你的 MultiPoint 服务系统之前，你将需要：  
  
-   **服务器的硬件组件**-在此时间安装任何其他的视频卡或其他系统组件。  
  
-   **工作站的硬件组件**-有关规划工作站对您的环境有关的信息，请参阅[选择你的 MultiPoint 服务系统的硬件](Selecting-Hardware-for-Your-MultiPoint-services-System.md)。
-   **视频卡的最新驱动程序**-如果您的 OEM 或设备制造商不提供这些，您需要从设备制造商的网站下载它们。  
  
-   **最新的 USB 零客户端驱动程序**-如果使用的 USB 零客户端工作站，则必须安装最新的 USB 零客户端驱动程序。  
  
    > [!IMPORTANT]  
    > 为 MultiPoint 服务的安装，必须安装 64 位版本的任何驱动程序。  
  
> [!TIP]  
> 如果使用不同版本的已安装的 Windows 的计算机上安装 MultiPoint 服务，您应该了解的视频卡品牌和型号设备管理器中之前启动的 Windows Server 安装并确保可以获取这些驱动程序适用于 Windows Server 2016。 打开设备管理器中，打开**计算机管理**从**启动**屏幕。 然后，在控制台树中，单击**设备管理器**。