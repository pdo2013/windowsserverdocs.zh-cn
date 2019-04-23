---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: 收集网络信息
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d88cc2fafd9aa4fc221efc901c48fa41ef007a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867038"
---
# <a name="collecting-network-information"></a>收集网络信息

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

设计 Active Directory 域服务 (AD DS) 中的有效站点拓扑的第一步是请查阅你的组织网络组来收集信息并与他们定期交流有关物理网络拓扑的信息。  
  
## <a name="creating-a-location-map"></a>创建位置地图

创建表示你的组织的物理网络基础结构的位置映射。 在位置映射中，标识包含计算机组的 10 兆比特 / 秒 (Mbps) 或更高版本的内部连接 （局域网 (LAN) 速度或更高） 的地理位置。  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>列出通信链接和可用带宽

创建位置地图后, 的文档类型的通信链接，其链接速度和每个位置之间的可用带宽。 从您的网络小组获取广域网 (wan) 拓扑。 常见的 WAN 线路类型和其带宽的列表，请参阅"确定成本"中的部分[创建站点链接设计](../../ad-ds/plan/Creating-a-Site-Link-Design.md)。 你将需要更高版本在站点拓扑设计过程中创建站点链接此信息。  
  
带宽是指你可以通过传输通信通道中给定的时间内的数据量。 可用带宽是指由 AD DS 使用实际可用的带宽量。 你可以从你网络的组，获取可用带宽的信息也可以使用协议分析器，例如网络监视器来分析每个链接上的流量。 有关安装网络监视器的信息，请参阅文章[监视网络流量](https://go.microsoft.com/fwlink/?LinkId=107058)。  
  
记录每个位置和链接到它的其他位置。 此外，记录的类型的通信链接和其可用带宽。 若要帮助您列出通信链接和可用带宽的工作表，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，和打开"地理位置和通信链接"(DSSTOPO_1.doc)。  
  
## <a name="listing-ip-subnets-within-each-location"></a>列出每个位置内的 IP 子网

文档通信链接和每个位置之间的可用带宽后，记录每个位置内的 IP 子网。 如果您不已知道子网掩码和每个位置内的 IP 地址，请查阅你网络的组。  
  
AD DS 将与站点关联工作站，通过比较了与每个站点相关联的子网的工作站的 IP 地址。 在域控制器添加到域中，AD DS 还会检查其 IP 地址，并将它们放在最合适的站点。  
  
工作表可帮助您列出每个位置内的 IP 子网，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)、 下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开"位置和子网"(DSSTOPO_2.doc)。  
  
> [!NOTE]  
> 除了 IP 版本 4 (IPv4) 地址，Windows Server 还支持 IP 版本 6 (IPv6) 子网前缀。 工作表可帮助您列出的 IPv6 子网前缀，请参阅[附录 a:位置和子网前缀](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md)。  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>列出的域和每个位置的用户数

在一个位置中的呈现每个区域域的用户数是其中一个因素确定放置区域的域控制器和全局编录服务器，这是站点拓扑设计过程中的下一步。 例如，计划的区域的域控制器放置在包含 100 多个区域的域用户，以便它们可以仍登录到域如果 WAN 链路失败的位置。  
  
记录的位置，请在每个位置，并在每个位置中的呈现每个域的用户数表示的域。 若要帮助您在域和在每个位置中表示的用户数列出的工作表，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开"域和用户在每个位置"(DSSTOPO_3.doc)。  
