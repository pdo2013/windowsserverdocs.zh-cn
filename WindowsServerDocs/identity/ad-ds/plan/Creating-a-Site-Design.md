---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: 创建站点设计
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d896213708f8c3ec5de44a1f85fb4ebd86b8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819068"
---
# <a name="creating-a-site-design"></a>创建站点设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建站点设计，需要确定哪些位置将成为站点、 创建站点对象、 创建子网对象并将子网与站点相关联。  
  
## <a name="deciding-which-locations-will-become-sites"></a>确定哪些位置将成为站点

确定哪些位置创建站点的如下所示：  
  
- 创建计划以将域控制器的所有位置的站点。 请参阅"域控制器放置"(DSSTOPO_4.doc) 工作表，以确定包括域控制器的位置中所述的信息。  
- 创建这些位置包括服务器运行的应用程序需要创建站点的站点。 某些应用程序，如分布式文件系统命名空间 (DFSN)，使用站点对象来查找最近的服务器到客户端。  

   > [!NOTE]  
   > 如果你的组织有多个网络邻近快速、 可靠的连接，可以在单个 Active Directory 站点中包含所有这些网络的子网。 例如，如果往返返回网络中不同的两个服务器之间的延迟的子网是 10 毫秒或更低，可以在同一 Active Directory 站点中包含两个子网。 如果两个位置之间的网络延迟超过 10 毫秒，不应包括在单个 Active Directory 站点中的子网。 甚至当延迟为 10 毫秒或更低，您可以选择部署单独的站点，如果你想要细分的基于 Active Directory 的应用程序的站点间的通信。  

- 如果站点不需要的位置，将位置的子网添加到位置对其具有的最大的广域网 (wan) 速度和可用带宽的站点。  
  
将成为站点的网络地址和子网掩码，每个位置内的文档位置。 若要帮助记录站点的工作表，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)、 下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开"将子网与站点"(DSSTOPO_6.doc)。  
  
## <a name="creating-a-site-object-design"></a>创建站点对象设计

对于每个位置，您已决定要创建站点，计划在 Active Directory 域服务 (AD DS) 中创建站点对象。 将成为站点"与站点相关联的子网"工作表中的文档位置。  
  
有关如何创建站点对象的详细信息，请参阅文章[创建一个站点](https://go.microsoft.com/fwlink/?LinkId=107067)。  
  
## <a name="creating-a-subnet-object-design"></a>创建子网对象设计

对于每个 IP 子网和子网掩码与每个位置相关联，计划表示站点内的所有 IP 地址的 AD DS 中创建子网对象。  
  
创建 Active Directory 子网对象时，网络 IP 子网和子网掩码的信息将自动转换为网络前缀长度表示法格式<IP address> / <prefix length>。 例如，网络 IP 版本 4 (IPv4) 地址 172.16.4.0 与子网掩码 255.255.252.0 显示为 172.16.4.0/22。 除了 IPv4 地址、 Windows Server 2008 还支持 IP 版本 6 (IPv6) 子网前缀，例如，3FFE:FFFF:0:C000:: / 64。 有关每个位置中的 IP 子网的详细信息，请参阅中的"位置和子网"(DSSTOPO_2.doc) 工作表[收集网络信息](../../ad-ds/plan/Collecting-Network-Information.md)和[附录 a:位置和子网前缀](Appendix-A--Locations-and-Subnet-Prefixes.md)。  
  
关联与站点对象的引用"决定其位置将成为站点"部分，以确定哪个子网中的"关联子网与站点"(DSSTOPO_6.doc) 工作表的每个子网对象是要与哪个站点相关联。 Active Directory 子网对象与"关联子网与站点"(DSSTOPO_6.doc) 工作表中每个位置相关联的文档。  
  
有关如何创建子网对象的详细信息，请参阅文章[创建子网](https://go.microsoft.com/fwlink/?LinkId=107068)。
