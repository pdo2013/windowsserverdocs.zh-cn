---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: 创建站点链接桥设计
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 79e91481c357d05617ee0ddc716e2bf6e90b8b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408966"
---
# <a name="creating-a-site-link-bridge-design"></a>创建站点链接桥设计

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

站点链接桥连接两个或多个站点链接，并启用站点链接之间的传递。 网桥中的每个站点链接都必须具有一个与桥中的其他站点链接共同的站点。 知识一致性检查器（KCC）使用每个站点链接上的信息来计算一个站点链接中的站点与桥的其他站点链接中的站点之间的复制成本。 如果站点链接之间没有公共站点，KCC 也无法在同一站点链接桥连接的站点中的域控制器之间建立直接连接。  
  
默认情况下，所有站点链接都是可传递的。 建议你不要更改默认值 "**桥接所有站点链接**" （默认情况下启用）来启用传递。 但是，在以下情况下，你将需要禁用**桥接所有站点链接**并完成站点链接桥设计：  

- 你的 IP 网络未完全路由。 禁用**桥接所有站点链接**时，所有站点链接都被视为不可传递的，你可以创建和配置站点链接桥对象以建立网络的实际路由行为的模型。  
- 需要控制在 Active Directory 域服务（AD DS）中所做更改的复制流。 通过为站点链接 IP 传输禁用 "**桥接所有站点链接**" 并配置站点链接桥，站点链接桥将成为不相互连接的网络的等效项。 站点链接桥中的所有站点链接都可以进行可传递的路由，但不会在站点链接桥之外路由。  

有关如何使用 "Active Directory 站点和服务" 管理单元来禁用 "**桥接所有站点链接**" 设置的详细信息，请参阅文章[启用或禁用站点链接桥](https://go.microsoft.com/fwlink/?LinkId=107073)。  
  
## <a name="controlling-ad-ds-replication-flow"></a>控制 AD DS 复制流

在以下两种情况下，需要使用站点链接桥设计来控制复制流，包括通过防火墙控制复制故障转移和控制复制。  
  
### <a name="controlling-replication-failover"></a>控制复制故障转移

如果你的组织具有中心辐射型网络拓扑，则在中心站点中的所有域控制器都出现故障时，你通常不希望卫星站点创建到其他附属站点的复制连接。 在这种情况下，必须禁用 "**桥接所有站点链接**"，并创建站点链接桥，以便在卫星站点与另一个中心站点之间创建复制连接，该站点只是远离附属站点的一个或两个跃点。  
  
### <a name="controlling-replication-through-a-firewall"></a>通过防火墙控制复制

如果在两个不同的站点中代表同一个域的两个域控制器只允许通过防火墙进行通信，则可以禁用 "**桥接所有站点链接**"，并在firewall. 因此，如果你的网络是由防火墙分隔的，则建议你禁止传递站点链接，并为防火墙一侧的网络创建站点链接桥。 有关通过防火墙管理复制的信息，请参阅文章[Active Directory 防火墙分段的网络中](https://go.microsoft.com/fwlink/?LinkId=107074)。
