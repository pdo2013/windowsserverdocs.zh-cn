---
title: 挂起用户会话并使其保持为活动状态
description: 了解如何在无需断开连接的情况下从 MultiPoint 会话中挂起用户
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5263bce3-fe92-4398-8393-2e3a4e05d530
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: a7c94b9d1edd36efc8651e35dfabbc95239335cb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871531"
---
# <a name="suspend-and-leave-user-session-active"></a>挂起用户会话并使其保持为活动状态
当你不想结束用户的会话时，你可以断开或挂起用户的 MultiPoint 服务系统。 无需你为其断开会话，用户也可自主断开会话。 暂停用户会话时，会话将在 MultiPoint 服务系统的计算机内存中保持活动状态，直到关闭或重新启动计算机。 一旦关闭或重启计算机，所有挂起的会话都将结束，并且任何未保存的工作都将丢失。  
  
1.  在工作站模式下打开 "MultiPoint 管理器"，然后单击 "**工作站**" 选项卡。  
  
2.  在 "**计算机**" 列中，单击要挂起其会话的计算机的名称。  
  
3.  在“工作站任务”下，单击“挂起所有工作站”。  
  
挂起用户会话后，用户可以登录到相同或另一个工作站，并可在原始会话中继续工作。  
  
## <a name="see-also"></a>请参阅  
[管理用户桌面](manage-user-desktops-using-multipoint-dashboard.md)  
[注销或断开用户会话](Log-off-or-Disconnect-User-Sessions.md)