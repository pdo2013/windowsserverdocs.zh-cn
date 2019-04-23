---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: 设置站点链接属性
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4fa9a1fa8d2a463fe5f361a5a27ee2b9e3edc0f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870358"
---
# <a name="setting-site-link-properties"></a>设置站点链接属性

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

站点间复制将根据连接对象的属性。 当知识一致性检查器 (KCC) 创建连接对象时，它派生自的复制计划的站点链接对象的属性。 每个站点链接对象表示两个或多个站点之间广域网 (WAN) 连接。  
  
设置站点链接对象属性包括以下步骤：  
  
-   确定与该复制路径相关联的成本。 KCC 使用成本来确定复制相同的目录分区的两个站点之间复制的开销最少的路由。  
  
-   确定定义站点间复制时间计划可能发生。  
  
-   确定定义复制应发生的频率在何时可复制时间计划中定义的复制间隔。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [确定成本](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [确定计划](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [确定间隔](../../ad-ds/plan/Determining-the-Interval.md)  
  


