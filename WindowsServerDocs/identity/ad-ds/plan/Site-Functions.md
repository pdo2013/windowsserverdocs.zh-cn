---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: 站点函数
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 109f576bfdacf68a0eadc7dd84ddb9a4148e6dd9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408674"
---
# <a name="site-functions"></a>站点函数

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 Windows Server 2008 出于多种目的使用站点信息，包括路由复制、客户端关联、系统卷（SYSVOL）复制、分布式文件系统命名空间（DFSN）和服务位置。  
  
## <a name="routing-replication"></a>路由复制  
Active Directory 域服务（AD DS）使用一种多主机的存储和转发的复制方法。 域控制器将目录更改与第二个域控制器进行通信，后者随后会与第三个域控制器通信，依此类推，直到所有域控制器都接收到更改为止。 为了实现降低复制延迟和减少流量之间的最佳平衡，站点拓扑通过区分站点内发生的复制与站点之间发生的复制，来控制 Active Directory 复制。  
  
在站点中，复制经过优化，可实现速度、数据更新触发复制，而无需数据压缩就会发送数据。 相反，站点间的复制会被压缩，以最大程度地降低通过广域网（WAN）链路传输的成本。 当站点之间发生复制时，每个站点中的单个域控制器将收集并存储目录更改，并在计划的时间将其传递到另一个站点中的域控制器。  
  
## <a name="client-affinity"></a>客户端相关性  
域控制器使用站点信息向 Active Directory 的客户端通知有关最近站点中存在的域控制器的客户端。 例如，请考虑西雅图站点中的客户端，该客户端不知道其站点从属关系，并与亚特兰大站点的域控制器联系。 根据客户端的 IP 地址，亚特兰大中的域控制器确定客户端实际来自哪个站点，并将站点信息发送回客户端。 域控制器还向客户端通知所选的域控制器是否与最接近的域控制器。 客户端缓存亚特兰大域控制器提供的站点信息，对站点特定服务（SRV）资源记录（用于查找 AD DS 的域控制器的域名系统（DNS）资源记录）的查询，从而查找域同一站点中的控制器。  
  
通过在同一站点中查找域控制器，客户端可避免通过 WAN 链接进行通信。 如果客户端站点上没有域控制器，则相对于其他连接站点的成本最低的连接的域控制器将播发自己（在 DNS 中注册站点特定的服务（SRV）资源记录）域控制器。 在 DNS 中发布的域控制器是指由站点拓扑定义的最近站点中的域控制器。 此过程可确保每个站点都有一个用于身份验证的首选域控制器。  
  
有关查找域控制器的过程的详细信息，请参阅 Active Directory 集合（[https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)）。  
  
## <a name="sysvol-replication"></a>SYSVOL 复制  
SYSVOL 是在域中的每个域控制器上存在的文件系统中的文件夹的集合。 SYSVOL 文件夹为必须在整个域中复制的文件提供默认 Active Directory 位置，包括组策略对象（Gpo）、启动和关闭脚本以及登录和注销脚本。  Windows Server 2008 可以使用文件复制服务（FRS）或分布式文件系统复制（DFSR）将 SYSVOL 文件夹所做的更改从一个域控制器复制到其他域控制器。 FRS 和 DFSR 会根据你在站点拓扑设计期间创建的计划来复制这些更改。  
  
## <a name="dfsn"></a>DFSN  
DFSN 使用站点信息将客户端定向到在站点中承载请求的数据的服务器。 如果 DFSN 在客户端所在的站点中找不到数据的副本，则 DFSN 将使用 AD DS 中的站点信息来确定具有 DFSN 共享数据的文件服务器最近的客户端。  
  
## <a name="service-location"></a>服务位置  
通过在 AD DS 中发布文件和打印服务等服务，你允许 Active Directory 客户端在相同或最近的站点中查找请求的服务。 打印服务使用存储在 AD DS 中的位置属性，使用户无需知道其确切位置即可按位置浏览打印机。 有关设计和部署打印服务器的详细信息，请参阅设计和部署打印服务器（[https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)）。  
  


