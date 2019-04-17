---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: "收集网络的信息"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 174f11ca85a659f9d0c52e220d5ce37b1804f39b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="collecting-network-information"></a>收集网络的信息

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

请咨询你的组织的网络组收集信息并与他们定期通信有关你的物理网络拓扑设计生效站点拓扑 Active Directory 域服务 (广告 DS) 中的第一步是。  
  
## <a name="creating-a-location-map"></a>创建的位置的地图  
创建位置的地图，表示你的组织的物理网络基础结构。 打开位置的地图，识别包含的计算机具有组内部连接的 10 兆位 / 秒 (Mbps) 或更高版本（速度本地网络 (LAN) 或更高）地理位置。  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>列表通信链接并可带宽  
在创建位置地图之后, 的文档通信链接，其链接速度，可用带宽之间各位置的类型。 从你的网络组获取较宽区域广域网拓扑。 WAN 电路常见和其带宽列表，请参阅"如何确定成本"一节[创建站点链接设计](../../ad-ds/plan/Creating-a-Site-Link-Design.md)。 你将需要此信息以在以后网站拓扑设计过程中创建网站的链接。  
  
带宽是指你可以通过传输通信信道在给定时间内的数据量。 可用带宽指的实际可供使用广告 DS 带宽量。 从你的网络组，可以获得可用带宽信息或你可以使用网络监视器，如协议分析这一个组件，随 Windows Server 2008 分析上每一个链接的交通。 有关安装网络监视器的信息，请参阅监视网络通信 ([https://go.microsoft.com/fwlink/?LinkId=107058](https://go.microsoft.com/fwlink/?LinkId=107058))。  
  
每个位置以及链接到其他位置的文档。 此外，录制通信的链接，其可用带宽的类型。 以帮助你列出通信链接和可用带宽工作表中，请参阅 Windows Server 2003 部署工具包工作辅助 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and open "Geographic 位置和通信链接"(DSSTOPO_1.doc)。  
  
## <a name="listing-ip-subnets-within-each-location"></a>在每个位置列表 IP 子网  
你文档通信链接并在各个位置之间可用带宽后，记录每个位置内的 IP 个子网。 如果你不已知道子网掩码和每个位置内的 IP 地址，请咨询你的网络组。  
  
广告 DS 将某个站点与工作站关联通过比较工作站 IP 地址与每个站点个子网。 当您添加到某个域的域控制器，广告 DS 将还会检查它们的 IP 地址，并将它们放最适合的网站。  
  
以帮助你列出了每个位置内的 IP 子网工作表中，请参阅 Windows Server 2003 部署工具包工作辅助 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip >，并打开"位置和子网"(DSSTOPO_2.doc)。  
  
> [!NOTE]  
> 除了 IP 版本 4 (IPv4) 地址、Windows Server 2008 还支持 IP 版本 6 (IPv6) 子网前缀。 以帮助你列出 IPv6 子网前缀工作表中，请参阅[附录 a：位置和子网前缀](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md)。  
  
## <a name="listing-domains-and-number-of-users-for-each-location"></a>列出了域和每个位置的用户数  
便会出现在一个位置的每个区域的域的用户数是一种全球目录服务器和确定位置的区域域控制器的因素即站点拓扑设计过程的下一步。 例如，计划将区域域控制器放置包含超过 100 区域域用户，以便他们可以仍登录到域如果 WAN 链接无法正常工作的位置。  
  
录制的位置，在每个位置，并为每个便会出现在每个位置的域的用户数表示的域。 以帮助你列出的域和表示在每个位置的用户数工作表中，请参阅 Windows Server 2003 部署工具包工作辅助 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, and 打开"域和用户在每个位置"(DSSTOPO_3.doc)。  
  


