---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: "规划联合身份验证的服务器容量"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 618dc9419be965dedaaf7dc946da436a5001f121
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-capacity"></a>规划联合身份验证的服务器容量

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

容量联合身份验证的服务器规划可帮助你评估：  
  
-   哪些因素增长广告 FS 配置数据库的大小。  
  
-   每个联盟服务器的相应的硬件要求。  
  
-   联合身份验证的服务器进入每一个组织数。  
  
联合身份验证的服务器颁发给用户安全标记。 这些标记显示给信赖消耗方。 联合身份验证的服务器发出安全令牌验证的用户或后接收安全令牌之前颁发的合作伙伴联合身份验证服务。 当用户最初登录到的联合应用或其安全令牌到期时它们所访问的联合的应用从联合身份验证服务请求时安全标记。  
  
联合身份验证的服务器旨在容纳使用 Microsoft 网络负载平衡 \(NLB\) 技术 high\ 可用性服务器电场的日落配置。 在场配置联盟服务器可以请求提供服务独立，而无需访问为每个请求任何常见场组件。 因此，较小的开销采用出联合身份验证的服务器部署缩放。  
  
**建议：**  
  
-   对于 mission\ 关键或 high\ 可用性部署，我们建议你在每个至少两个联盟服务器每电场的日落，以提供故障的合作伙伴组织中创建的小联盟服务器场。  
  
-   使用高可用性和缩放出联合身份验证的服务器轻松需，出缩放推荐的方法是用于处理次数过多每秒特定联合身份验证服务请求。 本指南中的基本配置超出缩放不太可能产生显著容量处理提升。  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>广告 FS 配置数据库大小和增长  
广告 FS 配置数据库大小通常视为较小、和数据库大小不倾向于在广告 FS 部署关注。  广告 FS 配置数据库精确大小可以取决很大程度上信任关系和关联的 trust\ 相关的元数据的数量，索赔，如声称规则，并且监视配置为每个信任的设置。 随着信任配置数据库中的条目大量也如此需要更多的磁盘空间。  
  
有关广告 FS 配置数据库其他部署信息，请参阅[广告 FS 部署拓扑注意事项](AD-FS-Deployment-Topology-Considerations.md)。  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>内存，CPU 和磁盘空间要求  
幸运的是，联合身份验证的服务器的内存、CPU 和磁盘空间要求适中，并且不是可能是硬件决策驾车因素。 硬件要求的详细信息，请参阅[附录 a：查看广告 FS 要求](Appendix-A--Reviewing-AD-FS-Requirements.md)。  
  
> [!NOTE]  
> 在已由使用联盟服务器场通过专门的 SQL Server 配置存储广告 FS 配置数据库广告 FS 产品团队的测试，SQL Server 上的全部负载往往较低。 使用已配置为使用一个的 SQL Server four\ federation\ 服务器场一个测试，请 CPU 使用率没有超过 10%，尽管测试的目标利用率定联合身份验证的服务器。  
  
## <a name="bk_estimatefs"></a>估计数联合针对你的组织的服务器  
在努力优化计划联合身份验证的服务器的过程的硬件，广告 FS 产品团队开发的广告 FS 容量规划大小电子表格。 该 Excel 电子表格包括 calculator\ 类似的功能，将耗时预期的使用情况数据你提供有关用户在你的组织中，并为您的广告 FS 生产环境返回推荐的最佳联合身份验证的服务器数。  
  
> [!NOTE]  
> 此电子表格将推荐的联合服务器大量取决于广告 FS 产品团队测试过程中使用的硬件和网络规范。 因此，必须在此环境中理解的电子表格将推荐的联合服务器数。  在测试期间使用规格有关详细信息，请参阅标题为主题[规划广告 FS 服务器容量](Planning-for-AD-FS-Server-Capacity.md)。  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>使用广告 FS 容量计划大小电子表格  
当你使用该电子表格时，你将需要选择一个值 \ (任一**40%**，**60%**，或**80%**\) 最佳表示你所期望的总用户的百分比将身份验证请求发送至您联合身份验证的服务器在高峰使用时段。  
  
然后，你将需要选择一个值 \ (任一**1 分钟**，**15 分钟**，或**1 小时**\) 最佳表示预计持续的高峰使用时段的时间长度。 例如，你可能会总数谁会在 15 分钟的时间段内登录或 60%的用户将在一段 1 小时内登录的用户的值为估计 40%。 在一起，这些值定义大小建议将计算的山峰加载配置文件。  
  
接下来，你将需要指定的总需要基于用户是否的目标 claims\ 意识到应用的单个 sign\ 上访问的用户数：  
  
-   从物理连接到你的企业网络本地计算机登录到 Active Directory \（通过 Windows 的集成 authentication\)  
  
-   从计算机中并非物理连接到你的企业网络远程登录到 Active Directory \（通过 Windows 集成身份验证或用户名和 password\）  
  
-   从另一台组织，并且尝试从值得信赖的合作伙伴访问目标 claims\ 感知应用程序  
  
-   从 SAML 2.0 身份提供，并且尝试访问目标 claims\ 感知应用程序  
  
#### <a name="how-to-use-this-spreadsheet"></a>如何使用此电子表格  
每个联盟服务器电场的日落实例计划部署以确定建议联合身份验证的服务器的数量，你可以使用下面的步骤。  
  
1.  下载，然后打开[广告 FS 容量规划大小电子表格的 Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)或[广告 FS 容量规划大小电子表格的 Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
  
2.  中的单元格的右侧**期间峰值系统使用情况，会发生此我的用户进行身份验证的百分比**单元格、单击的单元格，然后使用的 drop\ 向下箭头，选择你预计的系统利用率级别，则**40%**，**60%**或**80%**以便进行部署。  
  
3.  右侧格中**在以下时间内**单元格、单击的单元格，然后使用 drop\ 向下箭头选择**1 分钟**，**15 分钟**，或**1 小时**选择峰值加载的持续时间。  
  
4.  右侧格中**Enter 估计多种内部应用 \ (如 SharePoint \(2007 or 2010\) 声称注意到 web applications\ 或)**单元格、键入内部应用程序，您将使用你的组织中的数。  
  
5.  右侧格中**Enter 估计多种在线应用 \（如 Office 365 Exchange Online、SharePoint Online 或 Lync Online\）**单元格、键入在线应用程序或服务，您将使用你的组织中的数。  
  
6.  下的单元格标题为**用户的数量**，将需要针对你的用户适用于示例应用场景每行数单 sign\ 上的访问权限类型。 此列应包含定义用户，而非每秒峰值用户的数量。 如果对应用程序访问尝试首先必须完成家庭领域发现页面，请键入**Y**。如果你不确定此选项，请键入**Y**。  
  
7.  查看以下推荐提供的值：  
  
    1.  总数量推荐联合身份验证的服务器，请参阅突出显示灰色下方右单元格。  
  
    2.  对于每个示例应用场景，建议服务器的数量，请参阅突出显示灰色行的单元格。  
  
> [!NOTE]  
> 将自动计算到单元格标题为右格中值**总数联盟服务器建议**底部的电子表格包含公式这将在每个单独的行其前面的所有值的总计添加额外的 20%缓冲区。 添加到使用公式**总数联盟服务器建议**版本此缓冲区入部署的联盟服务器以使其不太可能场上的整体的加载是否将触及其饱和度点数，建议你总中的单元格。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
