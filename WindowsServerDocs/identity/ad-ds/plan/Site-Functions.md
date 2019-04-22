---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: 站点函数
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a330a9dae8ab8d3b9de5d1fff0e52060908f520
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815808"
---
# <a name="site-functions"></a>站点函数

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

 Windows Server 2008 用于多种用途，包括复制路由、 客户端关联、 系统卷 (SYSVOL) 复制、 分布式文件系统命名空间 (DFSN) 和服务定位站点信息。  
  
## <a name="routing-replication"></a>复制路由  
Active Directory 域服务 (AD DS) 使用多主机、 存储和转发的复制方法。 域控制器进行通信，目录更改为第二个域控制器，传递到第三个，依此类推，直至所有域控制器已都接收到更改。 若要实现减少复制滞后时间和减少流量之间的最佳平衡，站点拓扑时，可控制 Active Directory 复制通过在站点内进行的站点间发生复制区别开来。  
  
在站点内，复制进行了优化的速度、 数据更新触发器复制和将数据发送无需使用所需的数据压缩。 相反，若要通过广域网 (wan) 链接传输的成本降至最低，压缩进行站点之间的复制。 站点之间进行复制时，每个域在每个站点的单个域控制器收集并将目录更改存储和通信在计划的时间到另一个站点中的域控制器。  
  
## <a name="client-affinity"></a>客户端相关性  
域控制器使用的站点信息以通知有关与客户端最近的站点中存在的域控制器的 Active Directory 客户端。 例如，考虑不知道其站点隶属关系，并联系亚特兰大站点中的域控制器的西雅图站点中的客户端。 在亚特兰大的域控制器基于客户端的 IP 地址，确定哪个站点确实是由客户端，并将站点信息发送回客户端。 域控制器还将告知客户端选择的域控制器是否是向其最近的一个。 客户端缓存提供亚特兰大，特定于站点的服务 (SRV) 资源记录 （用于查找适用于 AD DS 域控制器域名系统 (DNS) 资源记录） 的查询中的域控制器的站点信息并从而查找域同一站点内的控制器。  
  
通过在同一站点中查找域控制器，客户端可以通信避免通过 WAN 链接。 如果没有域控制器位于客户端站点，相对于其他连接的站点具有最低的成本连接的域控制器将公布自己 （在 DNS 中注册的特定于站点的服务 (SRV) 资源记录） 不具有在站点中域控制器。 在 DNS 中发布的域控制器是从最近的站点定义的站点拓扑。 此过程可确保，每个站点都有身份验证的首选的域控制器。  
  
查找域控制器的过程的详细信息，请参阅 Active Directory 集合 ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626))。  
  
## <a name="sysvol-replication"></a>SYSVOL 复制  
SYSVOL 是在域中每个域控制器存在的文件系统中文件夹的集合。 SYSVOL 文件夹提供了在整个域，包括组策略对象 (Gpo)、 启动和关闭脚本，以及登录和注销脚本，必须将复制的文件的默认 Active Directory 位置。  Windows Server 2008 可以使用文件复制服务 (FRS) 或分布式文件系统复制 (DFSR) 复制到 SYSVOL 文件夹从一个域控制器到其他域控制器所做的更改。 FRS 和 DFSR 复制根据在您的站点拓扑设计过程中创建的计划这些更改。  
  
## <a name="dfsn"></a>DFSN  
DFSN 使用站点信息来定向到承载站点内的请求的数据的服务器的客户端。 如果 DFSN 没有找到与客户端在同一站点中的数据的副本，DFSN 使用 AD DS 以确定哪些文件服务器并包含 DFSN 共享数据中的站点信息是靠近客户端。  
  
## <a name="service-location"></a>服务位置  
通过在 AD DS 中发布服务，如文件和打印服务，你可以允许 Active Directory 客户端找到请求的服务在同一或最近的站点。 打印服务使用存储在 AD DS 中的位置属性以使用户可以浏览的位置的打印机，而不必知道其精确位置。 有关设计和部署打印服务器的详细信息，请参阅设计和部署打印服务器 ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041))。  
  


