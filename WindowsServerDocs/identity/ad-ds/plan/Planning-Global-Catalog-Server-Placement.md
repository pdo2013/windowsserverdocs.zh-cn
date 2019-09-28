---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: 规划全局编录服务器放置
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fb61d917300e957534a688b73efd7e193d24ff6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408743"
---
# <a name="planning-global-catalog-server-placement"></a>规划全局编录服务器放置

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果你有单域林，全局编录布局需要进行规划。 在单域林中，将所有域控制器配置为全局编录服务器。 由于每个域控制器都在林中存储唯一的域目录分区，因此将每个域控制器配置为全局编录服务器不需要任何额外的磁盘空间使用情况、CPU 使用率或复制流量。 在单域林中，所有域控制器都充当虚拟全局编录服务器;也就是说，它们都可以响应任何身份验证或服务请求。 单域林的这一特殊条件是设计使然。 身份验证请求不需要与全局编录服务器联系，因为在有多个域时，用户可以是位于其他域中的通用组的成员。 但是，只有指定为全局编录服务器的域控制器才能响应全局编录端口3268上的全局编录查询。 为了简化此方案中的管理并确保一致的响应，将所有域控制器指定为全局编录服务器，消除了对哪些域控制器可以响应全局编录查询的问题。 具体来说，无论用户何时使用 Start\Search\For 人员或查找打印机或展开通用组，这些请求仅会发送到全局编录。  
  
在多域林中，全局编录服务器有利于用户登录请求和林范围的搜索。 下图显示了如何确定哪些位置需要全局编录服务器。  
  
![规划 gc 定位](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
在大多数情况下，建议在安装新的域控制器时包括全局编录。 以下例外适用：  
  
- 有限带宽：在远程站点中，如果远程站点和中心站点之间的广域网（WAN）链接受到限制，则可以使用远程站点中的通用组成员身份缓存来满足站点中用户的登录需求。  
- 基础结构操作主机角色不兼容：不要在域控制器上放置全局编录，此域控制器将托管域中的基础结构操作主机角色，除非该域中的所有域控制器都是全局编录服务器，或者林只有一个域。  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>基于应用程序要求添加全局编录服务器

某些应用程序（例如 Microsoft Exchange、消息队列（也称为 MSMQ）和使用 DCOM 的应用程序）不能提供足够的响应来响应潜在的 WAN 链路，因而需要高度可用的全局目录基础结构来提供较低的查询延迟. 确定任何通过慢速 WAN 链接执行的应用程序是否在位置运行，或者是否需要 Microsoft Exchange Server。 如果你的位置包括无法通过 WAN 链接提供足够响应的应用程序，则必须在该位置放置全局编录服务器，以降低查询延迟。  
  
> [!NOTE]  
> 只读域控制器（Rodc）可以成功升级到全局编录服务器状态。 但是，某些启用目录的应用程序不能支持将 RODC 作为全局编录服务器。 例如，Microsoft Exchange Server 的任何版本都不使用 Rodc。 但是，只要有可写域控制器可用，Microsoft Exchange Server 就会在包含 Rodc 的环境中工作。 Exchange Server 2007 实际上会忽略 Rodc。 Exchange Server 2003 在默认情况下也会忽略 Rodc，这种情况下，Exchange 组件会自动检测可用的域控制器。 Exchange Server 2003 未进行任何更改，因此无法识别只读目录服务器。 因此，尝试强制 Exchange Server 2003 服务和管理工具使用 Rodc 可能导致不可预知的行为。  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>为大量用户添加全局编录服务器

将全局编录服务器放置在包含超过100个用户的所有位置，以减少网络 WAN 链接拥塞，并防止在发生 WAN 链接故障时出现生产力损失。  
  
## <a name="using-highly-available-bandwidth"></a>使用高可用带宽

不需要将全局编录放置在不包括需要全局编录服务器的应用程序的位置上，不能包含超过100个用户，还会通过 WAN 链接（即 100% a）连接到包含全局编录服务器的其他位置Active Directory 域服务（AD DS）的 v。 在这种情况下，用户可以通过 WAN 链接访问全局编录服务器。  
  
漫游用户在任何位置首次登录时都需要联系全局编录服务器。 如果 WAN 链路上的登录时间不可接受，请将一个全局编录置于大量漫游用户访问的位置。  
  
## <a name="enabling-universal-group-membership-caching"></a>启用通用组成员身份缓存

对于包含少于100用户并且不包含大量需要全局编录服务器的漫游用户或应用程序的位置，你可以部署运行 Windows Server 2008 的域控制器并启用通用组成员身份缓存. 确保全局编录服务器不是已启用通用组成员身份缓存的域控制器中的一个或多个复制跃点，以便能够刷新缓存中的通用组信息。 有关通用组缓存的工作原理的信息，请参阅 "[全局编录工作](https://go.microsoft.com/fwlink/?LinkId=107063)原理" 一文。  
  
要使工作表可以帮助你记录计划在何处放置全局编录服务器和域控制器启用通用组缓存的位置，请参阅[Windows Server 2003 部署工具包的作业帮助](https://go.microsoft.com/fwlink/?LinkID=102558)，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开域控制器布局（DSSTOPO_4）。 在部署目录林根级域和地区性域时，请参阅有关需要放置全局编录服务器的位置的信息。  
