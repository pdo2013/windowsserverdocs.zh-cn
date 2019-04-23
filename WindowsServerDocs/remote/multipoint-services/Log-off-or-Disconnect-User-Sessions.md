---
title: 注销或断开用户会话
description: 了解如何将用户手动注销
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e9bbcdc-e33b-481e-8b46-787a4f6d58bc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 518e9dc9ba9603d988a7e21e08caa29db9f04bde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854938"
---
# <a name="log-off-or-disconnect-user-sessions"></a>注销或断开用户会话
MultiPoint 服务用户才能登录和注销其桌面会话，像使用任何 Windows 会话。 用户还可以断开连接或挂起其会话，以便 MultiPoint 服务工作站未使用，但其会话保持活动状态在 MultiPoint 服务系统的计算机内存中。  
  
此外，如果用户已离开其 MultiPoint 服务会话或忘记注销出系统管理用户，就可以结束用户会话。  
  
## <a name="logging-off-or-disconnecting-a-session"></a>注销或断开会话  
下表介绍了你或任何用户可以用以注销、挂起或结束会话的不同选项。  
  
|||  
|-|-|  
|**操作**|**Effect**|  
|单击**启动**，单击设置中，单击用户名 （右上角），然后单击**注销**。|会话将结束且任何用户都可登录工作站。|  
|依次单击“开始”、“设置”、“电源”，然后单击“断开”。|将断开会话并将其保留在计算机内存中。 工作站可以由同一用户或其他用户登录。|  
|单击**启动**，单击设置中，单击用户名 （右上角），然后单击**锁**|将锁定工作站并会将会话保留在计算机内存中。|  
  
## <a name="suspending-or-ending-a-users-session"></a>挂起或结束用户的会话  
下表介绍了作为管理用户，你可以用以断开或结束用户会话的不同选项。  
  
|||  
|-|-|  
|**操作**|**Effect**|  
|**挂起：** 在 MultiPoint 管理器中，使用**工作站**选项卡以挂起用户会话。 有关详细信息，请参阅[挂起用户会话并使其保持为活动状态](Suspend-and-Leave-User-Session-Active.md)主题。|用户的会话将结束并会将其保留在计算机内存中。 工作站可以由同一用户或其他用户登录。 用户可以登录到同一工作站或另一工作站，然后继续工作。|  
|**结束时间：** 在 MultiPoint 管理器中，使用**工作站**选项卡以结束用户的会话。 您也可以上结束所有用户会话**工作站**选项卡。有关详细信息，请参阅[结束用户会话](End-a-User-Session.md)主题。|用户的会话将结束且任何用户都可登录工作站。 用户的会话不再显示在“工作站”选项卡上，且不将其保留在计算机内存中。|  
  
## <a name="see-also"></a>请参阅  
[挂起和保留用户会话活动](Suspend-and-Leave-User-Session-Active.md)  
[结束用户会话](End-a-User-Session.md)  
[管理用户桌面](manage-user-desktops-using-multipoint-dashboard.md)  
[注销用户会话](Log-Off-User-Sessions.md)    