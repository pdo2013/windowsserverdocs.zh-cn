---
title: 在 MultiPoint 服务中设置直接连接视频的工作站
description: 了解如何在 MultiPoint Services 中创建直接连接视频的工作站
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: eda8d5eee0635370873adec5b1fde2d65fc9fd9c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871602"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>在 MultiPoint 服务中设置直接连接视频的工作站
在直接连接到视频的工作站上，监视器直接连接到 MultiPoint 服务器计算机上的视频端口。 然后，将键盘和鼠标连接到 USB 集线器，并将其与监视器相关联。  
  
下图显示了具有单个 MultiPoint 服务器计算机和四个直接连接到视频的工作站的 MultiPoint Server 环境。 有关详细信息，请参阅[MultiPoint Server 电台](MultiPoint-services-Stations.md)。  
  
**具有四个直接视频连接的 MultiPoint 服务系统**  
  
![基于 USB 的 MultiPoint Services 系统布局的图像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> 若要配置直接连接到视频的工作站，必须使用拉丁语键盘（如英语或西班牙语键盘）。  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>设置直接连接视频的工作站  
  
1.  将监视器电缆连接到计算机上的视频显示端口，如下所示。  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif) 
  
2.  将视频显示器的电源线连接到电源插座。  
  
3.  将 USB 集线器连接到计算机上的开放 USB 端口，如下所示。  
  
    ![MultiPoint Services USB 集线器连接的图像](./media/WMSUSBHubConnection.gif)  
  
4.  将键盘和鼠标连接到 USB 工作站集线器。  
  
    ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
5.  将任何其他外围设备（如耳机）连接到 USB 集线器。  
  
6.  如果使用的是外部供电集线器，请将集线器的电源线连接到电源插座。  
  
    > [!IMPORTANT]  
    > 强烈建议使用支持的集线器。 在当前条件下，系统行为可能不稳定。  
    >   
    > 用户不应将鼠标和键盘直接连接到计算机的 USB 端口。 这样做可能会导致不正确的将多个键盘和鼠标与同一工作站的关联，或者根本没有工作站。  
  
7.  按照监视器上显示的说明创建工作站。  
  
如果将多个直接连接视频连接工作站添加到 MultiPoint 服务环境，则可能会更改主站。 你可以轻松地查明哪个直接连接视频连接到了你的主站。  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>确定哪个直接连接视频工作站是主工作站  
  
1.  打开直接连接到计算机显示适配器（视频卡）的所有监视器。  
  
2.  启动（或重新启动） MultiPoint 服务计算机，并查看哪个监视器显示启动屏幕。 工作站是主工作站。  
  
    > [!NOTE]  
    > 在某些情况下，BIOS 启动信息同时显示在多个监视器上。 在这种情况下，可以将任何监视器视为用于访问 BIOS 的 "主站"。