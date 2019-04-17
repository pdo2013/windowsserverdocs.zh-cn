---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: "计划森林根域控制器的位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 374ce31c61c666302e2b00a8f365c289cb8799f8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="planning-forest-root-domain-controller-placement"></a>计划森林根域控制器的位置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

森林根域控制器需需要访问资源之外自己的域的客户端创建信任路径。 将森林根域控制器放置在中心位置和位置承载数据中心中。 给定的位置中的用户需要其他域位于同一位置的访问资源，并且不可靠之间 datacenter 和用户位置的网络可用性，如果你可以在的位置添加森林根域控制器，或创建快捷方式信任之间的两个域。 它是高效创建快捷方式信任域之间，除非你具有其他原因，以在该位置将森林根域控制器的更多费用。  
  
快捷方式信任帮助优化从位于任一域中的用户的身份验证请求。 有关域之间的快捷方式信任的详细信息，请参阅了解何时创建快捷方式信任 ([https://go.microsoft.com/fwlink/?LinkId=107061](https://go.microsoft.com/fwlink/?LinkId=107061))。  
  
为了帮助您中记录你森林根域控制器的位置工作表中，请参阅 Windows Server 2003 部署工具包工作辅助 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and open "Domain 控制器的位置"(DSSTOPO_4.doc)。  
  
你将需要创建森林根域时，请参考此信息。 有关部署的森林根域的详细信息，请参阅[部署 Windows Server 2008 森林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  


