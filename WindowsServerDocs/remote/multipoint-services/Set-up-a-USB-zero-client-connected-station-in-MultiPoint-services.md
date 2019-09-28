---
title: 在 MultiPoint 服务中设置 USB 零客户端连接工作站
description: 了解如何在 MultiPoint Services 中创建 USB 零客户端工作站
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 80a73065024e5c40f1ebf8efd64022ee6d48fbe8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395070"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>在 MultiPoint 服务中设置 USB 零客户端连接工作站
当使用 USB 零客户端创建 MultiPoint 服务工作站时，每个工作站的监视器将连接到 USB 零客户端上的视频端口，如下图所示。 有关此类和其他工作站类型的详细信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。
  
**带有一个直接连接到视频的工作站和两个 USB 零客户端连接工作站的 MultiPoint 服务系统**  
  
![USB 零客户端连接工作站](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> 在设置 USB 零客户端连接工作站之前，请确保安装视频卡和 USB 零客户端的最新驱动程序。 过时的驱动程序会阻止 MultiPoint 服务配置成功完成。 有关说明，请参阅[更新和安装设备驱动程序（如果需要](Update-and-install-device-drivers-if-needed.md)）。  
  
> [!IMPORTANT]  
> 如果使用的是 USB 上联网的零客户端，请按照供应商提供的说明进行操作，而不是使用此过程来使用以太网连接在网络上设置设备。  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>设置 USB 零客户端连接工作站  
  
1.  将视频显示器电缆连接到 USB 零客户端上的 DVI 或 VGA 视频显示端口，如下图所示。  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif)  
  
2.  将 USB 零客户端连接到计算机上的开放 USB 端口。  
  
    ![MultiPoint Services USB 集线器连接的图像](./media/WMSUSBHubConnection.gif)  
  
3.  将键盘和鼠标连接到 USB 零客户端。  
  
    ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
4.  如果你使用的是外部通电的 USB 零客户端，请将 USB 零客户端的电源线连接到电源插座。  
  
5.  将视频显示器的电源线连接到电源插座。  
  
6.  如果系统提示你将设备与工作站关联，请按照监视器上的说明完成安装。 （通常情况下，连接到服务器时，USB 零客户端连接的工作站会自动与工作站关联。）