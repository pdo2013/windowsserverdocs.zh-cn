---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: 将域控制器升级到 Windows Server 2012 R2 和 Windows Server 2012
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6dda30bd15bedab8ea5ca8ca2e9597e1cc196e43
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443050"
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>将域控制器升级到 Windows Server 2012 R2 和 Windows Server 2012

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题提供有关 Windows Server 2012 R2 和 Windows Server 2012 中的 Active Directory 域服务的背景信息，并解释了从 Windows Server 2008 或 Windows Server 2008 R2 升级域控制器的过程。  
  
## <a name="BKMK_UpgradeWorkflow"></a>域控制器升级步骤  
升级域的推荐方法是根据需要提升运行较新版本 Windows Server 的域控制器并降级较旧的域控制器。 该方法优于升级现有域控制器的操作系统。 此列表包括要执行之前将提升运行较新版本的 Windows Server 的域控制器的常规步骤：  
  
1. 验证目标服务器是否满足 [系统要求](https://technet.microsoft.com/library/dn303418.aspx)。  
2. 验证是否[应用程序兼容性](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)。  
3. 验证安全设置。 有关详细信息，请参阅 [与 Windows Server 2012 中 AD DS 有关的弃用功能及行为变化](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) 和 [Secure default settings in Windows Server 2008 和 Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault)。  
4. 从计划运行安装的计算机上检查与目标服务器的连接性。  
5. 检查所需操作主机角色的可用性：  

   - 若要安装运行 Windows Server 2012 在现有域和林中的第一个 DC，运行安装的计算机需要连接至架构主机以便运行 adprep/forestprep 和基础结构主机以便运行 adprep /domainprep。  
   - 若要在已扩展林架构的域中安装第一个 DC，则只需连接至结构主机即可。  
   - 若要在现有林中安装或删除域，则需要连接至域命名主机。  
   - 任何域控制器安装也要求连接至 RID 主机。  
   - 如果正在现有林中安装第一个只读域控制器，则需要为每一个应用程序目录分区（也称作非域命名上下文或 NDNC）连接至结构主机。  

6. 请确保提供必要的凭据以运行 AD DS 安装。  

   |安装操作|凭据要求|  
   |-----------------------|---------------------------|  
   |安装一个新林|目标服务器上的本地管理员|  
   |在现有林中安装一个新域|Enterprise Admins|  
   |在现有域中安装另一个 DC|Domain Admins|  
   |运行 adprep /forestprep|Schema Admins、Enterprise Admins 和 Domain Admins|  
   |运行 adprep /domainprep|Domain Admins|  
   |运行 adprep /domainprep /gpprep|Domain Admins|  
   |运行 adprep /rodcprep|Enterprise Admins|  

   你可以委托权限以安装 AD DS。 有关详细信息，请参阅 [安装管理任务](https://technet.microsoft.com/library/cc773327(WS.10).aspx)。  

下面的链接提供了通过 Windows PowerShell cmdlet 和服务器管理器，提升新的和副本 Windows Server 2012 域控制器的分步式指导说明：  

- [安装 Active Directory 域服务 (级别 100)](https://technet.microsoft.com/library/hh472162.aspx)  
- [安装新 Windows Server 2012 Active Directory 林 (级别 200)](https://technet.microsoft.com/library/jj574166.aspx)  
- [在现有域 (级别 200) 中安装副本 Windows Server 2012 域控制器](https://technet.microsoft.com/library/jj574134.aspx)  
- [安装新的 Windows Server 2012 Active Directory 子域或树域 (级别 200)](https://technet.microsoft.com/library/jj574105.aspx)  
- [安装 Windows Server 2012 Active Directory 只读域控制器 (RODC) (级别 200)](https://technet.microsoft.com/library/jj574152.aspx)  
- [设置了 Windows Server 2012 域控制器 (EN-US) 的循序渐进指南](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  

## <a name="windows-update-considerations"></a>Windows 更新注意事项

在 Windows 8 发布前，Windows 更新曾管理自己的内部计划以检查更新，并下载和安装它们。 它要求 Windows 更新代理始终在后台运行，这会消耗内存和其他系统资源。  
  
Windows 8 和 Windows Server 2012 引入了一种名为 [自动维护](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)的新功能。 自动维护整合了许多不同的功能，每个都曾用于管理自身的计划和执行逻辑。 这种整合允许所有这些组件使用更少的系统资源、持续工作、遵守新设备类型的新 [连接待机](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) 状态，并在便携设备上消耗更少的电量。  
  
由于 Windows 更新是 Windows 8 和 Windows Server 2012 中的自动维护的一部分，因此它自身内部用于设置日期和时间以安装更新的计划不再有效。 要帮助确保企业中的所有设备和计算机的重新启动行为一致且可预测（包括运行 Windows 8 和 Windows Server 2012 的设备和计算机），请参阅 Microsoft 知识库文章 [2885694](https://support.microsoft.com/kb/2885694) （或参阅 2013 年 10 月累积汇总 [2883201](https://support.microsoft.com/kb/2883201)），然后配置 WSUS 博客文章 [使 Windows 8 和 Windows Server 2012 的 Windows 更新体验更可预测 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx)中描述的策略设置。  

## <a name="BKMK_NewWS2012R2"></a>什么是 Windows Server 2012 R2 中的 AD DS 中的新增功能？

下表概述了 Windows Server 2012 R2 中的 AD DS 的新增功能，并提供关于其适用情况的更详细信息的链接。 有关某些功能的更为详细的解释（包括其要求），请参阅 [Windows Server 2012 R2 中的 Active Directory 的新增功能](https://technet.microsoft.com/library/dn268294.aspx)。  

|功能|描述|  
|-----------|---------------|  
|[Workplace Join](https://technet.microsoft.com/library/dn280945.aspx)|使信息工作人员可以将其个人设备加入他们的公司，以访问公司资源和服务。|  
|[Web 应用程序代理](https://technet.microsoft.com/library/dn280942.aspx)|使用新的远程访问角色服务提供对 Web 应用程序的访问权限。|  
|[Active Directory 联合身份验证服务](https://technet.microsoft.com/library/hh831502.aspx)|AD FS 具有简化的部署和改进功能，以使用户可以从个人设备访问资源，并帮助 IT 部门管理访问控制。|  
|[SPN 和 UPN 唯一性](https://technet.microsoft.com/library/dn535779.aspx)|运行 Windows Server 2012 R2 的域控制器阻止创建重复的服务主体名称 (SPN) 和用户主体名称 (UPN)。|  
|[Winlogon 自动重启登录 (ARSO)](https://technet.microsoft.com/library/dn535772.aspx)|使锁屏应用程序可重新启动并在 Windows 8.1 设备上可用。|  
|[TPM 密钥证明](https://technet.microsoft.com/library/dn581921.aspx)|使 CA 可以在颁发的证书中以加密方式证明证书申请者私钥实际上由受信任的平台模块 (TPM) 保护。|  
|[凭据保护和管理](https://technet.microsoft.com/library/dn408190.aspx)|用于减少凭据被盗的新凭据保护和域身份验证控件。|  
|[不推荐使用的文件复制服务 (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|还弃用 Windows Server 2003 域功能级别，因为在该功能级别上，FRS 用于复制 SYSVOL。 这意味着，当你在运行 Windows Server 2012 R2 的服务器上创建新域时，域功能级别必须是 Windows Server 2008 或更新版本。 您仍然可以添加到现有的域具有 Windows Server 2003 域功能级别; 运行 Windows Server 2012 R2 的域控制器您只是不能创建一个新的域在该级别。|  
|[新的域和林功能级别](../active-directory-functional-levels.md)|Windows Server 2012 R2 具有新的功能级别。 Windows Server 2012 R2 DFL 上提供新功能。|  
|[LDAP 查询优化器更改](https://technet.microsoft.com/library/dn535775.aspx)|对复杂查询的 LDAP 搜索效率和 LDAP 搜索时间进行了性能改进。|  
|[1644 事件改进](https://technet.microsoft.com/library/dn535775.aspx)|LDAP 搜索结果统计信息已添加到事件 ID 1644 以帮助解决疑难问题。|  
|[Active Directory 复制吞吐量改进](https://technet.microsoft.com/library/dn535775.aspx)|将最大 AD 复制吞吐量从 40Mbps 调整到 600 Mbps 左右|  

## <a name="BKMK_WhatsNewAD"></a>什么是 Windows Server 2012 中 AD DS 中的新增功能？

下表概述了 Windows Server 2012 中的 AD DS 的新增功能，并提供关于其适用情况的更详细信息的链接。 有关详细说明的某些功能，包括其要求，请参阅[What's New in Active Directory 域服务 (AD DS)](https://technet.microsoft.com/library/hh831477.aspx)。  
  
|功能|描述|  
|-----------|---------------|  
|基于 Active Directory 的激活 (AD BA)；请参阅 [批量激活概述](https://technet.microsoft.com/library/hh831612.aspx)|可以简化配置分发的任务及批量软件许可证的管理。|  
|[Active Directory 联合身份验证服务 (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|增加了通过服务器管理器安装角色、简化的信任设置、自动的信任管理、支持 SAML 协议等。|  
|Active Directory 丢失的页面刷新事件|记录带有 jet 错误 1119 的 NTDS ISAM 事件 530，以便检测有无 Active Directory 数据库的丢失页面刷新事件。|  
|[Active Directory 回收站用户界面](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Active Directory 管理中心 (ADAC) 新增了回收站功能（这项功能最初在 Windows Server 2008 R2 中引入）的图形用户界面 (GUI) 管理。|  
|[Active Directory 复制和拓扑 Windows PowerShell cmdlet](https://technet.microsoft.com/library/hh831757.aspx)|支持使用 Windows PowerShell 创建和管理 Active Directory 站点、站点链接、连接对象等。|  
|[动态访问控制](https://technet.microsoft.com/library/hh831717.aspx)|新增基于声明的授权平台，可以改进旧的访问控制模型。|  
|[细化密码策略用户界面](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|ADAC 新增了 GUI 支持，可用于创建、编辑和分配最初在 Windows Server 2008 中加入的 PSO。|  
|[组托管服务帐户 (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|一种称作 gMSA 的新安全主体类型。 可以使用相同的 gMSA 帐户在多台主机上运行各种服务。|  
|[DirectAccess 脱机加入域](https://technet.microsoft.com/library/jj574150.aspx)|可以通过包含 DirectAccess 先决条件，扩展脱机加入域。|  
|[通过虚拟域控制器 (DC) 克隆快速部署](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|通过使用 Windows PowerShell cmdlet 克隆现有的虚拟化域控制器，可以快速部署虚拟化 DC。|  
|[RID 池更改](https://technet.microsoft.com/library/jj574229.aspx)|添加新的监视事件及配额，防止过度消耗全局 RID 池。 如果最初的全局 RID 池已经用完，则可以选择将池的大小扩大一倍。|  
|安全的时间服务|通过在线删除机密、删除 MD5 哈希函数并要求服务器使用 Windows 8 时间客户端进行身份验证，可以提高 W32tm 的安全性|  
|[虚拟化 Dc 的 USN 回滚保护](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|意外还原虚拟化 DC 的快照备份将不再会导致 USN 回滚。|  
|[Windows PowerShell 历史记录查看器](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|允许管理员查看在使用 ADAC 时执行的 Windows PowerShell 命令。|  
  
### <a name="BKMK_"></a>自动维护和更改由 Windows 更新应用更新后重新启动行为

在 Windows 8 发布前，Windows 更新曾管理自己的内部计划以检查更新，并下载和安装它们。 它要求 Windows 更新代理始终在后台运行，这会消耗内存和其他系统资源。  
  
Windows 8 和 Windows Server 2012 引入了一种名为 [自动维护](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)的新功能。 自动维护整合了许多不同的功能，每个都曾用于管理自身的计划和执行逻辑。 这种整合允许所有这些组件使用更少的系统资源、持续工作、遵守新设备类型的新 [连接待机](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) 状态，并在便携设备上消耗更少的电量。  
  
由于 Windows 更新是 Windows 8 和 Windows Server 2012 中的自动维护的一部分，因此它自身内部用于设置日期和时间以安装更新的计划不再有效。 若要帮助企业中所有设备和计算机确保一致且可预测的重新启动行为（包括那些运行 Windows 8 和 Windows Server 2012 的计算机），你可以配置以下组策略设置：  

- **计算机配置 |策略 |管理模板 |Windows 组件 |Windows Update |配置自动更新**  
- **计算机配置 |策略 |管理模板 |Windows 组件 |Windows Update |不为登录的用户重新启动**  
- **计算机配置 |策略 |管理模板 |Windows 组件 |维护计划程序 |维护随机延迟**  

下表列出了一些如何配置这些设置以提供所需的重新启动行为的示例。  

|||  
|-|-|  
|**方案**|**推荐的配置**|  
|**托管的 WSUS**<br /><br />-安装更新一次每周<br />-在重新启动周五晚上 11 点|将计算机设置为自动安装，在所需时间之前阻止自动重新启动<br /><br />策略  ：配置自动更新（已启用）<br /><br />配置自动更新：4-自动下载并计划安装<br /><br />策略  ：不为登录的用户 （已禁用） 重新启动<br /><br />**WSUS 截止时间**：设置为周五晚上 11 点|  
|**托管的 WSUS**<br /><br />-跨不同的小时/天交错安装|为应该一起更新的计算机的不同组设置目标组<br /><br />为之前的方案使用上述步骤<br /><br />为不同的目标组设置不同的截止时间|  
|**非 WSUS 管理-不支持的截止时间**<br /><br />-交错安装在不同的时间|策略  ：配置自动更新（已启用）<br /><br />配置自动更新：4-自动下载并计划安装<br /><br />**注册表项：** 启用 Microsoft 知识库文章中讨论的注册表项[2835627](https://support.microsoft.com/kb/2835627)<br /><br />**策略：** 自动维护随机延迟（已启用）<br /><br />为 6 小时随机延迟将“常规维护随机延迟”  设置为 PT6H 以提供以下行为：<br /><br />-更新将安装配置的维护时间加上随机延迟<br /><br />-重新启动每台计算机将需要进行完全 3 天更高版本<br /><br />此外，为每个计算机组设置不同的维护时间|  

有关 Windows 工程团队已实现这些更改的原因的详细信息，请参阅 [在 Windows Update 的自动更新中尽量减少重新启动](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx)。  

## <a name="BKMK_InstallationChanges"></a>AD DS 服务器角色安装变更

在 Windows Server 2003 到 Windows Server 2008 R2 版本中，你需要在运行 Active Directory 安装向导 (Dcpromo.exe) 之前，先运行 Adprep.exe 命令行工具的 x86 或 X64 版本，Dcpromo.exe 包含可供选择的变体，既可以从媒体安装，也可以进行无人参与的安装。  
  
从 Windows Server 2012 开始，通过使用 Windows PowerShell 中的 ADDSDeployment 模块执行命令行安装。 基于 GUI 的升级是通过使用全新的 AD DS 配置向导在服务器管理器中执行的。 为了简化安装过程，ADPREP 已经被集成到 AD DS 安装中，并且可以根据需要自动运行。 基于 Windows PowerShell 的 AD DS 配置向导会自动将目标的域的 Dc 添加，然后相关域控制器上远程运行所需的 ADPREP 命令中的架构和基础结构主机角色。  
  
AD DS 安装向导中的先决条件检查可以在开始安装之前识别潜在的错误。 错误的条件能够得到纠正以消除仅完成部分升级的顾虑。 该向导还将导出一个 Windows PowerShell 脚本，其中包含图形安装期间指定的所有选项的。  
  
综上所述，AD DS 安装变更简化了 DC 角色的安装过程，减少了出现管理错误的可能性，当你在全局区域和域中部署多个域控制器时尤其如此。  
有关 GUI 和基于 Windows PowerShell 的安装的更多详细信息，包括命令行语法以及分步式向导的指导说明，请参阅 [安装 Active Directory 域服务](https://technet.microsoft.com/library/hh472162.aspx)。 如果管理员需要独立于现有林中 Windows Server 2012 DC 安装过程，而控制某个 Active Directory 林中架构变更的引入，则该管理员仍然可以使用提升的命令提示符运行 Adprep.exe 命令。  

## <a name="BKMK_DeprecatedFeatures"></a>已弃用的功能和 Windows Server 2012 中 AD ds 相关的行为更改

与 AD DS 有关的变化包括：  

- **不推荐使用的 Adprep32.exe**
   - Adprep.exe 只有一种版本，可以根据需要在运行 Windows Server 2008 或更高版本的 64 位服务器上运行它。 你可以远程运行 Adprep.exe，如果在 32 位操作系统或 Windows Server 2003 上托管目标操作主机角色，则必须远程运行该程序。  
- **弃用 Dcpromo.exe**
   - 尽管 Windows Server 2012 中仅它仍可以运行使用应答文件或命令行参数为组织提供时间以现有的自动化转换为新的 Windows PowerShell 安装选项，不推荐使用 Dcpromo。  
- **针对用户帐户禁用 lm 哈希**
  - Windows Server 2008、Windows Server 2008 R2 和 Windows Server 2012 上安全模板中的安全默认设置均启用了 NoLMHash 策略，而该项策略在 Windows 2000 和 Windows Server 2003 域控制器的安全模板中则是被禁用的。 请参阅知识库文章 [946405](https://support.microsoft.com/kb/946405)中介绍的步骤，按照要求，禁用依赖于 LMHash 客户端的 NoLMHash 策略。  

从 Windows Server 2008 开始，域控制器还包含下列安全默认设置，运行 Windows Server 2003 或 Windows 2000 的域控制器相比。

|||||  
|-|-|-|-|  
|加密类型或策略|Windows Server 2008 默认设置|Windows Server 2012 和 Windows Server 2008 R2 默认设置|备注|  
|AllowNT4Crypto|Disabled|Disabled|第三方服务器消息块 (SMB) 客户端可能与域控制器上的安全默认设置不兼容。 在所有情况下，可以通过放宽这些设置来允许交互操作，但这终将是以牺牲安全性为代价。 有关详细信息，请参阅[文章 942564](https://go.microsoft.com/fwlink/?LinkId=164558)在 Microsoft 知识库文章 (https://go.microsoft.com/fwlink/?LinkId=164558) 。|  
|DES|Enabled|Disabled|[文章 977321](https://go.microsoft.com/fwlink/?LinkId=177717)在 Microsoft 知识库文章 （ https://go.microsoft.com/fwlink/?LinkId=177717)|  
|集成身份验证的 CBT/扩展保护|不可用|Enabled|请参阅[Microsoft 安全公告 (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) 并[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251) Microsoft 知识库中的文章 (https://go.microsoft.com/fwlink/?LinkId=178251) 。<br /><br />查看并安装的修补程序[文章 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) 根据需要在 Microsoft 知识库。|  
|LMv2|Enabled|Disabled|[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251)在 Microsoft 知识库文章 （ https://go.microsoft.com/fwlink/?LinkId=178251)|  

## <a name="BKMK_SysReqs"></a>操作系统要求

Windows Server 2012 的最低系统要求是下表中列出的。 有关系统要求的更多信息以及预安装信息，请参阅 [安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 安装一个新的 Active Directory 林时，并无其他额外的系统要求。但是为了提高域控制器、LDAP 客户端请求以及启用了 Active Directory 的应用程序的性能，应当增加足够的内存以此来缓存 Active Directory 数据库中的内容。 如果要升级现有域控制器或将新域控制器添加到现有林，请查阅下一部分，以确保服务器满足磁盘空间要求。  

|||  
|-|-|  
|处理器|1.4 GHz 64 位处理器|  
|RAM|512 MB|  
|可用磁盘空间要求|32 GB|  
|屏幕分辨率|800 x 600 或更高|  
|其他|DVD 驱动器、键盘、Internet 访问权限|  

### <a name="BKMK_DiskSpaceDCWin8"></a>升级域控制器的磁盘空间要求

本部分介绍了仅为从 Windows Server 2008 或 Windows Server 2008 R2 升级域控制器的磁盘空间要求。 有关将域控制器升级到早期版本的 Windows Server 的磁盘空间要求的详细信息，请参阅 [升级到 Windows Server 2008 的磁盘空间要求](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) 或 [升级到 Windows Server 2008 R2 的磁盘空间要求](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2)。  
  
为了容纳自定义的和应用程序驱动的架构扩展、应用程序以及管理员启动的索引，需要向托管 Active Directory 数据库和日志文件的磁盘分配空间；此外，还需要为部署域控制器期间（通常为 5 至 8 年）添加到目录中的对象和属性提供空间。 在部署时就分配适当的磁盘空间通常是一项可靠的投资，相比较而言，如果完成部署后再扩展磁盘存储空间，则需要花费更多的成本。 有关详细信息，请参阅 [Active Directory 域服务的容量规划](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx)。  
  
在计划升级的域控制器上，请在开始升级操作系统之前，先确保托管 Active Directory 数据库 (NTDS.DIT) 的驱动器至少具有相当于 NTDS.DIT 文件大小 20% 的可用磁盘空间。 如果卷上没有足够的可用磁盘空间，升级将会失败并且升级兼容性报告将返回错误，指明可用磁盘空间不足：  
  
在这种情况下，可以尝试 Active Directory 数据库的脱机碎片整理，以重新获得附加空间，然后重试升级。 有关详细信息，请参阅 [压缩目录数据库文件（脱机碎片整理）](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx)。  
  
### <a name="available-skus"></a>可用的 SKU

有 4 个版本的 Windows Server：Foundation、Essentials、Standard 和 Datacenter。
其中，支持 AD DS 角色的两种版本分别是 Standard 和 Datacenter。  
  
在以前的发行版本中，Windows Server 各个版本在其服务器角色的支持方面、处理器计数以及大量内存支持方面均有不同。 Windows Server Standard 和 Datacenter 版本支持所有功能和基础硬件，但其虚拟化权限的不同标准版本允许两个虚拟实例和数据中心的允许无限制虚拟实例版本。  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>支持加入 Windows Server 域的 Windows 客户端和 Windows Server 操作系统

具有运行 Windows Server 2012 或更高版本的域控制器的域成员计算机支持下列 Windows 客户端和 Windows Server 操作系统：  
  
- 客户端操作系统：Windows 8.1，Windows 8、 Windows 7，Windows Vista
   - 运行 Windows 8.1 或 Windows 8 的计算机还可以加入具有运行早期版本 Windows Server（包括 Windows Server 2003 或更高版本）的域控制器的域。 但在此情况下，某些 Windows 8 功能需要其他配置或者可能不可用。 有关这些功能的详细信息和在下层域中管理 Windows 8 客户端的其他建议，请参阅 [在 Windows Server 2003 域中运行 Windows 8 成员计算机](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx)。  
- 服务器操作系统：Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2、Windows Server 2003  

## <a name="BKMK_UpgradePaths"></a>支持就地升级路径

运行 64 位版本的 Windows Server 2008 或 Windows Server 2008 R2 的域控制器可以升级到 Windows Server 2012。 运行 Windows Server 2003 或 Windows Server 2008 32 位版本的域控制器则无法升级。 若要替换它们，请在域中安装运行更新版本的 Windows Server 的域控制器，然后删除运行 Windows Server 2003 的域控制器。  

|如果运行下列版本|可以升级到这些版本|  
|-------------------------------------|-------------------------------------|  
|带有 SP2 的 Windows Server 2008 Standard<br /><br />或者<br /><br />带有 SP2 的 Windows Server 2008 Enterprise|Windows Server 2012 Standard<br /><br />或者<br /><br />Windows Server 2012 Datacenter|  
|带有 SP2 的 Windows Server 2008 Datacenter|Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|带有 SP1 的 Windows Server 2008 R2 Standard<br /><br />或者<br /><br />带有 SP1 的 Windows Server 2008 R2 Enterprise|Windows Server 2012 Standard<br /><br />或者<br /><br />Windows Server 2012 Datacenter|  
|带有 SP1 的 Windows Server 2008 R2 Datacenter|Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
有关支持的升级路径的详细信息，请参阅 [Windows Server 2012 的评估版本和升级选项](https://go.microsoft.com/fwlink/?LinkId=260917)。 注意：你无法将运行 Windows Server 2012 评估版的域控制器直接转换为零售版本。 相反，应该在服务器上安装另一个运行零售版本的域控制器，并从运行评估版的域控制器中删除 AD DS。  
  
由于的已知问题，无法升级到 Windows Server 2012 的服务器核心安装中运行 Windows Server 2008 R2 的服务器核心安装的域控制器。 在升级过程的后期，升级将会终止，彻底黑屏。 如果重新启动此类 DC，则会显示 boot.ini 文件中的一个选项，允许回滚到以前的操作系统版本。 再次重新启动将触发系统自动回滚到以前的操作系统版本。 直到有可用的解决方案，建议安装新的域控制器运行而不是就地升级现有域控制器运行 Windows Server 的服务器核心安装的服务器核心安装的 Windows Server 20122008 R2。 有关详细信息，请参阅知识库文章 [2734222](https://support.microsoft.com/kb/2734222)。  

## <a name="BKMK_FunctionalLevels"></a>功能级别的功能和要求

Windows Server 2012 要求 Windows Server 2003 林功能级别。 也就是说，可以添加到现有的 Active Directory 林中运行 Windows Server 2012 的域控制器之前，林功能级别必须是 Windows Server 2003 或更高版本。 这意味着运行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的域控制器可以在同一个林中操作，但是不支持运行 Windows 2000 Server 的域控制器，而且它会阻止安装运行 Windows Server 2012 的域控制器。 如果林包含运行 Windows Server 2003 或更高版本的域控制器，但是域功能级别仍是 Windows 2000，则安装也会被阻止。  
  
在将 Windows Server 2012 域控制器添加到林之前，必须先删除 Windows 2000 域控制器。 对于这种情况，请考虑下列工作流：  

1. 安装运行 Windows Server 2003 或更高版本的域控制器。 这些域控制器可以部署在 Windows Server 的评估版上。 作为先决条件，该步骤还要求为此操作系统版本 [运行 adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) 。  
2. 删除 Windows 2000 域控制器。 具体来说，从域中适当降级或强制性删除 Windows Server 2000 域控制器，并且使用“Active Directory 用户和计算机”，将所有已删除的域控制器的域控制器帐户删除。  
3. 将林功能级别提升到 Windows Server 2003 或更高版本。  
4. 安装运行 Windows Server 2012 的域控制器。  
5. 删除运行 Windows Server 早期版本的域控制器。  

新的 Windows Server 2012 域功能级别启用一个新功能： **KDC 支持声明、 复合身份验证和 Kerberos 保护**KDC 管理模板策略具有两个设置 (**始终为提供声明**并**未保护身份验证请求失败**) 需要 Windows Server 2012 域功能级别。  
  
Windows Server 2012 林功能级别不提供任何新功能，但它可以确保在林中创建的任何新域将自动运行在 Windows Server 2012 的域功能级别。 Windows Server 2012 域功能级别不提供声明、 复合身份验证和 Kerberos 保护的 KDC 支持之外的其他新功能。 但它会确保域中的所有域控制器都运行 Windows Server 2012。 有关不同功能级别提供的其他功能的详细信息，请参阅 [了解 Active Directory 域服务 (AD DS) 功能级别](../active-directory-functional-levels.md)。  
  
林功能级别设置为某个值之后，您不能回滚或降低林功能级别，但存在以下例外： 提升到 Windows Server 2012 林功能级别后，您可以将它降级到 Windows Server 2008 R2。 如果尚未启用 Active Directory 回收站，您还可以降低林功能级别从 Windows Server 2012 到 Windows Server 2008 R2 或 Windows Server 2008 或 Windows Server 2008 R2 到 Windows Server 2008。 如果将林功能级别设置为 Windows Server 2008 R2，则不能将其回滚，例如，为 Windows Server 2003。  
  
域功能级别设置为某个值之后，您不能回滚或降低域功能级别，但存在以下例外： 当将域功能级别提升到 Windows Server 2008 R2 或 Windows Server 2012，并且如果林函数命名nal 级别为 Windows Server 2008 或更低时，可以滚动域功能级别降级到 Windows Server 2008 或 Windows Server 2008 R2 的选项。 您可以降低域功能级别仅从 Windows Server 2012 到 Windows Server 2008 R2 或 Windows Server 2008 或 Windows Server 2008 R2 到 Windows Server 2008。 如果域功能级别设置为 Windows Server 2008 R2，则不能将其回滚，例如，为 Windows Server 2003。  
  
有关较低功能级别提供的功能的详细信息，请参阅 [了解 Active Directory 域服务 (AD DS) 功能级别](../active-directory-functional-levels.md)。  
  
超过功能级别时，运行 Windows Server 2012 的域控制器提供了在运行早期版本的 Windows Server 的域控制器不可用的其他功能。 例如，运行 Windows Server 2012 的域控制器可以使用虚拟域控制器克隆，而运行早期版本的 Windows Server 的域控制器不能。 但是，虚拟域控制器克隆和 Windows Server 2012 中的虚拟域控制器保护无任何功能级别要求。  

> [!NOTE]  
> Microsoft Exchange Server 2013 要求林功能级别为 Windows server 2003 或更高版本。  

## <a name="BKMK_ServerRoles"></a>与其他服务器角色和 Windows 操作系统的 AD DS 互操作性

下列 Windows 操作系统不支持 AD DS：  
  
- Windows MultiPoint Server  
- Windows Server 2012 Essentials  
  
无法将 AD DS 安装在同时运行下列服务器角色或角色服务的服务器上：  
  
- Hyper-V Server  
- 远程桌面连接代理  
  
## <a name="BKMK_OpsMasters"></a>操作主机角色

Windows Server 2012 中的一些新功能影响操作主机角色：  

- PDC 仿真器必须运行 Windows Server 2012，以支持克隆虚拟域控制器。 克隆 DC 存在附加先决条件。 有关详细信息，请参阅 [Active Directory 域服务 (AD DS) 虚拟化](https://technet.microsoft.com/library/hh831734.aspx)。  
- PDC 仿真器运行 Windows Server 2012 时，将创建新的安全主体。  
- RID 主体具有新 RID 颁发和监视功能。 改进包括更好的事件日志记录、更合适的限制以及在紧急情况下将总体 RID 池分配增加 1 位的功能。 有关详细信息，请参阅 [Managing RID Issuance](../../ad-ds/manage/Managing-RID-Issuance.md)。  

> [!NOTE]  
> 尽管它们不是操作主机角色，但 AD DS 安装中的另一个更改是默认情况下，运行 Windows Server 2012 的所有域控制器上安装 DNS 服务器角色和全局编录。  

## <a name="BKMK_Virtual"></a>虚拟化域控制器

中开始 Windows Server 2012 中的 AD DS 改进支持域控制器的安全虚拟化和克隆域控制器的功能。 而克隆域控制器又支持在新域中快速部署其他域控制器和其他好处。 有关详细信息，请参阅[Active Directory 域服务简介&#40;AD DS&#41;虚拟化&#40;Level 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。  

## <a name="BKMK_Admin"></a>管理 Windows Server 2012 服务器

使用[远程服务器管理工具为 Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)来管理域控制器和运行 Windows Server 2012 的其他服务器。 您可以在运行 Windows 8 的计算机上运行 Windows Server 2012 远程服务器管理工具。  

## <a name="BKMK_AppCompat"></a>应用程序兼容性

下表包含了常见的集成 Active Directory 的 Microsoft 应用程序。 下表列出了可安装应用程序的 Windows Server 版本以及引入 Windows Server 2012 DC 是否会影响应用程序的兼容性。  

|产品|说明|  
|-----------|---------|  
|[Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|带有 SP2 的 Configuration Manager 2007（包括 Configuration Manager 2007 R2 和 Configuration Manager 2007 R3）：<br /><br />-   Windows 8 Pro<br />-Windows 8 企业版<br />-   Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter**注意：** 尽管这些应用程序作为客户端可以得到完全支持，但是，目前尚不支持使用 Configuration Manager 2007 操作系统部署功能将这些应用程序部署为操作系统。 此外，任何 Windows Server 2012 的 SKU 上都不支持站点服务器或站点系统。|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|不支持在 Windows Server 2012 上安装 Microsoft Office SharePoint Server 2007。|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|在 Windows Server 2012 服务器上安装和操作 SharePoint 2010 时， <br />要求提供 SharePoint 2010 Service Pack 2<br /><br />在 Windows Server 2012 服务器上安装和操作 SharePoint 2010 Foundation 时，要求提供 SharePoint 2010 Foundation Service Pack 2<br /><br />无法在 Windows Server 2012 上安装 SharePoint Server 2010（没有 Service Pack）<br /><br />SharePoint Server 2010 必备安装程序 (PrerequisiteInstaller.exe) 失败，出现错误"此程序存在兼容性问题。" 单击"运行程序而不获取帮助"时会显示错误"验证是否可以安装 SharePoint &#124; SharePoint Server 2010 （没有 service pack) 不能安装在 Windows Server 2012。"|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|针对服务器场中数据库服务器的最低要求：<br /><br />Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter 的 64 位版本，或者 Windows Server 2012 Standard 或 Datacenter 的 64 位版本<br /><br />针对带有内置数据库的单个服务器的最低要求：<br /><br />Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter 的 64 位版本，或者 Windows Server 2012 Standard 或 Datacenter 的 64 位版本<br /><br />针对服务器场中前端 Web 服务器和应用程序服务器的最低要求：<br /><br />Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter 的 64 位版本，或者 Windows Server 2012 Standard 或 Datacenter 的 64 位版本。|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Configuration Manager Service Pack 1：<br /><br />随着 Service Pack 1 的发布，Microsoft 将会向我们的客户端支持矩阵添加下列操作系统：<br /><br />-   Windows 8 Pro<br />-Windows 8 企业版<br />-   Windows Server 2012 Standard<br />-   Windows Server 2012 Datacenter<br /><br />可以将所有站点服务器角色 - 包括站点服务器、SMS 提供程序以及管理点 - 部署到具有下列操作系统版本的服务器中：<br /><br />-   Windows Server 2012 Standard<br />-   Windows Server 2012 Datacenter|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 要求与 Windows Server 2008 R2 或 Windows Server 2012 一起使用。 它无法运行在服务器核心安装上。 它可以运行在 [虚拟服务器](https://technet.microsoft.com/library/gg399035.aspx)上。|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|如果安装了 [Lync Server 2012 年 10 月的累计更新](https://support.microsoft.com/?kbid=2493736) ，则可以将 Lync Server 2010 安装到全新（并非升级）的 Windows Server 2012 安装中。 不支持针对现有的 Lync Server 2010 安装，将操作系统升级至 Windows Server 2012。 此外，Windows Server 2012 上也不支持 Microsoft Lync Server 2010 群聊服务器。|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint Protection Service Pack 1 将更新客户端支持矩阵以包含下列操作系统：<br /><br />-   Windows 8 Pro<br />-Windows 8 企业版<br />-   Windows Server 2012 Standard<br />-   Windows Server 2012 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|FEP 2010 更新汇总 1 将更新客户端支持矩阵以包括下列操作系统：<br /><br />-   Windows 8 Pro<br />-Windows 8 企业版<br />-   Windows Server 2012 Standard<br />-   Windows Server 2012 Datacenter|  
|Forefront Threat Management Gateway (TMG)|只支持 TMG 在 Windows Server 2008 和 Windows Server 2008 R2 上运行。 有关详细信息，请参阅 [Forefront TMG 系统要求](https://technet.microsoft.com/library/dd896981.aspx)。|  
|Windows Server 更新服务|此版本的 WSUS 已经支持基于 Windows 8 的计算机或支持基于 Windows Server 2012 的计算机作为客户端。|  
|Windows Server Update Services 3.0|更新的知识库文章 [2734608](https://support.microsoft.com/kb/2734608) 允许运行 Windows Server Update Services (WSUS) 3.0 SP2 的服务器为运行 Windows 8 或 Windows Server 2012 的计算机提供更新：**注意：** 具有独立 WSUS 3.0 SP2 环境的客户或具有包含 WSUS 3.0 SP2 的 System Center Configuration Manager 2007 Service Pack 2 环境的客户应当了解 [2734608](https://support.microsoft.com/kb/2734608)，以便正确地将基于 Windows 8 的计算机或基于 Windows Server 2012 的计算机作为客户端进行管理。|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|下列服务器角色支持 Windows Server 2012 Standard 和 Datacenter：架构主机、全局编录服务器、域控制器、邮箱和客户端访问服务器角色<br /><br />林功能级别：Windows Server 2003 或更高版本<br /><br />源：Exchange 2013 系统要求|  
|Exchange 2010|[来源：Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />可以在 Windows Server 2012 成员服务器上安装带有 Service Pack 3 的 Exchange 2010。<br /><br />对于 Windows Server 2008 R2，[Exchange 2010 系统要求](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) 列出了最新支持的架构主机、全局编录服务器和域控制器。<br /><br />林功能级别：Windows Server 2003 或更高版本|  
|SQL Server 2012|源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Windows Server 2012 上支持 SQL Server 2012 RTM。|  
|SQL Server 2008 R2|源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />要求在 Windows Server 2012 上安装带有 Service Pack 1 的 SQL Server 2008 R2 或更高版本。|  
|SQL Server 2008|源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />要求在 Windows Server 2012 上安装带有 Service Pack 3 的 SQL Server 2008 或更高版本。|  
|SQL Server 2005|源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />不支持在 Windows Server 2012 上进行安装。|  

## <a name="BKMK_KnownIssues"></a>已知的问题

下表列出了与 AD DS 安装有关的已知问题。  

||||  
|-|-|-|  
|知识库文章编号及标题|影响的技术区域|问题/描述|  
|[2830145](https://support.microsoft.com/kb/2830145):在域环境中，SID S-1-18-1 和 SID S-1-18-2 无法映射到基于 Windows 7 或 Windows Server 2008 R2 的计算机上|AD DS 管理/应用兼容|映射 SID S-1-18-1 和 SID S-1-18-2 的应用程序（Windows Server 2012 中的新增应用程序）可能失败，因为 SID 无法在基于 Windows 7 或基于 Windows Server 2008 R2 的计算机上解析。 若要解决此问题，请在域中基于 Windows 7 和 Windows Server 2008 R2 的计算机上安装修补程序。|  
|[2737129](https://support.microsoft.com/kb/2737129):当你为 Windows Server 2012 自动准备现有域时，不执行组策略准备|AD DS 安装|作为在域中安装第一个运行 Windows Server 2012 的 DC 的一部分，Adprep /domainprep /gpprep 不会自动运行。 如果以前从未在域中运行它，则必须对它进行手动运行。|  
|[2737416](https://support.microsoft.com/kb/2737416):基于 Windows PowerShell 的域控制器部署将重复警告|AD DS 安装|警告不仅会在先决条件验证期间出现，而且还会在安装期间重复出现。|  
|[2737424](https://support.microsoft.com/kb/2737424):当你尝试从域控制器删除 Active Directory 域服务时，出现“指定域名的格式无效”错误|AD DS 安装|当域中仍存在预创建的 RODC 帐户时，如果删除域中最后一个 DC，则会显示此错误。 这会影响 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008。|  
|[2737463](https://support.microsoft.com/kb/2737463):域控制器不启动、发生 c00002e2 错误或显示“选择一个选项”|AD DS 安装|没有启动 DC 是因为管理员使用了 Dism.exe、Pkgmgr.exe 或 Ocsetup.exe 来删除 DirectoryServices-DomainController 角色。|  
|[2737516](https://support.microsoft.com/kb/2737516):Windows Server 2012 服务器管理器中的 IFM 验证限制|AD DS 安装|如知识库文章中所述，IFM 验证可能存在限制。|  
|[2737535](https://support.microsoft.com/kb/2737535):Install-addsdomaincontroller cmdlet 返回 RODC 的参数集错误|AD DS 安装|当你尝试将服务器与 RODC 帐户关联时，如果指定的实际参数已经填充到预创建的 RODC 帐户中，则会收到一个错误消息。|  
|[2737560](https://support.microsoft.com/kb/2737560):“无法执行 Exchange 架构冲突检查”错误，而且先决条件检查失败|AD DS 安装|当你在现有域中配置第一个 Windows Server 2012 DC 时，先决条件检查失败。这是因为 DC 缺少网络服务的 SeServiceLogonRight，或者是因为 WMI 或 DCOM 协议被阻止。|  
|[2737797](https://support.microsoft.com/kb/2737797):带有 -Whatif 参数的 AddsDeployment 模块显示不正确的 DNS 结果|AD DS 安装|-WhatIf 参数显示 DNS 服务器将不会安装，但它将是。|  
|[2737807](https://support.microsoft.com/kb/2737807):“域控制器选项”页上不提供“下一步”按钮|AD DS 安装|“域控制器选项”页面上的“下一步”按钮被禁用是因为目标 DC 的 IP 地址没有映射到现有子网或站点，或者是因为没有正确键入或确认 DSRM 密码。|  
|[2737935](https://support.microsoft.com/kb/2737935):Active Directory 安装停留在“正在创建 NTDS 设置对象”阶段|AD DS 安装|安装挂起是因为本地管理员密码匹配域管理员密码，或者是因为网络问题阻止完成关键复制。|  
|[2738060](https://support.microsoft.com/kb/2738060):当你使用 Install-AddsDomain 远程创建子域时，显示“访问被拒绝”错误消息|AD DS 安装|使用 Invoke-Command cmdlet 运行 Install-ADDSDomain 时，如果 DNSDelegationCredential 有一个错误的密码，则会收到此错误。|  
|[2738697](https://support.microsoft.com/kb/2738697):通过服务器管理器配置服务器时，出现“服务器不可操作”域控制器配置错误|AD DS 安装|尝试在工作组计算机上安装 AD DS 时会收到此错误，因为 NTLM 身份验证被禁用。|  
|[2738746](https://support.microsoft.com/kb/2738746):登录到本地管理员域帐户后，收到访问被拒绝错误|AD DS 安装|如果你使用本地管理员帐户而不是内置的管理员帐户登录，然后创建新域，则该帐户将不会添加到 Domain Admins 组中。|  
|[2743345](https://support.microsoft.com/kb/2743345):“系统找不到指定的文件”Adprep /gpprep 错误或工具故障|AD DS 安装|运行 adprep /gpprep 时会收到此错误，因为结构主机正在实现一个非连续命名空间|  
|[2743367](https://support.microsoft.com/kb/2743367):在 64 位版本的 Windows Server 2003 上出现 Adprep“不是有效的 Win32 应用程序”错误|AD DS 安装|收到此错误是因为 Windows Server 2012 Adprep 无法在 Windows Server 2003 上运行。|  
|[2753560](https://support.microsoft.com/kb/2753560):在 Windows Server 2012 上出现 ADMT 3.2 和 PES 3.1 安装错误|ADMT|根据设计，无法在 Windows Server 2012 上安装 ADMT 3.2。|  
|[2750857](https://support.microsoft.com/kb/2750857):DFS 复制诊断报告不能在 Internet Explorer 10 中正确显示|DFS 复制|由于 Internet Explorer 10 中的变化，无法正确显示 DFS 复制诊断报告。|  
|[2741537](https://support.microsoft.com/kb/2741537):用户可以看到远程组策略更新|组策略|这是因为计划任务运行在每个登录用户的上下文中。 Windows 任务计划程序设计要求在此方案中出现一个交互式提示。|  
|[2741591](https://support.microsoft.com/kb/2741591):在 GPMC 基础结构状态选项的 SYSVOL 中未显示 ADM 文件|组策略|因为 GPMC 基础结构状态未遵循自定义筛选规则，GP 复制可能报告"正在进行复制"。|  
|[2737880](https://support.microsoft.com/kb/2737880):配置 AD DS 期间出现“无法启动服务”错误|虚拟 DC 克隆|当安装或删除 AD DS，或者克隆 AD DS 时，会收到此错误，因为 DS 角色服务器服务已被禁用。|  
|[2742836](https://support.microsoft.com/kb/2742836):使用 VDC 克隆功能时，为每个域控制器创建了两个 DHCP 租约|虚拟 DC 克隆|出现这种情况是因为在克隆之前，克隆的域控制器收到一个租约，而且在克隆结束时，又收到了一个租约。|  
|[2742844](https://support.microsoft.com/kb/2742844):域控制器克隆失败，服务器在 Windows Server 2012 中以 DSRM 模式重新启动|虚拟 DC 克隆|由于克隆因知识库文章中所列的任意原因而失败，所以克隆的 DC 以 DSRM 模式启动。|  
|[2742874](https://support.microsoft.com/kb/2742874):域控制器克隆不会重新创建所有服务主体名称|虚拟 DC 克隆|因为域重命名过程中的限制，克隆后的 DC 上没有重新创建某些由三部分构成的 SPN。|  
|[2742908](https://support.microsoft.com/kb/2742908):克隆域控制器后出现“无可用登录服务”错误|虚拟 DC 克隆|当你在克隆虚拟化的 DC 后尝试登录时，会收到此错误，这是因为克隆失败并且 DC 是以 DSRM 模式启动的。 以管理员身份登录时可以解决此克隆故障。|  
|[2742916](https://support.microsoft.com/kb/2742916):域控制器克隆失败，dcpromo.log 中出现错误 8610|虚拟 DC 克隆|克隆失败，因为 PDC 仿真器未曾执行域分区的入站复制，这可能是由于角色传送造成的。|  
|[2742927](https://support.microsoft.com/kb/2742927):“索引超出范围”New-AdDcCloneConfig 错误|虚拟 DC 克隆|在克隆虚拟 DC 期间，当运行 New-ADDCCloneConfigFile cmdlet 后会收到此错误，这可能是因为没有从提升的命令提示符中运行该 cmdlet，或者是因为你的访问令牌不包含管理员组。|  
|[2742959](https://support.microsoft.com/kb/2742959):域控制器克隆失败，出现错误 8437：“为这个复制操作指定了一个无效的参数”|虚拟 DC 克隆|克隆失败，因为指定了无效的克隆名称或重复的 NetBIOS 名称。|  
|[2742970](https://support.microsoft.com/kb/2742970):DC 克隆失败，没有 DSRM、重复的源和克隆计算机|虚拟 DC 克隆|克隆后的虚拟 DC 使用重复名称作为源 DC，以目录服务修复模式 (DSRM) 启动，这是因为没有在正确的位置创建 DCCloneConfig.xml 文件，或者因为源 DC 在克隆前已经重新启动。|  
|[2743278](https://support.microsoft.com/kb/2743278):域控制器克隆错误 0x80041005|虚拟 DC 克隆|克隆后的 DC 以 DSRM 模式启动，因为仅指定了一个 WINS 服务器。 如果指定了任意的 WINS 服务器，则必须同时指定首选的和备用的 WINS 服务器。|  
|[2745013](https://support.microsoft.com/kb/2745013):如果在 Windows Server 2012 中运行 New-AdDcCloneConfigFile，则会出现“该服务器不可操作”的错误消息|虚拟 DC 克隆|在运行 New-ADDCCloneConfigFile cmdlet 后会收到此错误，这是因为服务器无法联系全局编录服务器。|  
|[2747974](https://support.microsoft.com/kb/2747974):域控制器克隆事件 2224 提供了不正确的指导|虚拟 DC 克隆|事件 ID 2224 错误地指出在克隆之前必须删除托管服务帐户。 必须删除独立的 MSA，然而组 MSA 并不阻止克隆。|  
|[2748266](https://support.microsoft.com/kb/2748266)：在升级到 Windows 8 后，无法解锁 BitLocker 加密的驱动器|BitLocker|当你尝试解锁从 Windows 7 升级的计算机上的驱动器时收到"找不到应用程序"错误。|  

## <a name="see-also"></a>请参阅

[Windows Server 2012 评估资源](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Windows Server 2012 评估指南](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[安装和部署 Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
