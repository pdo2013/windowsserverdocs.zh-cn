---
title: Datacenter 防火墙概述
description: 可以使用此主题以了解 Datacenter 防火墙，这是一种网络层、5 元（源代码和目的地的 IP 地址协议、源代码和目的地端口号）、Windows Server 2016 中的状态，租户防火墙。
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
ms.openlocfilehash: 0c9b9fb5b0fb9aa09ed783b2b66a8ad370a627c3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="datacenter-firewall-overview"></a>Datacenter 防火墙概述

>适用于：Windows Server（半年通道），Windows Server 2016

Datacenter 防火墙是一种新随附于 Windows Server 2016 服务。 它是网络层，5 元协议、 源代码和目的地端口号 （源代码和目的地的 IP 地址），状态、 租户防火墙。 作为一项服务服务提供商提供和部署时租户管理员可以安装和配置防火墙策略，以帮助保护其虚拟网络从有害来自 Internet 的通信和 intranet 网络。  
  
![数据中心网络堆栈中的防火墙](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
服务提供商管理员或租户管理员可以管理通过网络控制器和 northbound Api Datacenter 防火墙策略。  
  
Datacenter 防火墙云服务提供商提供以下优势：  
  
-   可以对租户提供强烈可缩放、更易管理，并 diagnosable 的软件防火墙解决方案  
  
-   自由地将租户虚拟机移动到不同的计算主机，而不会破坏租户防火墙策略  
  
    -   作为 vSwitch 端口主机代理防火墙部署  
  
    -   租户虚拟机获取分配，到其 vSwitch 主机代理防火墙的策略  
  
    -   在每个 vSwitch 端口独立运行虚拟机的实际主机配置防火墙规则  
  
-   提供的保护，以其租户虚拟机独立于租户来宾操作系统  
  
Datacenter 防火墙您提供的租户以下优势：  
  
-   定义来帮助保护面向虚拟网络上的工作负载 Internet 的防火墙规则功能  
  
-   定义防火墙规则，以帮助保护交通之间位于同一 L2 虚拟子网以及在不同 L2 虚拟子网虚拟机之间虚拟机的功能  
  
-   能够定义防火墙规则，以帮助保护并隔离之间租户网络通信本地网络和其虚拟网络服务提供商处  
  


