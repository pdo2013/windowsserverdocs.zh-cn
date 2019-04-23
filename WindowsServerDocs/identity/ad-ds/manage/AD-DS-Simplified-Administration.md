---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: AD DS 简化管理
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 863e5352253d53941e64b52d1ca58d565a3aa8b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890588"
---
# <a name="ad-ds-simplified-administration"></a>AD DS 简化管理

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍了功能和优势的 Windows Server 2012 域控制器部署和管理和以前的操作系统 DC 部署和新的 Windows Server 2012 实现之间的差异。  
  
Windows Server 2012 引入了下一代 Active Directory 域服务简化管理，并已在最彻底域重新构想自 Windows 2000 Server。 AD DS 简化管理吸取 Active Directory 十二年的经验，为架构师和管理员创造更加可支持、更灵活、更直观的管理体验。 这意味着创建现有技术的新版本以及扩展在 Windows Server 2008 R2 中发布的组件的功能。  
  
AD DS 简化管理是对域部署的重构。  
  
- AD DS 角色部署现在是新服务器管理器体系结构的一部分，并且允许远程安装  
- 即使在使用新的 AD DS 配置向导时，Windows PowerShell 现在仍是 AD DS 部署和配置引擎。  
- 架构扩展、林准备和域准备自动成为域控制器升级的一部分，并且不再需要特殊服务器（例如架构主机）上的单独任务。  
- 升级现在包括先决条件检查，它可验证林和域对新域控制器的准备情况，并降低升级失败的概率  
- Windows PowerShell 的 Active Directory 模块现在包括用于复制拓扑管理、动态访问控制以及其他操作的 cmdlet  
- Windows Server 2012 林功能级别不实现新功能，只有一部分新 Kerberos 功能需要域功能级别，这使管理员无需频繁地需要相似的域控制器环境  
- 添加对虚拟化域控制器的完全支持，以包括自动化部署和回滚保护  
   - 有关虚拟化的域控制器的详细信息，请参阅[Active Directory 域服务简介&#40;AD DS&#41;虚拟化&#40;Level 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。

此外，还有很多管理和维护方面的改进：  

- Active Directory 管理中心包括图形 Active Directory 回收站、细化密码策略管理和 Windows PowerShell 历史记录查看器
- 新的服务器管理器有特定于 AD DS 的接口，用于性能监视、最佳实践分析、关键服务以及事件日志  
- 组托管服务帐户支持使用相同安全主体的多台计算机  
- 在相对标识符 (RID) 颁发和监视方面的改进，可用于提高成熟 Active Directory 域中的可管理性  

AD DS 从 Windows Server 2012 中添加如其他新功能获益：  

- NIC 组合和数据中心桥接  
- 启动后的 DNS 安全性和更快的 AD 集成区域可用性  
- Hyper-V 可靠性和可扩展性改进  
- BitLocker 网络解锁  
- 其他 Windows PowerShell 组件管理模块  

## <a name="adprep-integration"></a>ADPREP 集成

Active Directory 林架构扩展和域准备现在将集成到域控制器配置过程中。 如果将新域控制器升级到现有林，则进程会检测升级状态且架构扩展和林准备将自动进行。 安装第一个 Windows Server 2012 域控制器的用户必须仍然是是企业管理员或架构管理员，或提供有效的备用凭据。  
  
Adprep.exe 保留在 DVD 上以用于单独林和域准备。 Windows Server 2012 中包含的工具版本可以向后兼容到 Windows Server 2008 x64 和 Windows Server 2008 R2。 Adprep.exe 还支持远程 forestprep 和 domainprep，就像基于 ADDSDeployment 的域控制器配置工具一样。  
  
有关 Adprep 和以前的操作系统林准备的信息，请参阅 [运行 Adprep (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  

## <a name="server-manager-ad-ds-integration"></a>服务器管理器 AD DS 集成

![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
服务器管理器可充当服务器管理任务的中心。 它仪表板样式的外观会定期刷新已安装角色和远程服务器组的视图。 服务器管理器提供本地和远程服务器的集中式管理，而无需访问控制台。  
  
Active Directory 域服务是这些中心角色;通过在域控制器或远程服务器管理工具在 Windows 8 上运行服务器管理器，在您的林中的域控制器上查看最新的重要问题。  
  
这些视图包括：  
  
- 服务器可用性  
- 有关 CPU 和内存使用率过高的性能监视器警报  
- 特定于 AD DS 的 Windows 服务的状态  
- 事件目录中最近与目录服务相关的警告和错误条目  
- 根据一组 Microsoft 建议的规则进行的域控制器最佳实践分析  

## <a name="active-directory-administrative-center-recycle-bin"></a>Active Directory 管理中心回收站

![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 引入了 Active Directory 回收站，它可恢复已删除的 Active Directory 对象，而无需从备份还原、重新启动 AD DS 服务或重新启动域控制器。  
  
Windows Server 2012 使用 Active Directory 管理中心中的图形界面增强了基于 Windows PowerShell 的现有还原功能。 这样，管理员可以启用回收站并在林的域上下文中找到或还原已删除对象，所有操作均无需直接运行 Windows PowerShell cmdlet。 Active Directory 管理中心和 Active Directory 回收站仍然在内部使用 Windows PowerShell，因此以前的脚本和过程仍然有价值。  
  
有关 Active Directory 回收站的信息，请参阅 [Active Directory 回收站分步指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx)。  
  
## <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Active Directory 管理中心细化密码策略

![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 引入了细化密码策略，它允许管理员为每个域配置多个密码和帐户锁定策略。 这使域可以根据用户和组执行灵活的解决方案以强制执行较严格或较宽松的密码规则。 它没有管理界面，而且需要管理员使用 Ldp.exe 或 Adsiedit.msc 配置它。 Windows Server 2008 R2 引入了 Windows PowerShell 的 Active Directory 模块，它向管理员提供 FGPP 的命令行界面。  
  
Windows Server 2012 对细化密码策略提供了一个图形界面。 Active Directory 管理中心是此新对话框的主页，这将向所有管理员提供简化的 FGPP 管理。  
  
有关细化密码策略的信息，请参阅 [AD DS 细化密码和帐户锁定策略分步指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx)。  
  
## <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Active Directory 管理中心 Windows PowerShell 历史记录查看器

![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 引入了 Active Directory 管理中心，它取代了 Windows 2000 中创建的较早版本的 Active Directory 用户和计算机管理单元。 Active Directory 管理中心向当时全新的 Windows PowerShell 的 Active Directory 模块创建图形管理界面。  
  
尽管 Active Directory 模块包含上百种 cmdlet，但对于管理员来说，学习曲线可能很陡峭。 由于 Windows PowerShell 很大程度上集成到 Windows 管理的策略，Active Directory 管理中心现在包括可支持你在图形界面中查看 cmdlet 执行的查看器。 你可以使用简单的界面搜索、复制、清除历史记录以及添加注释。 其目的是使管理员使用图形界面来创建和修改对象，然后在历史记录查看器中查看它们以了解 Windows PowerShell 脚本的详细信息并修改这些示例。  

## <a name="ad-replication-windows-powershell"></a>AD 复制 Windows PowerShell

![简化的管理](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 将其他 Active Directory 复制 cmdlet 添加到 Active Directory Windows PowerShell 模块。 这些函数允许配置新的或现有的站点、子网、连接、站点链接和网桥。 它们还返回 Active Directory 复制元数据、复制状态、队列和最新版本向量信息。 复制 cmdlet 的引入（结合部署和其他现有 AD DS cmdlet）使单独使用 Windows PowerShell 管理林成为可能。 这将为希望在没有图形界面的情况下设置和管理 Windows Server 2012 的管理员创造新的机会，这将进而减少操作系统的攻击面并降低服务要求。 这在将服务器部署到高安全性网络（例如保密 Internet 协议路由器 (SIPR) 和企业 DMZ）中时尤其重要。  
  
有关 AD DS 站点拓扑和复制的详细信息，请参阅 [Windows Server 技术参考](https://technet.microsoft.com/library/cc739127(WS.10).aspx)。  

## <a name="rid-management-and-issuance-improvements"></a>RID 管理和颁发改进

Windows 2000 Active Directory 引入了 RID 主机，可将相对标识符的池颁发到域控制器，以创建安全信任项（例如用户、组和计算机）的安全标识符。  默认情况下，此全局 RID 控件限制为在域中创建的共 2<sup>30</sup> （或 1,073,741,823）个 SID。 SID 无法返回到池或重新颁发。 随着时间推移，大型域可能开始在 RID 上低效运行，或者事故可能会导致不必要的 RID 消耗并最终耗尽。  
  
Windows Server 2012 处理了自 1999 年第一批 Active Directory 域创建以来的大量 RID 颁发和管理问题，随着 AD DS 日渐成熟，客户和 Microsoft 客户支持发现了这些问题。 这些问题包括：  

- 定期 RID 消耗警告将写入事件日志  
- 管理员验证 RID 池时的事件日志  
- 现在已对 RID 策略 RID 块大小强制执行最大上限  
- 当全局 RID 空间不足时，将强制执行并记录人工 RID 上限，使管理员可以在全局空间耗尽前采取行动。
- 全局 RID 空间现在可以增加一位，将大小加倍为 2<sup>31</sup> (2,147,483,648 SID)  

有关 RID 和 RID 主机的详细信息，请查看 [安全标识符的工作原理](https://technet.microsoft.com/library/cc778824(WS.10).aspx)。  
  
## <a name="ad-ds-role-deployment-and-management-architecture"></a>AD DS 角色部署和管理体系结构

在部署或管理 AD DS 角色时，服务器管理器和 ADDSDeployment Windows PowerShell 依赖以下核心程序集发挥功能：  

- Microsoft.ADroles.Aspects.dll  
- Microsoft.ADroles.Instrumentation.dll  
- Microsoft.ADRoles.ServerManager.Common.dll  
- Microsoft.ADRoles.UI.Common.dll  
- Microsoft.DirectoryServices.Deployment.Types.dll  
- Microsoft.DirectoryServices.ServerManager.dll  
- Addsdeployment.psm1  
- Addsdeployment.psd1  

都依赖 Windows PowerShell 及其远程调用命令来实现远程角色安装和配置。  

![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  

Windows Server 2012 还从 LSASS.EXE 重构出大量以前的升级操作，作为以下对象的一部分：  

- DS 角色服务器服务 （DsRoleSvc)  
- DSRoleSvc.dll（由 DsRoleSvc 服务加载）  

为了升级、降级或克隆虚拟域控制器，此服务必须存在并且正在运行。 默认情况下，AD DS 角色安装添加此服务并设置“手动”的开始类型。 不要禁用此服务。  

## <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep 和先决条件检查体系结构

Adprep 不再要求在架构主机上运行。 它可从运行 Windows Server 2008 x64 或更高版本的计算机远程运行。  
  
> [!NOTE]  
> 如果架构主机的连接在导入时丢失，Adprep 使用 LDAP 导入 Schxx.ldf 文件并且不会自动重新连接。 作为导入过程的一部分，架构主机设置为特定模式而且禁用自动重新连接，因为如果 LDAP 在连接丢失后重新连接，重新建立的连接将不会处于特定模式下。 在该情况下，架构将不会正确更新。  
  
先决条件检查可确保某些条件为 true。 这些条件是成功安装 AD DS 所必需的条件。 如果未满足某些所需的条件，可以在继续安装之前解决它们。 它还检测林或域尚未准备就绪，因此 Adprep 部署代码可自动运行。  

### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep 可执行文件、DLL、LDF、文件

- ADprep.dll  
- Ldifde.dll  
- Csvde.dll  
- Sch14.ldf - Sch56.ldf  
- Schupgrade.cat  
- *dcpromo.csv  

以前位于 ADprep.exe 中的 AD 准备代码将重构到 adprep.dll 中。 这使 ADPrep.exe 和 ADDSDeployment Windows PowerShell 模块可以将库用于相同的任务并具有相同的功能。 Adprep.exe 包括在安装媒体中，但是自动化进程不直接调用它 - 只有管理员手动运行它。 它仅可以在 Windows Server 2008 x64 和更高版本的操作系统上运行。 Ldifde.exe 和 csvde.exe 同样已将版本重构为 DLL，它们由准备进程加载。 架构扩展仍然使用通过签名验证的 LDF 文件，和在以前的操作系统版本中一样。  
  
![简化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> Windows Server 2012 没有 32 位 Adprep32.exe 工具。 若要准备林或域，你必须有至少一个 Windows Server 2008 x64、Windows Server 2008 R2 或 Windows Server 2012 计算机，且该计算机作为域控制器、成员服务器运行或在工作组中运行。 Adprep.exe 不在 Windows Server 2003 x64 上运行。  
  
## <a name="BKMK_PrereuisiteChecking"></a>先决条件检查

根据操作，内置于 ADDSDeployment Windows PowerShell 托管代码的先决条件检查系统在不同模式下工作。 以下表格介绍了每项测试以及使用它的时间，并说明了它的验证原理和对象。 在出现验证失败且错误不足以解决该问题时的情况时，这些表格非常有用。  
  
这些测试始终作为事件 ID **103** 登录任务类别 **核心**下的 **DirectoryServices-Deployment**操作事件日志通道。  
  
### <a name="prerequisite-windows-powershell"></a>先决条件 Windows PowerShell

所有域控制器部署 cmdlet 都有 ADDSDeployment Windows PowerShell cmdlet。 它们具有与关联 cmdlet 基本相同的参数。  

- Test-ADDSDomainControllerInstallation  
- Test-ADDSDomainControllerUninstallation  
- Test-ADDSDomainInstallation  
- Test-ADDSForestInstallation  
- Test-ADDSReadOnlyDomainControllerAccountCreation  

通常无需运行这些 cmdlet；它们已默认使用部署 cmdlet 自动执行。  

#### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>必备项测试

||||  
|-|-|-|  
|测试名称|协议<br /><br />已使用|说明和备注|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|验证你在现有伙伴域控制器上有“使计算机和用户帐户可以受信任且可以委派”(SeEnableDelegationPrivilege) 权限。 这需要你构造的 tokenGroups 属性的访问权限。<br /><br />在联系 Windows Server 2003 域控制器时不使用。 你必须在升级前手动确认此权限|  
|VerifyADPrep<br /><br />先决条件（林）|LDAP|使用 rootDSE namingContexts 属性和架构命名上下文 fsmoRoleOwner 属性发现并联系架构主机。 确定 AD DS 安装需要哪些预备操作（forestprep、domainprep 或 rodcprep）。 验证架构 objectVersion 是否和预料的一样以及它是否需要进一步扩展。|  
|VerifyADPrep<br /><br />先决条件（域和 RODC）|LDAP|使用 rootDSE namingContexts 属性和基础结构容器 fsmoRoleOwner 属性发现并联系基础结构主机。 对于 RODC 安装，此测试发现域命名主机，并确保它处于联机状态。|  
|CheckGroup<br /><br />Membership|LDAP、<br /><br />RPC over SMB (LSARPC)|根据操作验证用户是 Domain Admins 还是 Enterprise Admins 组的成员（添加或降级域控制器对应 DA，添加或删除域则对应 EA）|  
|CheckForestPrep<br /><br />GroupMembership|LDAP、<br /><br />RPC over SMB (LSARPC)|验证用户是 Schema Admins 和 Enterprise Admins 组的成员，并且在现有域控制器上具有管理审核和安全事件日志 (SesScurityPrivilege) 权限|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP、<br /><br />RPC over SMB (LSARPC)|验证用户是 Domain Admins 组的成员并且在现有域控制器上有管理审核和安全事件日志 (SesScurityPrivilege) 权限|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP、<br /><br />RPC over SMB (LSARPC)|验证用户是 Enterprise Admins 组的成员并且在现有域控制器上有管理审核和安全事件日志 (SesScurityPrivilege) 权限|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|通过在 rootDSE 属性 becomeSchemaMaster 上设置一个虚拟值，验证架构主机已在其重新启动后至少复制一次|  
|VerifySFUHotFix<br /><br />已应用|LDAP|验证现有林架构不包含具有 OID 1.2.840.113556.1.4.7000.187.102 的 UID 属性的已知问题 SFU2 扩展<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP、WMI、DCOM、RPC|验证现有林架构没有继续包含问题 Exchange 2000 扩展-Exch-助手的名称，ms-Exch-LabeledURI，和 ms Exch 房屋标识符 ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />一致性|LDAP|验证现有林架构具有一致的（未由第三方错误修改）核心属性和类。|  
|DCPromo|DRSR over RPC、<br /><br />LDAP、<br /><br />DNS<br /><br />RPC over SMB (SAMR)|验证命令行语法已传递到升级代码和测试升级。 在新建时，验证林或域尚不存在。|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP、DRSR over SMB、RPC over SMB (LSARPC)|通过针对 NTDS 设置对象的选项属性检查 NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004) 来验证指定为复制伙伴的现有域控制器已启用出站复制。|  
|VerifyMachineAdmin<br /><br />密码|DRSR over RPC、<br /><br />LDAP、<br /><br />DNS<br /><br />RPC over SMB (SAMR)|验证 DSRM 的安全模式密码集符合域复杂性要求。|  
|VerifySafeModePassword|*N/A*|验证本地管理员密码集符合计算机安全策略复杂性要求。|  
