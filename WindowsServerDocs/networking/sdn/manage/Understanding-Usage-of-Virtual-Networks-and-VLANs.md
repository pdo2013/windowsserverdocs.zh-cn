---
title: 了解 Vlan 虚拟网络以及使用情况
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fcf84c5c1f0be2fa1c7524592f8d02e4b11d3a2b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="understanding-usage-of-virtual-networks-and-vlans"></a>了解 Vlan 虚拟网络以及使用情况

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解 HYPER-V 网络虚拟化虚拟网络以及如何从虚拟本地网络 (Vlan) 的而有所不同。  
  
软件定义网络 (SDN) 在 Windows Server 2016 基于编程覆盖层内 HYPER-V 虚拟交换机用来的虚拟网络策略。 你可以创建覆盖虚拟网络，也称为虚拟网络 HYPER-V 网络虚拟化。   
  
部署 HYPER-V 网络虚拟化时，通过封装层 3 IP 和第二层以太网标题包含 （或多个物理） 从网络覆盖-或隧道-标题 （例如，VXLAN 或 NVGRE） 与原始租户虚拟机的第二层以太网框架创建覆盖层的网络。 覆盖虚拟网络式通过 24 特虚拟网络标识符 (VNI) 租户交通隔离，并允许重叠 IP 地址。 VNI 组成的虚拟子网 ID (VSID)、 逻辑切换 ID 和隧道 id。  
  
此外，每个租户分配给路由域 （类似于虚拟路由和转移-VRF），以便 （每个表示 VNI） 的多个虚拟子网前缀可以直接路由到彼此。 跨租户 （或跨路由域） 而无需经过网关路由不支持。   
  
由名为提供程序逻辑网络逻辑网络表示物理网络的每个租户封装的交通隧道传输。 包含一个或多个子网逻辑该提供商的网络，每个表示 IP 前缀和 （可选） VLAN 802.1 q 标记。  
  
你可以创建其他逻辑网络并子网供基础结构需随身携带管理通信，存储流量实时迁移交通拥挤等。  
  
Microsoft SDN 不支持使用 Vlan 的隔离租户网络。 租户隔离单独通过使用 HYPER-V 网络虚拟化覆盖虚拟网络和封装。 


