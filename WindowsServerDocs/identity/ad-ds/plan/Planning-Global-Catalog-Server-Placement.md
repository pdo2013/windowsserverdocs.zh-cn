---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: "计划全球目录服务器位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c32393640e929adf9681f511ed3707b85a3784db
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="planning-global-catalog-server-placement"></a>计划全球目录服务器位置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

全球目录放置要求规划除，如果你有一个域森林。 在单个域树林中，将配置所有域控制器为全球的目录服务器。 每个域控制器存储林中仅域目录分区，因为配置为全球的目录服务器的每个域控制器不需要任何其他磁盘空间的使用情况、CPU 使用率或复制通信中。 在单个域树林中，所有域控制器都充当虚拟全球目录服务器;也就是说，他们可以所有响应身份验证或请求服务。 森林单域此特殊条件是设计。 身份验证请求不需要联系全球目录服务器时有多个域，并且用户可以是在不同的域中存在通用组成员的那样。 但是，在全球目录 3268 全球目录查询可以响应指定为全球的目录服务器的域控制器。 简化管理此方案中的并确保一致响应，将所有域控制器都指定为全球的目录服务器消除担心有关的域控制器可以响应全球目录查询的问题。 具体来说，随时的用户使用 Start\Search\For 人脉或查找打印机或展开通用组，这些请求转仅向全球目录。  
  
在多个域林全球目录服务器促进用户登录请求和树林搜索。 下图显示如何确定哪些位置需要全球目录服务器。  
  
![计划 gc 位置](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
在大多数情况下，建议你包括全球目录，当你安装新的域控制器。 以下例外均适用：  
  
-   有限的带宽：在远程的站点，如果宽的区域广域网连结远程网站和中心网站限制，可用于通用组成员缓存远程网站中适应登录需要的站点中的用户。  
  
-   基础结构操作主机角色不兼容所引起：不要承载基础操作主机角色域中的，除非在域中的所有域控制器将都是全球目录服务器或森林具有只有一个域域控制器上放置全球目录。  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>根据应用程序的要求添加全球目录服务器  
某些应用程序，如 Microsoft Exchange、消息队列 (也称为 MSMQ) 和应用程序使用 DCOM 不潜在 WAN 链接上提供足够的响应，并因此需要高可用全球目录基础结构提供查询低延迟。 确定是否位置或位置是否需要 Microsoft Exchange Server 中运行的任何应用程序通过 slow WAN 链接很好地运行。 如果你的位置中包含的应用程序不会通过 WAN 链接提供足够的响应，必须将全球目录服务器以减少查询延迟的位置。  
  
> [!NOTE]  
> Read-only 域控制器 (Rodc) 可以成功提升为全球的目录服务器状态。 但是，某些目录启用应用程序不能为全球的目录服务器支持 RODC。 例如，任何版本的 Microsoft Exchange Server 不使用 Rodc。 但是，Microsoft Exchange Server 适用的环境中包含，只要有可写的域控制器。 Exchange Server 2007 有效地忽略 Rodc。 Exchange Server 2003 还会忽略 Rodc 在默认情况 Exchange 组件自动检测可用的域控制器。 为 Exchange Server 2003 未不进行任何更改以了解只读目录服务器。 因此，尝试强制 Exchange Server 2003 服务和管理工具，用于 Rodc 可能会导致意外行为。  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>添加了大量的用户的全球目录服务器  
将全球目录服务器放在所有地区包含超过 100 用户以减少网络 WAN 链接拥挤并阻止 WAN 链接故障情形的工作效率丢失。  
  
## <a name="using-highly-available-bandwidth"></a>使用可用具有高带宽  
你不需要将全球目录放在一个位置不包括需要全球目录服务器的应用程序，包括少于 100 用户，并还通过连接到另一个位置，包括全球目录服务器 WAN 链接为 100%适用于 Active Directory 域服务 (广告 DS)。 在此情况下，用户可以 WAN 的链接上访问全球目录服务器。  
  
漫游用户需要联系全球目录服务器，无论他们登录的第一次的任意位置。 如果无法接受 WAN 的链接上登录时，将已访问过的大量漫游用户的位置放全球目录。  
  
## <a name="enabling-universal-group-membership-caching"></a>启用通用组成员缓存  
对于的位置，包括少于 100 用户和，不包括大量漫游用户或需要全球目录服务器的应用程序，可部署正在运行 Windows Server 2008 和启用通用组成员缓存的域控制器。 确保全球目录服务器不可从域控制器在其通用组成员缓存已启用，以便可以刷新通用组信息缓存中的多个复制跃点。 有关如何通用组缓存的工作原理的信息，请参阅全球目录的工作原理 ([https://go.microsoft.com/fwlink/?LinkId=107063](https://go.microsoft.com/fwlink/?LinkId=107063))。  
  
为了帮助您中记录你计划与通用组缓存启用放置全球目录服务器和域控制器工作表中，请参阅 Windows Server 2003 部署工具包 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开 (DSSTOPO_4.doc) 域控制器的位置。 查看有关你需要将全球目录服务器放部署森林根域和区域时的位置的信息。  
  


