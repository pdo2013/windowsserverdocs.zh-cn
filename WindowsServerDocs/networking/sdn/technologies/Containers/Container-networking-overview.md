---
title: 容器网络概述
description: 本主题是 Windows 容器的网络堆栈概述，并包含其他指导创建、 配置和管理容器网络有关的链接。
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
ms.openlocfilehash: fd2f022948208d4aacce2994ff053e77384b28fc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="container-networking-overview"></a>容器网络概述

>适用于：Windows Server（半年通道），Windows Server 2016

本主题是 Windows 容器的网络堆栈概述，并包含其他指导创建、 配置和管理容器网络有关的链接。

Windows Server 容器是一个简洁操作系统虚拟化方法，用于从其他服务在同一容器主机上运行的分隔应用程序或服务。 若要启用此功能，每个容器具有操作系统、 进程，文件系统、 注册表以及 IP 地址的视图。

Windows 容器类似于网络关于希望在虚拟机的功能。 每个容器已连接到哪些传入和传出的通信的虚拟交换机虚拟网络适配器。 强制容器同一台主机上的分隔，为每个 Windows Server 和 HYPER-V 容器容器该网络适配器安装到其中创建网络盒。 Windows Server 容器使用主机 vNIC 吸附到虚拟交换机用来。 HYPER-V 容器使用综合 VM NIC （不会受到实用程序 VM） 连接到虚拟交换机用来。 

容器端点可以将附加到主机本地网络 (如 NAT)、 物理网络或通过 Microsoft 软件定义网络 (SDN) 堆栈创建覆盖虚拟网络。 

有关如何创建和管理容器网络的覆盖层/SDN 部署的详细信息，请参阅[Windows 容器网络](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking)MSDN 上的指南。

有关如何创建和管理容器网络的覆盖层 SDN 虚拟网络的详细信息，请参阅[租户虚拟网络连接容器端点](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md)。 