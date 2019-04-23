---
title: 数据中心防火墙概述
description: 可以使用本主题以了解数据中心防火墙，这是网络层、 5 元组 （协议、 源和目标端口号、 源和目标 IP 地址）、 Windows Server 2016 中的对象的有状态的多租户防火墙。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f1de50dc61639f4985c9d28fdde6072af650f42e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890828"
---
# <a name="datacenter-firewall-overview"></a>数据中心防火墙概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

数据中心防火墙是包含 Windows Server 2016 的新服务。 它是网络层、 5 元组 （协议、 源和目标端口号、 源和目标 IP 地址）、 有状态的多租户防火墙。 当部署并作为服务提供商服务提供，租户管理员可以安装和配置防火墙策略帮助保护其不需要来自 Internet 的流量从虚拟网络和 intranet 网络。  
  
![网络堆栈中的数据中心防火墙](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
服务提供商管理员或租户管理员可以管理通过网络控制器和 northbound Api 的数据中心防火墙策略。  
  
数据中心防火墙为云服务提供商提供以下优势：  
  
-   可以向租户提供高度可缩放、 易管理性，且套基于软件的防火墙解决方案  
  
-   可以自由地将租户虚拟机移动到不同的计算主机，而不会中断租户防火墙策略  
  
    -   部署为 vSwitch 端口主机代理的防火墙  
  
    -   租户的虚拟机获取分配给他们 vSwitch 主机代理的防火墙策略  
  
    -   在每个 vSwitch 端口，独立于运行虚拟机的实际主机中配置防火墙规则  
  
-   提供租户独立于租户的来宾操作系统的虚拟机的保护  
  
数据中心防火墙为租户提供以下优势：  
  
-   定义防火墙规则，以帮助保护虚拟网络上的工作负荷的面向 Internet 的功能  
  
-   能够定义防火墙规则，以帮助保护之间的虚拟机上的相同 L2 虚拟子网以及计算机上不同 L2 虚拟子网的虚拟机之间的流量  
  
-   能够定义防火墙规则，以帮助保护和隔离租户之间的网络流量的本地网络和服务提供商在其虚拟网络  
  


