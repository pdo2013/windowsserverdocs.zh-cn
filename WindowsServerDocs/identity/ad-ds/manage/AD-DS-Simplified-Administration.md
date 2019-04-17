---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: "广告 DS 简体中文管理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6232e281c47f3b5b4627bc9d8ccf53269aafc390
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-simplified-administration"></a>广告 DS 简体中文管理

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍的新功能和 Windows Server 2012 域控制器部署和管理和之前的操作系统 DC 部署和新的 Windows Server 2012 实现之间的差异的好处。  
  
Windows Server 2012 引入新一代的 Active Directory 域服务简体中文管理，且最字根域重新构想自 Windows 2000 Server。 广告 DS 简体中文管理捕获的 Active Directory 的 12 年中经验，并使更多支持、更灵活、更直观的管理体验设计师和管理员。 这意味着创建新版本的现有的技术，以及扩展组件发布了 Windows Server 2008 R2 的能力。  
  
广告 DS 简体中文管理是 reimagining 的域部署。  
  
-   广告 DS 角色部署现在位于新服务器管理器的体系结构并允许远程安装  
  
-   广告 DS 部署和配置引擎现在是 Windows PowerShell，即使在使用新的广告 DS 配置向导  
  
-   方案扩展、森林准备域准备将自动域控制器升级的一部分和不再需要单独的任务特殊服务器如方案母版  
  
-   推广现在包含先决条件检查验证林和域降低的失败促销有机会使新的域控制器准备工作  
  
-   Active Directory 的 Windows PowerShell 模块现在包含 cmdlet 复制拓扑管理、动态访问控制，和其他操作  
  
-   Windows Server 2012 树林级别不实现新功能，并且仅的新 Kerberos 功能，免除频繁的管理员子集需要域功能级别功能需要同类域控制器环境  
  
-   对于虚拟化域控制器，以便包括自动的部署和回退保护添加了完全支持  
  
关于虚拟化的域控制器的详细信息，请参阅[Active Directory 域服务 & #40; 简介广告 DS & #41;虚拟化 & #40;级别 100 & #41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
此外，还有许多管理和维护改进：  
  
-   Active Directory 管理中心包括图形 Active Directory 回收站、Fine-Grained 密码策略管理和 Windows PowerShell 历史记录查看器  
  
-   新的服务器管理器具有特定的广告 DS 接口的性能监视和最佳做法分析、关键服务，并事件日志  
  
-   组托管服务帐户支持使用相同的安全主体的多台计算机  
  
-   改进相对标识符 (RID) 颁发和监视更好的可管理性成熟的 Active Directory 域中  
  
最后，广告 DS 利润从其他包含在 Windows Server 2012，如的新功能：  
  
-   NIC 组合和 Datacenter 桥接  
  
-   DNS 安全和之后启动更快广告集成区域可用性  
  
-   Hyper-V 可靠性和可扩展性改进  
  
-   BitLocker 网络解锁  
  
-   其他 Windows PowerShell 组件管理模块  
  
## <a name="technical-overview"></a>Technical 概述  
  
### <a name="adprep-integration"></a>ADPREP 集成  
Active Directory 森林架构扩展和域准备现在将集成到域控制器配置过程。 如果升级到现有林新的域控制器，该过程检测升级状态，并自动发生的方案扩展和域准备阶段。 安装的第一个 Windows Server 2012 域控制器的用户仍必须是管理员企业和方案管理员或提供有效的其他凭据。  
  
Adprep.exe 将保留在单独的森林和域准备 DVD。 附带 Windows Server 2012 的工具的版本是 Windows Server 2008 到向后兼容 x64 和 Windows Server 2008 R2。 Adprep.exe 还支持远程 forestprep 和域，就像 ADDSDeployment 基于域控制器配置工具。  
  
有关 Adprep 和之前的操作系统森林准备工作的信息，请参阅[运行 Adprep（Windows Server 2008 R2）](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  
  
### <a name="server-manager-ad-ds-integration"></a>服务器管理器广告 DS 集成  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
服务器管理器充当中心以便获取相关服务器管理任务。 其仪表板样式外观定期刷新已安装的角色和远程服务器组视图。 服务器管理器中提供本地或远程服务器，而无需主机访问的集中的的管理。  
  
Active Directory 域服务是一个这些中心的角色：运行服务器管理器的域控制器或在 Windows 8 远程服务器管理工具，你看到你森林中的域控制器上的重要最近的问题。  
  
这些视图包括：  
  
-   服务器可用性  
  
-   性能和监视器警报高 CPU 和内存使用情况  
  
-   Windows 服务特定于广告 DS 的状态  
  
-   最近目录服务相关警告和错误的条目事件日志中  
  
-   根据一组 Microsoft 建议规则域控制器的最佳实践分析  
  
### <a name="active-directory-administrative-center-recycle-bin"></a>Active Directory 管理中心回收站  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 引入了 Active Directory 回收站的不要从备份中还原、重新启动广告 DS 服务，或重启一次域控制器恢复已删除 Active Directory 对象。  
  
Windows Server 2012 增强了使用 Active Directory 管理中心中的新图形界面现有的 Windows PowerShell 基于还原功能。 这使管理员启用回收站、找到或还原已删除的森林，而无需直接运行 Windows PowerShell cmdlet 所有域上下文中的对象。 Active Directory 管理中心和 Active Directory 回收站仍然使用 Windows PowerShell 下键盘盖，因此以前脚本和过程仍有价值。  
  
了解有关 Active Directory[回收站，请参阅 Active Directory 回收站 Step-by-Step 指南（Windows Server 2008 R2）](https://technet.microsoft.com/library/dd392261(WS.10).aspx)。  
  
### <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Active Directory 管理中心精准密码策略  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 引入 Fine-Grained 密码策略，这使管理员配置每个域多个密码和帐户锁定策略。 这允许域强制执行更多或限制较少基于用户和组的密码规则灵活解决方案。 它具有没有管理界面和所需的管理员，可以使用 Ldp.exe 或 Adsiedit.msc 配置。 Windows Server 2008 R2 的 Windows PowerShell，它授予管理员命令行界面到 FGPP 引入 Active Directory 模块。  
  
Windows Server 2012 Fine-Grained 密码策略带来图形的界面。 Active Directory 管理中心是此新对话框中，可对所有管理员带来简化的 FGPP 管理家庭。  
  
有关 Fine-Grained 密码策略的信息，请参阅[广告 DS Fine-Grained 密码和帐户锁定策略 Step-by-Step 指南（Windows Server 2008 R2）](https://technet.microsoft.com/library/cc770842(WS.10).aspx)。  
  
### <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Active Directory 管理中心 Windows PowerShell 历史记录查看器  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 引入了 Active Directory 管理中心较旧的 Active Directory 用户和计算机贴靠的在 Windows 2000 创建取代。 Active Directory 管理中心的 Windows PowerShell 创建图形到然后新 Active Directory 模块管理界面。  
  
尽管 Active Directory 模块包含通过 100 cmdlet，学习曲线的管理员可以险。 由于 Windows PowerShell 很大程度集成到 Windows 管理的策略，Active Directory 管理中心现在包含可用于查看 cmdlet 执行图形界面中的查看器。 你可以搜索、复制、清除历史记录，并添加注释使用简单的界面。 打算是管理员才能够图形界面用于创建和修改对象，然后将它们查看在历史记录查看器中了解有关 Windows PowerShell 脚本的详细信息和修改示例。  
  
### <a name="ad-replication-windows-powershell"></a>广告复制 Windows PowerShell  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 向 Active Directory 的 Windows PowerShell 模块的其他 Active Directory 复制 cmdlet。 这些允许新的或现有的站点、个子网、连接、站点链接和桥配置。 它们也会返回 Active Directory 复制元数据、复制状态、排队和的最新版本矢量信息。 复制 cmdlet-结合部署和其他现有广告 DS cmdlet-引入使得可能管理使用 Windows PowerShell 单独森林。 这会创建新管理员祝配置并不具有图形界面，然后减少操作系统的整个攻击面管理 Windows Server 2012 和服务要求的机会。 部署到如机密 Internet 协议路由器 (SIPR) 和公司 Dmz 高安全网络的服务器时，这是非常重要。  
  
有关广告 DS 站点拓扑和复制的详细信息，请参阅[Windows Server Technical 参考](https://technet.microsoft.com/library/cc739127(WS.10).aspx)。  
  
### <a name="rid-management-and-issuance-improvements"></a>删除的管理和颁发改进  
Windows 2000 Active Directory 引入了删除的主相对标识符到域控制器，以便创建的安全信任的安全标识符 (Sid) 的问题池喜欢用户、组和计算机。  默认情况下，此全球 RID 空间限于 2<sup>30</sup>（或 1073741823）在某个域中创建的总 Sid。 Sid 无法返回到池，或将。 随着时间的推移大域可能会开始在 Rid，运行低，或者意外可能导致不必要的 RID 消耗和最终耗尽。  
  
Windows Server 2012 的大量由客户与 Microsoft 客户支持发现作为广告 DS RID 颁发和管理问题成熟以来的第一个 Active Directory 域在 1999 年，在创建地址。 其中包括：  
  
-   定期 RID 消耗警告写入事件日志  
  
-   当管理员使无效 RID 池日志事件  
  
-   最大的笔帽上不用阻止大小现在强制执行 RID 策略  
  
-   人工 RID 天花板内现在强制和记录全球 RID 空间不足时，允许管理员全球空间处于耗尽之前采取的操作  
  
-   现在可以翻倍 2 到大小的一位增加全球 RID 空间<sup>31</sup> (2147483648 Sid)  
  
有关 Rid 和不用主机的详细信息，请参阅[安全标识符工作原理](https://technet.microsoft.com/library/cc778824(WS.10).aspx)。  
  
## <a name="new-ad-ds-deployment-architecture"></a>新的广告 DS 部署建筑  
  
### <a name="ad-ds-role-deployment-and-management-architecture"></a>广告 DS 角色部署和管理建筑  
服务器管理和 ADDSDeployment Windows PowerShell 依赖以下 core 程序集功能部署或管理广告 DS 角色时：  
  
-   Microsoft.ADroles.Aspects.dll  
  
-   Microsoft.ADroles.Instrumentation.dll  
  
-   Microsoft.ADRoles.ServerManager.Common.dll  
  
-   Microsoft.ADRoles.UI。Common.dll  
  
-   Microsoft.DirectoryServices.Deployment.Types.dll  
  
-   Microsoft.DirectoryServices.ServerManager.dll  
  
-   Addsdeployment.psm1  
  
-   Addsdeployment.psd1  
  
这两依赖 Windows PowerShell 和其远程调用-命令远程角色安装和配置。  
  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  
  
Windows Server 2012 还重构许多退出 LSASS.EXE:  
  
-   DS 角色服务器服务 (DsRoleSvc)  
  
-   DSRoleSvc.dll（由 DsRoleSvc 服务加载）  
  
该服务必须存在推广、降级，或复制虚拟域控制器才能运行。 广告 DS 角色安装添加该服务，并设置的手册，默认情况下开始键入。 不要禁用此服务。  
  
### <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep 和先决条件检查建筑  
不会再 adprep 架构母版上运行。 可以从计算机中运行 Windows Server 2008 远程运行 x64 或更高版本。  
  
> [!NOTE]  
> Adprep LDAP 用于 Schxx.ldf 文件导入并不会自动重新连接如果在导入方案母版连接丢失。 作为导入的过程的一部分，方案母版设置中特定的模式，并自动重新连接已禁用，因为如果 LDAP 重新连接的连接会丢失后，重新建立的连接不会在特定的模式。 在此情况下，将无法正确更新的模式。  
  
先决条件检查确保某些条件为 true。 这些条件所需的广告 DS 安装成功。 如果未满足某些所需的条件，他们可以继续安装之前解决它。 它也可以检测到的一个森林或域没有准备好，以便 Adprep 部署代码自动运行。  
  
#### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep 可执行文件 Dll，LDFs 文件  
  
-   ADprep.dll  
  
-   Ldifde.dll  
  
-   Csvde.dll  
  
-   Sch14.ldf-Sch56.ldf  
  
-   Schupgrade.cat  
  
-   *dcpromo.csv  
  
以前存放在 ADprep.exe 广告准备代码被重构 adprep.dll 为。 这样 ADPrep.exe 和 ADDSDeployment Windows PowerShell 模块库用于相同的任务，并具有相同的功能。 Adprep.exe 附带的安装媒体，但自动化的进程不要它直接调用-仅管理员手动运行。 仅可以运行 Windows Server 2008 上 x64 和更高版本操作系统。 Ldifde.exe 和 csvde.exe 还具有重构版本为加载的准备过程中的 Dll。 方案扩展仍使用签名验证 LDF 文件，如在以前的操作系统版本。  
  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> 没有为 Windows Server 2012 32 位 Adprep32.exe 工具。 你必须至少一个 Windows Server 2008 64，Windows Server 2008 R2 或 Windows Server 2012 计算机 x 运行域控制器，成员服务器以，还是在工作组中，准备森林和域。 Adprep.exe 不在运行 Windows Server 2003 x 64。  
  
#### <a name="BKMK_PrereuisiteChecking"></a>先决条件检查  
先决条件检查内置 ADDSDeployment Windows PowerShell 管理代码系统的工作方式不同的模式，根据该操作。 下表介绍了在使用它时，每个测试和方式的说明它的验证。 这些表可能很有用，如果有的问题，其中验证失败，错误不足以解决该问题。  
  
这些测试登录**DirectoryServices 部署**任务类别下的操作事件日志信道**核心**、始终为事件 ID **103**。  
  
##### <a name="prerequisite-windows-powershell"></a>先决条件 Windows PowerShell  
有各种域控制器部署 cmdlet ADDSDeployment Windows PowerShell cmdlet。 他们有其关联 cmdlet 的参数大约相同。  
  
-   Test-ADDSDomainControllerInstallation  
  
-   Test-ADDSDomainControllerUninstallation  
  
-   Test-ADDSDomainInstallation  
  
-   Test-ADDSForestInstallation  
  
-   Test-ADDSReadOnlyDomainControllerAccountCreation  
  
不需要运行这些 cmdlet，通常;他们已自动执行与部署 cmdlet 默认情况。  
  
##### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>先决条件测试  
  
||||  
|-|-|-|  
|测试名称|协议<br /><br />使用|解释和笔记|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|验证你有"使计算机和用户帐户被委派"(SeEnableDelegationPrivilege) 的现有的合作伙伴域控制器上的权限。 此操作需要访问构造的 tokenGroups 属性。<br /><br />不使用时联系 Windows Server 2003 域控制器。 你必须手动确认在升级之前此权限|  
|VerifyADPrep<br /><br />先决条件（森林）|LDAP|发现并联系人架构主机使用 rootDSE namingContexts 特性和方案命名上下文 fsmoRoleOwner 属性。 确定哪些准备操作（forestprep、域，或者 rodcprep）所需的广告 DS 安装。 验证架构 objectVersion 预期运行了并且如果它进一步需要扩展。|  
|VerifyADPrep<br /><br />先决条件（域和 RODC）|LDAP|发现并联系人基础主机使用 rootDSE namingContexts 特性和基础结构容器 fsmoRoleOwner 属性。 在 RODC 安装的情况下此测试发现域名主机，并确保它处于联机状态。|  
|CheckGroup<br /><br />会员|LDAP，<br /><br />RPC 通过 SMB (LSARPC)|验证用户属于域管理员或企业管理员组中，具体取决于操作 (DA 添加或降级域控制器，EA 添加或删除某个域中)|  
|CheckForestPrep<br /><br />GroupMembership|LDAP，<br /><br />RPC 通过 SMB (LSARPC)|验证用户所在架构管理员企业管理员进行分组，并且具有管理审核和特权现有的域控制器上的安全事件日志 (SesScurityPrivilege)|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP，<br /><br />RPC 通过 SMB (LSARPC)|验证用户域管理组成员，并且具有管理审核和特权现有的域控制器上的安全事件日志 (SesScurityPrivilege)|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP，<br /><br />RPC 通过 SMB (LSARPC)|验证该用户的企业管理员组成员，并且具有管理审核和特权现有的域控制器上的安全事件日志 (SesScurityPrivilege)|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|验证架构主机已复制至少运行一次，因为它通过设置上 rootDSE 特性 becomeSchemaMaster 伪值重新启动|  
|VerifySFUHotFix<br /><br />应用|LDAP|验证现有森林架构不包含具有 OID 1.2.840.113556.1.4.7000.187.102 UID 特性的已知的问题 SFU2 扩展<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP，WMI，DCOM RPC|验证现有森林方案不包含问题 Exchange 2000 扩展 ms 在依然-Exch-助手-名称、ms-Exch-LabeledURI，以及 ms Exch 家标识符 ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />一致性|LDAP|验证现有森林架构（不正确的第三方修改）具有一致属性并类核心。|  
|DCPromo|通过 RPC，DRSR<br /><br />LDAP，<br /><br />DNS<br /><br />RPC 通过 SMB (SAMR)|验证传递到推广代码和测试提升的命令行语法。 验证森林或域尚不存在如果创建新。|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP，DRSR 轻 SMB，RPC 通过 SMB (LSARPC)|验证复制合作伙伴具有站复制启用通过检查各自的对象选项特性 NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004) 中指定的现有域控制器|  
|VerifyMachineAdmin<br /><br />密码|通过 RPC，DRSR<br /><br />LDAP，<br /><br />DNS<br /><br />RPC 通过 SMB (SAMR)|验证设置为 DSRM 满足复杂性要求的域的安全模式下密码。|  
|VerifySafeModePassword|*不适用*|验证本地管理员密码组满足计算机安全策略复杂性要求。|  
  


