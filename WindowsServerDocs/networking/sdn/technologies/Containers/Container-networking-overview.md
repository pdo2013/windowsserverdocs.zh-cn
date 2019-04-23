---
title: 容器网络概述
description: 本主题概述了用于 Windows 容器网络堆栈，并包括有关创建、 配置和管理容器网络的附加指导的链接。
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.date: 09/04/2018
ms.openlocfilehash: 72b1ac739d9012ac7b90e97abe22e5f321ddba63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886138"
---
# <a name="container-networking-overview"></a>容器网络概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，我们为你的网络堆栈概述 Windows 容器中的和其中包含指向有关创建、 配置和管理容器网络的附加指导。

Windows Server 容器是轻量操作系统虚拟化方法，将应用程序或服务从运行在同一容器主机的其他服务分离出来。 Windows 容器的作用类似于虚拟机。 启用时，每个容器都有一个单独的视图的操作系统、 进程、 文件系统、 注册表和 IP 地址，你可以连接到虚拟网络。 

Windows 容器与容器主机和主机上运行的所有容器共享内核。 由于共享内核空间，这些容器要求具有相同的内核版本和配置。 容器提供应用程序通过进程和命名空间隔离技术隔离。

>[!IMPORTANT]
>Windows 容器不提供恶意安全边界，不应使用隔离不受信任的代码。 

使用 Windows 容器，你可以部署的 HYPER-V 主机，其中在 VM 的主机创建一个或多个虚拟机。 内部 VM 主机中，创建容器，以及网络访问是通过虚拟机内运行的虚拟交换机。 存储在存储库中的可重用映像可用于将操作系统和服务部署到容器。 每个容器具有连接到虚拟交换机，转发入站和出站流量的虚拟网络适配器。 可以将容器终结点附加到本地主机网络 （如 NAT)、 物理网络或覆盖通过 SDN 堆栈创建的虚拟网络。

强制实施的同一主机上的容器之间的隔离，您创建每个 Windows Server 和 HYPER-V 容器网络隔离的舱。 Windows Server 容器使用主机 vNIC 连接到虚拟交换机。 Hyper-V 容器使用合成 VM NIC（不公开到实用工具 VM）连接到虚拟交换机。 

## <a name="related-topics"></a>相关主题 

- [Windows 容器网络](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture):了解如何创建和管理容器网络覆盖/SDN 部署。

- [将容器终结点连接到租户虚拟网络](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md):了解如何创建和管理 SDN 的覆盖虚拟网络的容器网络。 