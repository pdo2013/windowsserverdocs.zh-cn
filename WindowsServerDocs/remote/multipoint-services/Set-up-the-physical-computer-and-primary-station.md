---
title: 设置物理计算机和主工作站
description: 了解如何将第一个系统，主站，MultiPoint Services 中设置
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6569c4963d31b72216943bf29b71411e702caf84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812008"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>设置物理计算机和主工作站
安装 MultiPoint 服务之前，需要设置你的 MultiPoint 服务系统主工作站。 如果将使用局域网 (LAN) 将计算机连接到 LAN。  
  
一个*工作站*是通过其访问 MultiPoint 服务的终结点。 *主站*是启动时启动 MultiPoint 服务的第一个工作站。 管理员可以使用它来访问启动菜单和设置。 主站连接可以访问系统配置和故障排除期间启动和 MultiPoint 服务之前才可用的信息系统正在运行。 后启动时，可以使用主站，就像任何其他工作站。  
  
主工作站必须直接视频连接的工作站。 以下过程介绍如何连接到 MultiPoint 服务计算机的所需的硬件。  
  
有关工作站的详细信息，请参阅[MultiPoint 工作站](multipoint-services-stations.md)。 进行硬件选择的帮助，请参阅[选择你的 MultiPoint 服务系统的硬件](Selecting-Hardware-for-Your-MultiPoint-services-System.md)。 有关连接的信息其他站配合 MultiPoint 服务的类型，请参阅[连接到 MultiPoint 服务计算机的其他工作站](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。  
  
> [!NOTE]  
> 若要创建视频连接的工作站，必须使用拉丁语键盘 （如英语或西班牙语语言键盘）。  
  
## <a name="to-set-up-your-primary-station"></a>若要设置主工作站  
  
1.  请确保运行 MultiPoint 服务的计算机是处于关闭状态，拔出。  
  
2.  将监视器的电源线连接到电源插座，并连接到的计算机上的视频显示端口的显示器电缆连接，如下所示。  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif)  
  
3.  如果 USB 键盘和鼠标，将使用工作站，请完成以下步骤：  
  
    1.  将外部 USB 集线器连接到的计算机上的开放 USB 端口，如下所示。  
  
        ![MultiPoint Services USB 集线器连接图像](./media/WMSUSBHubConnection.gif)  
  
    2.  连接到 USB 集线器的 USB 键盘和鼠标。  
  
        ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > 如果 MultiPoint 服务计算机上有 ps/2 端口，您可以如果需要请使用 ps/2 键盘和鼠标直接插入计算机。 但是，此安装程序具有明显的限制。 用户不能使用音频设备，web 相机和闪存驱动器上 PS/2 的工作站。  
  
    3.  如果使用外部供电的集线器，连接到电源插座的电源线的中心。  
  
        > [!IMPORTANT]  
        > 我们强烈建议使用供电的集线器。 未得到充分的当前条件可能会导致不稳定的系统行为。  
        >   
        > 用户应附加到计算机的 USB 端口直接鼠标和键盘。 执行此操作很可能会导致不正确的多个键盘和鼠标到同一个工作站，或没有工作站关联根本。  
  
        > [!NOTE]  
        > MultiPoint 服务处于控制台模式时，系统的主板上的主机音频设备才可用。 若要确保不间断地使用外部 USB 集线器的工作站的音频，必须使用 USB 音频设备插入到集线器。  
  
## <a name="to-connect-the-computer-to-the-lan"></a>若要将计算机连接到 LAN  
  
-   如果您有一个 LAN，将计算机连接到您的网络使用网络电缆。