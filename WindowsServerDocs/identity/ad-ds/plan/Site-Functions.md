---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: "站点的功能"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f40122d84185c69fa19f5158c2b1c370ec1bfe82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="site-functions"></a>站点的功能

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

 Windows Server 2008 出于许多，包括路由复制、 客户端的相关性、 系统卷 (SYSVOL) 复制、 分布式文件系统命名空间 (DFSN) 和位置服务使用网站的信息。  
  
## <a name="routing-replication"></a>路由复制  
Active Directory 域服务 (广告 DS) 使用复制的主机、 官方商城前进方法。 域控制器通信目录到第二个的域控制器，然后进行通信到第三个，等，直到所有域控制器都收到更改的更改。 若要获得减少复制延迟和减少流量之间的最佳余额，站点拓扑通过将站点内进行的复制发生的站点之间的区别开来控制 Active Directory 复制。  
  
站点在复制优化对于 speeddata 更新触发器复制，，并发送无需开销所需的数据压缩的数据。 相反的站点之间复制压缩大限度地减少通过宽的区域广域网链接传输成本。 复制时的站点之间，每个各个站点的域的单个域控制器收集和存储目录更改和通信它们在计划的时间向其他站点中的域控制器。  
  
## <a name="client-affinity"></a>客户端相关性  
域控制器使用网站信息有关域控制器在最近的站点，为客户端存在通知 Active Directory 客户端。 例如，请考虑西雅图站点，不知道其网站隶属和联系人从亚特兰大站点域控制器中的客户端。 根据客户端的 IP 地址，域控制器在亚特兰大确定哪些客户实际上是从，并发送给客户的网站的信息的站点。 此外，域控制器会通知客户端选的域控制器是否是与其最近的一个。 客户端缓存站点信息提供的域控制器在亚特兰大、 查询站点特定服务 (SRV) 资源记录 （用于为 DS 广告定位域控制器域名系统 (DNS) 资源记录），并因此同一站点中发现域控制器。  
  
通过在同一站点查找域控制器，客户端将通过 WAN 链接避免通信。 如果没有域控制器在客户端站点所在的位置，域控制器相对于其他连接网站具有最低成本连接将公布 （在 DNS 注册站点特定服务 (SRV) 资源记录） 自己中没有域控制器该站点。 将发布在 DNS 域控制器是从最近的站点规定网站拓扑。 此过程确保每个站点具有身份验证的首选的域控制器。  
  
查找域控制器的进程的详细信息，请参阅 Active Directory 集锦 ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626))。  
  
## <a name="sysvol-replication"></a>SYSVOL 复制  
SYSVOL 处于存在域中的每个该域控制器的文件系统的文件夹的集锦。 SYSVOL 文件夹提供必须在某个域，包括 (Gpo) 的组策略对象、 启动和关闭脚本和登录和注销脚本复制的文件的默认 Active Directory 位置。  Windows Server 2008 可以使用的文件复制服务 (FRS) 或分布式文件系统复制 (DFSR) 复制到其他域控制器一个域控制器 SYSVOL 文件夹所作的更改。 FRS 和 DFSR 复制根据你的网站拓扑设计过程中创建的日程安排这些更改。  
  
## <a name="dfsn"></a>DFSN  
DFSN 使用站点信息来直接客户端，服务器所在在站点请求的数据。 如果 DFSN 找不到的数据副本内与客户端相同的网站，DFSN 使用站点广告 DS 以确定哪些文件服务器具有 DFSN 共享数据以信息是最接近客户端。  
  
## <a name="service-location"></a>位置服务  
通过在广告 DS 发布服务，如文件和打印的服务，您允许 Active Directory 客户端定位在同一或最近的网站请求的服务。 打印服务使用存储在广告 DS 位置属性让用户不知道其精确位置浏览的位置的打印机。 有关设计和部署打印服务器的详细信息，请参阅设计和部署打印服务器 ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041))。  
  


