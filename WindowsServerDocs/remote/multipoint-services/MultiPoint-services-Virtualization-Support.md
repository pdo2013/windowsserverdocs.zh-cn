---
title: MultiPoint Services 虚拟化支持
description: 描述如何将 MultiPoint 服务与 Hyper-v 配合使用
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7b94b4a4015e58402a62cf74f9abbb3eb2333f26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395193"
---
# <a name="multipoint-services-virtualization-support"></a>MultiPoint Services 虚拟化支持
MultiPoint 服务通过两种方式支持 Hyper-v 角色：  
  
-   MultiPoint Services 可在运行 Hyper-v 的服务器上部署为来宾操作系统。  
  
-   MultiPoint 服务可用作虚拟化服务器。   
  
在虚拟机上运行 MultiPoint 服务提供了使用 Hyper-v 工具来管理操作系统的情况。 这些工具包括检查点和回滚功能，并允许你导出和导入虚拟机。 对于较大的安装，可以通过在单个物理服务器上运行多个 MultiPoint 服务虚拟计算机来合并服务器。 可能的方案包括：  
  
-   单个教室或实验室有超过20个座位。 你可以在一台物理计算机上部署多个虚拟机，而不是部署多个运行 MultiPoint 服务的物理计算机。  
  
    > [!NOTE]  
    > 可以通过单个 MultiPoint 管理器控制台来管理多个多点服务器，无论是物理的还是虚拟的。  
  
-   MultiPoint server 在虚拟机上运行，在同一物理计算机上有另一个服务器基础结构。 在这种情况下，此服务器基础结构将为网络集中域、安全和数据。 MultiPoint 服务器提供远程桌面服务和集中桌面。  
  
> [!NOTE]  
> 在虚拟机上运行 MultiPoint 服务时，支持 USB over 以太网和 RDP 客户端工作站。 不支持直接视频和 USB 零客户端连接工作站。  
  
有关 Hyper-v 角色的详细信息，请参阅[hyper-v](../../virtualization/hyper-v/hyper-v-on-windows-server.md)。  