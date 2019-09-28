---
title: DNS 资源记录管理
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe318a8ac17c650d8dbf2339e72b561de529c4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405678"
---
# <a name="dns-resource-record-management"></a>DNS 资源记录管理

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题提供有关使用 IPAM 管理 DNS 资源记录的信息。  
  
> [!NOTE]  
> 除了本主题之外，本部分还提供了以下 DNS 资源记录管理主题。  
>   
> -   [添加 DNS 资源记录](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [删除 DNS 资源记录](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [筛选 DNS 资源记录的视图](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [查看特定 IP 地址的 DNS 资源记录](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>资源记录管理概述  
当你在 Windows Server 2016 中部署 IPAM 时，你可以执行服务器发现以将 DHCP 和 DNS 服务器添加到 IPAM 服务器管理控制台。 然后，IPAM 服务器每六个小时从其配置为管理的 DNS 服务器动态收集 DNS 数据。 IPAM 维护存储此 DNS 数据的本地数据库。 IPAM 向您提供收集服务器数据的日期和时间的通知，并告诉您下一天和来自 DNS 服务器的数据收集时间。  
  
下图中的黄色状态栏显示了 IPAM 通知的用户界面位置。  
  
![IPAM 通知](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
收集的 DNS 数据包括 DNS 区域和资源记录信息。 你可以配置 IPAM 以从首选 DNS 服务器收集区域信息。  IPAM 同时收集基于文件的区域和 Active Directory 区域。  
  
> [!NOTE]  
> IPAM 只收集来自已加入域的 Microsoft DNS 服务器的数据。 IPAM 不支持第三方 DNS 服务器和未加入域的服务器。  
  
下面是 IPAM 收集的 DNS 资源记录类型的列表。  
  
-   AFS 数据库  
  
-   ATM 地址  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   主机 A 或 AAAA  
  
-   主机信息  
  
-   专线  
  
-   MX  
  
-   名称服务器  
  
-   指针（PTR）  
  
-   负责人  
  
-   路由  
  
-   服务定位  
  
-   SOA  
  
-   SRV  
  
-   文本  
  
-   已知服务  
  
-   WINS  
  
-   WINS-R  
  
-   X.X.X。X  
  
在 Windows Server 2016 中，IPAM 提供 IP 地址清单、DNS 区域和 DNS 资源记录之间的集成：  
  
-   你可以使用 IPAM 从 DNS 资源记录自动构建 IP 地址清单。  
  
-   你可以从 DNS A 和 AAAA 资源记录手动创建 IP 地址清单。  
  
-   可以查看特定 DNS 区域的 DNS 资源记录，并根据类型、IP 地址、资源记录数据和其他筛选选项筛选记录。  
  
-   IPAM 会自动在 IP 地址范围和 DNS 反向查找区域之间创建映射。  
  
-   IPAM 为位于反向查找区域中的 PTR 记录创建 IP 地址，其中包含在该 IP 地址范围内。 如果需要，还可以手动修改此映射。  
  
IPAM 允许您对 IPAM 控制台中的资源记录执行以下操作。  
  
-   创建 DNS 资源记录  
  
-   编辑 DNS 资源记录  
  
-   删除 DNS 资源记录  
  
-   创建关联的资源记录  
  
IPAM 会自动记录使用 IPAM 控制台进行的所有 DNS 配置更改。  
  
## <a name="see-also"></a>请参阅  
[管理 IPAM](Manage-IPAM.md)  
  


