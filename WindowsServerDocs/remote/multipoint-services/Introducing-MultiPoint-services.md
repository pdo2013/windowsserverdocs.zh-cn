---
title: 引入 MultiPoint Services
description: 提供 MultiPoint 服务，让多个用户共享系统的方式的概述
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 86d240092282e7cc29eebe638e5a97312e22baff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844208"
---
# <a name="introducing-multipoint-services"></a>引入 MultiPoint Services
Windows Server 2016 中的 multiPoint 服务角色允许多个用户，每个都有其自己的独立于和熟悉 Windows 体验，同时共享一台计算机。有几种方法，用户可以访问其会话。 一种方法是通过为服务器，并使用远程处理[远程桌面应用程序](../remote-desktop-services/clients/remote-desktop-clients.md)使用任何设备。 另一种方法是通过物理工作站连接到 MultiPoint server 的工作站：  
  
-   直接向计算机上的视频端口  
  
-   通过专用 USB 零客户端 （也称为多功能 USB 集线器） 以及类似的以太网 over USB 设备。  
  
-   通过局域网 (LAN)  
  
每种方法在更多详细信息中所述[MultiPoint 服务工作站](MultiPoint-services-Stations.md)本文档中更高版本。  
  
本文档介绍了需要考虑当你打算部署 MultiPoint 服务的以下因素：  
  
-   台式计算机与你的 MultiPoint 服务系统使用哪种类型：你将需要会话、 虚拟机或 Windows 电脑？  
  
-   [为你的 MultiPoint 服务系统选择硬件](Selecting-Hardware-for-Your-MultiPoint-services-System.md):您应做哪些硬件决策？  
  
-   [硬件要求和性能建议](Hardware-Requirements-and-Performance-Recommendations.md):MultiPoint Services 所必需的是什么硬件？  
  
-   [MultiPoint Services 站点规划](MultiPoint-services-Site-Planning.md):运行 MultiPoint 服务和其工作站的计算机将位于何处，以及将如何配置它们？  
  
-   [网络注意事项和用户帐户](Network-Considerations-and-User-Accounts.md):MultiPoint 服务系统部署到其中的网络环境可能会影响如何管理用户帐户。 什么是你的网络环境？ 如何管理用户帐户？  
  
-   [存储与 MultiPoint 服务文件](Storing-Files-with-MultiPoint-services.md):将用户文件存储在何处，以及将如何访问它们？  
  
-   [预部署清单](Predeployment-Checklist.md)  