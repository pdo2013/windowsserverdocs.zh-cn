---
title: 软件定义的网络 (SDN)
description: 软件定义的网络 (SDN) 提供了一种方法，可用于在数据中心内集中配置和管理物理和虚拟网络设备（如路由器、交换机和网关）。 使用本主题，了解如何在 Windows Server、 System Center 和 Microsoft Azure 中提供的软件定义网络 (SDN) 技术。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.date: 08/09/2018
ms.openlocfilehash: a6c4db97b1c55d5114eba09251685ef0f2b840bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855228"
---
# <a name="sdn-in-windows-server-overview"></a>在 Windows Server 概述 SDN

>适用于：Windows 服务器 （半年频道），Windows Server 2016


软件定义的网络 (SDN) 提供了一种方法，可用于在数据中心内集中配置和管理物理和虚拟网络设备（如路由器、交换机和网关）。 可以使用现有的 SDN 兼容设备以实现虚拟网络与物理网络之间的更深入地集成。 虚拟网络的 HYPER-V 虚拟交换机、 HYPER-V 网络虚拟化和 RAS 网关元素设计为 SDN 基础结构中必不可少的元素。 

>[!Note]
>HYPER-V 主机和运行 SDN 基础结构服务器，如网络控制器和软件负载平衡节点的虚拟机 (Vm) 必须安装 Windows Server 2016 Datacenter edition。 
>
>包含唯一租户工作负荷 Vm 连接到 SDN 控制网络的 HYPER-V 主机可以使用 Windows Server 2016 标准版。

SDN 之所以可行，因为网络平面不再绑定到网络设备本身。 但是，其他实体，如 System Center 2016 数据中心管理软件使用网络平面。 SDN 可以动态地管理你的数据中心网络提供一种自动集中式的方式来满足你的应用程序和工作负荷的要求。 

可以使用 SDN 到：

- 动态创建、 保护和以满足不断发展的您的应用程序需要将网络连接
- 以非破坏性的方式提高工作负荷的部署速度
- 包含从分散到你的网络的安全漏洞
- 定义和控制策略来管理物理和虚拟网络 
- 实现一致地在规模较大的网络策略

SDN 可以完成所有这些操作的同时还能降低整体基础结构成本。



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>数据中心和云网络产品团队联系

如果有兴趣讨论与 Microsoft 或其他 SDN 客户的 SDN 技术，有多种方法，用于进行联系。

有关详细信息，请参阅[联系数据中心和云网络团队](contact-sdn-team.md)。
