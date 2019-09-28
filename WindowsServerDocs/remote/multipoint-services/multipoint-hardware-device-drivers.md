---
title: 收集安装所需的硬件和设备驱动程序
description: 有关需要为 MultiPoint 服务安装的驱动程序的信息
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cfbb8c8b68768c72b869df539c93f05e7e01d256
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394701"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>收集安装所需的硬件和设备驱动程序
在开始部署你的 MultiPoint 服务系统之前，你将需要：  
  
-   **服务器的硬件组件**-此时安装任何其他视频卡或其他系统组件。  
  
-   **工作站的硬件组件**-有关为你的环境规划工作站的信息，请参阅[选择 MultiPoint 服务系统的硬件](Selecting-Hardware-for-Your-MultiPoint-services-System.md)。
-   **视频卡的最新驱动程序**-如果 OEM 或设备制造商未提供这些驱动程序，你将需要从设备制造商的网站下载它们。  
  
-   **最新的 usb 零客户端驱动程序**-如果使用的是 usb 零客户端工作站，则必须安装最新的 usb 零客户端驱动程序。  
  
    > [!IMPORTANT]  
    > 对于 MultiPoint 服务安装，必须安装任何驱动程序的64位版本。  
  
> [!TIP]  
> 如果在已安装了不同版本的 Windows 的计算机上安装 MultiPoint 服务，则在开始 Windows Server 安装之前应了解设备管理器视频卡的品牌和型号，并确保可以获得适用于 Windows Server 2016。 打开设备管理器，从 "**开始**" 屏幕打开 "**计算机管理**"。 然后，在控制台树中，单击 "**设备管理器**"。