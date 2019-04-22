---
title: 挂起用户会话并使其保持为活动状态
description: 了解如何挂起用户从 MultiPoint 会话而无需断开用户连接
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
ms.openlocfilehash: cc4310e6f7609464cf037b750bec6e5e805e0b26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815218"
---
# <a name="suspend-and-leave-user-session-active"></a>挂起用户会话并使其保持为活动状态
可以断开连接或当不希望结束用户会话挂起 MultiPoint 服务系统中的用户。 无需你为其断开会话，用户也可自主断开会话。 当挂起用户会话时，在关闭或重启计算机前，该会话将会在 MultiPoint 服务系统的计算机内存中保持活动状态。 一旦关闭或重启计算机，所有挂起的会话都将结束，并且任何未保存的工作都将丢失。  
  
1.  在工作站模式下打开 MultiPoint Manager，然后单击**工作站**选项卡。  
  
2.  在中**计算机**列中，单击你想要挂起其会话的计算机的名称。  
  
3.  在“工作站任务”下，单击“挂起所有工作站”。  
  
挂起用户会话后，用户可以登录到相同或另一个工作站，并可在原始会话中继续工作。  
  
## <a name="see-also"></a>请参阅  
[管理用户桌面](manage-user-desktops-using-multipoint-dashboard.md)  
[注销或断开用户会话](Log-off-or-Disconnect-User-Sessions.md)