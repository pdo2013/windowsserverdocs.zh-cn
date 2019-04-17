---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: "升级到 Windows Server 2012 R2 和 Windows Server 2012 的域控制器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e317c5a939d417bac844c4080223d7b5e0eec149
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>升级到 Windows Server 2012 R2 和 Windows Server 2012 的域控制器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题提供了有关在 Windows Server 2012 R2 和 Windows Server 2012 的 Active Directory 域服务的背景信息并介绍升级从 Windows Server 2008 或 Windows Server 2008 R2 域控制器的过程。  
  
-   [域控制器升级步骤](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradeWorkflow)  
  
-   [什么是 Windows Server 2012 中的新增功能？](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewEight)  
  
-   [在 Windows Server 2012 R2 的广告 DS 中的新增功能是什么？](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_NewWS2012R2)  
  
-   [在 Windows Server 2012 的广告 DS 中的新增功能是什么？](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewAD)  
  
-   [广告 DS server 角色安装变化](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_InstallationChanges)  
  
-   [不建议使用的功能和与在 Windows Server 2012 的广告 DS 相关的行为更改](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)  
  
-   [操作系统系统要求](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_SysReqs)  
  
-   [受支持的就地升级路径](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradePaths)  
  
-   [功能功能和给要求](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_FunctionalLevels)  
  
-   [与其他服务器的角色以及 Windows 操作系统的广告 DS 互操作性](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_ServerRoles)  
  
-   [操作主机角色](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_OpsMasters)  
  
-   [运行 Windows Server 2012 的虚拟化域控制器](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Virtual)  
  
-   [Windows Server 2012 服务器管理](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Admin)  
  
-   [应用程序兼容性](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)  
  
-   [已知的问题](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_KnownIssues)  
  
## <a name="BKMK_UpgradeWorkflow"></a>域控制器升级步骤  
推荐的方法，升级域是提升运行较新版本的 Windows Server 和降级较旧的域控制器根据需要的域控制器。 方法是适用于升级了现有的域控制器在操作系统。 此列表涉及之前推广运行较新版本的 Windows Server 的域控制器的常规步骤：  
  
1.  确认目标服务器满足[系统要求](https://technet.microsoft.com/library/dn303418.aspx)。  
  
2.  验证[应用程序兼容性](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)。  
  
3.  检查安全设置。 有关详细信息，请参阅[与在 Windows Server 2012 的广告 DS 相关建议不再使用功能和行为更改](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)和[安全默认设置，在 Windows Server 2008 和 Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault)。  
  
4.  从计算机计划运行安装到目标服务器检查连接。  
  
5.  检查可用性必要操作主机的角色：  
  
    -   若要安装的第一个 DC 现有域和森林中运行 Windows Server 2012，你在运行安装该计算机需要连接到运行 adprep /forestprep 成功以便架构主机和才能运行 adprep /domainprep 的基础结构母版。  
  
    -   若要安装已被扩展森林架构某个域中的第一个域控制器，只需连接到的基础结构母版。  
  
    -   若要安装或在现有的树林中删除某个域，你需要连接到域命名主机。  
  
    -   任何域控制器安装还需要连接到 RID 主机。  
  
    -   如果您的第一个只读域控制器安装现有的树林中，你将需要为每个应用目录分区，也称为非域命名上下文或 NDNC 连接到主服务器基础结构。  
  
6.  请务必提供必要的凭据，才能运行广告 DS 安装。  
  
    |安装操作|凭据要求|  
    |-----------------------|---------------------------|  
    |安装新森林|目标服务器上的本地管理员|  
    |在现有的树林中安装新域|企业管理员|  
    |现有的域中安装其他直流|域管理员|  
    |运行 adprep /forestprep 成功|方案管理员企业管理员，域管理员|  
    |运行 adprep /domainprep|域管理员|  
    |运行 adprep /domainprep /gpprep|域管理员|  
    |运行 adprep /rodcprep|企业管理员|  
  
    你可以委派安装广告 DS 的权限。 有关详细信息，请参阅[安装管理任务](https://technet.microsoft.com/library/cc773327(WS.10).aspx)。  
  
在下面的链接，可以找到步骤步骤的说明进行操作，以促进新和使用 Windows PowerShell cmdlet 和服务器管理器副本 Windows Server 2012 域控制器：  
  
-   [安装 Active Directory 域服务 (级别 100)](https://technet.microsoft.com/library/hh472162.aspx)  
  
-   [安装新的 Windows Server 2012 的 Active Directory 森林 (级别 200)](https://technet.microsoft.com/library/jj574166.aspx)  
  
-   [将副本 Windows Server 2012 域控制器安装在现有的域 (级别 200)](https://technet.microsoft.com/library/jj574134.aspx)  
  
-   [安装新的 Windows Server 2012 的 Active Directory 孩子或树域 (级别 200)](https://technet.microsoft.com/library/jj574105.aspx)  
  
-   [安装 Windows Server 2012 的 Active Directory Read-Only 域控制器 (RODC) (级别 200)](https://technet.microsoft.com/library/jj574152.aspx)  
  
-   [设置为 Windows Server 2012 域控制器 (EN-US) 的分步指南](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  
  
## <a name="BKMK_WhatsNewEight"></a>什么是 Windows Server 2012 中的新增功能？  
按服务器角色列出的新功能，如下表中所列出技术区域。 更多白皮书、 视频的演示，以及有关在 Windows Server 2012 的其他功能的演示，请参阅[服务器和云平台](https://www.microsoft.com/server-cloud/default.aspx)。  
  
||||  
|-|-|-|  
|[Active Directory 证书服务（广告客户服务）](https://technet.microsoft.com/library/hh831373.aspx)|[Active Directory 权限管理服务 (广告 RMS)](https://technet.microsoft.com/library/hh831554.aspx)|[BitLocker 驱动器加密](https://technet.microsoft.com/library/hh831412.aspx)|  
|[分支缓存](https://technet.microsoft.com/library/jj127252.aspx)|[动态主机配置协议 (DHCP)](https://technet.microsoft.com/library/jj200226.aspx)|[域名系统 (DNS)](https://technet.microsoft.com/library/jj200224.aspx)|  
|[故障转移群集](https://technet.microsoft.com/library/hh831414.aspx)|[文件服务器资源管理器](https://technet.microsoft.com/library/hh831746.aspx)|[组策略](https://technet.microsoft.com/library/jj574108.aspx)|  
|[Hyper-V](https://technet.microsoft.com/library/hh831410.aspx)|[IP 地址管理 (IPAM)](https://technet.microsoft.com/library/jj200214.aspx)|[Kerberos 身份验证](https://technet.microsoft.com/library/hh831747.aspx)|  
|[托管服务帐户](https://technet.microsoft.com/library/hh831451.aspx)|[网络](https://technet.microsoft.com/library/jj200215.aspx)|[远程桌面服务](https://technet.microsoft.com/library/hh831527.aspx)|  
|[安全审核](https://technet.microsoft.com/library/hh849638.aspx)|[服务器管理器](https://blogs.technet.com/b/servermanager/archive/2012/06/27/server-manager-power-of-many-simplicity-of-one.aspx)|[智能卡](https://technet.microsoft.com/library/hh849637.aspx)|  
|[TLS 月 SSL (Schannel SSP)](https://technet.microsoft.com/library/hh831771.aspx)|[Windows 部署服务](https://technet.microsoft.com/library/hh974416.aspx)|[Windows PowerShell 3.0](https://technet.microsoft.com/library/hh857339)|  
  
### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a>自动维护和更改，更新应用通过 Windows 更新后重新启动行为  
之前的版本的 Windows 8，Windows 更新管理其自己的内部计划，检查更新，并且用于下载并安装它们。 它所需的 Windows 更新代理始终在后台运行，内存和其他系统资源消耗。  
  
Windows 8 和 Windows Server 2012 引入了一项新功能称为[自动维护](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)。 每使用其自己的计划管理和执行逻辑，自动维护整合了许多不同的功能。 这种整合允许所有这些组件以使用更少系统资源，能正常工作，尊重新[连接待机](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx)对新型设备状态和消耗较少电池电量的便携设备。  
  
因为 Windows 更新自动维护在 Windows 8 和 Windows Server 2012 的一部分，不再有效自己内部计划设置的日期和时间安装更新。 为了帮助确保在你的企业中一致且可预测重新启动的所有设备和计算机的行为，包括那些运行 Windows 8 和 Windows Server 2012，请参阅 Microsoft 知识库文章[2885694](https://support.microsoft.com/kb/2885694) (或查看 2013 年 10 月累积汇总[2883201](https://support.microsoft.com/kb/2883201))，然后配置 WSUS 博客文章中所述的策略设置[启用适用于 Windows 8 和 Windows Server 2012 (KB 2885694) 的更多可预测的 Windows 更新体验](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx)。  
  

## <a name="BKMK_NewWS2012R2"></a>在 Windows Server 2012 R2 的广告 DS 中的新增功能是什么？  
下表适用的更多详细信息的链接总结广告 DS 在 Windows Server 2012 R2 的新功能。 有关详细说明的某些功能，包括其要求，请参阅[什么是在 Windows Server 2012 R2 的 Active Directory 中的新增功能](https://technet.microsoft.com/library/dn268294.aspx)。  
  
|功能|描述|  
|-----------|---------------|  
|[加入工作区](https://technet.microsoft.com/library/dn280945.aspx)|允许信息的人员加入他们的个人设备与他们的公司公司资源和服务的访问。|  
|[Web 应用程序代理服务器](https://technet.microsoft.com/library/dn280942.aspx)|可以使用新的远程访问角色服务的 web 应用程序访问。|  
|[Active Directory 联合身份验证服务](https://technet.microsoft.com/library/hh831502.aspx)|广告 FS 已简化的部署和改进，可使用户可以访问来自个人设备的资源并帮助管理访问控制 IT 部门。|  
|[SPN 并 UPN 唯一](https://technet.microsoft.com/library/dn535779.aspx)|运行 Windows Server 2012 R2 域控制器阻止重复服务主体名称 (Spn) 创建和用户主要命名 (Upn)。|  
|[Winlogon 自动重启登录 (ARSO)](https://technet.microsoft.com/library/dn535772.aspx)|启用锁定屏幕应用程序会重新启动和适用于 Windows 8.1 设备。|  
|[TPM 关键审计](https://technet.microsoft.com/library/dn581921.aspx)|使 Ca 加密证实中证书申请者专用密钥真正受保护的受信任的平台模块 (TPM) 颁发的证书。|  
|[凭据保护和管理](https://technet.microsoft.com/library/dn408190.aspx)|新增凭据保护和域身份验证控件，以减少凭据被盗。|  
|[Deprecation 的文件复制服务 (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|因为此功能的级别，在 FRS 用于复制 SYSVOL，也不建议使用 Windows Server 2003 域功能级别。 当你在运行 Windows Server 2012 R2 的服务器上创建一个新的域的方式，必须域功能级别是 Windows Server 2008 或更高版本。 你仍然可以将添加到已 Windows Server 2003 域功能级别; 现有域运行 Windows Server 2012 R2 域控制器你只需无法创建新的域该级别。|  
|[新的域和森林功能级别](../active-directory-functional-levels.md)|有 Windows Server 2012 R2 的新功能级别。 可以在 Windows Server 2012 R2 DFL 使用新的功能。|  
|[LDAP 查询优化更改](https://technet.microsoft.com/library/dn535775.aspx)|LDAP 搜索的效率和 LDAP 搜索查询时间的复杂性能改进。|  
|[1644 事件改进](https://technet.microsoft.com/library/dn535775.aspx)|LDAP 搜索结果统计信息添加到事件 ID 1644 来帮助进行疑难解答。|  
|[Active Directory 复制吞吐量改进](https://technet.microsoft.com/library/dn535775.aspx)|调整到大约 600 Mbps 40Mbps 最大广告复制吞吐量|  
  
## <a name="BKMK_WhatsNewAD"></a>在 Windows Server 2012 的广告 DS 中的新增功能是什么？  
下表总结带有适用的更多详细信息的链接广告 DS Windows Server 2012，在新功能。 有关详细说明的某些功能，包括其要求，请参阅[Active Directory 域服务 (广告 DS) 中的新增功能是什么](https://technet.microsoft.com/library/hh831477.aspx)。  
  
|功能|描述|  
|-----------|---------------|  
|请参阅 active Directory 基于激活 （广告栏）[批量激活概述](https://technet.microsoft.com/library/hh831612.aspx)|简化了配置 distribution 以及管理批量软件许可证的任务。|  
|[Active Directory 联合身份验证服务 (广告 FS)](https://technet.microsoft.com/library/hh831502.aspx)|添加了角色安装通过服务器管理器简体中文信任设置、 自动信任管理和 SAML 协议支持的详细信息。|  
|Active Directory 丢失的页面 flush 事件|NTDS 这些事件 530 jet 错误-1119 记录检测到 Active Directory 数据库丢失的页面齐平事件。|  
|[Active Directory 回收站 Bin 用户界面](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Active Directory 管理中心 (ADAC) 添加了对 Windows Server 2008 R2 最初引入回收站 bin 功能的 GUI 管理。|  
|[Active Directory 复制和拓扑 Windows PowerShell cmdlet](https://technet.microsoft.com/library/hh831757.aspx)|支持的创建和管理 Active Directory 的站点、 站点链接、 连接对象更使用 Windows PowerShell 和。|  
|[动态访问控制](https://technet.microsoft.com/library/hh831717.aspx)|新索赔基于授权平台，增强了旧版访问控制模型。|  
|[精细的密码策略用户界面](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|ADAC 添加 GUI 创建、 编辑和 Pso 最初 Windows Server 2008 中添加的分配的支持。|  
|[组托管服务帐户 (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|安全主要新型称为 gMSA。 运行多个主机上的服务可以使用相同的 gMSA 帐户运行。|  
|[DirectAccess 脱机域加入](https://technet.microsoft.com/library/jj574150.aspx)|扩展离线状态下的域加入了包括 DirectAccess 先决条件。|  
|[快速部署通过复制虚拟域控制器 (DC)](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|可以通过复制使用 Windows PowerShell cmdlet 现有虚拟域控制器快速部署虚拟化的 Dc。|  
|[删除池更改](https://technet.microsoft.com/library/jj574229.aspx)|添加了新监视事件和配额为避免过度消耗全球 RID 池。 如果原始池成为耗尽 （可选） 翻倍全球 RID 池中的大小。|  
|安全时间服务|可通过从 wire 删除机密删除 MD5 希功能，需要与 Windows 8 时间客户端进行身份验证的服务器 W32tm 增强安全性|  
|[虚拟化 Dc USN 回退保护](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|意外还原的虚拟化 Dc 快照备份不会再导致 USN 回退。|  
|[Windows PowerShell 查看历史记录](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|允许管理员可以查看时使用 ADAC 执行的 Windows PowerShell 命令。|  
  
### <a name="BKMK_"></a>自动维护和更改，更新应用通过 Windows 更新后重新启动行为  
之前的版本的 Windows 8，Windows 更新管理其自己的内部计划，检查更新，并且用于下载并安装它们。 它所需的 Windows 更新代理始终在后台运行，内存和其他系统资源消耗。  
  
Windows 8 和 Windows Server 2012 引入了一项新功能称为[自动维护](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)。 每使用其自己的计划管理和执行逻辑，自动维护整合了许多不同的功能。 这种整合允许所有这些组件以使用更少系统资源，能正常工作，尊重新[连接待机](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx)对新型设备状态和消耗较少电池电量的便携设备。  
  
因为 Windows 更新自动维护在 Windows 8 和 Windows Server 2012 的一部分，不再有效自己内部计划设置的日期和时间安装更新。 以帮助确保你的企业，包括 Windows 8 和 Windows Server 2012，运行中的所有设备和计算机一致且可预测的重新启动行为可以配置以下组策略设置：  
  
-   **计算机配置 |策略 |管理模板 |Windows 组件 |Windows 更新 |配置自动更新**  
  
-   **计算机配置 |策略 |管理模板 |Windows 组件 |Windows 更新 |不与登录的用户重新启动**  
  
-   **计算机配置 |策略 |管理模板 |Windows 组件 |维护计划 |维护随机延迟**  
  
下表列出了一些有关如何配置这些设置，以便提供所需重新启动行为的示例。  
  
|||  
|-|-|  
|**方案**|**推荐的配置**|  
|**WSUS 托管**<br /><br />-安装每周运行一次更新<br />-重新星期五启动晚上 11|将自动安装，请阻止自动重新启动到所需的时间的计算机设置<br /><br />**策略**： 配置 （启用） 的自动更新<br /><br />配置自动更新： 4-将自动下载，并计划安装<br /><br />**策略**： 任何与登录的用户 （禁用） 重启<br /><br />**WSUS 期限**： 晚上 11 到周五设置|  
|**WSUS 托管**<br /><br />-交错安装跨不同的小时数天|设置为不同的计算机应一起更新组目标组<br /><br />上方的步骤用于以前方案<br /><br />为不同的目标组设置不同截止日期|  
|**不 WSUS 托管-不支持截止日期**<br /><br />-交错安装在不同时间|**策略**： 配置 （启用） 的自动更新<br /><br />配置自动更新： 4-将自动下载，并计划安装<br /><br />**注册表项：**启用 Microsoft 知识库文章中所述的注册表项[2835627](https://support.microsoft.com/kb/2835627)<br /><br />**策略：**自动维护随机延迟 （启用）<br /><br />设置**定期维护随机延迟**到 6 小时随机延迟的 PT6H 提供了以下问题：<br /><br />更新将安装在配置的维护时间以及随机延迟<br /><br />-为每台计算机会进行准确 3 天后，请重新启动<br /><br />或者，设置为计算机的每组不同维护时间|  
  
为什么有关详细信息的 Windows 的工程团队实现这些更改，请参阅[最小化 Windows 更新中的自动更新后重启](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx)。  
  
## <a name="BKMK_InstallationChanges"></a>广告 DS server 角色安装变化  
通过 Windows Server 2008 R2 的 Windows Server 2003 中, 运行 x86 或 X64 Adprep.exe 命令行工具运行 Active Directory 安装向导 Dcpromo.exe，，Dcpromo.exe 之前的版本具有可选型号安装或从媒体无人照看安装。  
  
开始在 Windows Server 2012，在 Windows PowerShell 使用 ADDSDeployment 模块执行命令行安装。 基于 GUI 促销执行服务器管理器中使用全新的广告 DS 配置向导。 简化在安装过程，ADPREP 集成广告 DS 安装，并根据需要自动运行。 基于 Windows PowerShell 广告 DS 配置向导将自动面向方案和基础结构主机角色 Dc 了添加，然后远程相关的域控制器上运行所需的 ADPREP 命令域中。  
  
在开始安装之前，广告 DS 安装向导中的先决条件检查找出潜在的错误。 可以更正错误条件以消除从部分完成升级的问题。 该向导也会导出一个包含所有图形安装过程中指定的选项的 Windows PowerShell 脚本。  
  
总之，广告 DS 安装变化简化 DC 角色安装进程，并减少管理错误的可能性，尤其是当你将跨全局地区和域部署多个域控制器。  
有关 GUI 详细信息和基于 Windows PowerShell 的安装，包括命令行语法和分步向导中的说明，请参阅[安装 Active Directory 域服务](https://technet.microsoft.com/library/hh472162.aspx)。 为想要控制架构更改独立于 Windows Server 2012 Dc 现有的树林中安装的 Active Directory 树林中引入的管理员，Adprep.exe 可仍运行命令在提升了权限的命令提示符。  
  
## <a name="BKMK_DeprecatedFeatures"></a>不建议使用的功能和与在 Windows Server 2012 的广告 DS 相关的行为更改  
有一些与广告 DS 相关的更改：  
  
-   **Adprep32.exe deprecation**  
  
    只有一个 Adprep.exe 版本，并且可以根据需要运行 Windows Server 2008 64 位服务器上运行或更高版本。 它可以远程，运行，并且必须远程运行，如果该针对或 Windows Server 2003 32 位操作系统上主机角色托管的操作。  
  
-   **Dcpromo.exe deprecation**  
  
    尽管在 Windows Server 2012 仅它仍可以运行与答案文件或命令行参数以提供组织时间过渡到新的 Windows PowerShell 安装选项现有自动化中不再 Dcpromo。  
  
-   **Lm 哈希禁用用户帐户**  
  
    在 Windows Server 2008、 Windows Server 2008 R2 和 Windows Server 2012 上的模板启用 NoLMHash 策略，这禁用安全模板 Windows 2000 和 Windows Server 2003 域控制器中的安全安全默认值。 禁用 NoLMHash 策略要求，lm 哈希依赖于客户端的使用知识库文章中的步骤[946405](https://support.microsoft.com/kb/946405)。  
  
开始与 Windows Server 2008，域控制器还具有以下安全默认设置，相比于运行 Windows Server 2003 或 Windows 2000 的域控制器。  
  
|||||  
|-|-|-|-|  
|加密类型或策略|Windows Server 2008 默认|Windows Server 2012 和 Windows Server 2008 R2 默认|评论|  
|AllowNT4Crypto|已禁用|已禁用|第三方服务器消息块 (SMB) 客户端可能与域控制器上的默认安全设置不兼容。 在所有的情况下，这些设置可以放松允许互操作性，但只付费安全的情况下。 有关详细信息，请参阅[文章 942564](https://go.microsoft.com/fwlink/?LinkId=164558) Microsoft 知识库 (https://go.microsoft.com/fwlink/?LinkId=164558) 中。|  
|DES|启用|已禁用|[文章 977321](https://go.microsoft.com/fwlink/?LinkId=177717) Microsoft 知识库 (https://go.microsoft.com/fwlink/?LinkId=177717) 中|  
|扩展 CBT/集成身份验证的保护|不适用|启用|请参阅[Microsoft 安全公告 (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) 和[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251) Microsoft 知识库 (https://go.microsoft.com/fwlink/?LinkId=178251) 中。<br /><br />查看并安装中的修复程序[文章 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) Microsoft 知识库根据需要中。|  
|LMv2|启用|已禁用|[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251) Microsoft 知识库 (https://go.microsoft.com/fwlink/?LinkId=178251) 中|  
  
## <a name="BKMK_SysReqs"></a>操作系统系统要求  
下表列出 Windows Server 2012 的最低系统要求。 有关系统要求的详细信息和预安装的信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 没有任何额外的系统要求，安装新的 Active Directory 森林，但你应添加内存不足，无法以提高性能的域控制器、 LDAP 客户的请求，以及 Active Directory 启用应用程序缓存 Active Directory 数据库的内容。 如果你有升级了现有的域控制器，或向现有的森林添加了新的域控制器，检查以确保服务器符合磁盘空间要求的下一步部分。  
  
|||  
|-|-|  
|处理器|1.4 Ghz 64 位处理器|  
|RAM|512 MB|  
|可用磁盘空间要求|为 32 GB|  
|屏幕分辨率|800 x 600 或更高版本|  
|其他|DVD 驱动器、 键盘、 Internet 访问权限|  
  
### <a name="BKMK_DiskSpaceDCWin8"></a>升级域控制器磁盘空间要求  
本部分介绍仅的升级的 Windows Server 2008 或 Windows Server 2008 R2 域控制器磁盘空间要求。 有关升级到较早版本的 Windows Server 的域控制器的磁盘空间要求的详细信息，请参阅[磁盘空间要求升级到 Windows Server 2008](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008)或[磁盘空间要求升级到 Windows Server 2008 R2](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2)。  
  
调整大小的磁盘，以自定义和应用程序驱动方案扩展、 应用程序和管理员启动索引，以及为对象和特性，你将添加到的目录部署的域控制器 （通常为 5 到 8 年） 的生命周期内空间容纳承载 Active Directory 数据库和日志文件。 部署次直接调整大小通常是相比更大部署后展开磁盘存储所需的触摸成本好投资。 有关详细信息，请参阅[Active Directory 域服务的容量计划](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx)。  
  
在你计划升级的域控制器，请务必承载 Active Directory 的驱动器数据库 (NTDS。DIT) 具有表示至少 20%NTDS 的可用磁盘空间。在开始操作系统升级之前 DIT 文件。 如果卷上没有足够的可用磁盘空间，在升级可以失败，并且升级的兼容性报告返回指示足够的可用磁盘空间的错误：  
  
在此情况下，可以尝试脱机碎片整理的 Active Directory 的数据库，若要重新获取更多空间，然后重新尝试升级。 有关详细信息，请参阅[压缩目录数据库文件 （离线碎片整理）](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx)。  
  
### <a name="available-skus"></a>可用的 Sku  
有 4 版本的 Windows Server： 基础、 Essentials、 标准和数据中心。   
支持广告 DS 角色两个版本是标准型和数据中心。  
  
在以前的版本，Windows Server 版本不在支持服务器的角色、 处理器数量以及大型内存支持相同。 无限制的虚拟实例允许 Datacenter edition 为和 Windows Server 标准型和 Datacenter edition 支持的所有功能和基础的硬件，但其虚拟化权限-在不同的 Standard edition 允许两个虚拟实例。  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Windows 客户端和 Windows Server 受支持加入域 Windows Server 操作系统  
以下 Windows 客户端和 Windows Server 操作系统是受支持的与运行 Windows Server 2012 的域控制器域成员计算机或更高版本：  
  
-   客户端操作系统： Windows 8.1、 Windows 8、 Windows 7、 Windows Vista 
  
    运行 Windows 8.1 或 Windows 8 的计算机，还有能加入域了域控制器该运行较早版本的 Windows Server、 Windows Server 2003 包括或更高版本。 在此情况下不过，Windows 8 的某些功能可能需要额外的配置或可能不可用。 有关这些功能和其他管理下层域中的 Windows 8 客户的建议的详细信息，请参阅[Windows Server 2003 域中运行的 Windows 8 成员计算机](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx)。  
  
-   服务器操作系统： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003 R2、 Windows Server 2003  
  
## <a name="BKMK_UpgradePaths"></a>受支持的就地升级路径  
运行 Windows Server 2008 或 Windows Server 2008 R2 64 位版本的域控制器可以升级到 Windows Server 2012。 你无法升级运行 Windows Server 2003 或 32 位版本的 Windows Server 2008 的域控制器。 替换它们，请安装在域中，运行更高版本的 Windows Server 的域控制器，并删除域控制器 Windows Server 2003。  
  
|如果你运行的这些版本|你可以升级到这些版本|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard sp2<br /><br />或<br /><br />Windows Server 2008 Enterprise sp2|Windows Server 2012 标准<br /><br />或<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 Datacenter sp2|Windows Server 2012 Datacenter|  
|Windows Server 2008 的 Web|Windows Server 2012 标准|  
|Windows Server 2008 R2 Standard sp1<br /><br />或<br /><br />Windows Server 2008 R2 sp1 企业版|Windows Server 2012 标准<br /><br />或<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter sp1|Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 的 Web|Windows Server 2012 标准|  
  
有关支持的升级路径的详细信息，请参阅[评估版本和升级选项适用于 Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917)。 注意： 你不能转换到零售版本直接将运行 Windows Server 2012 评估版本域控制器。 相反，在服务器上运行的零售版安装额外的域控制器，并从在评估版本运行的域控制器删除广告 DS。  
  
一个已知问题，由于无法升级运行 Windows Server 2012 的服务器 Core 安装到服务器核心安装的 Windows Server 2008 R2 域控制器。 升级将在升级过程中晚稳定黑屏挂起。 重新启动此类 Dc 公开 boot.ini 文件中的一个选项来回退到以前的操作系统版本。 重启系统触发自动回退到以前的操作系统版本。 可用的解决方案之前，建议您安装新的域控制器运行 Windows Server 2012 的而不是就地升级了现有的运行的服务器 Core 安装的 Windows Server 2008 R2 域控制器服务器核心安装。 有关详细信息，请参阅知识库文章[2734222](https://support.microsoft.com/kb/2734222)。  
  
## <a name="BKMK_FunctionalLevels"></a>功能功能和给要求  
 Windows Server 2012 要求 Windows Server 2003 森林功能级别。 也就是说，可以添加到现有的 Active Directory 林运行 Windows Server 2012 的域控制器之前，森林功能级别必须是 Windows Server 2003 或更高版本。 这意味着运行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的域控制器可以运行林中相同，但在运行 Windows 2000 Server 的域控制器并不受支持，并会阻止运行 Windows Server 2012 域控制器安装。 林中包含域控制器在运行 Windows Server 2003 或更高版本功能森林但级别仍是 Windows 2000，还可以阻止安装。  
  
Windows 2000 域控制器必须之前添加到你的森林 Windows Server 2012 域控制器中删除。 在此情况下，请考虑以下工作流程：  
  
1.  将运行 Windows Server 2003 或更高版本的域控制器安装。 可以将这些域控制器部署 Windows Server 评估版上。 此步骤还需要[运行 adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx)该操作系统发布的先决条件。  
  
2.  删除 Windows 2000 域控制器。 具体来说，完美降级或强制地域和使用 Active Directory 用户和计算机中删除所有删除的域控制器域控制器帐户中删除 Windows Server 2000 域控制器。  
  
3.  提升森林功能级别对 Windows Server 2003 或更高版本。  
  
4.  安装在运行 Windows Server 2012 的域控制器。  
  
5.  删除运行较早版本的 Windows Server 的域控制器。  
  
新的 Windows Server 2012 域功能级别使一项新功能： **KDC 支持索赔，复合身份验证、 Kerberos 程度**KDC 管理模板策略有两个设置 (**始终提供索赔**和**失败 unarmored 身份验证请求**)，需要在 Windows Server 2012 域功能级别。  
  
Windows Server 2012 森林功能级别不提供任何新的功能，但它确保创建林中任何新域将自动能在 Windows Server 2012 域功能级别。 Windows Server 2012 域功能级别不提供的索赔，复合身份验证、 Kerberos 程度 KDC 支持以外的其他新功能。 但它可以确保任何域控制器在域中的运行 Windows Server 2012。 有关可在不同的功能级别其他功能的详细信息，请参阅[了解 Active Directory 域服务 (广告 DS) 功能级别](../active-directory-functional-levels.md)。  
  
你为某个特定值设置森林功能级别后，你不能回退或降低森林功能级别，有以下例外： 提升与 Windows Server 2012 的森林功能级别后，你可以将其降至 Windows Server 2008 R2。 如果 Active Directory 回收站尚未启用，你还可以降低功能从 Windows Server 2008 R2 或 Windows Server 2008 Windows Server 2012 或 Windows Server 2008 R2 的级别回收件夹 Windows Server 2008 森林。 如果森林功能级别设置为 Windows Server 2008 R2，它无法回滚，例如，对 Windows Server 2003。  
  
你为某个特定值设置域功能级别后，你不能回退或降低域功能级别，有以下例外： 时筹集域功能级别对 Windows Server 2008 R2 或 Windows Server 2012 和森林功能级别是 Windows Server 2008 或更低，如果你拥有的选项推出域功能级别返回到 Windows Server 2008 或 Windows Server 2008 R2。 你可以降低仅从 Windows Server 2008 R2 或 Windows Server 2008 Windows Server 2012 或 Windows Server 2008 对 Windows Server 2008 R2 域功能级别。 如果域功能级别设置为 Windows Server 2008 R2，它无法回滚，例如，对 Windows Server 2003。  
  
有关可以在较低的功能级别的功能的详细信息，请参阅[了解 Active Directory 域服务 (广告 DS) 功能级别](../active-directory-functional-levels.md)。  
  
超出功能级别，运行 Windows Server 2012 的域控制器提供其他功能的运行较早版本的 Windows Server 的域控制器上不可用。 例如，运行 Windows Server 2012 的域控制器可以用于虚拟域控制器复制，而不能运行较早版本的 Windows Server 的域控制器。 但虚拟域控制器复制和虚拟域控制器在 Windows Server 2012 的安全措施没有任何功能级别要求。  
  
> [!NOTE]  
> Microsoft Exchange Server 2013 需要森林功能级别的 Windows server 2003 或更高版本。  
  
## <a name="BKMK_ServerRoles"></a>与其他服务器的角色以及 Windows 操作系统的广告 DS 互操作性  
广告 DS 不支持在以下 Windows 操作系统上：  
  
-   Windows 多点 Server  
  
-   Windows Server 2012 软件包  
  
广告 DS 无法安装以下服务器角色或角色服务也运行的服务器上：  
  
-   Hyper V 服务器  
  
-   远程桌面连接经纪人  
  
## <a name="BKMK_OpsMasters"></a>操作主机角色  
在 Windows Server 2012 的一些新功能影响操作主机角色：  
  
-   PDC 模拟器必须运行 Windows Server 2012 支持复制虚拟域控制器。 没有其他先决条件复制 Dc。 有关详细信息，请参阅[Active Directory 域服务 (广告 DS) 虚拟化](https://technet.microsoft.com/library/hh831734.aspx)。  
  
-   当 PDC 模拟器运行 Windows Server 2012 上创建新的安全原则。  
  
-   删除母版有新 RID 颁发和监控的功能。 改进包括更好的事件日志记录，更合适的限制，并能够-在紧急情况下-增加整体 RID 一位池分配。 有关详细信息，请参阅[管理不用颁发](../../ad-ds/manage/Managing-RID-Issuance.md)。  
  
> [!NOTE]  
> 但它们不是操作主机角色，广告 DS 安装在其他更改是，默认情况下，在所有运行 Windows Server 2012 的域控制器安装 DNS 服务器角色和全球目录。  
  
## <a name="BKMK_Virtual"></a>虚拟化域控制器  
在 Windows Server 2012 的广告 DS 开头改进启用更安全地虚拟化域控制器以及复制域控制器的功能。 反过来复制域控制器使其他域控制器在新域和其他享有其利益的快速部署。 有关详细信息，请参阅[Active Directory 域服务 & #40; 简介广告 DS & #41;虚拟化 & #40;级别 100 & #41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
## <a name="BKMK_Admin"></a>Windows Server 2012 服务器管理  
使用[远程服务器管理工具适用于 Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)管理域控制器和其他运行 Windows Server 2012 的服务器。 你可以在运行 Windows 8 计算机上运行 Windows Server 2012 远程服务器管理工具。  
  
## <a name="BKMK_AppCompat"></a>应用程序兼容性  
下表介绍了常用的 Active Directory 集成的 Microsoft 应用程序。 该表涵盖哪些版本的，可以安装应用程序的 Windows Server 和 Windows Server 2012 Dc 引入是否影响应用程序兼容性。  
  
|产品|笔记|  
|-----------|---------|  
|[Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2007 sp2 （包括 Configuration Manager 2007 R2 和 Configuration Manager 2007 R3）：<br /><br />Windows 8 专业版<br />Windows 8 企业版<br />Windows Server 2012 标准<br />Windows Server 2012 Datacenter**注意：**这些将为客户端完全支持，但没有任何套餐，添加用于部署这些作为操作系统使用 Configuration Manager 2007 操作系统部署功能的支持。 此外，任何站点服务器或站点系统将受 Windows Server 2012 任何 SKU 上。|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|Microsoft Office SharePoint Server 2007 不支持在 Windows Server 2012 上进行安装。|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|SharePoint 2010 Service Pack 2 需要安装和操作 <br />Windows Server 2012 服务器 SharePoint 2010<br /><br />SharePoint 2010 基础 Service Pack 2 需要安装和 SharePoint 2010 基础 Windows Server 2012 服务器上的操作<br /><br />Windows Server 2012 上的 （不带 service pack) 的 SharePoint Server 2010 安装过程无法正常工作<br /><br />SharePoint Server 2010 先决条件安装程序 (PrerequisiteInstaller.exe) 失败，错误"该程序有兼容性问题。" 单击"运行程序，而没有弄帮助"显示错误"确认如果可以安装 SharePoint & #124;SharePoint Server 2010 （不带 service pack) 无法安装在 Windows Server 2012。"|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|最低要求场的数据库服务器<br /><br />64 位版本的 Windows Server 2008 R2 Service Pack 1 (SP1) 的标准、 企业版或 Datacenter 或 64 位版本的 Windows Server 2012 标准版还是 Datacenter<br /><br />使用内置的数据库单个服务器的最低要求：<br /><br />64 位版本的 Windows Server 2008 R2 Service Pack 1 (SP1) 的标准、 企业版或 Datacenter 或 64 位版本的 Windows Server 2012 标准版还是 Datacenter<br /><br />前端的 web 服务器和农场里的应用程序服务器最低要求：<br /><br />64 位版本的 Windows Server 2008 R2 Service Pack 1 (SP1) 的标准、 企业版或 Datacenter 或 64 位版本的 Windows Server 2012 标准版还是数据中心。|  
|[Microsoft 系统中心配置管理器 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|系统中心 2012年配置管理器 Service Pack 1:<br /><br />Microsoft 将向我们发布的 Service Pack 1 的客户端支持列表中添加以下操作系统：<br /><br />Windows 8 专业版<br />Windows 8 企业版<br />Windows Server 2012 标准<br />Windows Server 2012 数据中心<br /><br />可以到使用下面的操作系统版本的服务器部署-包括网站服务器、 短信提供商和管理点的所有站点服务器角色：<br /><br />Windows Server 2012 标准<br />Windows Server 2012 数据中心|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 需要与 Windows Server 2008 R2 或 Windows Server 2012。 它能运行的服务器 Core 安装。 可以在上运行它[虚拟服务器](https://technet.microsoft.com/library/gg399035.aspx)。|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|如果可以在新 （不升级后） 安装 Windows Server 2012 上安装 Lync Server 2010 [Lync Server 2012 年 10 月累积更新](https://support.microsoft.com/?kbid=2493736)安装。 升级到 Windows Server 2012 的操作系统的 Lync Server 2010 现有安装的不受支持。 Microsoft Lync Server 2010 组聊天 Server 也在 Windows Server 2012 不支持。|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|系统中心 2012年端点保护 Service Pack 1 将更新包含以下操作系统的客户端支持列表<br /><br />Windows 8 专业版<br />Windows 8 企业版<br />Windows Server 2012 标准<br />Windows Server 2012 数据中心|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|更新汇总 1 FEP 2010 将更新包含以下操作系统的客户端支持列表：<br /><br />Windows 8 专业版<br />Windows 8 企业版<br />Windows Server 2012 标准<br />Windows Server 2012 数据中心|  
|Forefront 威胁管理网关 (TMG)|TMG 支持仅在 Windows Server 2008 和 Windows Server 2008 R2 上运行。 有关详细信息，请参阅[Forefront TMG 系统要求](https://technet.microsoft.com/library/dd896981.aspx)。|  
|Windows Server Update Services|在此版本的 WSUS 已经支持基于 Windows 8 或 Windows Server 2012 基于计算机客户端。|  
|Windows Server Update Services 3.0|更新知识库文章[2734608](https://support.microsoft.com/kb/2734608)正在运行 Windows Server Update Services (WSUS) 3.0 SP2 的使服务器更新提供给运行的 Windows 8 或 Windows Server 2012 的计算机：**注意：**独立 WSUS 3.0 SP2 环境或系统中心配置管理器 2007 Service Pack 2 环境 WSUS 3.0 sp2 的客户需要[2734608](https://support.microsoft.com/kb/2734608)正确管理基于 Windows 8 或基于 Windows Server 2012 的客户端作为的计算机。|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|Windows Server 2012 标准和 Datacenter 支持以下的角色： 架构主机全球目录 server、 域控制器，邮箱和客户端访问服务器角色<br /><br />森林功能级别： Windows Server 2003 或更高版本<br /><br />源： Exchange 2013 系统要求|  
|Exchange 2010|[源： Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />在 Windows Server 2012 成员服务器上，可以安装 Service Pack 3 Exchange 2010。<br /><br />[Exchange 2010 系统要求](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx)列出了最新受支持的方案主、 全球目录和域控制器视为 Windows Server 2008 R2。<br /><br />森林功能级别： Windows Server 2003 或更高版本|  
|SQL Server 2012|源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Windows Server 2012 上支持 RTM 的 SQL Server 2012。|  
|SQL Server 2008 R2|源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />需要 SQL Server 2008 R2 Service Pack 1 或更高版本在 Windows Server 2012 上安装。|  
|SQL Server 2008|源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />需要 SQL Server 2008 Service Pack 3 或更高版本在 Windows Server 2012 上安装。|  
|SQL Server 2005|源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />不受支持，以在 Windows Server 2012 上安装。|  
  
## <a name="BKMK_KnownIssues"></a>已知的问题  
下表列出了相关的广告 DS 安装的已知的问题。  
  
||||  
|-|-|-|  
|知识库文章号和标题|技术区域影响|问题描述月|  
|[2830145](https://support.microsoft.com/kb/2830145)： 无法在 Windows 7 或 Windows Server 2008 R2 基于域环境中的计算机上映射 SID S 1-18 1 和 SID S 1 18 2|广告 DS 管理应用程序兼容性|因为 Sid 不能解决在基于 Windows 7 或 Windows Server 2008 R2 的计算机上，映射 SID S 1-18 1 和 SID-1-18-2，是 Windows Server 2012 中的新增功能的应用可能会失败。 若要解决此问题，请在基于 Windows 7 和 Windows Server 2008 R2 基于计算机在域中安装修补程序。|  
|[2737129](https://support.microsoft.com/kb/2737129)： 时自动准备用于 Windows Server 2012 的现有域，则不会执行组策略准备|广告 DS 安装|Adprep /domainprep /gpprep 不的过程中安装的域中运行 Windows Server 2012 的第一个 DC 会自动运行。 如果它已永远不会运行之前域中，必须手动运行。|  
|[2737416](https://support.microsoft.com/kb/2737416): Windows PowerShell 基于域控制器部署重复警告|广告 DS 安装|警告可以先决条件验证过程出现，并且在安装期间然后再出现。|  
|[2737424](https://support.microsoft.com/kb/2737424)： 当你尝试从域控制器删除 Active Directory 域服务的错误"无效指定的域名称的格式"|广告 DS 安装|如果你要删除的最后一个 DC 在域中预创建的 RODC 帐户仍然存在，会出现此错误。 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008，这会影响。|  
|[2737463](https://support.microsoft.com/kb/2737463)： 域控制器未启动，c00002e2 错误发生，或将显示"选择一个选项"|广告 DS 安装|由于管理员 Dism.exe、 Pkgmgr.exe 或使用 Ocsetup.exe 要删除 DirectoryServices 域控制器角色 DC 无法启动。|  
|[2737516](https://support.microsoft.com/kb/2737516): Windows Server 2012 服务器管理器中的 IFM 验证限制|广告 DS 安装|知识库文章中所述，IFM 验证可以让限制。|  
|[2737535](https://support.microsoft.com/kb/2737535)： 安装 AddsDomainController cmdlet 返回参数设置 RODC 错误|广告 DS 安装|当你尝试要附加到 RODC 帐户的服务器，如果你在 RODC 预先创建帐户中指定已经填充的参数，你可以收到错误。|  
|[2737560](https://support.microsoft.com/kb/2737560):"无法执行 Exchange 架构冲突检查"错误，以及先决条件检查失败|广告 DS 安装|当你现有的域中配置第一个 Windows Server 2012 域控制器，因为 Dc 缺少 SeServiceLogonRight 网络服务或因为 WMI 或 DCOM 协议被阻止，先决条件检查无法正常工作。|  
|[2737797](https://support.microsoft.com/kb/2737797):-Whatif 参数 AddsDeployment 模块显示不正确 DNS 结果|广告 DS 安装|-WhatIf 参数显示 DNS 服务器未安装，但它会。|  
|[2737807](https://support.microsoft.com/kb/2737807): 下一步按钮不可用域控制器选项页面上|广告 DS 安装|由于目标直流的 IP 地址没有映射到现有子网或网站，或 DSRM 密码不是键入并确认正确，域控制器选项页面上禁用下一步按钮。|  
|[2737935](https://support.microsoft.com/kb/2737935)： 安装 active Directory 停滞在"创建 NTDS 设置对象"阶段|广告 DS 安装|安装挂起的本地管理员密码匹配域管理员密码，因为或网络问题阻止关键复制完成。|  
|[2738060](https://support.microsoft.com/kb/2738060):"拒绝访问"错误消息，当你的孩子域远程使用创建安装 AddsDomain|广告 DS 安装|如果 DNSDelegationCredential 有错误的密码使用调用命令 cmdlet 运行安装 ADDSDomain 时，你会收到的错误。|  
|[2738697](https://support.microsoft.com/kb/2738697):"服务器不可操作"域控制器配置错误时您配置服务器使用服务器管理器|广告 DS 安装|当你尝试在工作组中计算机上安装广告 DS，因为已禁用 NTLM 身份验证，你会收到此错误。|  
|[2738746](https://support.microsoft.com/kb/2738746)： 接收登录到的本地管理员域帐户后拒绝错误访问|广告 DS 安装|当你使用的本地管理员帐户，而不是内置的管理员帐户登录，然后创建一个新的域帐户不可加入域管理员组。|  
|[2743345](https://support.microsoft.com/kb/2743345):"系统找不到指定文件"Adprep /gpprep 错误或工具系统崩溃|广告 DS 安装|你收到此错误的基础结构母版实现因为运行 adprep /gpprep 时命名空间断开连接|  
|[2743367](https://support.microsoft.com/kb/2743367): Adprep"不有效 Win32 应用"错误在 Windows Server 2003，64 位版本|广告 DS 安装|你收到此错误，因为无法在 Windows Server 2003 运行 Windows Server 2012 Adprep。|  
|[2753560](https://support.microsoft.com/kb/2753560): ADMT 3.2 和 PE 3.1 Windows Server 2012 上的安装错误|ADMT|不能 ADMT 3.2 Windows Server 2012 上安装的设计。|  
|[2750857](https://support.microsoft.com/kb/2750857): DFS 复制 Internet Explorer 10 中无法正确显示诊断报告|DFS 复制|因为更改 Internet Explorer 10 中无法正确显示 DFS 复制诊断报告。|  
|[2741537](https://support.microsoft.com/kb/2741537)： 远程组策略更新会对用户可见|组策略|这是由于计划任务的每个用户都已登录的环境中运行。 Windows 任务计划程序设计需要在此情况下交互的提示。|  
|[2741591](https://support.microsoft.com/kb/2741591): ADM 文件中不会出现在 SYSVOL GPMC 基础结构状态选项|组策略|GP 复制可以报告"复制正在进行"，因为 GPMC 基础结构状态不遵循自定义过滤规则。|  
|[2737880](https://support.microsoft.com/kb/2737880)： 期间广告 DS 配置"无法启动的服务"的错误|虚拟 DC 复制|由于 DS 角色服务器服务已停用安装或删除广告 DS 或复制，时收到此错误。|  
|[2742836](https://support.microsoft.com/kb/2742836)： 当你使用复制功能直流 V 两个 DHCP 租赁为每个域控制器创建|虚拟 DC 复制|因为复制的域控制器前复制并再次已完成复制接收租赁发生这种情况。|  
|[2742844](https://support.microsoft.com/kb/2742844)： 复制失败并服务器域控制器在 Windows Server 2012 DSRM 中会重新启动|虚拟 DC 复制|复制的 DC DSRM 中开始，因为各种原因知识库文章中列出的任何复制失败。|  
|[2742874](https://support.microsoft.com/kb/2742874)： 复制域控制器不会重新创建所有服务的主要名称|虚拟 DC 复制|某些三部分 Spn 不重新创建复制 DC 上由于域重命名过程的限制。|  
|[2742908](https://support.microsoft.com/kb/2742908)： 复制域控制器后的"有没有登录服务器"错误|虚拟 DC 复制|当你尝试登录失败，因为复制复制虚拟化的 DC 并 DC DSRM 开始使用后，你会收到此错误。 作为登录。 \administrator 解决克隆失败。|  
|[2742916](https://support.microsoft.com/kb/2742916)： 复制域控制器失败，错误 8610 dcpromo.log 中|虚拟 DC 复制|由于 PDC 模拟器不具有执行域分区有可能是由于的角色传送的入站的复制复制无法正常工作。|  
|[2742927](https://support.microsoft.com/kb/2742927):"退出范围是指数"新建 AdDcCloneConfig 错误|虚拟 DC 复制|复制虚拟 Dc，因为未从提升了权限的命令提示符中运行 cmdlet 或访问令牌不包含管理员组同时运行新建 ADDCCloneConfigFile cmdlet 后，你会收到的错误。|  
|[2742959](https://support.microsoft.com/kb/2742959)： 复制域控制器失败，错误 8437:"为这个复制操作指定无效的参数"|虚拟 DC 复制|复制失败，因为指定无效克隆名称或重复 NetBIOS 名称。|  
|[2742970](https://support.microsoft.com/kb/2742970)： 复制直流失败，没有 DSRM，重复源和克隆计算机|虚拟 DC 复制|复制虚拟 DC 靴子的猫中目录服务维修模式 (DSRM)，因为 DCCloneConfig.xml 文件未正确的位置中创建或源 DC 之前复制重新启动之前作为源直流使用相同的名称。|  
|[2743278](https://support.microsoft.com/kb/2743278)： 复制错误 0x80041005 域控制器|虚拟 DC 复制|复制的 DC 启动到 DSRM，因为指定只有一个 WINS 服务器。 如果指定任何 WINS 服务器，则必须指定首选和备用 WINS 服务器。|  
|[2745013](https://support.microsoft.com/kb/2745013):"服务器不工作"错误消息如果您运行 Windows Server 2012 新建 AdDcCloneConfigFile|虚拟 DC 复制|因为服务器无法联系全球目录服务器运行新建 ADDCCloneConfigFile cmdlet 后，你会收到此错误。|  
|[2747974](https://support.microsoft.com/kb/2747974)： 复制事件 2224年域控制器提供了错误的指南|虚拟 DC 复制|事件 ID 2224 错误，指出托管的服务帐户，必须复制之前已删除。 独立的 Msa，必须先删除，但组 Msa 不会阻止复制。|  
|[2748266](https://support.microsoft.com/kb/2748266)： 升级到 Windows 8 后，无法解锁 BitLocker 加密的驱动器|BitLocker|当你尝试解锁已从 Windows 7 升级的计算机上的某个驱动器，你会收到"应用找不到"的错误。|  
  
## <a name="see-also"></a>请参阅  
[Windows Server 2012 评估资源](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Windows Server 2012 评估指南](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[安装和部署 Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
  


