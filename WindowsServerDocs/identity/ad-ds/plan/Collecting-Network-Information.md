---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: 收集网络信息
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5642963caee47ac48f841a13b47852b6b7d9b241
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408985"
---
# <a name="collecting-network-information"></a>收集网络信息

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 域服务（AD DS）中设计有效站点拓扑的第一步是，查看组织的网络组，以便定期收集相关信息并与物理网络拓扑通信。  
  
## <a name="creating-a-location-map"></a>创建位置映射

创建一个位置映射，用于表示组织的物理网络基础结构。 在位置地图上，标识包含一组计算机的地理位置，这些计算机的内部连接为每秒10兆位（Mbps）或更高（局域网（LAN）速度或更高）。  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>列出通信链接和可用带宽

创建位置映射后，记录通信链接的类型、其链接速度和每个位置之间可用的带宽。 从网络组中获取广域网络（WAN）拓扑。 有关常见 WAN 线路类型及其带宽的列表，请参阅[创建站点链接设计](../../ad-ds/plan/Creating-a-Site-Link-Design.md)中的 "确定成本" 一节。 稍后你将需要此信息在站点拓扑设计过程中创建站点链接。  
  
带宽是指可以在给定时间内跨信道传输的数据量。 可用带宽是指可供 AD DS 实际使用的带宽量。 你可以从网络组获取可用的带宽信息，或者可以使用协议分析器（如网络监视器）分析每个链接上的流量。 有关安装网络监视器的信息，请参阅[监视网络流量](https://go.microsoft.com/fwlink/?LinkId=107058)的文章。  
  
记录每个位置以及链接到该位置的其他位置。 此外，记录通信链接类型及其可用带宽。 要使工作表可以帮助你列出通信链接和可用带宽，请参阅[Windows Server 2003 部署工具包的作业帮助](https://go.microsoft.com/fwlink/?LinkID=102558)、下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "地理位置和通信链接 "（DSSTOPO_1）。  
  
## <a name="listing-ip-subnets-within-each-location"></a>列出每个位置中的 IP 子网

记录通信链接和每个位置之间可用的带宽后，请记录每个位置中的 IP 子网。 如果你还不知道每个位置中的子网掩码和 IP 地址，请咨询你的网络组。  
  
AD DS 通过将工作站的 IP 地址与每个站点相关联的子网进行比较，将工作站与站点相关联。 向域中添加域控制器时，AD DS 还会检查其 IP 地址，并将其放置在最合适的站点中。  
  
要使工作表可以帮助你列出每个位置中的 IP 子网，请参阅[Windows Server 2003 部署工具包的作业帮助](https://go.microsoft.com/fwlink/?LinkID=102558)、下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "位置和子网" （DSSTOPO_2）。  
  
> [!NOTE]  
> 除了 IP 版本4（IPv4）地址以外，Windows Server 还支持 IP 版本6（IPv6）子网前缀。 要使工作表可以帮助你列出 IPv6 子网前缀，请参阅 [Appendix A：位置和子网前缀 @ no__t。  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>列出每个位置的域和用户数

位置中所代表的每个地区性域的用户数量是确定区域域控制器和全局编录服务器的位置的因素之一，这是站点拓扑设计过程中的下一步。 例如，计划将区域域控制器放置在包含超过100个区域域用户的位置，以便他们仍可在 WAN 链接失败时登录到域。  
  
记录位置、在每个位置显示的域，以及每个位置中表示的每个域的用户数量。 要使工作表可以帮助你列出每个位置中所表示的域和用户数，请参阅[Windows Server 2003 部署工具包的作业帮助](https://go.microsoft.com/fwlink/?LinkID=102558)、下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services。并打开 "每个位置中的域和用户" （DSSTOPO_3）。  
