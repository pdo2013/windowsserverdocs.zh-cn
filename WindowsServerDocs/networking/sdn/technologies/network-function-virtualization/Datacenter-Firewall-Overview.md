---
title: 数据中心防火墙概述
description: 本主题介绍了 Windows Server 2016 中的数据中心防火墙，即网络层、5元组（协议、源和目标端口号、源和目标 IP 地址）、有状态的多租户防火墙。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9562972f731a553dbc3e5558fcce1d5c51d539d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405889"
---
# <a name="datacenter-firewall-overview"></a>数据中心防火墙概述

>适用于：Windows Server（半年频道）、Windows Server 2016

数据中心防火墙是 Windows Server 2016 附带的一种新服务。 它是网络层、5元组（协议、源和目标端口号、源和目标 IP 地址）、有状态的多租户防火墙。 当服务提供商部署并作为服务提供时，租户管理员可以安装和配置防火墙策略来帮助保护其虚拟网络免受来自 Internet 和 intranet 网络的不需要的通信。  
  
![网络堆栈中的数据中心防火墙](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
服务提供商管理员或租户管理员可以通过网络控制器和 northbound Api 来管理数据中心防火墙策略。  
  
对于云服务提供商，数据中心防火墙具有以下优势：  
  
-   可提供给租户的高度可扩展、可管理和 diagnosable 的基于软件的防火墙解决方案  
  
-   无需中断租户防火墙策略即可自由地将租户虚拟机移动到不同的计算主机  
  
    -   部署为 vSwitch 端口主机代理防火墙  
  
    -   租户虚拟机获取分配给其 vSwitch 主机代理防火墙的策略  
  
    -   防火墙规则配置在每个 vSwitch 端口中，与运行虚拟机的实际主机无关  
  
-   向独立于租户来宾操作系统的租户虚拟机提供保护  
  
数据中心防火墙为租户提供了以下优势：  
  
-   能够定义防火墙规则来帮助保护虚拟网络上面向 Internet 的工作负荷  
  
-   能够定义防火墙规则，以帮助保护同一二级虚拟子网上的虚拟机和不同 L2 虚拟子网上的虚拟机之间的流量  
  
-   能够定义防火墙规则来帮助保护和隔离租户本地网络及其虚拟网络与服务提供商之间的网络流量  
  


