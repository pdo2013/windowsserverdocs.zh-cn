---
title: USB 零客户端连接工作站中设置 MultiPoint 服务
description: 了解如何在 MultiPoint Services 中创建 USB 零客户端工作站
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1a64373f4ed5e0d1ac96a0257ac5697ff94ffcbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878928"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>USB 零客户端连接工作站中设置 MultiPoint 服务
当使用 USB 零客户端来创建 MultiPoint 服务工作站时，针对每个工作站的监视器将连接到 USB 零客户端上的视频端口，如下图中所示。 有关此设置和其他工作站类型的详细信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。
  
**使用其中的一个直接视频连接的工作站和两个 USB 零客户端连接工作站的 multiPoint 服务系统**  
  
![USB 零客户端连接工作站](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> 设置 USB 零客户端连接工作站之前，请务必安装视频卡和 USB 零客户端的最新驱动程序。 过时的驱动程序可能会阻止成功完成的 MultiPoint 服务配置。 有关说明，请参阅[更新并根据需要安装设备驱动程序](Update-and-install-device-drivers-if-needed.md)。  
  
> [!IMPORTANT]  
> 如果使用 USB-over-以太网零客户端，按照供应商，而不是此过程中，若要使用的以太网连接来设置设备在网络上的说明。  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>若要设置 USB 零客户端连接工作站  
  
1.  连接视频显示器电缆连接到 USB 上的 DVI 或 VGA 视频显示端口零客户端，如以下插图所示。  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif)  
  
2.  将 USB 零客户端连接到计算机上的开放 USB 端口。  
  
    ![MultiPoint Services USB 集线器连接图像](./media/WMSUSBHubConnection.gif)  
  
3.  键盘和鼠标连接到 USB 零客户端。  
  
    ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
4.  如果使用外部供电的 USB 零客户端，连接到电源插座的电源线的 USB 零客户端。  
  
5.  将视频显示器的电源线连接到电源插座。  
  
6.  如果系统提示将设备与工作站相关联，以完成设置的监视器上按照的说明。 （通常情况下，USB 零客户端连接工作站是与工作站相关联自动添加到服务器时。）