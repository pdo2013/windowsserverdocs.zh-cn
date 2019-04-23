---
title: MultiPoint Services 虚拟化支持
description: 介绍如何使用 MultiPoint 服务的 HYPER-V
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 06d518dcea154ac2bab49a7d0e83a90f96be6e44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872518"
---
# <a name="multipoint-services-virtualization-support"></a>MultiPoint Services 虚拟化支持
MultiPoint 服务支持两种方法中的 HYPER-V 角色：  
  
-   MultiPoint 服务可部署为来宾操作系统上运行 HYPER-V 的服务器。  
  
-   MultiPoint 服务可以用作虚拟化服务器。   
  
虚拟机上运行 MultiPoint 服务提供了用于管理操作系统的 HYPER-V 工具使用。 这些工具包括检查点和回滚功能，并且允许您导出和导入虚拟机。 对于大型安装，可以在一台物理服务器上运行多个 MultiPoint 服务的虚拟计算机来合并服务器。 可能的方案包括：  
  
-   一个教室或实验室具有超过 20 个席位。 而不是部署多台运行 MultiPoint 服务的物理计算机，可以部署一台物理计算机上的多个虚拟机。  
  
    > [!NOTE]  
    > 无论是物理还是虚拟的通过一个 MultiPoint 管理器控制台，你可以管理多个 MultiPoint server。  
  
-   MultiPoint server 的虚拟机上运行与同一台物理计算机上的另一个服务器基础结构。 在这种情况下此服务器基础结构集中管理域、 安全性和网络数据。 MultiPoint server 提供了远程桌面服务，并可集中管理台式计算机。  
  
> [!NOTE]  
> 当虚拟机上运行 MultiPoint 服务，支持 USB over 以太网和 RDP 客户端工作站。 不支持直接视频和 USB 零客户端连接工作站。  
  
有关 HYPER-V 角色的详细信息，请参阅[HYPER-V](../../virtualization/hyper-v/hyper-v-on-windows-server.md)。  