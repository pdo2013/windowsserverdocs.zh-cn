---
title: 管理用户工作站
description: 了解如何在 MultiPoint Services 中管理用户工作站
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b418578d-3a4c-49b0-90db-8389b320b2f6
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 6de378284f5cd41f5c5c3228c8305176367b5dd5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871617"
---
# <a name="manage-user-stations"></a>管理用户工作站
本节讨论如何管理构成 MultiPoint 服务系统的工作站。 管理 MultiPoint 服务系统包括管理 MultiPoint 管理器的硬件和软件组件。 在 MultiPoint 服务系统中，桌面是为每个用户工作站提供的监视器上的软件用户界面。  
  
## <a name="station-status"></a>工作站状态  
可在“工作站”选项卡上查看每个桌面的以下状态类型。状态包括：  
  
-   登录的用户  
  
-   已挂起但在计算机上仍处于活动状态的用户会话  
  
-   正在使用的工作站以及使用它们的用户  
  
有关查看桌面状态的详细信息，请参阅[查看用户连接状态](View-User-Connection-Status.md)主题。  

>[!TIP] 
> 可以将友好名称分配给每个工作站，以便你更轻松地识别工作站。 使用“标识工作站”在分配的屏幕上显示工作站名称。
  
## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>从 MultiPoint 服务系统中注销标准用户的不同方法  
作为管理用户，你可随时从 Windows 注销，这不会影响 MultiPoint 服务系统中的活动用户。 标准用户也可断开其会话或从 MultiPoint 服务系统注销。 在磁盘保护启用的情况下，如果用户将离开一天，则他们应确保将他们的工作保存到计算机或外部存储设备上，以便 MultiPoint 服务系统关闭后，用户能够改日取回其保存的工作。  
  
作为管理用户，你可能需要结束标准用户的*会话*，而不是让用户注销。 可以通过以下两种方式之一结束标准用户的会话：  
  
-   结束会话并注销用户。 有关结束用户会话的详细信息，请参阅[结束用户会话](End-a-User-Session.md)主题。  
  
-   挂起用户以暂时结束用户会话，但使会话在 MultiPoint 服务系统的计算机内存中保持活动状态。 之后，挂起的用户可从相同或不同的工作站重新连接到会话并继续其工作。 有关挂起用户会话的详细信息，请参阅[挂起用户会话并使其保持活动状态](Suspend-and-Leave-User-Session-Active.md)主题。  
  
## <a name="set-a-station-to-automatically-log-on"></a>设置工作站以自动登录  
作为管理用户，你可以设置一个或多个工作站在运行 MultiPoint 服务的计算机启动时自动登录。 有关自动登录的详细信息，请参阅[设置工作站进行自动登录](Set-up-a-Station-for-Automatic-Logon.md)主题。  
  
## <a name="split-a-station"></a>拆分工作站  
任何分辨率高于 1024x768 的工作站显示器均可拆分为两个工作站。 有关拆分工作站的详细信息，请参阅[拆分用户工作站](Split-a-User-Station.md)主题。  
  
## <a name="see-also"></a>请参阅  
[查看用户连接状态](View-User-Connection-Status.md)  
[注销或断开用户会话](Log-off-or-Disconnect-User-Sessions.md)  
[暂停用户会话并使其保持活动状态](Suspend-and-Leave-User-Session-Active.md)  
[设置工作站自动登录](Set-up-a-Station-for-Automatic-Logon.md)  
[结束用户会话](End-a-User-Session.md)  
[拆分用户工作站](Split-a-User-Station.md)