---
title: 设置物理计算机和主工作站
description: 了解如何在 MultiPoint 服务中设置第一个系统（主要工作站）
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
ms.openlocfilehash: 01b405b679afa3815652b91bec63c786bd661cf7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871554"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>设置物理计算机和主工作站
安装 MultiPoint 服务之前，需要为 MultiPoint 服务系统设置主站。 如果使用局域网（LAN）将计算机连接到 LAN。  
  
*工作站*是一个终结点，通过它可以访问多点服务。 当启动 MultiPoint 服务时，*主工作站*是要启动的第一个工作站。 管理员可以使用它来访问启动菜单和设置。 主工作站提供对系统配置和故障排除信息的访问，仅在启动期间和 MultiPoint 服务系统运行之前提供。 启动后，可以像使用任何其他工作站一样使用主站。  
  
主站必须是直接连接视频的工作站。 以下过程描述如何将所需的硬件连接到 MultiPoint 服务计算机。  
  
有关工作站的详细信息，请参阅[MultiPoint 电台](multipoint-services-stations.md)。 有关做出硬件选择的帮助，请参阅[选择 MultiPoint 服务系统的硬件](Selecting-Hardware-for-Your-MultiPoint-services-System.md)。 有关将其他工作站类型连接到 MultiPoint 服务的信息，请参阅[将其他工作站连接到 Multipoint 服务计算机](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。  
  
> [!NOTE]  
> 若要创建视频连接工作站，必须使用拉丁语键盘（如英语或西班牙语键盘）。  
  
## <a name="to-set-up-your-primary-station"></a>设置主工作站  
  
1.  确保关闭并拔下运行 MultiPoint 服务的计算机。  
  
2.  将监视器的电源线连接到电源插座，然后将显示器电缆连接到计算机上的视频显示端口，如下所示。  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif)  
  
3.  如果工作站将使用 USB 键盘和鼠标，请完成以下步骤：  
  
    1.  将外部 USB 集线器连接到计算机上的开放 USB 端口，如下所示。  
  
        ![MultiPoint Services USB 集线器连接的图像](./media/WMSUSBHubConnection.gif)  
  
    2.  将 USB 键盘和鼠标连接到 USB 集线器。  
  
        ![USB 集线器输入设备连接图像](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > 如果 MultiPoint 服务计算机有 PS/2 端口，则可以根据需要使用 PS/2 键盘，并将鼠标直接插入计算机。 但是，此设置具有明显的限制。 用户不能在 PS/2 工作站上使用音频设备、web 摄像机和闪存驱动器。  
  
    3.  如果使用的是外部供电集线器，请将集线器的电源线连接到电源插座。  
  
        > [!IMPORTANT]  
        > 强烈建议使用支持的集线器。 在当前条件下，系统行为可能不稳定。  
        >   
        > 用户不应将鼠标和键盘直接连接到计算机的 USB 端口。 这样做可能会导致不正确的将多个键盘和鼠标与同一工作站的关联，或者根本没有工作站。  
  
        > [!NOTE]  
        > 只有在 MultiPoint 服务处于控制台模式下时，系统主板上的主机音频设备才可用。 若要确保使用外部 USB 集线器的工作站不间断音频，必须使用插入到集线器中的 USB 音频设备。  
  
## <a name="to-connect-the-computer-to-the-lan"></a>将计算机连接到 LAN  
  
-   如果有 LAN，请使用网络电缆将计算机连接到网络。