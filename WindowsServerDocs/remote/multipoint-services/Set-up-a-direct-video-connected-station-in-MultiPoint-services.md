---
title: 设置直接视频连接在 MultiPoint 服务工作站
description: 了解如何在 MultiPoint Services 中创建直接视频连接工作站
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
ms.openlocfilehash: 58197164c91ab6b69b0ef331c025287f593f94c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850738"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>设置直接视频连接在 MultiPoint 服务工作站
在直接连接视频的工作站，监视器直接连接到 MultiPoint Server 计算机上的视频端口。 键盘和鼠标然后连接到 USB 集线器，并与监视器关联。  
  
下图显示具有一台 MultiPoint Server 计算机和四个直接视频连接的工作站的 MultiPoint Server 环境。 有关详细信息，请参阅[MultiPoint Server 工作站](MultiPoint-services-Stations.md)。  
  
**MultiPoint 服务系统具有四个直接的视频连接**  
  
![基于 MultiPoint Services USB 系统布局的图像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> 若要配置 direct 视频连接的工作站，必须使用拉丁语键盘 （如英语或西班牙语语言键盘）。  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>若要设置直接视频连接工作站  
  
1.  监视器电缆连接到的计算机上的视频显示端口，如下所示。  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif) 
  
2.  将视频显示器的电源线连接到电源插座。  
  
3.  将 USB 集线器连接到的计算机上的开放 USB 端口，如下所示。  
  
    ![MultiPoint Services USB 集线器连接图像](./media/WMSUSBHubConnection.gif)  
  
4.  连接到 USB 工作站集线器的键盘和鼠标。  
  
    ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
5.  将任何其他外围设备，例如耳机，连接到 USB 集线器。  
  
6.  如果使用外部供电的集线器，连接到电源插座的电源线的中心。  
  
    > [!IMPORTANT]  
    > 我们强烈建议使用供电的集线器。 未得到充分的当前条件可能会导致不稳定的系统行为。  
    >   
    > 用户应附加到计算机的 USB 端口直接鼠标和键盘。 执行此操作很可能会导致不正确的多个键盘和鼠标到同一个工作站，或没有工作站关联根本。  
  
7.  按照监视器来创建工作站显示的说明。  
  
如果将多个直接连接视频的工作站添加到你的 MultiPoint 服务环境，可能会更改主工作站。 您可以轻松地找出其直接视频连接的工作站是主工作站。  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>若要了解其直接视频连接工作站是主工作站  
  
1.  启用直接连接到计算机的显示适配器 （视频卡） 的所有监视器。  
  
2.  启动 （或重新启动） 的 MultiPoint 服务计算机和任一监视器上显示的启动屏幕，请参阅。 该工作站是主工作站。  
  
    > [!NOTE]  
    > 在某些情况下，BIOS 启动信息是多个监视器上同时显示。 在这种情况下，任何监视器可用于访问 BIOS 视为"主站"。