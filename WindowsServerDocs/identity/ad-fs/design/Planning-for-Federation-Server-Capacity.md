---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: 联合服务器容量规划
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 418bc5d53a2bd11afa8563b07bbff76c89495715
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407971"
---
# <a name="planning-for-federation-server-capacity"></a>联合服务器容量规划

联合服务器的容量规划有助于您估算：  
  
-   哪些因素会增加 AD FS 配置数据库的大小。  
  
-   每个联合服务器的相应硬件要求。  
  
-   要放在每个组织中的联合服务器的数目。  
  
联合服务器向用户颁发安全令牌。 将这些令牌提供给信赖方以供使用。 联合服务器在对用户进行身份验证之后或收到先前由伙伴联合身份验证服务颁发的安全令牌后颁发安全令牌。 当用户首次登录到联合应用程序时，或者当用户访问联合应用程序的安全令牌过期时，将从联合身份验证服务请求安全令牌。  
  
联合服务器旨在容纳使用 Microsoft 网络负载平衡 \(NLB @ no__t 技术的高 @ no__t-0availability 服务器场配置。 场配置中的联合服务器可以独立处理请求，而无需访问每个请求的任何公用场组件。 因此，横向扩展联合服务器部署不会产生很大的开销。  
  
**针对**  
  
-   对于任务 @ no__t-0critical 或高级 @ no__t 1availability 部署，我们建议你在每个伙伴组织中创建一个小型联合服务器场，每个场至少有两个联合服务器，以提供容错能力。  
  
-   由于需要高可用性和轻松扩展联合服务器，因此建议使用外扩方法来处理特定联合身份验证服务每秒处理的大量请求。 在本指南中，超过基础配置的扩展不太可能产生大量的容量处理收益。  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>AD FS 配置数据库大小和增长  
AD FS 配置数据库的大小通常被视为较小，数据库大小往往不是 AD FS 部署中的主要考虑因素。  AD FS 配置数据库的准确大小在很大程度上取决于信任关系的数量和关联的 no__t 0related 元数据，例如声明、声明规则和为每个信任配置的监视设置。 随着配置数据库中的信任项数增加，需要更多的磁盘空间。  
  
有关 AD FS 配置数据库的其他部署信息，请参阅[AD FS 部署拓扑注意事项](AD-FS-Deployment-Topology-Considerations.md)。  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>内存、CPU 和磁盘空间要求  
幸运的是，联合服务器的内存、CPU 和磁盘空间需求适中，它们不太可能是硬件决策的驱动因素。 有关硬件要求的详细信息，请参阅 [Appendix A：查看 AD FS 要求 @ no__t。  
  
> [!NOTE]  
> 在由 AD FS 产品团队执行的测试中，使用配置有专用 SQL Server 的联合服务器场来存储 AD FS 配置数据库，SQL Server 倾向上的总体负载为 low。 在一次使用配置为使用单个 SQL Server 的四个 @ no__t-0federation @ no__t-1server 场进行测试的情况下，尽管将联合服务器置于目标利用率的测试，CPU 使用率不会超过 10%。  
  
## <a name="bk_estimatefs"></a>估计组织的联合服务器的数目  
为简化联合服务器的硬件规划过程，AD FS 产品团队开发了 AD FS 容量规划大小调整电子表格。 此 Excel 电子表格包括计算器 @ no__t-0like 功能，该功能将采用你为组织中的用户提供的预期使用情况数据，并为你的 AD FS 生产环境返回推荐的最佳数量的联合服务器。  
  
> [!NOTE]  
> 此电子表格将推荐的联合服务器数基于 AD FS 产品团队在测试期间使用的硬件和网络规格。 因此，必须在此上下文中理解电子表格将推荐的联合服务器的数目。  有关测试过程中使用的规范的详细信息，请参阅主题：[规划 AD FS 服务器容量](Planning-for-AD-FS-Server-Capacity.md)。  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>使用 AD FS 容量规划大小调整电子表格  
如果使用此电子表格，你将需要选择一个值 \(either **40%** 、 **60%** 或**80%** \)，它最能表示你预计在此期间将身份验证请求发送到联合服务器的总用户的百分比高峰使用期。  
  
然后，需要选择一个值 \(either **1 分钟**、 **15 分钟**或**1 小时**\)，它最能表示预期的高峰使用周期的持续时间。 例如，你可以将 40% 作为将在15分钟内登录的用户总数的值来估计，或者 60% 的用户将在1小时内登录。 这些值共同定义了用于计算调整大小建议的峰值负载配置文件。  
  
接下来，你将需要根据用户是否为以下值，指定将需要通过单一 sign @ no__t-0on 访问目标声明 @ no__t-1aware 应用程序的用户总数：  
  
-   从物理连接到公司网络的本地计算机登录 Active Directory @no__t 0through Windows 集成身份验证 @ no__t-1  
  
-   从未物理连接到公司网络的计算机远程登录 Active Directory @no__t 0through Windows 集成身份验证或用户名和密码 @ no__t-1  
  
-   来自其他组织，并尝试从受信任的合作伙伴访问目标声明 @ no__t-0aware 应用程序  
  
-   从 SAML 2.0 标识提供程序尝试访问目标声明 @ no__t-0aware 应用程序  
  
#### <a name="how-to-use-this-spreadsheet"></a>如何使用此电子表格  
对于你计划部署的每个联合服务器场实例，你可以使用以下步骤来确定建议的联合服务器数。  
  
1.  下载并打开适用于[Windows server 2012 R2 的 AD FS 容量规划大小调整电子表格](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)或[适用于 windows server 2016 的 AD FS 容量规划大小调整电子表格](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
  
2.  在**高峰系统使用期内的单元中，我希望此部分用户对单元格进行身份验证**，单击该单元，然后使用 drop @ no__t-1down 箭头选择估计的系统利用率级别，即**40%** 、**60%** 或**80%** （对于部署）。  
  
3.  在位于**以下时间段**内的单元中，单击该单元，然后使用 drop @ no__t-1down 箭头选择**1 分钟**、 **15 分钟**或**1 小时**，以选择高峰负载的持续时间。  
  
4.  在 "**输入估计的内部应用程序 @no__t @no__t 数-22007" 或 "2010 @ no__t" 或 "声明感知 web 应用程序" @ no__t**单元中，键入你将在其中使用的内部应用程序的数量内部.  
  
5.  在 "**输入估计的联机应用程序 @no__t 数-365 1such"、"SharePoint online" 或 "Lync online @ no__t-2** " 单元格中，键入将在组织中使用的联机应用程序或服务的数量。  
  
6.  在标题为 "**用户数**" 的单元格下，在应用到示例应用程序方案的每一行上键入一个数字，用户将需要对其进行单一 sign @ no__t-1on 访问。 此列应包含定义的用户数，而不是每秒的峰值用户数。 如果对应用程序进行的访问尝试必须首先通过 "主领域发现" 页，请键入 " **Y**"。如果不确定此选择，请键入**Y**。  
  
7.  查看以下建议的值：  
  
    1.  对于建议的联合服务器总数，请参阅灰色突出显示的右下角单元。  
  
    2.  对于每个示例应用场景建议的服务器数，请参阅灰色突出显示的行中的单元格。  
  
> [!NOTE]  
> 在标题为 "**建议联合服务器总数**" 的单元格右侧的单元中，将自动计算的值包含一个公式，该公式会将额外的 20% 缓冲区添加到所有其前面各个行中的值。 添加到**联合服务器总数**的公式建议将此缓冲区中的单元格构建到总的建议部署的联合服务器数，以使场中的总体负载几乎都无法达到其饱和点。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
