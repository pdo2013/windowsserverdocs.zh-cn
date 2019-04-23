---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: 确定计划
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dee63ce0fb687b2b722ce64614c54388fc544433
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838998"
---
# <a name="determining-the-schedule"></a>确定计划

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以通过设置站点链接的计划来控制站点链接的可用性。 当两个站点之间复制遍历多个站点链接时，所有相关的链接上的复制计划的交集确定两个站点之间的连接计划。  
  
若要设置站点链接计划的计划，创建包含直接相互复制的域控制器的站点链接之间的两个重叠的计划。 这些链接上使用的默认 （100%可用) 计划，除非你想要阻止在高峰期间的复制流量。 阻止复制优先级为其他流量，但您还会增加复制延迟。  
  
域控制器存储时间以协调世界时 (UTC)。 在站点链接对象计划的时间设置符合站点和在其设置计划的计算机的本地时间。 当域控制器联系不同站点和时区中的计算机时，域控制器上的计划显示按照计算机的站点的本地时间的时间设置。  
  


