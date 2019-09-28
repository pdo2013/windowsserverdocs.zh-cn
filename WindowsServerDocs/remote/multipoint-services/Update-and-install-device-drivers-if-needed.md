---
title: 更新并安装设备驱动程序（如果需要）
description: 了解如何检查和更新 MultiPoint Services 中的设备驱动程序
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 766e2175a16cd20a68730870c8980ed9c9204a3c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394883"
---
# <a name="update-and-install-device-drivers-if-needed"></a>更新并安装设备驱动程序（如果需要）
如果使用的是需要驱动程序的 USB 零客户端或外设，此时应安装驱动程序。 最好是**设备管理器**检查是否有任何驱动程序警报，并为这些设备安装驱动程序。  
  
通常，以下类型的设备需要最新的驱动程序：  
  
-   USB 零客户端  
  
-   USB over 以太网零客户端  
  
-   磁盘控制器  
  
-   网络适配器  
  
-   声音控制器  
  
-   USB 主机控制器

-   图形卡


## <a name="to-check-for-driver-alerts-in-device-manager"></a>在设备管理器中检查驱动程序警报  
  
1.  打开 "开始" 屏幕。  
  
2.  键入 "**计算机管理**"，然后单击结果中的 "**计算机管理**"。  
  
3.  在 "计算机管理" 控制台树中，单击 "**设备管理器**"。  
  
4.  在右侧的 "系统设备" 中，检查可能影响 MultiPoint Server 的驱动程序警报。  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>在 MultiPoint 管理器中安装设备驱动程序  
  
1.  若要打开 MultiPoint 管理器，请搜索 "MultiPoint 管理器"，然后单击结果中的 " **Multipoint 管理器**"。  
  
2.  在 MultiPoint 管理器中，单击 "**主页**" 选项卡，然后单击 "**切换到控制台模式**"。  
  
3.  若要安装设备驱动程序，请双击驱动程序文件，然后按照说明安装驱动程序。  
  
4.  重复上述步骤以安装所有必需的驱动程序。  
  
    > [!NOTE]  
    > 如果安装需要重新启动计算机，则在安装下一个驱动程序之前，需要切换回控制台模式。 MultiPoint server 始终在工作站模式下启动。 若要切换到控制台模式，请转到 MultiPoint 管理器中的 "**主页**" 选项卡，然后单击 "**切换到控制台模式**"。