---
title: 更新并安装设备驱动程序（如果需要）
description: 了解如何检查和更新 MultiPoint Services 中的设备驱动程序
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 66477634e06df217656876b084ae37be8cb311c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829128"
---
# <a name="update-and-install-device-drivers-if-needed"></a>更新并安装设备驱动程序（如果需要）
如果使用 USB 零客户端或外围设备所需驱动程序，应在这一次安装驱动程序。 它是一个好办法还检查**设备管理器**任何驱动程序，将发出警报和安装这些设备的驱动程序。  
  
通常情况下，最新驱动程序所需的以下类型的设备：  
  
-   USB 零客户端  
  
-   USB-over-以太网零客户端  
  
-   磁盘控制器  
  
-   网络适配器  
  
-   声音控制器  
  
-   USB 主控制器

-   图形卡


## <a name="to-check-for-driver-alerts-in-device-manager"></a>若要检查的驱动程序警报在设备管理器  
  
1.  打开开始屏幕。  
  
2.  类型**计算机管理**，然后单击**计算机管理**结果中。  
  
3.  在计算机管理控制台树中单击**设备管理器**。  
  
4.  在右侧的系统设备中，检查可能会影响 MultiPoint Server 的驱动程序警报。  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>在 MultiPoint 管理器中安装设备驱动程序  
  
1.  要打开 MultiPoint Manager，请搜索"MultiPoint 管理器"，然后单击**MultiPoint 管理器**结果中。  
  
2.  在 MultiPoint 管理器中，单击**主页**选项卡，然后依次**切换到控制台模式**。  
  
3.  若要安装设备驱动程序，请双击驱动程序文件，并按照说明进行操作来安装驱动程序。  
  
4.  重复上述步骤，安装所有必需的驱动程序。  
  
    > [!NOTE]  
    > 如果安装需要重启计算机，需要切换回控制台模式，然后再安装下一步的驱动程序。 多点服务器始终以工作站模式下启动。 若要切换到控制台模式下，请转到**主页**选项卡在 MultiPoint 管理器中，单击**切换到控制台模式**。