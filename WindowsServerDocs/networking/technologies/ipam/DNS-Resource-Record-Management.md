---
title: DNS 资源记录管理
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2db802fa928a4051fbe409fc0ba60d774bb0428
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284055"
---
# <a name="dns-resource-record-management"></a>DNS 资源记录管理

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题提供有关使用 IPAM 管理 DNS 资源记录的信息。  
  
> [!NOTE]  
> 除了本主题，以下 DNS 资源记录管理主题可用于此部分。  
>   
> -   [添加 DNS 资源记录](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [删除 DNS 资源记录](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [筛选 DNS 资源记录视图](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [查看用于特定的 IP 地址的 DNS 资源记录](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>资源记录管理概述  
当您部署 Windows Server 2016 中的 IPAM 时，可以执行服务器发现将 DHCP 和 DNS 服务器添加到 IPAM 服务器管理控制台。 IPAM 服务器然后动态 DNS 数据每隔六个小时收集从 DNS 服务器配置为管理。 IPAM 维护一个本地数据库，它将此 DNS 数据的存储。 IPAM 可为您提供的一天和收集服务器数据，以及告诉您下一天的时间和时间的 DNS 服务器中的数据集合将发生时的通知。  
  
下图中的黄色状态栏显示了 IPAM 通知的用户界面位置。  
  
![IPAM 通知](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
收集的 DNS 数据包括 DNS 区域和资源记录信息。 可以配置 IPAM 以收集你首选的 DNS 服务器的区域信息。  IPAM 收集基于文件的和 Active Directory 区域。  
  
> [!NOTE]  
> IPAM 仅从已加入域的 Microsoft DNS 服务器收集数据。 IPAM 不支持第三方 DNS 服务器和非域加入的服务器。  
  
下面是收集由 IPAM 的 DNS 资源记录类型的列表。  
  
-   AFS 数据库  
  
-   ATM 地址  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   主机 A 或 AAAA  
  
-   主机信息  
  
-   ISDN  
  
-   MX  
  
-   名称服务器  
  
-   指针 (PTR)  
  
-   负责人员  
  
-   通过路由  
  
-   服务位置  
  
-   SOA  
  
-   SRV  
  
-   Text  
  
-   已知的服务  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
在 Windows Server 2016 中，IPAM 提供 IP 地址清单、 DNS 区域和 DNS 资源记录之间的集成：  
  
-   可以使用 IPAM 来自动生成的 DNS 资源记录的 IP 地址清单。  
  
-   从 DNS A 和 AAAA 资源记录，可以手动创建 IP 地址清单。  
  
-   您可以查看为特定的 DNS 区域的 DNS 资源记录和筛选记录基于类型、 IP 地址、 资源记录数据和其他筛选选项。  
  
-   IPAM 会自动创建的 IP 地址范围和 DNS 反向查找区域之间的映射。  
  
-   IPAM 创建反向查找区域中存在和相应的 IP 地址范围中包括哪些的 PTR 记录的 IP 地址。 如果需要您可以手动修改此映射。  
  
IPAM 还允许您从 IPAM 控制台执行以下操作的资源记录。  
  
-   创建 DNS 资源记录  
  
-   编辑 DNS 资源记录  
  
-   删除 DNS 资源记录  
  
-   创建关联的资源记录  
  
IPAM 会自动记录使用 IPAM 控制台进行的所有 DNS 配置更改。  
  
## <a name="see-also"></a>请参阅  
[管理 IPAM](Manage-IPAM.md)  
  


