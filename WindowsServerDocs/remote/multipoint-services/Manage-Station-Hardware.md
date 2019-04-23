---
title: 管理工作站硬件
description: 概述了如何管理 MultiPoint 工作站硬件
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b8539-b17a-4e01-9576-860600466451
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 3a1cdfd12c9bd6a21fec9cbfffae04573ef5ea98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841628"
---
# <a name="manage-station-hardware"></a>管理工作站硬件
MultiPoint 服务系统包含单个计算机和至少一个工作站。 工作站硬件通常由工作站集线器、 鼠标、 键盘和视频显示器组成。 通常将工作站物理连接到计算机。  
  
下图显示了一个具有四个工作站的 MultiPoint 服务系统的布局示例。 每个工作站使用 USB 集线器和多显示器视频卡连接到 MultiPoint 服务计算机。 此插图未显示使用多功能集线器连接的工作站。  
   
![基于 MultiPoint Services USB 系统布局的图像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
本节中所含主题介绍了如何查看连接到 MultiPoint 服务系统的硬件的状态，并提供了可用来设置 MultiPoint 服务工作站的 USB 设备及其他外围硬件设备的类型的详细信息。 以下是对本节中所含主题的简短说明，可帮助你选择硬件以及设置 MultiPoint 服务工作站。  
  
## <a name="view-hardware-status"></a>查看硬件状态  
在 MultiPoint 管理器中，你可以使用**工作站**选项卡以查看工作站和连接到 MultiPoint 服务计算机的硬件设备的状态。 有关如何查看 MultiPoint 服务计算机以及连接到此计算机的硬件设备的状态的详细信息，请参阅[查看硬件状态](View-Hardware-Status.md)主题。  
  
## <a name="work-with-usb-devices"></a>使用 USB 设备  
USB 设备以及其他外围硬件设备连接到 MultiPoint Services 系统时，其工作方式在它们连接到 MultiPoint Services 计算机时与它们连接到 MultiPoint Services 工作站时不同。 [使用 USB 设备](Work-with-USB-Devices.md)主题介绍了不同的硬件设备在这些方案中的运作方式，并提供了如何使用工作站集线器的详细信息。  
  
## <a name="work-with-video-devices"></a>使用视频设备  
[使用视频设备](Work-with-Video-Devices.md)主题讨论了视频设备（例如显示器或投影仪）在连接到 MultiPoint 服务系统中的计算机或连接到 MultiPoint 服务工作站时的运作方式。  
  
## <a name="set-up-a-station"></a>设置工作站  
[设置工作站](Set-Up-a-Station.md)主题描述了如何将外围硬件设备连接到 MultiPoint 服务工作站集线器来创建 MultiPoint 服务工作站。 MultiPoint 服务支持两种类型的工作站集线器：  
  
-   从外部供电或总线供电的多端口*USB 集线器*，可以支持鼠标、 键盘和其他 USB 外围设备  
  
-   一个*多功能集线器*，可以包括集成视频控制器和鼠标、 键盘以及音频外围设备的端口  
  
两种类型的工作站集线器均通过 USB 电缆连接到计算机。 [设置工作站](Set-Up-a-Station.md)主题中的过程描述了如何将硬件设备连接到每种类型的工作站集线器。  
  
## <a name="see-also"></a>请参阅  
[查看硬件状态](View-Hardware-Status.md)  
[使用 USB 设备](Work-with-USB-Devices.md)  
[使用视频设备](Work-with-Video-Devices.md)  
[设置工作站](Set-Up-a-Station.md)