---
title: 了解虚拟网络和 Vlan 的使用情况
description: 在本主题中，你将了解 Hyper-v 网络虚拟化虚拟网络及其与虚拟局域网（Vlan）的不同之处。 通过 Hyper-v 网络虚拟化，你可以创建覆盖虚拟网络（也称为虚拟网络）。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: 854adf0e7bb2a8715e3d447c04e2f09c3470a781
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355834"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>了解虚拟网络和 Vlan 的使用情况

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，你将了解 Hyper-v 网络虚拟化虚拟网络及其与虚拟局域网（Vlan）的不同之处。 通过 Hyper-v 网络虚拟化，你可以创建覆盖虚拟网络（也称为虚拟网络）。



  
Windows Server 2016 中的软件定义网络（SDN）基于在 Hyper-v 虚拟交换机内覆盖虚拟网络的编程策略。 可以通过 Hyper-v 网络虚拟化创建覆盖虚拟网络（也称为虚拟网络）。 
  
部署 Hyper-v 网络虚拟化时，会通过将原始租户虚拟机的第2层以太网帧封装为覆盖或隧道标头（例如，VXLAN 或 NVGRE）和第3层 IP 和第2层以太网来创建覆盖网络是（或物理）网络中的标头。 覆盖虚拟网络由24位虚拟网络标识符（VNI）标识，用于维护租户通信隔离和允许重叠的 IP 地址。 VNI 由虚拟子网 ID （VSID）、逻辑交换机 ID 和隧道 ID 组成。  
  
此外，每个租户都分配有一个路由域（类似于虚拟路由和转发-VRF），以便可以直接相互路由多个虚拟子网前缀（每个前缀由 VNI 表示）。 不支持跨租户（或交叉路由域）路由，而不通过网关。   
  
每个租户的封装流量在其上进行隧道传输的物理网络由名为提供程序逻辑网络的逻辑网络表示。 此提供程序逻辑网络包含一个或多个子网，每个子网由一个 IP 前缀和一个 VLAN 802.1 q 标记（可选）表示。  
  
出于基础结构目的，你可以创建其他逻辑网络和子网来携带管理流量、存储流量、实时迁移流量等。  
  
Microsoft SDN 不支持通过使用 Vlan 隔离租户网络。 租户隔离仅通过使用 Hyper-v 网络虚拟化覆盖虚拟网络和封装来完成。 


