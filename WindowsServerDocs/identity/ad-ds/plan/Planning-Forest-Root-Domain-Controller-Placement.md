---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: 规划林根域控制器放置
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57eafcc884a827d98c249e2da0c0af6888abc5b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823638"
---
# <a name="planning-forest-root-domain-controller-placement"></a>规划林根域控制器放置

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

林根级域控制器所需的客户端需要访问其自身之外的域中的资源创建信任路径。 将目录林根级域控制器放置在中心位置以及位于托管数据中心的位置。 如果给定位置中的用户需要从同一位置中的其他域访问资源，并且在数据中心和用户位置之间的网络可用性是不可靠，可以在的位置添加一个目录林根域控制器或创建两个域之间的快捷方式信任。 它是更具成本效益，若要创建快捷方式信任域之间，除非有其他原因将林根级域控制器放置在该位置。  
  
帮助以优化来自任何一种位于用户的身份验证请求的快捷方式信任。 有关域之间的快捷方式信任的详细信息，请参阅文章[了解何时创建快捷方式信任](https://go.microsoft.com/fwlink/?LinkId=107061)。  
  
若要帮助你记录在目录林根域控制器放置工作表，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip然后打开"域控制器放置"(DSSTOPO_4.doc)。  
  
你将需要创建目录林根级域时，请参考此信息。 有关部署目录林根域的详细信息，请参阅[部署 Windows Server 2008 目录林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
