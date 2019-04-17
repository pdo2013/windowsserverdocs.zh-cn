---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: "广告 FS 配置数据库的作用"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3372e1f051ba7f900753a4961d948ddabdef6f4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-the-ad-fs-configuration-database"></a>广告 FS 配置数据库的作用
广告 FS 配置数据库存储表示 Active Directory 联合身份验证服务 \(AD FS\) \(that is, the Federation Service\) 的单个实例的配置数据。 广告 FS 配置数据库定义参数联合身份验证服务所需找出合作伙伴、证书，特性官方商城、索赔和有关这些关联实体各种数据的集。 你可以将此配置数据存储在 Microsoft SQL Server® 数据库或包含在 Windows Server 的 Windows 内部数据库 \(WID\) 功能® Windows Server 2008 R2 和 Windows Server 2008® 2012 年。  
  
> [!NOTE]  
> 广告 FS 配置数据库中的所有内容都可以存储的 WID 实例中或在实例 SQL 数据库中，但不是能同时。 这意味着你无法拥有使用 WID 和其他人使用的同一个实例广告 FS 配置数据库 SQL Server 数据库一些联合身份验证的服务器。  
  
你可以在本文中提供的内容以及使用以下信息[广告 FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)若要了解有关的选择 WID 或 SQL Server 存储广告 FS 配置数据库优缺点：  
  
WID 使用关系的数据存储和没有自行管理用户界面 \(UI\)。 而是管理员可以通过使用 snap\ 中广告 FS 管理、Fsconfig.exe 或 Windows PowerShell™ cmdlet 修改广告 FS 配置数据库中的内容。  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>使用 WID 存储广告 FS 配置数据库  
你可以创建广告 FS 配置数据库使用 WID 作为应用商店使用 Fsconfig.exe command\ 行工具或广告 FS 联盟服务器配置向导。 当您使用这些工具任一时，你可以选择以下选项来创建你的联合身份验证的服务器拓扑任一。 每个选项可用于存储广告 FS 配置数据库 WID:  
  
-   创建 stand\ 单独联盟服务器  
  
-   联合身份验证的服务器场中创建的第一个联盟服务器  
  
-   添加到联合身份验证的服务器场联合服务器  
  
如果您选择 stand\ 单独的选项，WID 用于存储广告 FS 配置数据库的单个实例。 不能在多个联合身份验证的服务器共享此实例。 它旨在的测试实验环境。 有关 stand\ 单独联合身份验证的服务器选项或如何将一个设置的详细信息，请参阅[独立联盟服务器使用 WID](https://technet.microsoft.com/library/gg982486.aspx)或[创建独立联合服务器](https://technet.microsoft.com/library/ee913579.aspx)。  
  
如果联盟服务器电场的日落选项中选择第一个联合身份验证的服务器时，可扩展性将允许其他联合身份验证的服务器，在以后添加到电场的日落，为配置 WID。 有关部署 WID 电场的日落或如何将一个设置的详细信息，请参阅[联合身份验证的服务器电场的日落使用 WID](https://technet.microsoft.com/library/gg982492.aspx)或[创建第一个联盟服务器联盟服务器电场的日落](https://technet.microsoft.com/library/dd807070.aspx)  
  
如果您选择添加联合身份验证的服务器选项，WID 配置为在设置的时间间隔复制到新联合身份验证的服务器的配置数据库更改。 有关添加到 WID 场联合服务器的详细信息，请参阅[联合身份验证的服务器电场的日落使用 WID](https://technet.microsoft.com/library/gg982492.aspx)或[添加到联合身份验证的服务器场联合服务器](https://technet.microsoft.com/library/ee913575.aspx)。  
  
> [!NOTE]  
> 使用 WID 联盟服务器场部署时，广告 FS 的某些功能可能不可用。 有权访问完整的功能集服务器场配置时，请考虑使用 Microsoft SQL Server 改用存储广告 FS 配置数据库。 有关详细信息，请参阅[广告 FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx)。  
  
### <a name="how-a-wid-federation-server-farm-works"></a>WID 联盟服务器农场里的工作原理  
本部分介绍重要介绍了如何 WID 联合身份验证的服务器场复制数据之间的主要联合身份验证的服务器和辅助联盟服务器的概念。 .  
  
#### <a name="primary-federation-server"></a>主要联合身份验证的服务器  
主要联盟服务器是一台运行 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 计算机® 2012 的已与广告 FS 联盟服务器配置向导联盟服务器角色中配置并且具有广告 FS 配置数据库读取/写入副本。 当你使用广告 FS 联盟服务器配置向导并选择的选项，可以创建新的联合身份验证服务并场中使该计算机的第一个联合身份验证的服务器时始终创建主要联合身份验证的服务器。 此电场的日落，也称为辅助联合身份验证的服务器，在所有其他联合身份验证的服务器必须同步到存储在本地广告 FS 配置数据库一份主要联盟服务器所做的更改。  
  
#### <a name="secondary-federation-servers"></a>辅助联合身份验证的服务器  
辅助联合身份验证的服务器存储一份广告 FS 配置数据库从主联合身份验证的服务器，但这些副本仅 read\。 辅助联合身份验证的服务器连接到，并通过定期检查是否已更改数据轮询它与电场的日落中的主要联合服务器同步的数据。 辅助联合身份验证的服务器存在以提供故障主要联合身份验证的服务器时充当在不同网络环境整个的站点中所做的 load\ 余额的访问权限请求。  
  
> [!NOTE]  
> 如果主要联盟服务器发生崩溃，并且处于脱机状态，所有辅助联合身份验证的服务器继续处理照常请求。 但是，直到主要联合身份验证的服务器再重新联机联合身份验证服务到所不做任何新的更改。 你还可以指定辅助联盟服务器通过使用 Windows PowerShell 成为主要联合身份验证的服务器。 有关详细信息，请参阅[与 Windows PowerShell 广告 FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
#### <a name="how-the-ad-fs-configuration-database-is-synchronized"></a>广告 FS 配置数据库同步的方式  
重要角色广告 FS 配置数据库播放，因为它均可在网络中的所有联盟服务器提供故障和 load\ 平衡功能处理请求时 \（当网络 load\ 平衡都 used\）。 但是，对于辅助联盟服务器执行该功能，必须同步主要联合身份验证的服务器存储的广告 FS 配置数据库。  
  
联合服务器添加电场的日落，将成为辅助联盟服务器在新计算机连接到主联盟服务器复制广告 FS 配置数据库的副本。 从该点前进新联盟服务器继续从定期，主要联盟服务器下载更新在下图所示。  
  
![广告 FS 角色](media/adfs2_WID.png)  
  
每个辅助联合身份验证的服务器轮询主要联盟服务器更改为每隔 5 分钟。 你可以调整此默认 five\ 分钟值或通过使用 Windows PowerShell cmdlet anytime 强制立刻同步。 有关如何执行此操作的详细信息，请参阅[与 Windows PowerShell 广告 FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
WID 同步过程也支持中间更改更高效的传输增量传输。 增量传输过程需要在网络上，已经提供了可观减少流量和换乘完成得更快。  
  
> [!NOTE]  
> 受支持的广告 FS 配置数据库迁移从 WID 到 SQL Server 实例。 有关如何执行此操作的详细信息，请参阅[广告 FS：将您的广告 FS 配置数据库迁移到 SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) TechNet Wiki 站点上。  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>使用 SQL Server 存储广告 FS 配置数据库  
你可以创建广告 FS 配置数据库使用 Fsconfig.exe command\ 行工具作为应用商店使用单个 SQL Server 数据库实例。 使用与广告 FS 配置数据库 SQL Server 数据库通过 WID 提供以下优势：  
  
-   管理员可以充分利用可用性的 SQL Server  
  
-   它提供其他性能增加高的交通。  
  
-   它提供 SAML 项目分辨率和 SAML/WS\-联盟令牌重播检测 \(described below\) 功能支持。  
  
术语"主联合服务器"不适用于时广告 FS 配置数据库存储中 SQL 数据库实例，因为所有联合身份验证的服务器可以同样读取和写入使用相同的群集的 SQL Server 实例广告 FS 配置数据库，在下图所示。  
  
![广告 FS 角色](media/adfs2_SQL.png)  
  
你可以使用 SQL Server 配置为服务器群集以确保由广告 FS 高度供服务传入的客户端请求一起协作的两个或多个服务器。 高可用性提供通过添加更多服务器可以在其中提高服务器容量 scale\ 出体系结构。 自动群集故障转移缓解有一点故障。  
  
你可以通过使用的网络 load\ 平衡和 SQL 群集技术提供的故障转移服务实现高发布。 有关如何 SQL Server 配置为高可用性的详细信息，请参阅[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)。  
  
### <a name="saml-artifact-resolution"></a>SAML 项目分辨率  
安全肯定标记语言 \(SAML\) 项目分辨率是基于部分介绍了如何信赖方可以直接从索赔提供商检索令牌 SAML 2.0 协议端点。 在分辨率进程的第一阶段，浏览器的客户端联系人资源联盟服务器，并提供的项目。 在第二个阶段，资源联盟服务器发送到托管某个地方帐户合作伙伴组织中以解决项目消息 SAML 项目端点 URL 的项目。 中的最后阶段，该帐户联合身份验证的服务器发出令牌代表的浏览器的客户端联合身份验证的服务器中。  
  
> [!NOTE]  
> 如果你是帐户的合作伙伴公司中的管理员，请确保分配或联盟被动网站中 IIS 绑定 SSL 证书，该链接到 Windows 根证书计划的成员根证书，\ (<ComputerName>\\Sites\\Default WebSite\\adfs\\ls\) 电场的日落中的所有帐户联盟服务器上。 这一点很重要防止资源联合身份验证的服务器，无需手动添加本地计算机受信任的人证书官方商城的 SSL 证书或无法解决的已发布你的组织中的项目。  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS-联盟令牌重播检测  
术语*令牌重播*指帐户合作伙伴组织中的浏览器客户端尝试向资源联盟服务器发送同一个标记它深受从帐户联合身份验证的服务器多次进行身份验证的行为。  用户单击时，会发生此法案**重新**努力重新提交身份验证页面他们的浏览器的按钮。  
  
广告 FS 提供被称为一项功能*标记重播检测*哪些多个令牌请求通过使用同一个标记可以检测并则被丢弃。 启用此功能后，令牌重播检测可通过确保永远不会多次使用同一标记保护完整性的身份验证 WS\ 联盟被动配置文件和 SAML WebSSO 配置文件中的请求。 应在的情况下安全高度关切如启用此功能在使用柜台时。  
  
在展台示例中，用户可以在所有网站登录，然后稍后恶意用户可以尝试重新提交前一的用户已加载的联合身份验证页面以使用浏览器历史记录。 此功能通过存储有关由以便检测令牌的后续重播和防止身份验证的多个尝试成功帐户合作伙伴公司每次成功进行身份验证的其他信息缓解了此问题。  
  

