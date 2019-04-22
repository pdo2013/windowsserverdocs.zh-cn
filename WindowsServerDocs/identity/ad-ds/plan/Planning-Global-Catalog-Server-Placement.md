---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: 规划全局编录服务器放置
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aefed1a2ed0d346f75ad4d135f8c0ae314fd7fc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820678"
---
# <a name="planning-global-catalog-server-placement"></a>规划全局编录服务器放置

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

全局编录放置，需要规划除外，如果你具有的单域林。 在单域林中，所有域控制器都配置为全局编录服务器。 因为每个域控制器的林中存储唯一的域目录分区，每个域控制器配置为全局编录服务器不需要任何额外的磁盘空间使用情况、 CPU 使用情况或复制流量中。 在单域林中，所有域控制器都充当虚拟全局编录服务器;也就是说，它们可以所有响应任何身份验证或服务请求。 此特殊条件适用于单一域林是默认设置。 身份验证请求不需要全局编录服务器联系存在多个域，并且用户可以在不同的域中存在一个通用组的成员时一样。 但是，只有指定为全局编录服务器的域控制器可以响应全局编录端口 3268 上的全局编录查询。 若要简化在此方案中的管理并确保一致的响应，并将所有域控制器都指定为全局编录服务器消除了有关哪些域控制器可以响应全局编录查询的问题。 具体来说，只要用户使用 Start\Search\For 人或查找打印机，或者扩展通用组，这些请求都仅针对全局编录。  
  
在多域林中，全局编录服务器，促进用户登录请求和全林性搜索。 下图显示如何确定哪些位置要求全局编录服务器。  
  
![规划 gc 的位置](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
在大多数情况下，建议安装新域控制器时包括全局编录。 存在以下例外：  
  
- 有限的带宽：在远程站点中，如果远程站点与中心站点之间广域网 (WAN) 链接是有限的您可以使用通用组成员身份缓存在远程站点中以适应站点中的用户的登录需求。  
- 基础结构操作主机角色不兼容：不主机基础结构操作主机域中的角色，除非域中的所有域控制器都是全局编录服务器或林仅有一个域的域控制器上放置全局编录。  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>添加基于应用程序要求的全局编录服务器

某些应用程序，如 Microsoft Exchange、 消息队列 (也称为 MSMQ) 和使用 DCOM 应用程序执行操作未导致的延迟的 WAN 链接来提供足够的响应，因此需要高度可用的全局编录基础结构提供低的查询滞后时间。 确定是否在执行效果不佳通过慢速 WAN 链接的任何应用程序运行位置或位置是否需要 Microsoft Exchange Server 中。 如果你的位置包括通过 WAN 链接不提供足够的响应的应用程序，必须将全局编录服务器放置在要缩短查询延迟的位置。  
  
> [!NOTE]  
> 只读域控制器 (Rodc) 可以成功提升为全局编录服务器状态。 但是，某些已启用目录的应用程序不能为全局编录服务器支持 RODC。 例如，任何版本的 Microsoft Exchange Server 不使用 Rodc。 但是，Microsoft Exchange Server 在包括 Rodc 的环境中运行，只要有可写域控制器可用。 Exchange Server 2007 有效地将忽略 Rodc。 Exchange Server 2003 还会忽略在 Exchange 组件自动检测可用的域控制器的默认条件中的 Rodc。 到 Exchange Server 2003 不进行了任何更改以使其了解的只读目录服务器。 因此，尝试强制 Exchange Server 2003 服务和管理工具来使用 Rodc 可能会导致不可预知的行为。  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>添加大量用户的全局编录服务器

将全局编录服务器放置在包含超过 100 个用户来减少交通拥塞的网络的 WAN 链接，并防止故障 WAN 链接的工作效率丢失的所有位置。  
  
## <a name="using-highly-available-bandwidth"></a>使用高度可用的带宽

不需要在不包括需要全局编录服务器的应用程序，包括少于 100 个用户，并且还通过为 100%的 WAN 链接连接到另一个位置中包含的全局编录服务器的位置放置全局编录Active Directory 域服务 (AD DS) 版提供。 在这种情况下，用户可以通过 WAN 链接访问全局编录服务器。  
  
漫游用户需要联系全局编录服务器，无论他们登录首次在任何位置。 如果通过 WAN 链接的登录时间不可接受，将在很多漫游用户访问的位置放置全局编录。  
  
## <a name="enabling-universal-group-membership-caching"></a>启用通用组成员身份缓存

对于包含少于 100 个用户且不包含大量的漫游用户或需要全局编录服务器的应用程序的位置，你可以部署域控制器都运行 Windows Server 2008 并启用通用组成员身份缓存。 请确保全局编录服务器不是从域控制器的通用组成员身份缓存启用，以便可以刷新缓存中的通用组信息的多个复制跃点。 有关如何通用组缓存工作原理的信息，请参阅文章[全局目录的工作原理](https://go.microsoft.com/fwlink/?LinkId=107063)。  
  
有关可帮助您在记录你打算将全局编录服务器和域控制器放置且已启用通用组缓存的工作表，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开域控制器放置 (DSSTOPO_4.doc)。 请参阅有关需要在部署目录林根域和地区域时放置全局编录服务器的位置的信息。  
