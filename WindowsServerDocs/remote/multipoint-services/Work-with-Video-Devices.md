---
title: 使用视频设备
description: 了解如何监视视频和投影仪使用 MultiPoint Services 中的工作站
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d828ea911aaff27a1df79d0380dfe92987c3d2aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844198"
---
# <a name="work-with-video-devices"></a>使用视频设备
了解视频设备（例如显示器或投影仪）在连接到 MultiPoint 服务系统中的计算机或连接到 MultiPoint 服务工作站时是如何工作的。  
  
## <a name="working-with-video-monitors"></a>使用视频显示器  
根据 MultiPoint 服务系统硬件的不同，有两种方式可以连接视频显示器：  
  
-   有关*USB 集线器基于系统*，连接视频显示器电缆连接到的计算机上，开放的视频端口，如下图中所示：  
  
    ![连接到基于 USB 集线器系统的视频图像](./media/WMSVideoConnection.gif)  
  
-   有关*多功能集线器基于系统*内置视频支持，请将视频显示器电缆连接到多功能集线器上的视频端口：  
  
    ![多功能集线器和视频连接图像](./media/WMSMultifunctionHubVideoConnection.gif)  
  
有关详细信息，请参阅[设置工作站](Set-Up-a-Station.md)主题。  
  
## <a name="working-with-video-projectors"></a>使用视频投影仪  
希望投影大型图像供其他用户查看时（例如在实验设置中），可以将视频投影仪连接到 MultiPoint 服务系统。 这两个 USB 集线器和多功能集线器基于工作站，可以将投影仪连接到工作站的两个选项：  
  
-   使用投影仪替换显示器，并将其用作该工作站的显示设备，如下图所示：  
  
    ![连接到计算机的投影仪图像](./media/WMSVideoProjectorConnection.gif)  
  
-   使用视频拆分器设备，可同时将投影仪和显示器连接到工作站的视频端口。  
  
    MultiPoint 服务将在两个显示设备上显示相同的图像。 不投影时，可将投影仪关闭，仅使用视频显示器。  
  
无论使用哪个选项，都请注意以下事项：  
  
-   连接视频显示器可能需要再次关联工作站以便 MultiPoint 服务能够正确识别新的显示器。 请按照工作站的视频显示设备上显示的说明进行操作。  
  
-   可能需要使用适配器或转换器设备以在 DVI 和 VGA 接口之间转换。  
  
-   使用“Y”型拆分器电缆可能会降低两个视频设备的视频质量。  
  
-   通过“Y”型拆分器电缆同时使用投影仪和显示器时，MultiPoint 服务会将两个设备的屏幕分辨率都调整到其最高支持的最低分辨率 - 通常是投影仪的分辨率。  
  
-   MultiPoint 服务不支持在多台显示器中扩展单个工作站的显示器。  
  
## <a name="see-also"></a>请参阅  
[管理工作站硬件](Manage-Station-Hardware.md)  
[设置工作站](Set-Up-a-Station.md) 