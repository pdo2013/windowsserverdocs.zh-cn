---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: "计划区域域控制器位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c5254560e9b1a10030fa37ec0966868986212a4a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="planning-regional-domain-controller-placement"></a>计划区域域控制器位置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

若要确保成本效益，计划尽可能放置少的区域域控制器。 首先，检查"地理位置和通信的链接"(DSSTOPO_1.doc) 工作表中使用[收集网络信息](../../ad-ds/plan/Collecting-Network-Information.md)以确定是否中心位置。  
  
若要放置区域域控制器，以便在每个中心位置表示每个域的套餐。 你将区域域控制器在所有中心位置后，评估需要进行卫星位置中放置区域域控制器。 取消从卫星位置的不必要的区域域控制器减少维护远程服务器基础结构需支持成本。  
  
此外，确保域控制器在中心和卫星位置的物理安全性，以便未经授权的人员将无法对其进行访问。 不将写的域控制器放置在中心和卫星在其中不能保证域控制器物理安全的位置。 具有物理可写的域控制器访问的人员可以攻击的系统：  
  
-   通过从另一操作系统开始域控制器上访问物理磁盘。  
  
-   删除（和可能会替换）物理磁盘域控制器上。  
  
-   获取并处理的域控制器系统状态备份副本。  
  
添加的位置，则可以在其中保证他们物理安全于仅可写的区域域控制器。  
  
在使用不适当的物理安全的地点部署只读域控制器 (RODC) 是建议的解决方案。 帐户密码除外 RODC 保存的 Active Directory 对象和可写的域控制器拥有的属性。 但是，不能对存储在 RODC 数据库进行更改。 更改必须是可写的域控制器上进行并且再将其复制回 RODC。  
  
若要客户端登录和访问本地文件服务器的身份验证，大多数企业，请将区域域控制器，以便在给定位置表示所有区域域。 但是，你必须评估是否业务位置要求其客户端具有本地身份验证或客户可以依赖身份验证和查询宽的区域广域网链接上时考虑许多变量。 下图显示如何确定是否将域控制器放置在卫星位置。  
  
![套餐区域 dc 位置](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>现场专业技术可用性  
域控制器需要持续管理出于各种原因。 仅将区域域控制器放包含可以管理域控制器，或者是确保可以远程管理域控制器的人员的位置。  
  
在通常差物理安全分支机构环境和人员小信息技术知道，部署 RODC 通常是建议的解决方案。 本地 RODC 管理权限可以委派给任何域用户中，而无需授予该用户的域或其他域控制器任何用户权限。 这样本地 branch 用户在登录到 RODC 并在服务器上，如升级驱动程序中执行维护工作。 但是，branch 用户无法登录到任何其他域控制器上或在域中执行任何其他的管理任务。 在这样一来，branch 用户可以委派有效地而不会影响的域中，还是森林其余部分的安全管理 RODC 分支办公室中的能力。  
  
## <a name="wan-link-availability"></a>WAN 链接可用性  
如果位置中不包括可以验证用户域控制器，wan 体验频繁中断会用户导致重大工作效率丢失。 如果 WAN 链接可用性不是百分之百远程站点无法容忍服务中断，将在其中的用户需要能够登录，或按下 WAN 链接时交换服务器访问的位置放区域域控制器。  
  
## <a name="authentication-availability"></a>身份验证可用性  
某些企业，例如银行，需要用户在任何时候进行身份验证。 在一个位置 WAN 链接可用性不是百分之百放置区域域控制器，但用户需要随时身份验证。  
  
## <a name="logon-performance-over-wan-links"></a>通过 WAN 链接登录性能  
非常可靠 WAN 链接可用性是否放置在位置域控制器取决于登录性能要求通过 WAN 的链接。 通过 WAN 影响登录性能的因素包括链接速度和提供带宽，数字的用户和使用情况配置文件，以及与复制流量登录网络流量。  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>WAN 链接速度和带宽利用率  
活动的单个用户可以 congest slow WAN 链接。 WAN 的链接上登录性能是否接受将域控制器放在一个位置。  
  
平均带宽利用率百分比表示如何拥堵网络链接。 如果网络链接具有大于接受值的平均带宽使用，请在该位置位置域控制器。  
  
### <a name="number-of-users-and-usage-profiles"></a>用户的数量以及使用情况配置文件  
有助于确定您是否需要将在该位置的区域域控制器数用户和他们使用的配置文件中给定的位置。 若要避免生产力丢失，如果 WAN 链接无法正常工作，将已 100 或多个用户的位置放区域域控制器。  
  
使用个人资料表示用户如何使用网络资源。 你不需要将域控制器放置包含仅少数用户不经常访问网络资源的位置。  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>与复制流量登录网络通信  
域控制器在中不可用的 Active Directory 客户端的同一个位置，如果客户端将创建网络上的登录的交通。 创建物理网络的登录网络流量影响通过几种因素的影响，包括组成员;组策略对象 (Gpo;) 的数目和大小登录脚本;和功能，例如离线状态下的文件夹，文件夹重定向漫游配置文件。  
  
另一方面，域控制器在给定的位置放生成网络上的复制通信。 频率和的更新进行了域控制器上托管分区上数量来影响的创建网络的复制流量。 添加或更改用户与用户属性、更改密码，和添加或更改全局组、打印机或卷，包括不同类型的更新可域控制器上托管分区上进行。  
  
若要确定你需要将区域域控制器放在一个位置，比较创建的创建者放置在位置域控制器复制流量没有域控制器成本与的位置登录流量费用。  
  
例如，请考虑有分支机构通过 slow 链接到总部和可以轻松地添加哪些域控制器在连接的网络。 如果的一些站点远程用户的每日登录和目录查找通讯导致多个网络流量比复制和 branch 的公司的所有数据，请考虑添加和 branch 的域控制器。  
  
如果网络流量比更重要降低成本维护域控制器，域控制器中的将集中和没有将在位置的任何区域域控制器或考虑将 Rodc 放置在位置。  
  
以帮助你记录的区域域控制器和每个便会出现在每个位置的域的用户数位置工作表中，请参阅任务指导，以便进行部署 Windows Server 2003 工具包 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开"域控制器的位置"(DSSTOPO_4.doc)。  
  
你将需要你需要将区域域控制器放部署区域时的位置信息，请参考。 有关部署区域的域的详细信息，请参阅[部署 Windows Server 2008 区域域](https://technet.microsoft.com/library/cc755118.aspx)。  
  


