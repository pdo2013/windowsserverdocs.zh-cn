---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: "创建站点设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2bcbf30159721e1fc2e12af103ca6f3e79f3ec97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-design"></a>创建站点设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

创建站点设计涉及决定在哪些位置将成为站点、创建站点对象、创建子网对象和将子网关联的站点。  
  
## <a name="deciding-which-locations-will-become-sites"></a>决定在哪些位置将成为站点  
决定在哪些位置创建网站，如下所示：  
  
-   创建您计划放置域控制器的所有位置的网站。 请参阅"域控制器的位置"(DSSTOPO_4.doc) 工作表来确定位置，包括域控制器中记录的信息。  
  
-   创建包含运行的应用程序需要站点要创建的服务器这些位置的网站。 某些应用程序，如分布式文件系统命名空间 (DFSN)，使用站点对象查找最接近服务器向客户端。  
  
    > [!NOTE]  
    > 如果你的组织有多个网络，近与 fast、可靠地连接，你可以在一处 Active Directory 包括所有这些网络的子网。 例如，如果往返退货网络采用不同的两个服务器之间的延迟子网是 10 毫秒或者更少，包括两个子网在相同的 Active Directory 站点。 两个位置的网络反应大于 10 毫秒，如果你不应包含在一处 Active Directory 个子网。 即使当延迟 10 毫秒或更少，你可以选择部署单独的网站，如果你想要分段基于 Active Directory 的应用程序的站点之间的交通。  
  
-   如果网站未所需的某个位置，添加对其位置具有最大的宽区域网速 (WAN) 和可用带宽网站的位置的子网。  
  
将站点网络地址并在每个位置的子网掩码成为的文档位置。 为了帮助您记录站点工作表中，请参阅 Windows Server 2003 部署工具包工作辅助 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及打开"用站点关联子网"(DSSTOPO_6.doc)。  
  
## <a name="creating-a-site-object-design"></a>创建站点对象设计  
对于每个决定已创建的站点的位置，计划 Active Directory 域服务 (广告 DS) 中创建站点对象。 将成为站点，"关联的站点的子网"工作表中的文档位置。  
  
有关如何创建站点对象的详细信息，请参阅创建站点 ([https://go.microsoft.com/fwlink/?LinkId=107067](https://go.microsoft.com/fwlink/?LinkId=107067))。  
  
## <a name="creating-a-subnet-object-design"></a>创建子网对象设计  
对于每 IP 子网和关联了每个位置的子网掩码，计划在广告 DS 代表站点中的所有 IP 地址创建子网对象。  
  
在创建子网 Active Directory 对象时，关于 IP 子网和子网掩码信息将自动翻译成网络前缀长度法格式<IP address>/<prefix length>。 例如，网络 IP 版本 4 (IPv4) 地址 172.16.4.0 子网掩码 255.255.252.0 显示为 172.16.4.0 月 22 日。 除了 IPv4 地址 Windows Server 2008 还支持 IP 版本 6 (IPv6) 子网前缀等 3FFE:FFFF:0:C000:: / 64。 在每个位置 IP 子网有关详细信息，请参阅中的"位置和子网"(DSSTOPO_2.doc) 工作表[收集网络信息](../../ad-ds/plan/Collecting-Network-Information.md)和[附录 a：位置和子网前缀](Appendix-A--Locations-and-Subnet-Prefixes.md)。  
  
关联与站点对象参考"关联的站点的子网"(DSSTOPO_6.doc) 工作表"决定该位置将变得站点"部分，以确定哪些子网在每个子网对象就是与的网站关联。 记录 Active Directory 子网对象关联的"相关联的站点的子网"(DSSTOPO_6.doc) 工作表中每个位置。  
  
有关如何创建子网对象的详细信息，请参阅创建一个子网 ([https://go.microsoft.com/fwlink/?LinkId=107068](https://go.microsoft.com/fwlink/?LinkId=107068))。  
  


