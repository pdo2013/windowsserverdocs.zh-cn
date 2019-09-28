---
title: 引入 MultiPoint Services
description: 概述 MultiPoint 服务，一种允许多个用户共享系统的方法
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: a0497f9dfd39648a94d9fb832f4404491955c06a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395354"
---
# <a name="introducing-multipoint-services"></a>引入 MultiPoint Services
Windows Server 2016 中的 MultiPoint 服务角色允许多个用户，每个用户都有自己的独立且熟悉的 Windows 体验，同时共享一台计算机。用户可以通过多种方式访问其会话。 其中一种方法是通过在任何设备上使用[远程桌面应用](../remote-desktop-services/clients/remote-desktop-clients.md)远程访问服务器。 另一种方法是通过工作站连接到 MultiPoint 服务器：  
  
-   直接到计算机上的视频端口  
  
-   通过专用 USB 零客户端（也称为 "多功能 USB 集线器"）以及类似的 USB 以太网设备。  
  
-   通过局域网（LAN）  
  
本文档后面的[MultiPoint Services 工作站](MultiPoint-services-Stations.md)中更详细地介绍了上述每种方法。  
  
本文档介绍在计划部署 MultiPoint 服务时要考虑的以下因素：  
  
-   用于 MultiPoint 服务系统的桌面类型：是否需要会话、虚拟机或 Windows 电脑？  
  
-   [为 MultiPoint 服务系统选择硬件](Selecting-Hardware-for-Your-MultiPoint-services-System.md)：你应做出哪些硬件决策？  
  
-   [硬件要求和性能建议](Hardware-Requirements-and-Performance-Recommendations.md)：MultiPoint 服务需要什么硬件？  
  
-   [MultiPoint Services 站点规划](MultiPoint-services-Site-Planning.md)：运行 MultiPoint 服务及其工作站的计算机位于何处，以及如何配置这些计算机？  
  
-   [网络注意事项和用户帐户](Network-Considerations-and-User-Accounts.md)：MultiPoint 服务系统所部署到的网络环境可能会影响用户帐户的管理方式。 什么是网络环境？ 如何管理用户帐户？  
  
-   [用 MultiPoint Services 存储文件](Storing-Files-with-MultiPoint-services.md)：将用户文件存储到何处，以及如何访问这些文件？  
  
-   [预部署清单](Predeployment-Checklist.md)  