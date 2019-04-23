---
title: 设置拆分屏幕工作站
description: 介绍如何设置 MultiPoint 服务，以便两个用户可以共享单个系统
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2d01df6175eee99fd1374cd5af270cbcc764ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849938"
---
# <a name="set-up-a-split-screen-station"></a>设置拆分屏幕工作站
因此，两个用户可以同时使用系统，可以设置拆分屏幕工作站。

任何监视器的最小 x 720、 1200年的分辨率时连接到工作站支持的分割屏幕功能，可以拆分为两个工作站。 当工作站拆分后，监视器必须显示的桌面将移到屏幕的左半部分，并且新工作站显示在屏幕的右半部分。 若要完成创建新的工作站，您需要将键盘、 鼠标和 USB 集线器映射到工作站。 当工作站拆分完成后，一个用户可以登录到左侧的工作站，同时另一个用户可以登录到右侧的工作站。  
  
拆分屏幕工作站获得多种好处：  
  
-   您可以通过适应 MultiPoint 服务系统上的更多用户降低成本和空间。  
  
-   两个用户可以在一起，并排，进行项目协作。  
  
-   MultiPoint 仪表板用户可演示一个工作站上的过程，而一名学生老师其他工作站上。  
  
下图显示了具有 （右侧） 拆分屏幕工作站的 MultiPoint 服务系统。  
  
![拆分屏幕工作站](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>拆分屏幕工作站的要求  
若要创建拆分屏幕工作站，监视器和工作站必须满足以下要求：  
  
-   监视器必须具有 1200年的分辨率 x720 或更高版本。  
  
-   如果使用的 USB-over-以太网零客户端，请与硬件供应商，以了解是否支持拆分屏幕工作站。 许多 USB-over-以太网零客户端设备都有限制，阻止其配置为拆分屏幕工作站。  
  
## <a name="setting-up-a-split-screen-station"></a>设置拆分屏幕工作站  
使用以下过程来添加拆分屏幕工作站的第二个中心，然后拆分中 MultiPoint 服务工作站。 最后的过程说明如何返回到单个工作站的拆分屏幕工作站。  
  
> [!NOTE]  
> 拆分一个工作站时，将挂起该工作站上的活动会话。 用户必须登录到工作站，再次以拆分发生后继续工作。  
  
**若要添加另一个中心与键盘和鼠标：**  
  
1.  下图中所示，将 USB 集线器连接到的计算机上的开放 USB 端口。  
  
    ![MultiPoint server USB 集线器连接的映像](./media/WMSUSBHubConnection.gif)  
  
2.  连接到 USB 集线器的键盘和鼠标。  
  
    ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
3.  连接任何其他外围设备，例如耳机到 USB 集线器。  
  
4.  如果使用外部供电的集线器，连接到电源插座的电源线的中心。  
  
**若要拆分工作站：**  
  
1.  在 MultiPoint 管理器中，单击**工作站**选项卡。  
  
2.  下**工作站**，单击想要拆分的工作站的名称。  
  
3.  下**选定项任务**，单击**拆分工作站**。  
  
    原始屏幕移动到监视器中的左半部分，并在同一显示器的右半部分创建新工作站的屏幕。  
  
4.  通过新添加的键盘上按指定的字母指示当创建新的工作站**创建 MultiPoint Server 工作站**显示器的右半部分显示屏幕。  
  
当工作站拆分后，一个用户可以登录到左侧的工作站，同时另一个用户登录到右侧的工作站。  
  
**若要拆分工作站恢复成单个工作站：**  
  
1.  在 MultiPoint 管理器中，单击**工作站**选项卡。  
  
2.  下**工作站**，单击你想为拆分的工作站的名称。  
  
3.  下**选定项任务**，单击**取消拆分工作站**。