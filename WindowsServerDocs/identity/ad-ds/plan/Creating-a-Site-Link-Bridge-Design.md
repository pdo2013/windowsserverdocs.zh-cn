---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: 创建站点链接桥设计
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a194aa2fe2594c518d310cd86549945487d101e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813988"
---
# <a name="creating-a-site-link-bridge-design"></a>创建站点链接桥设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

站点链接桥连接两个或多个站点链接，并使站点链接之间的传递性。 每个站点链接桥中的必须具有与另一个站点链接桥中相同的站点。 知识一致性检查器 (KCC) 使用每个站点链接上的信息来计算中的其他站点链接桥的站点和站点链接中的站点之间的复制成本。 如果没有公共站点站点链接之间，KCC 还无法建立由相同的站点链接桥连接的站点中的域控制器之间的直接连接。  
  
默认情况下，所有站点链接都是传递的。 我们建议保持传递性情况下不会更改的默认值启用**桥接所有站点链接**（默认情况下启用）。 但是，你将需要禁用**桥接所有站点链接**，如果完成站点链接都桥设计：  

- IP 网络没有完全路由。 当禁用**桥接所有站点链接**，将所有站点链接都视为不可传递，并且可以创建和配置站点链接都桥对象进行建模您的网络的实际路由行为。  
- 需要控制复制的 Active Directory 域服务 (AD DS) 中所做的更改。 通过禁用**桥接所有站点链接**站点链接 IP 传输和配置站点链接都桥，站点链接都桥变得不相互连接的网络的等效项。 站点链接桥中的所有站点链接可以间接地，都路由，但不是会都路由之外的站点链接桥。  

详细了解如何使用 Active Directory 站点和服务管理单元中禁用**所有站点链接都搭桥**设置，请参阅文章[启用或禁用站点链接桥](https://go.microsoft.com/fwlink/?LinkId=107073)。  
  
## <a name="controlling-ad-ds-replication-flow"></a>控制 AD DS 复制流

两个方案中，您需要控制复制流的站点链接桥设计包括控制复制故障转移以及控制通过防火墙复制。  
  
### <a name="controlling-replication-failover"></a>控制复制故障转移

如果你的组织具有的中心辐射型网络拓扑，您通常不希望附属站点以创建与其他附属站点的复制连接，如果在中心站点中的所有域控制器都失败。 在这种情况下，您必须禁用**桥接所有站点链接**并创建站点链接桥，以便在附属站点和另一个是远离附属站点只是一个或两个跃点的中心站点之间创建复制连接。  
  
### <a name="controlling-replication-through-a-firewall"></a>控制通过防火墙复制

如果专门允许表示两个不同站点中的相同域的两个域控制器来相互通信只能通过防火墙，则可以禁用**桥接所有站点链接**和创建的站点链接桥防火墙的同一端的站点。 因此，如果你的网络被防火墙分开的我们建议你禁用站点链接的可传递性和防火墙的一侧创建的网络站点链接桥。 有关管理复制通过防火墙的信息，请参阅文章[防火墙分段的网络中的 Active Directory](https://go.microsoft.com/fwlink/?LinkId=107074)。
