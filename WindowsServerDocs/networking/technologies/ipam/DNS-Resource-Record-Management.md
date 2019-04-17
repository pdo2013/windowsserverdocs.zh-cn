---
title: DNS 资源录制管理
description: 本主题介绍的 IP 地址管理 (IPAM) 管理指南中的 Windows Server 2016 的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ed781ef37243b80ea9da8aad27a29046b8dc8c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="dns-resource-record-management"></a>DNS 资源录制管理

>适用于：Windows Server（半年通道），Windows Server 2016

本主题提供有关使用 IPAM 管理的资源 DNS 记录的信息。  
  
> [!NOTE]  
> 本主题中，除了以下 DNS 资源录制管理主题可以在此部分中都可用。  
>   
> -   [添加 DNS 资源记录](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [删除 DNS 资源记录](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [筛选 DNS 资源记录视图](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [查看 DNS 资源记录特定的 IP 地址](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>资源录制管理概述  
部署 Windows Server 2016 中的 IPAM 时，你可以执行服务器发现向 IPAM 服务器管理控制台添加 DHCP 和 DNS 服务器。 IPAM 服务器然后动态 DNS 从收集数据每 6 小时配置为在 DNS 服务器管理。 IPAM 维护本地数据库存储 DNS 此类数据的位置。 IPAM 为您提供通知的一天服务器了所收集的数据，以及告知你下一天的时间和来自 DNS 服务器的数据收集将发生时的时间。  
  
黄色状态栏在下图显示 IPAM 通知的用户界面的位置。  
  
![IPAM 通知](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
收集 DNS 数据包括 DNS 区域和资源记录信息。 您可以配置 IPAM 用于收集来自你的首选 DNS 服务器的区域的信息。  IPAM 收集基于文件的两和 Active Directory 的区域。  
  
> [!NOTE]  
> 仅从域加入 Microsoft DNS 服务器，IPAM 收集数据。 第三方 DNS 服务器和非域连接的服务器不受 IPAM。  
  
下面是 DNS 收集 IPAM 的资源录制类型的列表。  
  
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
  
-   负责人  
  
-   通过路线  
  
-   位置服务  
  
-   SOA  
  
-   SRV  
  
-   文本  
  
-   众所周知服务  
  
-   WINS  
  
-   WINS R  
  
-   X.25  
  
Windows Server 2016，IPAM 提供 IP 地址库存、DNS 区域和 DNS 资源记录之间的集成：  
  
-   你可以使用 IPAM 自动版本从 DNS 资源记录 IP 地址清单。  
  
-   从 DNS A 和 AAAA 资源记录，可以手动创建 IP 地址清单。  
  
-   你可以查看 DNS 资源特定 DNS 区域，记录和筛选记录基于类型、IP 地址、资源记录数据和其他的筛选选项。  
  
-   IPAM 会自动创建 ip 地址和 DNS 反向查找区域之间有对应关系。  
  
-   IPAM 创建 PTR 记录反向查找区域中的当前存在，其中包含该 IP 地址范围内的 IP 地址。 如果需要你还可以手动修改此映射。  
  
IPAM 允许你从 IPAM 控制台执行下列操作资源记录。  
  
-   创建的资源 DNS 记录  
  
-   编辑 DNS 资源记录  
  
-   删除 DNS 资源记录  
  
-   创建相关的资源记录  
  
IPAM 自动登录使用 IPAM 控制台进行的所有 DNS 配置更改。  
  
## <a name="see-also"></a>请参阅  
[管理 IPAM](Manage-IPAM.md)  
  


