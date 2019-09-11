---
title: 设置拆分屏幕工作站
description: 描述如何设置 MultiPoint 服务，以便两个用户可以共享单个系统
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
ms.openlocfilehash: a0f1ea32112865c7120a3fe0af0c9f413032a32e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871591"
---
# <a name="set-up-a-split-screen-station"></a>设置拆分屏幕工作站
您可以设置一个拆分屏幕工作站，使两个用户可以同时使用系统。

当连接到支持拆分屏幕功能的工作站时，任何分辨率为最小 1200 x720 的监视器都可以拆分为两个工作站。 拆分工作站后，监视器显示的桌面将移到屏幕的左半部分，并且在屏幕的右半部分会显示一个新的工作站。 若要完成新工作站的创建，需要将键盘、鼠标和 USB 集线器映射到工作站。 当工作站拆分完成后，一个用户可以登录到左侧的工作站，同时另一个用户可以登录到右侧的工作站。  
  
拆分屏幕工作站有多个优点：  
  
-   可以通过在 MultiPoint 服务系统上容纳更多用户来降低成本和节省空间。  
  
-   两个用户可以在项目中并行协作。  
  
-   MultiPoint 仪表板用户可以在一台工作站上演示过程，而学生则可以在其他工作站上演示。  
  
下图显示了一个具有拆分屏幕工作站（在右侧）的 MultiPoint 服务系统。  
  
![拆分屏幕工作站](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>拆分屏幕工作站的要求  
若要创建拆分屏幕工作站，监视器和工作站必须满足以下要求：  
  
-   监视器的分辨率必须为 1200 x720 或更高。  
  
-   如果使用的是 USB over 以太网零客户端，请与硬件供应商联系，以确定是否支持拆分屏幕电台。 许多 USB over 以太网零客户端设备具有阻止其配置为拆分屏幕工作站的限制。  
  
## <a name="setting-up-a-split-screen-station"></a>设置拆分屏幕工作站  
使用以下过程为拆分屏幕工作站添加另一个集线器，然后在 MultiPoint Services 中拆分工作站。 最后一个过程说明如何将拆分屏幕站返回到单个工作站。  
  
> [!NOTE]  
> 拆分一个工作站时，将挂起该工作站上的活动会话。 用户必须再次登录到工作站，才能在进行拆分后恢复工作。  
  
**使用键盘和鼠标添加另一个集线器的步骤：**  
  
1.  将 USB 集线器连接到计算机上的开放 USB 端口，如下图所示。  
  
    ![MultiPoint server USB 集线器连接的图像](./media/WMSUSBHubConnection.gif)  
  
2.  将键盘和鼠标连接到 USB 集线器。  
  
    ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
3.  将任何其他外围设备（如耳机）连接到 USB 集线器。  
  
4.  如果使用的是外部供电集线器，请将集线器的电源线连接到电源插座。  
  
**若要拆分工作站：**  
  
1.  在 MultiPoint 管理器中，单击 "**工作站**" 选项卡。  
  
2.  在 "**工作站**" 下，单击要拆分的工作站的名称。  
  
3.  在 "**选定项任务**" 下，单击 "**拆分工作站**"。  
  
    原始屏幕会移动到监视器的左半部分，并在同一监视器的右半部分创建新的工作站屏幕。  
  
4.  在监视器的右半部分**中，按**所示，在新添加的键盘上按指定的字母，以创建新的工作站。  
  
在拆分工作站后，一个用户可以登录到左侧工作站，另一个用户可以登录到正确的工作站。  
  
**若要将拆分工作站返回到单一工作站：**  
  
1.  在 MultiPoint 管理器中，单击 "**工作站**" 选项卡。  
  
2.  在 "**工作站**" 下，单击要撤消的工作站的名称。  
  
3.  在 "**选定项任务**" 下，单击 "**撤消站**"。