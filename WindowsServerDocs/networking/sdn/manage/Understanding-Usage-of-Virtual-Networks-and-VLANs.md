---
title: 了解虚拟网络和 Vlan 的使用情况
description: 在本主题中，您了解有关 HYPER-V 网络虚拟化的虚拟网络以及从虚拟局域网 (Vlan) 之间的区别。 使用 HYPER-V 网络虚拟化，您可以创建覆盖层的虚拟网络，也称为虚拟网络。
manager: dougkim
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
ms.date: 08/26/2018
ms.openlocfilehash: d126e97a91e4c61ecff00cc2b5a527618b2d4d0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875528"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>了解虚拟网络和 Vlan 的使用情况

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，您了解有关 HYPER-V 网络虚拟化的虚拟网络以及从虚拟局域网 (Vlan) 之间的区别。 使用 HYPER-V 网络虚拟化，您可以创建覆盖层的虚拟网络，也称为虚拟网络。



  
软件定义网络 (SDN) Windows Server 2016 中基于编程策略覆盖内的虚拟网络的 HYPER-V 虚拟交换机。 可以创建覆盖层的虚拟网络，也称为虚拟网络，使用 HYPER-V 网络虚拟化。 
  
当您部署的 HYPER-V 网络虚拟化时，通过封装与一个覆盖-或隧道-标头，（例如，VXLAN 或 NVGRE） 和第 3 层 ip 地址和第 2 层以太网的原始租户虚拟机的第 2 层以太网帧创建覆盖网络标头下衬 （或物理） 的网络。 覆盖虚拟网络标识由 24 位虚拟网络标识符 (VNI) 来维护租户通信隔离并允许重叠的 IP 地址。 VNI 组成虚拟子网 ID (VSID)、 逻辑交换机 ID 和隧道 id。  
  
此外，每个租户分配给路由域 （类似于虚拟路由和转发的 VRF），以便多个虚拟子网前缀 （每个表示 VNI） 可以直接转到每个其他。 跨租户 （或跨路由域） 而无需通过网关不支持路由。   
  
由名为提供程序逻辑网络的逻辑网络表示物理网络的每个租户的封装的流量进行隧道传输。 此提供程序逻辑网络由一个或多个的子网组成，每个表示 IP 前缀和 （可选） VLAN 802.1q 标记。  
  
可以创建其他逻辑网络和子网用于基础结构以执行管理流量，存储流量实时迁移流量，等等。  
  
Microsoft SDN 不支持使用 Vlan 的隔离租户网络。 租户隔离仅通过使用 HYPER-V 网络虚拟化覆盖区上的虚拟网络并封装。 


