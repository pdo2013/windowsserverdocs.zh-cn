---
title: 什么是适用于 Windows Server 的 SDN 中的新增功能
description: 本主题提供有关新的软件定义网络功能的 Windows Server 1709 的信息
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: aef2bc32f249550d4e8d33d4b871ca98010e2f49
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446290"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Windows Server 2019 中 SDN 的新增功能

>适用于：Windows Server（半年频道）


|                         **功能**                          |                                                                                                                                                                                         **说明**                                                                                                                                                                                         | **新的/更新** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [加密的网络](vnet-encryption/sdn-vnet-encryption.md) | 虚拟网络加密允许加密标记为启用加密。 的子网中相互通信的虚拟机之间的虚拟网络流量 它还利用虚拟子网上的数据报传输层安全性 (DTLS) 来加密数据包。 DTLS 可以防止能够访问物理网络的任何人进行窃听、篡改和伪造。 |       新增       |
|    [防火墙审核](security/sdn-firewall-auditing.md)    |                                                                                            防火墙审核是在 Windows Server 2019 SDN 防火墙的新功能。 当启用 SDN 防火墙时，获取记录通过 SDN 防火墙规则 (Acl)，启用日志记录处理任何流。                                                                                            |       新增       |
| [虚拟网络对等](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      虚拟网络对等互连可以无缝连接两个虚拟网络。 对等互连，出于连接目的后, 的虚拟网络会显示为一个。                                                                                                                      |       新增       |
|           [出口计数](manage/sdn-egress.md)            |                  在 Windows Server 2019 此新功能允许 SDN 为出站数据传输提供了用量计量表。 通过添加此功能，网络控制器保留每个虚拟网络的所有 IP 范围加入允许列表使用 SDN 的信息，并考虑未包含在为这些范围之一的目标发往任何数据包计费出站数据传输。                   |       新增       |

---



