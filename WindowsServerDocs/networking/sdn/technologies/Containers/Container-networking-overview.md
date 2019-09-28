---
title: 容器网络概述
description: 本主题概述 Windows 容器的网络堆栈，并提供有关创建、配置和管理容器网络的其他指南的链接。
manager: ravirao
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.date: 09/04/2018
ms.openlocfilehash: 352b4303b7cf08a0c53712e46a309b8365c10d08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355674"
---
# <a name="container-networking-overview"></a>容器网络概述

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，我们将概述 Windows 容器的网络堆栈，并提供有关创建、配置和管理容器网络的附加指导的链接。

Windows Server 容器是一种轻型操作系统虚拟化方法，用于将应用程序或服务与在同一容器主机上运行的其他服务区分开来。 Windows 容器的作用类似于虚拟机。 启用后，每个容器都有一个单独的操作系统、进程、文件系统、注册表和 IP 地址的视图，可以连接到虚拟网络。 

Windows 容器与容器主机和主机上运行的所有容器共享内核。 由于共享内核空间，这些容器要求具有相同的内核版本和配置。 容器通过进程和命名空间隔离技术提供应用程序隔离。

>[!IMPORTANT]
>Windows 容器不提供恶意安全边界，且不应用于隔离不受信任的代码。 

使用 Windows 容器，你可以部署 Hyper-v 主机，你可以在其中在 VM 主机上创建一个或多个虚拟机。 VM 主机内会创建容器，网络访问通过虚拟交换机在虚拟机中运行。 可以使用存储在存储库中的可重用映像将操作系统和服务部署到容器中。 每个容器都有一个连接到虚拟交换机的虚拟网络适配器，可转发入站和出站流量。 可以将容器终结点连接到本地主机网络（如 NAT）、通过 SDN 堆栈创建的物理网络或覆盖虚拟网络。

若要在同一主机上的容器之间强制隔离，请为每个 Windows Server 和 Hyper-v 容器创建一个网络隔离舱。 Windows Server 容器使用主机 vNIC 连接到虚拟交换机。 Hyper-V 容器使用合成 VM NIC（不公开到实用工具 VM）连接到虚拟交换机。 

## <a name="related-topics"></a>相关主题 

- [Windows 容器网络](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture)：了解如何创建和管理用于非覆盖/SDN 部署的容器网络。

- [将容器终结点连接到租户虚拟网络](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md)：了解如何创建和管理包含 SDN 的虚拟网络的容器网络。 