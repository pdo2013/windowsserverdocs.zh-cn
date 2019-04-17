---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: "创建网站链接大桥设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 58dda7c1f56fa3799b902ab5458e71323047ec73
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-bridge-design"></a>创建网站链接大桥设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

网站的链接大桥连接两个或多个站点链接并可以让传递之间网站的链接。 大桥的每个站点链接必须某个站点与这座大桥的其他站点链接。 知识一致性检查 (KCC) 在各个网站的链接上使用的信息，来计算的一个网站链接中的站点和站点中的其他站点链接这座大桥之间复制成本。 如果没有常见的站点之间网站的链接，KCC 也无法建立直接域控制器在同一站点链接大桥通过连接的站点之间的连接。  
  
默认情况下，所有站点链接都可传递。 我们建议你继续通过未更改默认值的启用了传递**桥所有站点链接**（默认情况下启用）。 但是，你将需要禁用**桥所有站点链接**并完成站点链接都大桥设计，如果：  
  
-   不完全路由 IP 网络。 当你禁用**桥所有站点链接**算是非传递，所有站点链接，你可以创建和配置站点链接都大桥对象进行建模实际路由你的网络的行为。  
  
-   你需要控制复制流 Active Directory 域服务 (广告 DS) 中所做的更改。 通过禁用显示器的**桥所有站点链接**站点链接 IP 传输和配置网站链接都大桥，网站链接都大桥成为相当于断开连接的网络。 网站的链接大桥内的所有站点链接可以都路由间接，但它们不会在站点链接大桥之外将都发送。  
  
有关如何使用 Active Directory 的站点和服务管理单元中禁用**桥所有站点链接**设置，请参阅启用或禁用网站链接桥 ([https://go.microsoft.com/fwlink/?LinkId=107073](https://go.microsoft.com/fwlink/?LinkId=107073))。  
  
## <a name="controlling-ad-ds-replication-flow"></a>控制广告 DS 复制流  
两个应用场景中，您需要站点链接大桥设计控制复制流包括控制复制故障转移和控制通过防火墙进行复制。  
  
### <a name="controlling-replication-failover"></a>控制复制故障转移  
如果你的组织有中心分支网络拓扑，你通常不希望卫星站点创建复制连接到其他卫星站点如果中心站点中的所有域控制器都失败。 在这种情况下，你必须禁用**桥所有站点链接**并创建网站链接桥以便卫星站点和是只需一两次跳跃安装离开卫星网站的另一个中心网站之间创建复制连接。  
  
### <a name="controlling-replication-through-a-firewall"></a>控制复制通过防火墙  
如果明确允许表示相同域两种不同的站点中的两个域控制器彼此仅通过防火墙进行通信，您可以禁用**桥所有站点链接**并创建一侧相同的防火墙站点链接桥的站点。 因此，如果你的网络隔开防火墙，我们建议你禁用传递性的站点链接并创建一侧的防火墙站点链接桥该网络。 有关管理通过防火墙复制信息，请参阅分段由防火墙网络中的 Active Directory ([https://go.microsoft.com/fwlink/?LinkId=107074](https://go.microsoft.com/fwlink/?LinkId=107074))。  
  


