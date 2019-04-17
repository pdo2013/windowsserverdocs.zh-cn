---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: "确定计划"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0ec953b34475c50e62553a9ba95e4d45d9904bf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-schedule"></a>确定计划

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以通过设置站点链接的日程安排控制站点链接可用性。 当在两个的站点之间复制遍历多个站点链接时，所有相关的链接上的复制日程安排交叉点确定两个的站点之间的连接时间。  
  
若要设置站点链接计划计划，创建包含直接复制彼此的域控制器站点链接之间的两个重叠计划。 除非你想要阻止在高峰时段复制流量，请使用默认值（100%可用) 计划在这些链接。 阻止复制，优先级赋予其他交通，但还会增加复制延迟。  
  
域控制器存储时间协调世界时 (UTC)。 在站点链接对象计划的时间设置符合站点和计算机依据该计划的设置的本地时间。 域控制器联系人的其他站点和时区的区域中的计算机，域控制器上的计划将显示时间设置根据计算机的站点的本地时间。  
  


