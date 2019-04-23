---
title: 设置工作站
description: 了解如何设置在 MultiPoint 服务工作站
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dce05b6c-795e-43b2-9920-026550b873c5
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2bba32f27ae01052a693d78f152d4487a04bd9bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880678"
---
# <a name="set-up-a-station"></a>设置工作站
MultiPoint 服务工作站通常包括工作站集线器、鼠标、键盘和视频显示器。 本主题描述如何将硬件设备连接到工作站集线器，以创建 MultiPoint 服务工作站。  
  
工作站集线器是一种将外围设备连接到 MultiPoint 服务系统中的计算机的硬件设备。 MultiPoint 服务支持两种类型的工作站集线器：  
  
-   **USB 集线器：** 通用多端口 USB 扩展集线器符合通用串行总线 2.0 或更高版本规范。 此类集线器通常有两个、四个或更多个 USB 端口，可以将多个 USB 设备连接到计算机上的单个 USB 端口。 USB 集线器是可能由外部供电或总线供电的通常独立设备。 作为工作站集线器配合 MultiPoint 服务使用时，建议使用带有四个或更多端口的集线器。  
  
    > [!IMPORTANT]  
    > 如果你打算将键盘和鼠标以外的 USB 设备连接到集线器，为达到最佳性能，建议使用外部供电的集线器。  
  
-   **多功能集线器：** 扩展集线器连接到的计算机通过 USB 端口，以及启用各种到中心，包括视频显示器的非 USB 设备的连接。 多功能集线器由特定硬件制造商生产，可能需要安装特定于设备的驱动程序。  
  
如要添加工作站到 MultiPoint 服务系统，首先确保具有足够的连接端口可供要使用的工作站硬件利用。 此外，必须确保适当数量的安全*客户端访问许可证 (Cal)* 为 MultiPoint 服务系统。  
  
## <a name="setting-up-station-hardware"></a>设置工作站硬件  
本节中的过程描述如何将 MultiPoint 服务工作站硬件连接到不同类型的工作站集线器。  
  
### <a name="to-set-up-a-station-with-a-usb-hub"></a>通过 USB 集线器设置工作站  
  
1.  连接新工作站前，先结束所有用户会话，然后关闭计算机和 MultiPoint 服务系统中其他用电设备。  
  
2.  将新的视频显示器电缆连接到计算机上的视频显示端口，如下图所示：  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif)  
  
3.  将新的 USB 集线器连接到计算机上的开放 USB 端口：  
  
    ![MultiPoint Server USB 集线器连接图像](./media/WMSUSBHubConnection.gif)  
  
4.  将键盘和鼠标连接到 USB 集线器：  
  
    ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
5.  将视频显示器的电源线连接到电源插座。  
  
6.  打开计算机。  
  
7.  MultiPoint 服务启动。 按照新的工作站视频显示器上显示的指令将设备关联到新的工作站。  
  
### <a name="to-set-up-a-station-with-a-multifunction-hub"></a>通过多功能集线器设置工作站  
  
1.  连接新工作站前，先结束所有用户会话，然后关闭计算机和 MultiPoint 服务系统中其他用电设备。  
  
2.  将新的视频显示器电缆连接到多功能集线器上的 DVI 或 VGA 视频显示端口，如下图所示：  
  
    ![多功能集线器和视频连接图像](./media/WMSMultifunctionHubVideoConnection.gif)  
  
3.  将新的多功能集线器连接到计算机上的开放 USB 端口：  
  
    ![多功能集线器连接图像](./media/WMSMultifunctionHubConnection.gif)  
  
4.  将键盘和鼠标连接到多功能集线器上的 PS2 或 USB 连接器：  
  
    ![多功能集线器和 PS2 连接器图像](./media/WMSMultifunctionHubPS2Connection.gif)  
  
5.  将视频显示器的电源线连接到电源插座。  
  
6.  打开计算机。  
  
7.  MultiPoint 服务启动。 如果出现提示，则按照新工作站的视频显示器中出现的指令将设备关联到新的工作站。  
  
## <a name="see-also"></a>请参阅  
[结束用户会话](End-a-User-Session.md)  
[重新启动或关闭](Restart-or-Shut-Down.md)  
[管理工作站硬件](Manage-Station-Hardware.md)  
[使用 USB 设备](Work-with-USB-Devices.md)