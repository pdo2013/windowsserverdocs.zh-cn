---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: "虚拟化域控制器疑难解答"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 63f96e7a3035bc25f7a7a349acb6bf26f62a2a3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-troubleshooting"></a>虚拟化域控制器疑难解答

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题提供的疑难解答虚拟化的域控制器功能的详细的方法。  
  
-   [故障排除虚拟化域控制器复制](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [疑难解答虚拟化的域控制器安全还原](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>简介  
改进自己的技能，疑难解答，最重要的方式是版本的测试实验，并且严格检查 normal、 工作方案。 如果你遇到了错误，它们是明显更容易理解，因为然后具有稳固域控制器升级的工作方式的基础。 这允许你生成您分析和网络的分析技巧。 这会对所有分布式的系统技术，只需虚拟化的域控制器部署。  
  
高级故障排除域控制器配置的关键元素是：  
  
1.  线性分析结合对焦，并且关注的详细信息。  
  
2.  了解网络捕获分析  
  
3.  了解内置日志  
  
第一个和第二个都范围之外的本主题中，但三某些详细介绍此内容。 虚拟化的域控制器疑难解答需要逻辑和线性的方法。 该键接近使用提供的数据问题，仅当你已经用尽提供的输出和日志记录借助复杂的工具和分析。  
  
## <a name="BKMK_TshootVDCCloning"></a>故障排除虚拟化域控制器复制  
此部分介绍：  
  
-   [以获取疑难解答工具](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [登录选项](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [常规方法以获取疑难解答域控制器复制](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [服务器 Core 和事件日志](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [具体问题疑难解答](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
疑难解答策略的虚拟化的域控制器复制遵循下面的通用格式：  
  
![虚拟 dc 疑难解答](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>以获取疑难解答工具  
  
#### <a name="BKMK_LoggingOptions"></a>登录选项  
内置的日志，则使用域控制器复制解决问题的最重要的工具。 所有这些日志已启用，并且配置为最大的详细级别，默认情况。  
  
|||  
|-|-|  
|**操作**|**日志**|  
|**复制**|-事件 viewer\Windows logs\System<br />-事件 viewer\Applications 和服务 logs\Directory 服务<br />-%systemroot%\debug\dcpromo.log|  
|**升级**|-%systemroot%\debug\dcpromo.log<br />-事件 viewer\Applications 和服务 logs\Directory 服务<br />-事件 viewer\Windows logs\System<br />-事件 viewer\Applications 和服务 logs\File 复制服务<br />-事件 viewer\Applications 和服务 logs\DFS 复制|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>用于域控制器配置疑难解答工具和命令  
若要解决无法通过日志介绍此内容的问题，请使用以下工具作为起始点：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   网络监视器 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>常规方法以获取疑难解答域控制器复制  
  
1.  启动到 DS 修复模式 (DSRM) VM？ 这表示是必需的疑难解答。 若要在 DSRM 登录，使用**。 \Administrator**帐户，并指定 DSRM 密码。  
  
    1.  检查 Dcpromo.log。  
  
        1.  是否初始克隆步骤成功，但失败域控制器升级？  
  
        2.  错误指示吗问题与本地域控制器或广告 DS 环境中，如 PDC 模拟器从返回错误？  
  
    2.  检查系统和目录服务的事件日志和 dccloneconfig.xml CustomDCCloneAllowList.xml  
  
        1.  执行兼容的应用程序需要在 CustomDCCloneAllowList.xml 允许列表？  
  
        2.  复制或在 dccloneconfig.xml 无效是 IP 地址或计算机名称？  
  
        3.  中的无效 Active Directory 站点 dccloneconfig.xml？  
  
        4.  IP 地址设置不会在 dccloningconfig.xml 和没有 DHCP 服务器？  
  
        5.  联机和 RPC 协议提供是 PDC 模拟器？  
  
        6.  是的域控制器克隆域控制器组成员的？ 是的权限**允许创建自身的副本 DC**域根该组上设置？  
  
        7.  Dccloneconfig.xml 文件是否包含防止正确分析的语法错误？  
  
        8.  虚拟机监控程序受支持？  
  
        9. 复制开始成功后，是否域控制器推广无法？  
  
        10. 自动生成域控制器名称 (9999) 超出最大数有多大？  
  
        11. 被重复的 MAC 地址？  
  
2.  是否该副本的主名称源 DC 相同？  
  
    1.  是否允许位置之一 Dccloneconfig.xml 文件？  
  
3.  插入正常模式下启动 VM 和复制完成后，但域控制器无法正常工作？  
  
    1.  首先检查是否更改克隆主机名称。 如果主机名为不同，已完成至少部分复制。  
  
    2.  域控制器是否有源域控制器 dccloneconfig.xml，从重复的 IP 地址，但源域控制器在复制处于脱机状态？  
  
    3.  如果域控制器广告，视为不得不复制无任何正常后推广问题的问题。  
  
    4.  如果未公布域控制器，检查目录服务、 系统、 应用程序、 文件复制和 DFS 复制事件日志中的促销后错误。  
  
#### <a name="disabling-dsrm-boot"></a>禁用显示器的 DSRM 启动  
一旦引导至 DSRM 任何错误，由于诊断失败的原因，如果 dcpromo.log 并不表示： 复制无法重试，修复失败的原因并重置 DSRM 标志。 失败的克隆没有在下次重新启动; 自行返回到正常模式你必须以尝试再次复制删除 DS 恢复模式下启动标志。 所有这些步骤需要以提升管理员身份运行。  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>删除与 Msconfig.exe DSRM  
若要打开关闭使用 GUI DSRM 启动，请使用系统配置工具：  
  
1.  运行 msconfig.exe  
  
2.  在**启动**选项卡下**启动选项**，取消选择**安全启动**(已所选的选项**Active Directory 修复**已启用)  
  
3.  单击确定以及提示时重新启动  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>删除与 Bcdedit.exe DSRM  
若要从命令行关闭 DSRM 启动，使用启动配置数据官方商城编辑器：  
  
1.  打开 CMD 提示符并运行：  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  重启计算机：  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe 操作也适用于 Windows PowerShell 主机。 有命令：  
>   
> Bcdedit.exe /deletevalue 安全引导  
>   
> 重启计算机  
  
### <a name="BKMK_ServerCoreEvents"></a>服务器 Core 和事件日志  
事件日志中包含许多关于虚拟化的域控制器克隆操作有用的信息。 默认情况下，Windows Server 2012 计算机安装是服务器核心安装，这意味着没有任何图形界面，因此无法运行本地事件查看器管理单元。  
  
若要复查所运行的服务器 Core 安装的服务器上的事件日志：  
  
-   本地运行 Wevtutil.exe 工具  
  
-   本地运行 PowerShell cmdlet 获取 WinEvent  
  
-   如果你已启用"远程事件日志管理"组 （或等效端口），以允许站的通信的 Windows 高级防火墙规则，你可以管理使用远程 Eventvwr.exe、 wevtutil.exe 或获取 Winevent 事件日志。 这可以在使用 Windows PowerShell 3.0 中的 NETSH.exe、 组策略或新组 NetFirewallRule cmdlet 服务器核心安装完成。  
  
> [!WARNING]  
> 不要图形 shell 将重新添加到计算机时 DSRM。 Windows 维护服务堆栈 (CBS) 无法在安全模式或 DSRM 正常运行。 尝试添加功能或 DSRM 中的角色将无法完成并导致计算机处于不稳定状态，直到它正常启动。 由于虚拟化的域控制器克隆 DSRM 中无法正常，启动，并且应该不会启动通常在大多数情况下，则无法安全添加图形 shell。 执行此操作不受支持，并且可能会使你与不可用的服务器。  
  
### <a name="BKMK_SpecificProblems"></a>具体问题疑难解答  
  
#### <a name="events"></a>事件  
向克隆域控制器 VM 目录服务事件日志写入的所有虚拟化的域控制器克隆事件。 应用程序、 文件复制服务，并 DFS 复制的事件日志，还可能包含失败复制的一些有用的疑难解答信息。 到 PDC 仿真器 RPC 呼叫期间的失败可能是 PDC 仿真器的事件日志中可用。  
  
下面是 Windows Server 2012 复制特定的事件在目录服务事件日志中，使用笔记和推荐的错误的解决方法。  
  
##### <a name="directory-services-event-log"></a>目录服务事件日志  
  
|||  
|-|-|  
|**事件 ID**|**2160**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|在本地* <COMPUTERNAME> *发现了虚拟域控制器复制配置文件。<br /><br />在找到虚拟域控制器复制配置文件： %1<br /><br />虚拟域控制器克隆配置文件中的存在指示本地虚拟域控制器另一个虚拟域控制器的副本。 * <COMPUTERNAME> *将开始复制本身。|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。 检查 DSA 工作目录、 %systemroot%\ntds 和根处的任何本地或可移动磁盘 dcclconeconfig.xml 文件。|  
  
|||  
|-|-|  
|**事件 ID**|**2161**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|在本地* <COMPUTERNAME> *找不到虚拟域控制器复制配置文件。 本地计算机不是复制的 DC。|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。 检查 DSA 工作目录、 %systemroot%\ntds 和根处的任何本地或可移动磁盘 dcclconeconfig.xml 文件。|  
  
|||  
|-|-|  
|**事件 ID**|**2162**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|虚拟域控制器复制失败。<br /><br />请检查系统事件日志和虚拟域控制器复制尝试对应的错误的详细信息的 %systemroot%\debug\dcpromo.log 中记录的事件。<br /><br />错误代码： %1|  
|**笔记和分辨率**|按照消息说明进行操作，此错误是全部捕获。|  
  
|||  
|-|-|  
|**事件 ID**|**2163**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|DsRoleSvc 服务已经启动要复制的本地虚拟域控制器。|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。 检查 DSA 工作目录、 %systemroot%\ntds 和根处的任何本地或可移动磁盘 dcclconeconfig.xml 文件。|  
  
|||  
|-|-|  
|**事件 ID**|**2164**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*无法启动复制本地虚拟域控制器 DsRoleSvc 服务。|  
|**笔记和分辨率**|检查 DS 角色服务器服务 (DsRoleSvc) 的服务设置并确保其开始键入设置为手动。 验证任何第三方程序正在阻止该服务的开头。|  
  
|||  
|-|-|  
|**事件 ID**|**2165**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*无法启动期间的本地虚拟域控制器复制的线程。<br /><br />错误代码: %1<br /><br />错误消息: %2<br /><br />线程名称: %3|  
|**笔记和分辨率**|联系 Microsoft 产品的支持人员|  
  
|||  
|-|-|  
|**事件 ID**|**2166**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*需要 RPCSS 服务以启动到 DSRM 重新启动。 等待 RPCSS 用于初始化进入失败正在运行的状态。<br /><br />错误代码: %1|  
|**笔记和分辨率**|检查系统事件日志和服务设置 RPC 服务器服务 (Rpcss)|  
  
|||  
|-|-|  
|**事件 ID**|**2167**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*不能初始化虚拟域控制器知识。 查看以前的事件日志条目的详细信息。<br /><br />其他数据<br /><br />故障代码 %1|  
|**笔记和分辨率**|按照消息说明进行操作，此错误是全部捕获。|  
  
|||  
|-|-|  
|**事件 ID**|**2168**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|Microsoft 的 Windows ActiveDirectory_DomainService<br /><br />在受支持的虚拟机监控程序运行的域控制器。 检测到 VM 代 ID。<br /><br />当前值 VM 代 ID: %1|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2169**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|没有检测到无 VM 代 ID。 DC 驻留在物理计算机、 HYPER-V 的低级别版本或虚拟机监控程序不支持 VM 代 id。<br /><br />其他数据<br /><br />返回检查 VM 代： 1%时失败代码|  
|**笔记和分辨率**|如果不打算复制，这是一个成功事件。 否则为检查系统事件日志中，并查看虚拟机监控程序产品支持的文档。|  
  
|||  
|-|-|  
|**事件 ID**|**2170**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|警告|  
|**消息**|已检测到代 ID 更改。<br /><br />缓存 DS （旧值） 在代 ID: 1%<br /><br />当前在虚拟机 （新值） 代 ID: %2<br /><br />在应用虚拟机快照、 虚拟机导入操作或之后实时迁移操作后，会发生代 ID 更改。 *<COMPUTERNAME>*将创建新的调用 ID 恢复域控制器。 不应使用虚拟机快照恢复虚拟化的域控制器。 受支持的方法，若要还原或回退 Active Directory 域服务数据库中的内容是使用 Active Directory 域服务感知备份应用程序进行系统状态备份还原。|  
|**笔记和分辨率**|如果想要复制，这是一个成功事件。 否则，检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2171**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|已检测到无代 ID 更改。<br /><br />缓存 DS （旧值） 在代 ID: 1%<br /><br />当前在虚拟机 （新值） 代 ID: %2|  
|**笔记和分辨率**|这是一个成功事件，如果不打算复制，并且应看到在虚拟化 DC 每次重启。 否则，检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2172**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|阅读域控制器计算机对象的 msDS GenerationId 属性。<br /><br />msDS GenerationId 特性值: %1|  
|**笔记和分辨率**|如果想要复制，这是一个成功事件。 否则，检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2173**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|无法读取域控制器计算机对象的 msDS GenerationId 属性。 这可能由数据库交易失败，或代 id 不存在本地数据库中。 后 dcpromo 或直流不虚拟域控制器在首次重新启动期间 msDS GenerationId 不存在。<br /><br />其他数据<br /><br />故障代码 %1|  
|**笔记和分辨率**|如果想要复制，这是一个成功事件和复制完成后，它是首次 VM 重新启动。 此外可以忽略非虚拟域控制器上。 否则，检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2174**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|DC 是简虚拟域控制器克隆以及恢复虚拟的域控制器快照。|  
|**笔记和分辨率**|如果不打算复制，这是一个成功事件。 否则，检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2175**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|不受支持平台上，存在虚拟域控制器克隆配置文件。|  
|**笔记和分辨率**|发生这种情况时找到 dccloneconfig.xml 但 VM 代 ID 无法找到，例如当 dccloneconfig.xml 文件位于物理计算机上或在虚拟机监控程序不支持 VM 代 id。|  
  
|||  
|-|-|  
|**事件 ID**|**2176**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|重命名虚拟域控制器克隆配置文件。<br /><br />其他数据<br /><br />旧文件名称: %1<br /><br />新文件名称: %2|  
|**笔记和分辨率**|重命名预期时启动的源 VM 备份，因为未更改 VM 代 ID。 这可以防止源域控制器在尝试复制。|  
  
|||  
|-|-|  
|**事件 ID**|**2177**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|重命名虚拟域控制器克隆配置文件失败。<br /><br />其他数据<br /><br />文件名称: %1<br /><br />错误代码: %%32|  
|**笔记和分辨率**|重命名尝试预期时启动的源 VM 备份，因为未更改 VM 代 ID。 这可以防止源域控制器在尝试复制。 手动重命名该文件并调查可能会阻止文件重命名的已安装的第三方产品。|  
  
|||  
|-|-|  
|**事件 ID**|**2178**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|检测到虚拟域控制器克隆配置文件，但尚未更改 VM 代 ID。 本地 DC 是仿制源直流。 重命名克隆配置文件。|  
|**笔记和分辨率**|因为未更改 VM 代 ID 预期时启动的源 VM 恢复正常。 这可以防止源域控制器在尝试复制。|  
  
|||  
|-|-|  
|**事件 ID**|**2179**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|已设置为以下参数域控制器计算机对象的 msDS GenerationId 属性：<br /><br />GenerationID 特性: %1|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2180**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|警告|  
|**消息**|设置了域控制器计算机对象的 msDS GenerationId 属性失败。<br /><br />其他数据<br /><br />故障代码 %1|  
|**笔记和分辨率**|检查 Dcpromo.log 和系统事件日志。 查找特定错误 MS TechNet、 MS 知识库和 MS 博客确定其常用含义和解决基于这些结果。|  
  
|||  
|-|-|  
|**事件 ID**|**2182**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|内部的事件： 要复制远程 DSA 目录服务已要求：|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2183**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|内部的事件： * <COMPUTERNAME> *完成要复制远程目录系统代理的请求。<br /><br />原始 DC 名称: %3<br /><br />请求克隆 DC 名称: %4<br /><br />请求克隆 DC 站点: %5<br /><br />其他数据<br /><br />错误值: %1%2|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2184**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*无法为复制 DC 创建域控制器帐户。<br /><br />原始 DC 名称: %1<br /><br />允许的数复制 DC:%2<br /><br />可以通过复制生成的域控制器帐户的数量限制* <COMPUTERNAME>*超出了。|  
|**笔记和分辨率**|如果不降级域控制器，根据命名约定的单个源域控制器名称可以仅自动生成 9999 时间。 使用<computername>XML 从不同命名 DC 生成一个唯一的新名称或克隆中元素。|  
  
|||  
|-|-|  
|**事件 ID**|**2191**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*设置以下的注册表值，禁用 DNS 更新。<br /><br />注册表项 %1<br /><br />注册表值： %2<br /><br />注册表值数据： %3<br /><br />克隆过程本地计算机可能具有相同短时间克隆源计算机的计算机名称。 因此，客户端无法发送请求本地计算机进行复制到在此期间将禁用 DNS A 和 AAAA 记录注册。 克隆过程复制完毕后将再次启用 DNS 更新。|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2192**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*设置以下的注册表值，禁用 DNS 更新失败。<br /><br />注册表项 %1<br /><br />注册表值： %2<br /><br />注册表值数据： %3<br /><br />错误代码: %4<br /><br />错误消息: %5<br /><br />克隆过程本地计算机可能具有相同短时间克隆源计算机的计算机名称。 因此，客户端无法发送请求本地计算机进行复制到在此期间将禁用 DNS A 和 AAAA 记录注册。|  
|**笔记和分辨率**|检查应用和系统事件日志。 调查第三方应用程序可能会阻止注册表更新。|  
  
|||  
|-|-|  
|**事件 ID**|**2193**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*设置以下的注册表值，以启用 DNS 更新。<br /><br />注册表项 %1<br /><br />注册表值： %2<br /><br />注册表值数据： %3<br /><br />克隆过程本地计算机可能具有相同短时间克隆源计算机的计算机名称。 因此，客户端无法发送请求本地计算机进行复制到在此期间将禁用 DNS A 和 AAAA 记录注册。|  
|**笔记和分辨率**|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2194**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*设置以下的注册表值，以启用 DNS 更新失败。<br /><br />注册表项 %1<br /><br />注册表值： %2<br /><br />注册表值数据： %3<br /><br />错误代码: %4<br /><br />错误消息: %5<br /><br />克隆过程本地计算机可能具有相同短时间克隆源计算机的计算机名称。 因此，客户端无法发送请求本地计算机进行复制到在此期间将禁用 DNS A 和 AAAA 记录注册。|  
|**笔记和分辨率**|检查应用和系统事件日志。 调查第三方应用程序可能会阻止注册表更新。|  
  
|||  
|-|-|  
|**事件 ID**|**2195**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|设置 DSRM 启动失败。<br /><br />错误代码: %1<br /><br />错误消息: %2<br /><br />当虚拟域控制器复制故障或虚拟域控制器克隆配置文件显示在不受支持的虚拟机监控程序本地计算机将重新启动是 DSRM 到问题的疑难解答。 设置 DSRM 启动失败。|  
|**笔记和分辨率**|检查应用和系统事件日志。 调查第三方应用程序可能会阻止注册表更新。|  
  
|||  
|-|-|  
|**事件 ID**|**2196**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|无法启用关机权限。<br /><br />错误代码: %1<br /><br />错误消息: %2<br /><br />当虚拟域控制器复制故障或虚拟域控制器克隆配置文件显示在不受支持的虚拟机监控程序本地计算机将重新启动是 DSRM 到问题的疑难解答。 启用关机特权失败。|  
|**笔记和分辨率**|检查应用和系统事件日志。 调查第三方应用程序可能会阻止特权使用量。|  
  
|||  
|-|-|  
|**事件 ID**|**2197**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|无法启动系统关闭。<br /><br />错误代码: %1<br /><br />错误消息: %2<br /><br />当虚拟域控制器复制故障或虚拟域控制器克隆配置文件显示在不受支持的虚拟机监控程序本地计算机将重新启动是 DSRM 到问题的疑难解答。 发起系统关闭失败。|  
|**笔记和分辨率**|检查应用和系统事件日志。 调查第三方应用程序可能会阻止特权使用量。|  
  
|||  
|-|-|  
|**事件 ID**|**2198**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*创建或修改的以下复制的 DC 对象失败。<br /><br />其他数据：<br /><br />对象：<br /><br />%1<br /><br />错误值： %2<br /><br />%3|  
|**笔记和分辨率**|查找特定错误 MS TechNet、 MS 知识库和 MS 博客确定其常用含义和解决基于这些结果。|  
  
|||  
|-|-|  
|**事件 ID**|**2199**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*创建以下复制的 DC 对象，因为已经存在对象失败。<br /><br />其他数据：<br /><br />源哥伦比亚特区：<br /><br />%1<br /><br />对象：<br /><br />%2|  
|**笔记和分辨率**|验证 dccloneconfig.xml 未指定现有域控制器或，份 dccloneconfig.xml 已使用的多个克隆而无需编辑名称。 如果仍意外冲突，确定哪些管理员提升请联系他们讨论应降级现有的域控制器，如果清除现有的域控制器元数据，或如果克隆应使用不同的名称。|  
  
|||  
|-|-|  
|**事件 ID**|**2203**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|最后一个虚拟域控制器复制失败。 这是因为首次重新启动，然后因此这应该会重新尝试复制。 但是，均不得虚拟域控制器克隆配置文件已存在也虚拟机代 ID 更改检测到。 启动会进入 DSRM。<br /><br />最后一个虚拟域控制器复制失败： 1%<br /><br />虚拟域控制器克隆配置文件已存在: %2<br /><br />检测到虚拟机代 ID 变化: %3|  
|**笔记和分辨率**|如果复制丢失或无效 dccloneconfig.xml 由于之前，失败的预期|  
  
|||  
|-|-|  
|事件 ID|2210|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|<COMPUTERNAME> 创建克隆域控制器的对象失败。<br /><br />其他数据：<br /><br />复制 Id: %6<br /><br />克隆域控制器名称： %1<br /><br />重试循环： %2<br /><br />例外值： %3<br /><br />错误值： %4<br /><br />DSID: %5|  
|笔记和分辨率|查看系统和目录服务事件日志和有关进一步的详细信息复制失败的原因 dcpromo.log。|  
  
|||  
|-|-|  
|事件 ID|2211|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 已创建克隆域控制器的对象。<br /><br />其他数据：<br /><br />复制 Id: %3<br /><br />克隆域控制器名称： %1<br /><br />重试循环： %2|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2212|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 开始创建克隆域控制器的对象。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />副本名称： %2<br /><br />克隆站点： %3<br /><br />复制 RODC: %4|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2213|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 创建新为只读域控制器复制 KrbTgt 对象。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />新的 KrbTgt 对象 Guid: %2|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2214|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 将创建一个计算机对象克隆域控制器。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />原始域控制器： %2<br /><br />克隆域控制器： %3|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2215|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 将以下网址添加克隆域控制器。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />网站： %2|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2216|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 将创建克隆域控制器服务器容器。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />服务器容器： %2|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2217|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 将创建克隆域控制器服务器对象。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />服务器对象： %2|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2218|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 将创建克隆域控制器各自的对象。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />对象： %2|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2219|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 将创建连接克隆只读域控制器的对象。<br /><br />其他数据：<br /><br />复制 Id: %1|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2220|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 将创建克隆只读域控制器 SYSVOL 对象。<br /><br />其他数据：<br /><br />复制 Id: %1|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2221|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|<COMPUTERNAME> 生成一个随机密码复制的域控制器失败。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />克隆域控制器名称： %2<br /><br />错误： %3%4|  
|笔记和分辨率|查看更多详细信息计算机帐户密码为什么无法创建系统事件日志。|  
  
|||  
|-|-|  
|事件 ID|2222|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|<COMPUTERNAME> 无法为复制的域控制器设置密码。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />克隆域控制器名称： %2<br /><br />错误： %3%4|  
|笔记和分辨率|查看更多详细信息的原因可能未设置计算机帐户密码系统事件日志。|  
  
|||  
|-|-|  
|事件 ID|2223|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|<COMPUTERNAME> 成功设置复制的域控制器计算机帐户密码。<br /><br />其他数据：<br /><br />复制 Id: %1<br /><br />克隆域控制器名称： %2<br /><br />总重试次： %3|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2224|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|虚拟域控制器复制失败。 复制计算机托管服务帐户存在以下 %1:<br /><br />%2<br /><br />对于复制成功，必须先删除所有托管服务帐户。 这可以使用删除 ADComputerServiceAccount PowerShell cmdlet。|  
|笔记和分辨率|使用单独的 Msa 时所预期 （不组 MSA）。 不要*不*按照要删除帐户的事件建议-不正确的目标读者。 使用 Uninstall-AdServiceAccount- [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310)。<br /><br />-第一次发布了 Windows Server 2008 R2-独立 Msa 已更换组的 Msa (gMSA) 与 Windows Server 2012。 GMSAs 支持复制。|  
  
|||  
|-|-|  
|事件 ID|2225|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|信息|  
|消息|以下主体的安全的缓存的机密成功已从本地域控制器：<br /><br />%1<br /><br />复制只读域控制器之后, 将复制的域控制器上删除机密之前已缓存克隆源只读域控制器。|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|2226|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|无法从本地域控制器删除以下主体的安全的缓存机密信息：<br /><br />%1<br /><br />错误： %2 (%3)<br /><br />后复制只读域控制器，机密之前已缓存克隆源只读域控制器需要的克隆删除为了减少攻击可以从被盗或受到威胁克隆获取这些凭据风险。 如果主体的安全高权限的帐户，并且应对此受保护，请使用 rootDSE 操作 rODCPurgeAccount 手动清除它本地域控制器上的机密信息。|  
|笔记和分辨率|检查系统和目录服务事件日志中的详细信息。|  
  
|||  
|-|-|  
|事件 ID|2227|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|例外时发生尝试从本地域控制器删除缓存机密信息。<br /><br />其他数据：<br /><br />例外值： %1<br /><br />错误值： %2<br /><br />DSID: %3<br /><br />后复制只读域控制器，机密之前已缓存克隆源只读域控制器需要的克隆删除为了减少攻击可以从被盗或受到威胁克隆获取这些凭据风险。 如果这些安全主体任一高权限的帐户，并且应对此受保护，请使用 rootDSE 操作 rODCPurgeAccount 手动清除它本地域控制器上的机密信息。|  
|笔记和分辨率|检查系统和目录服务事件日志中的详细信息。|  
  
|||  
|-|-|  
|事件 ID|2228|  
|源|Microsoft 的 Windows ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|在此域控制器 Active Directory 数据库中的虚拟机代 ID 是此虚拟机的当前值不同。 但是，虚拟域控制器克隆配置文件 (DCCloneConfig.xml) 找不到以便域控制器复制未尝试执行。 如果有意域控制器复制操作，请确保 DCCloneConfig.xml 提供的任何一种方法受支持的位置。 此外，此域控制器的 IP 地址冲突与另一个域控制器 IP 地址。 若要确保不发生的任何服务中断，域控制器已配置启动会进入 DSRM 到。<br /><br />其他数据：<br /><br />重复的 IP 地址： %1|  
|笔记和分辨率|此保护机制停止重复域控制器尽可能 （将不是在使用 DHCP，例如时）。 添加有效 DcCloneConfig.xml 文件、 删除该 DSRM 标记，并重新尝试复制|  
  
|||  
|-|-|  
|事件 ID|29218|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制失败。 无法完成克隆操作，并复制的域控制器重新启动之前插入目录服务还原模式 (DSRM)。<br /><br />请查看之前记录事件和错误所对应的详细信息的 %systemroot%\debug\dcpromo.log 复制可重复使用此克隆映像并尝试虚拟域控制器。<br /><br />如果一个或多个日志条目表示克隆过程无法重试，必须牢固销毁图像。 否则为你可能会修复这些错误，请清除 DSRM 启动标志，，通常; 重新启动在重新启动时，将重试克隆操作。|  
|笔记和分辨率|查看系统和目录服务事件日志和有关进一步的详细信息复制失败的原因 dcpromo.log。|  
  
|||  
|-|-|  
|事件 ID|29219|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|信息|  
|消息|已成功虚拟域控制器复制。|  
|笔记和分辨率|如果意外，这是一个成功事件和的问题。|  
  
|||  
|-|-|  
|事件 ID|29248|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制未能获得 Winlogon 通知。 返回的错误代码是 %1 (%2)。<br /><br />有关此错误的详细信息，请查看 %systemroot%\debug\dcpromo.log 对应于复制尝试虚拟域控制器的错误。|  
|笔记和分辨率|联系 Microsoft 产品的支持人员|  
  
|||  
|-|-|  
|事件 ID|29249|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制未能分析虚拟域控制器配置文件。<br /><br />返回的 HRESULT 代码是 1%。<br /><br />配置文件: %2<br /><br />请配置文件中修复这些错误，然后重试克隆操作。<br /><br />有关此错误的详细信息，请参阅 %systemroot%\debug\dcpromo.log。|  
|笔记和分辨率|检查有使用 XML 编辑器语法错误 dclconeconfig.xml 文件和 DCCloneConfigSchema.xsd 方案文件。|  
  
|||  
|-|-|  
|事件 ID|29250|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制失败。 软件或服务当前启用复制虚拟域控制器上的虚拟域控制器复制的应用允许的程序列表中不存在。<br /><br />以下是丢失项：<br /><br />%2<br /><br />（如果有） %1 用作定义的包含列表。<br /><br />如果有非克隆安装的应用，无法完成克隆操作。<br /><br />请运行 Active Directory PowerShell Cmdlet 获取-ADDCCloningExcludedApplicationList 若要查看哪些应用程序复制计算机上安装，但不是包括在允许列表，然后将其添加到允许列表中，它们是否与虚拟域控制器复制兼容。 如果这些应用程序的任何不兼容的虚拟域控制器复制，请卸载它们再重新试克隆操作。<br /><br />复制过程根据下面的搜索顺序; 允许的应用列表文件 CustomDCCloneAllowList.xml，搜索虚拟域控制器使用找到的第一个文件和忽略所有其他：<br /><br />1.注册表值名称： HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2.DSA 工作目录文件夹所在的相同目录<br /><br />3 平板电脑。 %windir%\NTDS<br /><br />4.可移动读取/写入 media 顺序根部驱动器的驱动器号|  
|笔记和分辨率|按照说明消息|  
  
|||  
|-|-|  
|事件 ID|29251|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制克隆计算机的 IP 地址重置失败。<br /><br />返回的错误代码是 %1 (%2)。<br /><br />在网络配置虚拟域控制器配置文件中的各部分的错误配置时，可能会导致此错误。<br /><br />请参阅 %systemroot%\debug\dcpromo.log 有关对应于在虚拟域控制器复制尝试重置的 IP 地址的错误的详细信息。<br /><br />重置计算机复制计算机上的 IP 地址的详细信息，请访问 https://go.microsoft.com/fwlink/?LinkId=208030|  
|笔记和分辨率|验证 IP 信息中 dccloneconfig.xml 设置有效，并且不重复原始源计算机。|  
  
|||  
|-|-|  
|事件 ID|29253|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制失败。 克隆域控制器程序无法找到复制的计算机家庭域中复制计算机主域控制器 (PDC) 操作主机。<br /><br />返回的错误代码是 %1 (%2)。<br /><br />请验证主域控制器复制计算机家庭域中的分配给动态域控制器、 处于联机状态，并且正在运行。 验证复制的计算机所需的端口和协议轻具有 LDAP 月 RPC 连接至主域控制器。|  
|笔记和分辨率|验证复制的域控制器 IP 和信息 DNS 设置。 使用 Dcdiag.exe /test:locatorcheck 验证如果 PDCE 处于联机状态，请使用 Nltest.exe /server:* <PDCE> * /dclist:* <domain> *到有效 RPC、 获得网络捕获 PDCE 复制发生故障时和分析交通。|  
  
|||  
|-|-|  
|事件 ID|29254|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制到 1%主域控制器绑定失败。<br /><br />返回的错误代码是 %2 (%3)。<br /><br />请验证主域控制器 %1 处于联机状态，并且正在运行。 验证复制的计算机所需的端口和协议轻具有 LDAP 月 RPC 连接至主域控制器。|  
|笔记和分辨率|验证复制的域控制器 IP 和信息 DNS 设置。 使用 Dcdiag.exe /test:locatorcheck 验证如果 PDCE 处于联机状态，请使用 Nltest.exe /server:* <PDCE> * /dclist:* <domain> *到有效 RPC、 获得网络捕获 PDCE 复制发生故障时和分析交通。|  
  
|||  
|-|-|  
|事件 ID|29255|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制失败。<br /><br />尝试在主域控制器 %1 要复制的图像需要创建对象返回错误 %2 (%3)。<br /><br />请验证复制的域控制器具有复制本身的权限。 检查存在主域控制器之前目录服务事件日志中的相关事件。|  
|笔记和分辨率|查找特定错误 MS TechNet、 MS 知识库和 MS 博客确定其典型含义和解决基于这些结果。|  
  
|||  
|-|-|  
|事件 ID|29256|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|有人试图来到目录服务还原模式标志设置启动失败，错误代码 1%。<br /><br />请参阅 %systemroot%\debug\dcpromo.log 有关错误的详细信息。|  
|笔记和分辨率|检查目录服务日志和 dcpromo.log 有关详细信息。 检查应用和系统事件日志。 调查第三方应用程序可能会阻止特权使用量。|  
  
|||  
|-|-|  
|事件 ID|29257|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制已完成操作。 尝试重启计算机失败，错误代码 1%。<br /><br />请重新启动计算机后，才能完成克隆操作。|  
|笔记和分辨率|检查应用和系统事件日志。 调查第三方应用程序可能会阻止特权使用量。|  
  
|||  
|-|-|  
|事件 ID|29264|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|若要清除启动到目录服务还原模式标志有人试图失败，错误代码 1%。<br /><br />请参阅 %systemroot%\debug\dcpromo.log 有关错误的详细信息。|  
|笔记和分辨率|检查目录服务日志和 dcpromo.log 有关详细信息。 检查应用和系统事件日志。 调查第三方应用程序可能会阻止特权使用量。|  
  
|||  
|-|-|  
|事件 ID|29265|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|信息|  
|消息|已成功虚拟域控制器复制。 为 %2 重命名虚拟域控制器 %1 克隆配置文件。|  
|笔记和分辨率|不适用，这是一个成功事件。|  
  
|||  
|-|-|  
|事件 ID|29266|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|已成功虚拟域控制器复制。 尝试重命名虚拟域控制器克隆配置文件 %1 失败，错误代码 %2 (%3)。|  
|笔记和分辨率|手动重命名 dccloneconfig.xml 文件。|  
  
|||  
|-|-|  
|事件 ID|29267|  
|源|Microsoft 的 Windows-DirectoryServices-DSROLE-服务器|  
|严重性|错误|  
|消息|虚拟域控制器复制无法检查虚拟域控制器复制允许的应用程序的列表。<br /><br />返回的错误代码是 %1 (%2)。<br /><br />可能会导致此错误的一个语法错误克隆在允许列表文件 (当前检查该文件: %3)。 有关此错误的详细信息，请参阅 %systemroot%\debug\dcpromo.log。|  
|笔记和分辨率|按照说明事件|  
  
##### <a name="error-messages"></a>错误消息  
没有为失败虚拟化的域控制器复制; 直接交互式错误在系统日志所有克隆信息和目录服务日志域控制器升级登录 dcpromo.log。 但是，如果该服务器启动到 DS 恢复模式，调查立即，为促销或复制失败。  
  
Dcpromo.log 是第一个地方检查复制失败。 具体取决于列出失败，可能需要在系统日志以进一步诊断和目录服务以后查看。  
  
#### <a name="known-issues-and-support-scenarios"></a>已知的问题并支持的方案  
下面是在 Windows Server 2012 开发过程中看到的常见问题。 所有这些问题会"通过设计"，并且具有有效的解决方法或更适合用于首先避免这些的技术。 一些可能更高版本的 Windows Server 2012 的版本中得到解决。  
  
|||  
|-|-|  
|**问题**|**复制失败，DSRM**|  
|**症状**|复制到的目录服务还原模式靴子的猫|  
|**分辨率和笔记**|验证所有后面部分部署虚拟化域控制器部分中的步骤和[常规疑难解答域控制器复制的方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />以 kb 为单位 2742844 所述。|  
  
|||  
|-|-|  
|**问题**|**当使用 DHCP 要复制的额外 IP 租赁**|  
|**症状**|成功复制 DC 并使用 DHCP 后, 克隆首次启动采用 DHCP 租赁。 然后时将重命名服务器 DC 作为重新启动它，它会拍摄第二个 DHCP 租赁。 不发布的第一个 IP 地址，并使用"影子"租赁最终|  
|**分辨率和笔记**|手动删除未使用的地址租赁 DHCP 中或允许通常到期。 以 kb 为单位 2742836 所述。|  
  
|||  
|-|-|  
|**问题**|**在很长时间的延迟后复制到 DSRM 失败**|  
|**症状**|复制似乎在暂停"域控制器复制位于 X %完成时"的之间 8 并 15 分钟。 此操作，请后复制失败并启动到 DSRM。|  
|**分辨率和笔记**|复制的计算机无法从 DHCP 或 SLAAC，获取动态 IP 地址或重复的 IP 地址，使用或找不到 PDC。 由复制的多个试导致延迟。 解决网络允许复制的问题。<br /><br />以 kb 为单位 2742844 所述。|  
  
|||  
|-|-|  
|**问题**|**复制不重新创建所有服务的主要名称**|  
|**症状**|如果一套*三部分*服务主要名称 (SPN) 包括两个 NetBIOS 姓氏的某个端口和不端口以其他方式相同 NetBIOS 名称、 非端口条目未重新创建新的计算机名称。 例如：<br /><br />customspn 月 DC1:200 / app1 无效使用的符号*这将重新创建新的计算机名称*<br /><br />customspn/DC1 app1 符号无效使用*这不重新创建新的计算机名称*<br /><br />重新创建完整的名称，并重新创建 Spn 没有三个部分，无论端口。 例如，这些都重新创建成功克隆上：<br /><br />customspn / 的符号使用 DC1:202 无效*这将重新创建*<br /><br />customspn/DC1 无效使用的符号*这将重新创建*<br /><br />customspn/DC1.corp.contoso.com:202 符号无效使用*这将重新创建名称*<br /><br />customspn/DC1.corp.contoso.com 符号无效使用*这将重新创建*|  
|**分辨率和笔记**|这是 Windows 中，而不仅仅是中复制域控制器重命名进程的限制。 通过在任何项 scenario 重命名的逻辑没有处理三部分 SPN。 当他们根据需要重新创建任何缺少 Spn，最包含的 Windows 服务不受此问题。 其他应用程序可能需要手动输入 SPN 来解决问题。<br /><br />以 kb 为单位 2742874 所述。|  
  
|||  
|-|-|  
|**问题**|**复制无法正常工作，启动到 DSRM，常规网络错误**|  
|**症状**|复制到的目录服务维修模式靴子的猫。 有常规网络错误。|  
|分辨率和笔记|确保新克隆没有分配从源域控制器; 重复静态 MAC 地址你可以看到 VM 通过虚拟机监控程序主机源和克隆虚拟机上运行此命令将使用静态的 MAC 地址：<br /><br />获取 VM VMName*测试 vm* & #124;获取 VMNetworkAdapter & #124;佛罗里达 *<br /><br />将 MAC 地址更改为一个唯一的静态地址或切换到使用动态 MAC 地址。<br /><br />所述以 kb 为单位 2742844|  
  
|||  
|-|-|  
|**问题**|**复制失败，为的源 DC 重复启动到 DSRM**|  
|**症状**|一个新的克隆启动没有复制。 Dccloneconfig.xml 未重命名和服务器启动 DS 恢复模式。 目录服务事件日志中显示错误 2164<br /><br />*<COMPUTERNAME>*无法启动复制本地虚拟域控制器 DsRoleSvc 服务。|  
|**分辨率和笔记**|检查 DS 角色服务器服务 (DsRoleSvc) 的服务设置并确保其开始键入设置为手动。 验证任何第三方程序正在阻止该服务的开头。<br /><br />有关如何释放同时确保将更新获取复制站此辅助 DC 的详细信息，请参阅 Microsoft 知识库文章 2742970。|  
  
|||  
|-|-|  
|**问题**|**复制无法正常工作，启动到 DSRM，错误 8610**|  
|**症状**|复制到的目录服务还原模式靴子的猫。 Dcpromo.log 显示 8610 错误 （即 ERROR_DS_ROLE_NOT_VERIFIED 8610 或 0x21A2）|  
|**分辨率和笔记**|如果 PDC 可以是可发现，但它目前执行足够复制允许自行承担角色会发生这种情况。 例如，如果复制会启动，并且其他管理员移动到新直流的 PDCE FSMO 角色。<br /><br />以 kb 为单位 2742916 所述。|  
  
|||  
|-|-|  
|**问题**|**复制无法正常工作，启动到 DSRM，常规网络错误**|  
|**症状**|复制到的目录服务还原模式靴子的猫。 有常规网络错误。|  
|**分辨率和笔记**|确保新克隆没有分配从源域控制器; 重复静态 MAC 地址你可以看到 VM 通过 HYPER-V 主机源和克隆虚拟机上运行此命令将使用静态的 MAC 地址：<br /><br />获取 VM VMName*测试 vm* & #124;获取 VMNetworkAdapter & #124;佛罗里达 *<br /><br />将 MAC 地址更改为一个唯一的静态地址或切换到使用动态 MAC 地址。<br /><br />以 kb 为单位 2742844 所述。|  
  
|||  
|-|-|  
|**问题**|**复制无法正常工作，启动到 DSRM**|  
|**症状**|复制到的目录服务维修模式靴子的猫|  
|**分辨率和笔记**|确保 dccloneconfig.xml 包含方案清晰度 （请参阅 sampledccloneconfig.xml，行 2）：<br /><br />**< d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig">**<br /><br />所述以 kb 为单位 2742844|  
  
|||  
|-|-|  
|问题|**没有登录服务器有可用的错误 DSRM 登录**|  
|**症状**|复制到的目录服务维修模式靴子的猫。 你在尝试登录时收到错误消息：<br /><br />**目前不登录到服务器，可登录请求提供服务**|  
|**分辨率和笔记**|确保您使用 DSRM 管理员帐户，并且不是在域帐户登录。 使用向左箭头键并键入用户名：<br /><br />**。 \administrator**<br /><br />所述以 kb 为单位 2742908|  
  
|||  
|-|-|  
|**问题**|**仿制源 DSRM，错误为无法正常工作**|  
|**症状**|复制，期间出现故障 8437"创建克隆直流上的对象失败 PDC"(0x20f5)|  
|**分辨率和笔记**|重复计算机名称设置中 DCCloneConfig.xml 作为源 DC 或现有 DC。 计算机名称还需要 NetBIOS 计算机名称格式 （15 字符或更少，不 FQDN）。<br /><br />设置有效的唯一的名称修复 dccloneconfig.xml 文件。<br /><br />所述以 kb 为单位 2742959|  
  
|||  
|-|-|  
|**问题**|**新 addccloneconfigfile 错误"指数利用范围是"**|  
|**症状**|当新 addccloneconfigfile cmdlet 运行的，你会收到错误：<br /><br />指数是利用范围。 必须非负极小于的集锦大小。|  
|**分辨率和笔记**|你必须以管理员提升 Windows PowerShell 控制台运行 cmdlet。 在计算机上的本地管理员组成员缺少的情况下导致此错误。<br /><br />所述以 kb 为单位 2742927|  
  
|||  
|-|-|  
|**问题**|**复制失败，请重复 DC**|  
|**症状**|复制无复制，复制已有源 DC 靴子的猫|  
|**分辨率和笔记**|计算机复制和开始使用，但不包含 DcCloneConfig.xml 文件在所有受支持的位置，并且没有重复的 IP 地址与源代码域控制器。 必须以避免数据丢失正确删除 DC。<br /><br />所述以 kb 为单位 2742970|  
  
|||  
|-|-|  
|**问题**|**新 ADDCCloneConfigFile 失败，服务器不可操作错误检查源域控制器是否克隆域控制器组中的成员，是否 GC 不可用时。**|  
|**症状**|运行新建-ADDCCloneConfigFile 创建 dccloneconfig.xml 文件，当你收到错误：<br /><br />代码-服务器不可操作|  
|**分辨率和笔记**|从你运行的新 ADDCCloneConfigFile 并确认源域控制器克隆域控制器组中的成员已复制到该 GC 服务器与 GC 验证连接。<br /><br />作为一种刷新直流定位器缓存其中 GC 或直流可能已处于脱机最近的情况下运行以下命令：<br /><br />代码-nltest /dsgetdc: /GC /FORCE|  
  
### <a name="advanced-troubleshooting"></a>高级故障排除  
此模块搜寻会学习通过使用的高级疑难解答*工作*作为示例、 对发生的事情的一些说明日志。 如果您已了解成功虚拟化的域控制器操作如下所示，失败变得更加您的环境中明显。 按它们的源的升序与提供这些日志*预期*复制的域控制器在每个日志与相关的事件 （即使他们是警告和错误）。  
  
#### <a name="cloning-a-domain-controller"></a>复制域控制器  
在此示例中，克隆域控制器 DHCP 用于获取 IP 地址、 复制 SYSVOL 使用 FRS 或 DFSR （见相应日志如有必要），为全球的目录，并使用空白 dccloneconfig.xml 文件。  
  
##### <a name="directory-services-event-log"></a>目录服务事件日志  
目录服务日志包含绝大多数的事件基于克隆操作的信息。 虚拟机监控程序更改 VM 代 ID 和 NTDS 服务笔记它，使无效 RID 池，然后更改调用 id。 设置新的 VM 代 ID，并服务器复制 Active Directory 归巢的数据。 DFSR 服务已停止并且承载 SYSVOL 其数据库时会删除它，强制非授权同步站。 调整 USN 高水印。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**2160**|ActiveDirectory_DomainService|本地 Active Directory 域服务已找到虚拟域控制器克隆配置文件。<br /><br />在找到的虚拟域控制器克隆配置文件：<br /><br />*<path>*\DCCloneConfig.xml<br /><br />虚拟域控制器克隆配置文件中的存在指示本地虚拟域控制器另一个虚拟域控制器的副本。 将开始 Active Directory 域服务复制本身。|  
|**2191**|ActiveDirectory_DomainService|Active Directory 域服务设置以下的注册表值，禁用 DNS 更新。<br /><br />注册表项：<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />注册表值：<br /><br />UseDynamicDns<br /><br />注册表值数据：<br /><br />0<br /><br />克隆过程本地计算机可能具有相同短时间克隆源计算机的计算机名称。 因此，客户端无法发送请求本地计算机进行复制到在此期间将禁用 DNS A 和 AAAA 记录注册。 克隆过程复制完毕后将再次启用 DNS 更新。|  
|**2191**|ActiveDirectory_DomainService|Active Directory 域服务设置以下的注册表值，禁用 DNS 更新。<br /><br />注册表项：<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />注册表值：<br /><br />RegistrationEnabled<br /><br />注册表值数据：<br /><br />0<br /><br />克隆过程本地计算机可能具有相同短时间克隆源计算机的计算机名称。 因此，客户端无法发送请求本地计算机进行复制到在此期间将禁用 DNS A 和 AAAA 记录注册。 克隆过程复制完毕后将再次启用 DNS 更新。<br /><br />"信息 2 月 7 月 2012年 3:12:49 晚上 Microsoft-Windows ActiveDirectory_DomainService 2191 内部配置"Active Directory 域服务设置以下的注册表值，禁用 DNS 更新。<br /><br />注册表项：<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />注册表值：<br /><br />DisableDynamicUpdate<br /><br />注册表值数据：<br /><br />1<br /><br />克隆过程本地计算机可能具有相同短时间克隆源计算机的计算机名称。 因此，客户端无法发送请求本地计算机进行复制到在此期间将禁用 DNS A 和 AAAA 记录注册。 克隆过程复制完毕后将再次启用 DNS 更新。|  
|**2172**|ActiveDirectory_DomainService|阅读域控制器计算机对象的 msDS GenerationId 属性。<br /><br />msDS GenerationId 属性值：<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|已检测到代 ID 更改。<br /><br />缓存 DS （旧值） 在代 ID:<br /><br />*<Number>*<br /><br />当前在虚拟机 （新值） 代 ID:<br /><br />*<Number>*<br /><br />在应用虚拟机快照、 虚拟机导入操作或之后实时迁移操作后，会发生代 ID 更改。 Active Directory 域服务将创建新的调用 ID 恢复域控制器。 不应使用虚拟机快照恢复虚拟化的域控制器。 受支持的方法，若要还原或回退 Active Directory 域服务数据库中的内容是使用 Active Directory 域服务感知备份应用程序进行系统状态备份还原。|  
|**1109**|ActiveDirectory_DomainService|此目录服务器 invocationID 属性已更改。 在创建备份的时间的最高更新序列号如下所示：<br /><br />InvocationID 属性 （旧值）：<br /><br />*<GUID>*<br /><br />InvocationID 属性 （新值）：<br /><br />*<GUID>*<br /><br />更新的序列号：<br /><br />*<Number>*<br /><br />从备份的媒体还原目录服务器时更改 invocationID 配置提供可写的应用程序目录分区、 虚拟机快照在应用后，或实时迁移操作后虚拟机导入操作，已恢复。 不应使用虚拟机快照恢复虚拟化的域控制器。 还原或回退到受支持的方法 Active Directory 域服务数据库中的内容是使用 Active Directory 域感知服务的备份应用程序进行系统状态备份还原。|  
|**1000**|ActiveDirectory_DomainService|Microsoft Active Directory 域服务启动完成。|  
|**1394**|ActiveDirectory_DomainService|已清除所有防止 Active Directory 域服务数据库对更新的问题。 成功 Active Directory 域服务数据库到新的更新。 重新启动网络登录服务|  
|**2163**|ActiveDirectory_DomainService|DsRoleSvc 服务已经启动要复制的本地虚拟域控制器。|  
|**326**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎附加数据库 (月 1 日，C:\Windows\NTDS\ntds.dit)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.000、 [6] 介于 0.016、 [7] 0.000、 [8] 0.000、 [9] 0.000、 [10] 0.000、 [11] 0.000、 0.000 [12]。<br /><br />保存的缓存： 1|  
|**103**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎停止实例 (0)。<br /><br />脏关机： 0<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.032、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.031、 [10] 0.000、 [11] 0.000、 [12] 0.000、 [13] 0.000、 [14] 0.000、 0.000 [15]。|  
|**102**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎 (6.02.8225.0000) 开始新实例 (0)。|  
|**105**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎启动新实例 (0)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 介于 0.016、 [2] 0.000、 [3] 0.015、 [4] 0.078、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.046、 [10] 0.000、 0.000 [11]。|  
|**1004**|ActiveDirectory_DomainService|已成功关闭 active Directory 域服务。|  
|**102**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎 (6.02.8225.0000) 开始新实例 (0)。|  
|**326**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎附加数据库 (月 1 日，C:\Windows\NTDS\ntds.dit)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.015、 [3] 介于 0.016、 [4] 0.000、 [5] 0.031、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.000、 [10] 0.000、 [11] 0.000、 0.000 [12]。<br /><br />保存的缓存： 1|  
|**105**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎启动新实例 (0)。 (时间 = 1 秒钟后)<br /><br />内部计时序列: [1] 0.031、 [2] 0.000、 [3] 0.000、 [4] 0.391、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.031、 [10] 0.000、 0.000 [11]。|  
|**1109**|ActiveDirectory_DomainService|此目录服务器 invocationID 属性已更改。 在创建备份的时间的最高更新序列号如下所示：<br /><br />InvocationID 属性 （旧值）：<br /><br />*<GUID>*<br /><br />InvocationID 属性 （新值）：<br /><br />*<GUID>*<br /><br />更新的序列号：<br /><br />*<Number>*<br /><br />从备份的媒体还原目录服务器时更改 invocationID 配置提供可写的应用程序目录分区、 虚拟机快照在应用后，或实时迁移操作后虚拟机导入操作，已恢复。 不应使用虚拟机快照恢复虚拟化的域控制器。 还原或回退到受支持的方法 Active Directory 域服务数据库中的内容是使用 Active Directory 域感知服务的备份应用程序进行系统状态备份还原。|  
|**1168**|ActiveDirectory_DomainService|内部错误： Active Directory 域服务错误。<br /><br />其他数据<br /><br />错误值 （小数点）：<br /><br />2<br /><br />错误值 （十六进制）：<br /><br />2<br /><br />内部 ID:<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|下面的时间间隔，此域控制器提升到全球目录将会遭遇延迟。<br /><br />间隔 （分钟）：<br /><br />5<br /><br />此延迟，以便可以之前公布全球目录准备好所需的目录分区是必需的。 在注册表中，你可以指定的数秒目录系统代理将之前，等待升级到全球目录本地域控制器。 全球目录延迟广告注册表值的详细信息，请参阅资源工具包分发系统指南|  
|**103**|NTDS 这些|NTDS (536) NTDSA： 数据库引擎停止实例 (0)。<br /><br />脏关机： 0<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.047、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 介于 0.016、 [10] 0.000、 [11] 0.000、 [12] 0.000、 [13] 0.000、 [14] 0.000、 0.000 [15]。|  
|**1004**|ActiveDirectory_DomainService|已成功关闭 active Directory 域服务。|  
|**1539**|ActiveDirectory_DomainService|Active Directory 域服务无法禁用以下硬盘上的软件基于磁盘写入缓存。<br /><br />硬盘上：<br /><br />c:<br /><br />在系统失败，数据可能会丢失|  
|**2179**|ActiveDirectory_DomainService|已设置为以下参数域控制器计算机对象的 msDS GenerationId 属性：<br /><br />GenerationID 属性：<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|无法读取域控制器计算机对象的 msDS GenerationId 属性。 这可能由数据库交易失败，或代 id 不存在本地数据库中。 后 dcpromo 或直流不虚拟域控制器在首次重新启动期间 msDS GenerationId 不存在。<br /><br />其他数据<br /><br />错误代码：<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Microsoft Active Directory 域服务启动完成后，版本 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|已清除所有防止 Active Directory 域服务数据库对更新的问题。 成功 Active Directory 域服务数据库到新的更新。 网络登录服务已重新启动。|  
|**1128**|ActiveDirectory_DomainService|1128 知识一致性检查"复制连接已创建从下面的源目录服务本地目录服务。<br /><br />源目录服务：<br /><br />CN = 各自的*<Domain Controller DN>*<br /><br />本地目录服务：<br /><br />CN = 各自的*<Domain Controller DN>*<br /><br />其他数据<br /><br />原因代码：<br /><br />0x2<br /><br />创建点内部 ID:<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|源目录服务经过优化更新序列号 (USN) 呈现目标目录服务。 源和目的地的目录服务有常见复制伙伴。 目标目录服务处于最新的通用复制搭档，并使用此合作伙伴的备份安装源目录服务。<br /><br />目标目录服务 ID:<br /><br />*<GUID> (<FQDN>)*<br /><br />常见目录服务 ID:<br /><br />*<GUID>*<br /><br />常见财产 USN:<br /><br />*<Number>*<br /><br />因此，已使用以下设置配置目标目录服务的最新的矢量。<br /><br />以前的对象 USN:<br /><br />0<br /><br />以前属性 USN:<br /><br />0<br /><br />数据库 GUID:<br /><br />*<GUID>*<br /><br />对象 USN:<br /><br />*<Number>*<br /><br />属性 USN:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>系统事件日志  
在系统事件日志是克隆操作下一个提示。 按虚拟机监控程序告诉来宾计算机已复制或从快照还原，域控制器立即使其 RID 池，以避免重复安全主体稍后无效。 作为复制继续进行，各种预期的操作和消息出现时，大部分周围启动和停止的服务，并且某些预期导致此错误。 当完成整体复制成功的系统事件日志笔记。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**16654**|三千-目录-服务|帐户标识符 (Rid) 的池已失效。 在以下预期的情况下可能会导致该问题：<br /><br />1.从备份还原域控制器。<br /><br />2.从快照还原域控制器在虚拟机上运行。<br /><br />3.管理员已手动失效池|  
|**7036**|服务控制管理器|Active Directory 域服务服务输入正在运行的状态。|  
|**7036**|服务控制管理器|Kerberos 键 Distribution 中心服务输入正在运行的状态。|  
|**3096**|Netlogon|找不到主域该域控制器。|  
|**7036**|服务控制管理器|安全帐户管理器服务输入正在运行的状态。|  
|**7036**|服务控制管理器|服务器服务输入正在运行的状态。|  
|**7036**|服务控制管理器|Netlogon 服务输入正在运行的状态。|  
|**7036**|服务控制管理器|Active Directory Web 服务服务输入正在运行的状态。|  
|**7036**|服务控制管理器|DFS 复制服务输入正在运行的状态。|  
|**7036**|服务控制管理器|该文件复制服务服务输入正在运行的状态。|  
|**14533**|Microsoft 的 Windows DfsSvc|DFS 已经完成制作所有命名空间。|  
|**14531**|Microsoft 的 Windows DfsSvc|DFS 服务器完成初始化操作。|  
|**7036**|服务控制管理器|DFS Namespace 服务输入正在运行的状态。|  
|**7023**|服务控制管理器|站点间 Messaging 服务终止错误如下：<br /><br />指定的服务器无法执行请求的操作。|  
|**7036**|服务控制管理器|该站点间 Messaging 服务输入停止的状态。|  
|**5806**|Netlogon|已此域控制器上手动禁用动态更新。<br /><br />用户操作<br /><br />重新配置此域控制器使用的动态更新或从文件 %systemroot%\system32\config\netlogon.dns 手动添加 DNS 记录，向 DNS 数据库。"|  
|**16651**|三千-目录-服务|一个新帐户标识符池请求失败。 将重试操作，直到成功请求。 错误<br /><br />请求的 FSMO 操作失败。 不会联系当前 FSMO 所有者。|  
|**7036**|服务控制管理器|DNS 服务器服务输入正在运行的状态。|  
|**7036**|服务控制管理器|DS 角色服务器服务输入正在运行的状态。|  
|**7036**|服务控制管理器|Netlogon 服务输入停止的状态。|  
|**7036**|服务控制管理器|该文件复制服务服务输入停止的状态。|  
|**7036**|服务控制管理器|Kerberos 键 Distribution 中心服务输入停止的状态。|  
|**7036**|服务控制管理器|DNS 服务器服务输入停止的状态。|  
|**7036**|服务控制管理器|Active Directory 域服务服务输入停止的状态。|  
|**7036**|服务控制管理器|Netlogon 服务输入正在运行的状态。|  
|**7040**|服务控制管理器|开始类型的 Active Directory 域服务服务更改为禁用从自动启动。|  
|**7036**|服务控制管理器|Netlogon 服务输入停止的状态。|  
|**7036**|服务控制管理器|该文件复制服务服务输入正在运行的状态。|  
|**29219**|DirectoryServices DSROLE 服务器|已成功虚拟域控制器复制。|  
|**29223**|DirectoryServices DSROLE 服务器|此服务器现域控制器。|  
|**29265**|DirectoryServices DSROLE 服务器|已成功虚拟域控制器复制。 已于 C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml 重命名复制配置文件 C:\Windows\NTDS\DCCloneConfig.xml 虚拟域控制器。|  
|**1074**|User32|具有出于以下原因启动计算机 DC2 用户 NT AUTHORITY\SYSTEM 代表的重启过程 C:\Windows\system32\lsass.exe (DC2): 操作系统： 重新 （计划中）<br /><br />代码的原因： 0x80020004<br /><br />关机类型： 重启<br /><br />发表评论:"|  
  
##### <a name="dcpromolog"></a>DCPROMO。日志  
Dcpromo.log 包含目录服务事件日志不描述的实际推广部分的复制。 日志不提供的事件日志条目赋予的说明级别，因为此部分中的模块包含其他注释。  
  
复制开始升级过程意味着，DC 已清理的其当前配置，并且重新升级使用现有的广告数据库 （更像 IFM 推广），然后 DC 复制的广告的入站的更改增量和 SYSVOL，并复制已完成。  
  
> [!NOTE]  
> 日志已被删除日期列修改以提高可读性，本模块。  
  
> [!NOTE]  
> Dcpromo.log 进一步说明，请参阅的了解和 Windows Server 2012 在疑难解答广告 DS 简体中文管理。  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   开始克隆基于提升  
  
-   设置目录服务还原模式标志以便服务器无法启动备份通常为原始和原因命名或目录服务冲突  
  
-   更新目录服务事件日志  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   停止 NetLogon 服务，以便不公布域控制器  
  
```  
15:14:01 [INFO] Stopping service NETLOGON  
15:14:01 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:14:01 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:14:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:02 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:14:02 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:14:02 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:14:02 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:14:02 [INFO] StopService on NETLOGON returned 0  
15:14:02 [INFO] Configuring service NETLOGON to 1 returned 0  
15:14:02 [INFO] Updating service status to 4  
15:14:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   检查管理员指定自定义项打包 dccloneconfig.xml 文件。  
  
-   在此示例情况下它是一个空白的文件，以便自动生成的所有设置，并自动 IP 地址，则需要从网络  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   验证没有服务或安装的不是 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 的一部分的程序  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   由于未管理员指定 IP 信息的网络适配器，在启用 DHCP  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   找到 PDC 仿真器  
  
-   设置 （这种情况下自动生成） 的副本的网站  
  
-   设置 （这种情况下自动生成） 的副本的名字  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   创建新的克隆计算机对象  
  
-   重命名克隆匹配新名称  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   提供了一些推广设置，根据以前 dccloneconfig.xml 或生成自动规则  
  
```  
15:14:05 [INFO] vDC Cloning: Promotion parameters setting:  
15:14:05 [INFO] DNS Domain Name: root.fabrikam.com  
15:14:05 [INFO] Replica Partner: \\DC1.root.fabrikam.com  
15:14:05 [INFO] Site Name: Default-First-Site-Name  
15:14:05 [INFO] DS Database Path: C:\Windows\NTDS  
15:14:05 [INFO] DS Log Path: C:\Windows\NTDS  
15:14:05 [INFO] SysVol Root Path: C:\Windows\SYSVOL  
15:14:05 [INFO] Account: root.fabrikam.com\DC2-CL0001$  
15:14:05 [INFO] Options: DSROLE_DC_CLONING (0x800400)  
```  
  
-   开始升级  
  
```  
15:14:05 [INFO] Promote DC as a clone  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #3: Domain Controller cloning is at 15% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #4: Domain Controller cloning is at 16% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Validate supplied paths  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\SYSVOL.  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Path is on an NTFS volume  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #5: Domain Controller cloning is at 17% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Start the worker task  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #6: Domain Controller cloning is at 20% completion...  
15:14:05 [INFO] Request for promotion returning 0  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #7: Domain Controller cloning is at 21% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   停止，并将所有广告 DS 相关服务 NTDS、 NTFRS/DFSR、 KDC （DNS） 配置  
  
> [!NOTE]  
> 关机需要很长时间 DNS 服务预期在此情况下，因为它使用广告集成的区域不会再了之前 NTDS 服务已停止-甚至可以使用，请参阅下文在此部分中的模块 DNS 事件。  
  
```  
15:14:15 [INFO] Stopping service NTDS  
15:14:15 [INFO] Stopping service NtFrs  
15:14:15 [INFO] ControlService(STOP) on NtFrs returned 1(gle=0)  
15:14:15 [INFO] DsRolepWaitForService: waiting for NtFrs to enter one of 7 states  
15:14:15 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:16 [INFO] DsRolepWaitForService: exiting because NtFrs entered STOPPED state  
15:14:16 [INFO] DsRolepWaitForService(for any end state) on NtFrs service returned 0  
15:14:16 [INFO] ControlService(STOP) on NtFrs returned 0(gle=1062)  
15:14:16 [INFO] Exiting service-stop loop after service NtFrs entered STOPPED state  
15:14:16 [INFO] StopService on NtFrs returned 0  
15:14:16 [INFO] Configuring service NtFrs to 1 returned 0  
15:14:16 [INFO] Stopping service Kdc  
15:14:16 [INFO] ControlService(STOP) on Kdc returned 1(gle=0)  
15:14:16 [INFO] DsRolepWaitForService: waiting for Kdc to enter one of 7 states  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:17 [INFO] DsRolepWaitForService: exiting because Kdc entered STOPPED state  
15:14:17 [INFO] DsRolepWaitForService(for any end state) on Kdc service returned 0  
15:14:17 [INFO] ControlService(STOP) on Kdc returned 0(gle=1062)  
15:14:17 [INFO] Exiting service-stop loop after service Kdc entered STOPPED state  
15:14:17 [INFO] StopService on Kdc returned 0  
15:14:17 [INFO] Configuring service Kdc to 1 returned 0  
15:14:17 [INFO] Stopping service DNS  
15:14:17 [INFO] ControlService(STOP) on DNS returned 1(gle=0)  
15:14:17 [INFO] DsRolepWaitForService: waiting for DNS to enter one of 7 states  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:18 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:19 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:20 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:21 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:22 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:23 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:24 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:25 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:26 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:27 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:28 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:29 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:30 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:31 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:32 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:33 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:34 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:35 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:36 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:37 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:38 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:39 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:40 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:41 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:42 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:43 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:44 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:45 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:46 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:47 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:48 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:49 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:50 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:51 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:52 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:53 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:54 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:55 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:56 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:57 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:58 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:59 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:00 [INFO] DsRolepWaitForService: exiting because DNS entered STOPPED state  
15:15:00 [INFO] DsRolepWaitForService(for any end state) on DNS service returned 0  
15:15:00 [INFO] ControlService(STOP) on DNS returned 0(gle=1062)  
15:15:00 [INFO] Exiting service-stop loop after service DNS entered STOPPED state  
15:15:00 [INFO] StopService on DNS returned 0  
15:15:00 [INFO] Configuring service DNS to 1 returned 0  
15:15:00 [INFO] ControlService(STOP) on NTDS returned 1(gle=1062)  
15:15:00 [INFO] DsRolepWaitForService: waiting for NTDS to enter one of 7 states  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:01 [INFO] DsRolepWaitForService: exiting because NTDS entered STOPPED state  
15:15:01 [INFO] DsRolepWaitForService(for any end state) on NTDS service returned 0  
15:15:01 [INFO] ControlService(STOP) on NTDS returned 0(gle=1062)  
15:15:01 [INFO] Exiting service-stop loop after service NTDS entered STOPPED state  
15:15:01 [INFO] StopService on NTDS returned 0  
15:15:01 [INFO] Configuring service NTDS to 1 returned 0  
15:15:01 [INFO] Configuring service NTDS  
15:15:01 [INFO] Configuring service NTDS to 64 returned 0  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #8: Domain Controller cloning is at 22% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #9: Domain Controller cloning is at 25% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   强制 NT5DS (NTP) 与另一台域控制器 (通常 PDCE) 的时间同步  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   联系拥有克隆源域控制器帐户域控制器  
  
-   刷新任何现有 Kerberos 门票  
  
```  
15:15:02 [INFO] Searching for a domain controller for the domain root.fabrikam.com that contains the account DC2$  
15:15:02 [INFO] Located domain controller DC1.root.fabrikam.com for domain root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #10: Domain Controller cloning is at 26% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Directing kerberos authentication to DC1.root.fabrikam.com returns 0  
15:15:02 [INFO] DsRolepFlushKerberosTicketCache() successfully flushed the Kerberos ticket cache  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #11: Domain Controller cloning is at 27% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Using site Default-First-Site-Name for server \\DC1.root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   停止 NetLogon 服务，并将其开始类型设置  
  
```  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #12: Domain Controller cloning is at 29% completion...  
15:15:02 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:15:02 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:15:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:03 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:03 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:15:03 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:15:03 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:15:03 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:15:03 [INFO] StopService on NETLOGON returned 0  
15:15:03 [INFO] Configuring service NETLOGON to 1 returned 0  
15:15:03 [INFO] Stopped NETLOGON  
15:15:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:03 [INFO] vDC Cloning: Winlogon UI Notification #13: Domain Controller cloning is at 30% completion...  
```  
  
-   将 DFSR/NTFRS 服务时自动运行配置  
  
-   删除他们现有数据库文件，在接下来启动该服务时强制 SYSVOL 的非权威同步  
  
```  
15:15:03 [INFO] Configuring service DFSR  
15:15:03 [INFO] Configuring service DFSR to 256 returned 0  
15:15:03 [INFO] Configuring service NTFRS  
15:15:03 [INFO] Configuring service NTFRS to 256 returned 0  
15:15:03 [INFO] Removing DFSR Database files for SysVol  
15:15:03 [INFO] Removing FRS Database files in C:\Windows\ntfrs\jet  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edb.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00001.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00002.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbtmp.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\ntfrs.jdb  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\sys\edb.chk  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\temp\tmp.edb  
15:15:04 [INFO] Created system volume path  
15:15:04 [INFO] Configuring service DFSR  
15:15:04 [INFO] Configuring service DFSR to 128 returned 0  
15:15:04 [INFO] Configuring service NTFRS  
15:15:04 [INFO] Configuring service NTFRS to 128 returned 0  
15:15:04 [INFO] vDC Cloning: Winlogon UI Notification #14: Domain Controller cloning is at 40% completion...  
15:15:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   开始升级过程中使用现有的 NTDS 数据库文件  
  
-   联系人 RID 母版  
  
> [!NOTE]  
> 广告 DS 服务不实际上此处安装，这是过时 instrumentation 日志中  
  
```  
15:15:04 [INFO] Installing the Directory Service  
15:15:04 [INFO] Calling NtdsInstall for root.fabrikam.com  
15:15:04 [INFO] Starting Active Directory Domain Services installation  
15:15:04 [INFO] Validating user supplied options  
15:15:04 [INFO] Determining a site in which to install  
15:15:04 [INFO] Examining an existing forest...  
15:15:04 [INFO] Starting a replication cycle between DC1.root.fabrikam.com and the RID operations master (2008r2-01.root.fabrikam.com), so that the new replica will be able to create users, groups, and computer objects...  
15:15:04 [INFO] Configuring the local computer to host Active Directory Domain Services  
15:15:04 [INFO] EVENTLOG (Warning): NTDS General / Service Control : 1539  
Active Directory Domain Services could not disable the software-based disk write cache on the following hard disk.  
Hard disk:  
c:  
Data might be lost during system failures.  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Processing : 2041  
Duplicate event log entries were suppressed.  
See the previous event log entry for details. An entry is considered a duplicate if  
the event code and all of its insertion parameters are identical. The time period for  
this run of duplicates is from the time of the previous event to the time of this event.  
Event Code:  
80000603  
Number of duplicate entries:   
2  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Configuration : 2121  
This Active Directory Domain Services server is disabling the Recycle Bin. Deleted objects may not be undeleted at this time.  
```  
  
-   更改存在源计算机数据库中的现有调用 ID  
  
-   创建新此副本各自的对象  
  
-   复制在从合作伙伴域控制器广告对象增量  
  
> [!NOTE]  
> 即使复制列为所有对象，这是只需归入更新所需的元数据。 复制 NTDS 数据库中的所有保持不变的对象已经存在，并不需要复制再次，就像使用基于 IFM 升级。  
  
```  
15:15:10 [INFO] EVENTLOG (Informational): NTDS Replication / Replication : 1109  
The invocationID attribute for this directory server has been changed. The highest update sequence number at the time the backup was created is as follows:  
InvocationID attribute (old value):  
24e7b22f-4706-402d-9b4f-f2690f730b40  
InvocationID attribute (new value):  
f74cefb2-89c2-442c-b1ba-3234b0ed62f8  
Update sequence number:  
20520  
The invocationID is changed when a directory server is restored from backup media, is configured to host a writeable application directory partition, has been resumed after a virtual machine snapshot has been applied, after a virtual machine import operation, or after a live migration operation. Virtualized domain controllers should not be restored using virtual machine snapshots. The supported method to restore or rollback the content of an Active Directory Domain Services database is to restore a system state backup made with an Active Directory Domain Services-aware backup application.  
15:15:10 [INFO] EVENTLOG (Error): NTDS General / Internal Processing : 1168  
Internal error: An Active Directory Domain Services error has occurred.  
Additional Data  
Error value (decimal):  
2  
Error value (hexadecimal):  
2  
Internal ID:  
7011658  
15:15:11 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC1.root.fabrikam.com...  
15:15:11 [INFO] Replicating the schema directory partition  
15:15:11 [INFO] Replicated the schema container.  
15:15:12 [INFO] Active Directory Domain Services updated the schema cache.  
15:15:12 [INFO] Replicating the configuration directory partition  
15:15:12 [INFO] Replicating data CN=Configuration,DC=root,DC=fabrikam,DC=com: Received 2612 out of approximately 2612 objects and 94 out of approximately 94 distinguished name (DN) values...  
15:15:12 [INFO] Replicated the configuration container.  
15:15:13 [INFO] Replicating critical domain information...  
15:15:13 [INFO] Replicating data DC=root,DC=fabrikam,DC=com: Received 109 out of approximately 109 objects and 35 out of approximately 35 distinguished name (DN) values...  
15:15:13 [INFO] Replicated the critical objects in the domain container.  
```  
  
-   填充 GC 分区，在必要时与任何缺少的更新  
  
-   完成升级的关键广告 DS 部分  
  
```  
15:15:13 [INFO] EVENTLOG (Informational): NTDS General / Global Catalog : 1110  
Promotion of this domain controller to a global catalog will be delayed for the following interval.  
Interval (minutes):  
5  
This delay is necessary so that the required directory partitions can be prepared before the global catalog is advertised. In the registry, you can specify the number of seconds that the directory system agent will wait before promoting the local domain controller to a global catalog. For more information about the Global Catalog Delay Advertisement registry value, see the Resource Kit Distributed Systems Guide.  
15:15:14 [INFO] EVENTLOG (Informational): NTDS General / Service Control : 1000  
Microsoft Active Directory Domain Services startup complete, version 6.2.8225.0   
15:15:15 [INFO] Creating new domain users, groups, and computer objects  
15:15:16 [INFO] Completing Active Directory Domain Services installation  
15:15:16 [INFO] NtdsInstall for root.fabrikam.com returned 0  
15:15:16 [INFO] DsRolepInstallDs returned 0  
15:15:16 [INFO] Installed Directory Service  
```  
  
-   完成 SYSVOL 入站的复制  
  
```  
15:15:16 [INFO] vDC Cloning: Winlogon UI Notification #15: Domain Controller cloning is at 60% completion...  
15:15:16 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Completed system volume replication  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #16: Domain Controller cloning is at 70% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] SetProductType to 2 [LanmanNT] returned 0  
15:15:18 [INFO] Set the product type  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #17: Domain Controller cloning is at 71% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #18: Domain Controller cloning is at 72% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Set the system volume path for NETLOGON  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #19: Domain Controller cloning is at 73% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Replicating non critical information  
15:15:18 [INFO] User specified to not replicate non-critical data  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #20: Domain Controller cloning is at 80% completion...  
15:15:18 [INFO] Stopped the DS  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #21: Domain Controller cloning is at 90% completion...  
15:15:18 [INFO] Configuring service NTDS  
15:15:18 [INFO] Configuring service NTDS to 16 returned 0  
```  
  
-   启用 DNS 注册的客户端  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   运行 DefaultDCCloneAllowList.xml 由指定的 SYSPREP 模块<SysprepInformation>元素。  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   复制升级是完整  
  
-   删除 DSRM 启动标志服务器启动通常下一次以便  
  
-   重命名 dccloneconfig.xml，以使它不在下一步启动时在重新阅读  
  
-   重启计算机  
  
```  
15:15:32 [INFO] The attempted domain controller operation has completed  
15:15:32 [INFO] Updating service status to 4  
15:15:32 [INFO] DsRolepSetOperationDone returned 0  
15:15:32 [INFO] vDC Cloning: Set vDCCloningComplete event.  
15:15:32 [INFO] vDC Cloneing: Clearing Boot into DSRM flag succeeded.  
15:15:32 [INFO] vDC Cloning: Winlogon UI Notification #22: Cloning Domain Controller succeeded. Now rebooting...  
15:15:33 [INFO] vDC Cloning: Renamed vDC clone configuration file.  
15:15:33 [INFO] vDC Cloning: The old name is: C:\Windows\NTDS\DCCloneConfig.xml  
15:15:33 [INFO] vDC Cloning: The new name is: C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml  
15:15:34 [INFO] vDC Cloning: Release Ipv4 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] vDC Cloning: Release Ipv6 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] Rebooting machine  
```  
  
##### <a name="active-directory-web-services-event-log"></a>Active Directory Web 服务事件日志  
复制发生时，NTDS。DIT 数据库很长时间离线状态。 为此，ADWS 服务日志至少一个事件。 复制完毕后，ADWS 服务的启动方式、 笔记，那里尚不可有效的计算机证书而又 （那里可能需要也可能不是，具体取决于您的环境或不部署自动注册的 Microsoft PKI），然后启动新的域控制器实例。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**1202**|ADWS 实例事件。|这台计算机现在承载指定的目录实例，但 Active Directory Web 服务可能没有服务它。 Active Directory Web 服务会定期重新尝试此操作。<br /><br />目录实例： NTDS<br /><br />目录实例 LDAP 端口： 389<br /><br />目录实例 SSL 端口： 636|  
|**1000**|ADWS 实例事件。|开始 active Directory Web 服务|  
|**1008**|ADWS 实例事件。|Active Directory Web 服务已成功缩短其安全权限|  
|**1100**|ADWS 实例事件。|中指定的值<appsettings>不会出错已加载的 Active Directory Web 服务的配置文件的部分。|  
|**1400**|ADWS 实例事件。|ADWS 证书事件"Active Directory Web 服务找不到指定的证书名称服务器证书。 证书需要使用 SSL 月 TLS 连接。 若要使用 SSL 月 TLS 连接，确认计算机上安装了有效服务器身份验证证书从受信任认证颁发机构 （加拿大）。<br /><br />证书名称：*<Server FQDN>*|  
|**1100**|ADWS 实例事件。|中指定的值<appsettings>不会出错已加载的 Active Directory Web 服务的配置文件的部分。|  
|**1200**|ADWS 实例事件。|现在，active Directory Web 服务所服务指定的目录实例。<br /><br />目录实例： NTDS<br /><br />目录实例 LDAP 端口： 389<br /><br />目录实例 SSL 端口： 636|  
  
##### <a name="dns-server-event-log"></a>DNS 服务器事件日志  
复制进行，同时按广告 DS 数据库离线时仍在运行 DNS 服务 DNS 服务将体验简短预期的中断。 使用 Active Directory 集成 DNS，但使用标准主要或辅助 DNS 如果发生这种情况。 这些错误多次用户身份登录。 复制完成后，DNS 重新联机正常。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**4013**|DNS 服务器服务|DNS 服务器等待 Active Directory 域服务 (广告 DS) 发送信号，完成后的初始同步到的目录。 DNS 服务器服务无法启动，直到初始同步完毕，因为关键 DNS 数据可能不会复制到此域控制器。 如果广告 DS 事件日志中指示 DNS 名称解决问题，请考虑为在 Internet 协议属性这台计算机的 DNS 服务器列表添加此域另一个 DNS 服务器的 IP 地址。 将记录此事件直到广告 DS 终止状态初始同步已成功完成每两分钟。|  
|**4015**|DNS 服务器服务|DNS 服务器时遇到了 Active Directory 从严重错误。 检查 Active Directory 它正在正常运行。 （这可能空） 的扩展的错误调试信息"""。 事件数据包含错误。|  
|**4000**|DNS 服务器服务|无法打开 Active Directory DNS 服务器。  此 DNS 服务器配置获取和使用该区域目录中的信息，并且无法加载没有它的区域。  检查 Active Directory 它正在正常运行，并重新加载区域。 事件数据是错误代码。|  
|**4013**|DNS 服务器服务|DNS 服务器等待 Active Directory 域服务 (广告 DS) 发送信号，完成后的初始同步到的目录。 DNS 服务器服务无法启动，直到初始同步完毕，因为关键 DNS 数据可能不会复制到此域控制器。 如果广告 DS 事件日志中指示 DNS 名称解决问题，请考虑为在 Internet 协议属性这台计算机的 DNS 服务器列表添加此域另一个 DNS 服务器的 IP 地址。 将记录此事件直到广告 DS 终止状态初始同步已成功完成每两分钟。|  
|**2**|DNS 服务器服务|已启动 DNS 服务器。|  
|**4**|DNS 服务器服务|在完成操作 DNS 服务器后台加载的区域。 所有区域现都已适用于 DNS 更新和区域传输的为允许通过其个别区域配置。|  
  
##### <a name="file-replication-service-event-log"></a>文件复制 Service 事件日志  
复制在文件复制服务非授权同步从合作伙伴。 复制来实现此目的通过删除 NTFRS 数据库文件以及离开 SYSVOL 的内容不变，用作预植入的数据。 我们期望两次尝试进行同步。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**13562**|NtFrs|以下是轮询 FRS 副本域控制器 DC2.root.fabrikam.com 设置配置信息时遇到的文件复制服务的错误以及警告摘要。<br /><br />无法为域控制器绑定。 将在下一个轮询周期重试|  
|**13502**|NtFrs|复制服务已停止。|  
|**13565**|NtFrs|文件复制服务正在初始化向系统卷另一个域控制器中的数据。 计算机 DC2 不能成为域控制器，直到完成此过程。 然后将作为 SYSVOL 共享向系统卷。<br /><br />若要检查 SYSVOL 共享，在命令提示符下，键入：<br /><br />共享的网络<br /><br />当文件复制服务完成初始化过程时，将显示在 SYSVOL 共享。<br /><br />向系统卷初始化可能需要一些时间。 时间不依赖于向系统卷中的数据量，以及其他域控制器，域控制器之间复制间隔可用性。|  
|**13501**|NtFrs|在开始该文件复制服务|  
|**13502**|NtFrs|复制服务已停止。|  
|**13503**|NtFrs|复制服务已停止。|  
|**13565**|NtFrs|文件复制服务正在初始化向系统卷另一个域控制器中的数据。 计算机 DC2 不能成为域控制器，直到完成此过程。 然后将作为 SYSVOL 共享向系统卷。<br /><br />若要检查 SYSVOL 共享，在命令提示符下，键入：<br /><br />共享的网络<br /><br />当文件复制服务完成初始化过程时，将显示在 SYSVOL 共享。<br /><br />向系统卷初始化可能需要一些时间。 时间不依赖于向系统卷中的数据量，以及其他域控制器，域控制器之间复制间隔可用性。|  
|**13501**|NtFrs|正在启动文件复制服务。|  
|**13553**|NtFrs|文件复制服务成功以下副本集中到添加该计算机：<br /><br />"域系统卷 （SYSVOL 共享）"<br /><br />为该事件的相关信息如下所示：<br /><br />计算机 DNS 名称*<Domain Controller FQDN>*<br /><br />将副本组成员名称*<Domain Controller>*<br /><br />副本集根路径*<path>*<br /><br />将副本临时目录路径*<path>*<br /><br />将副本工作目录路径*<path>*|  
|**13520**|NtFrs|该文件复制服务移动现有文件<path>到* <path> *\NtFrs_PreExisting___See_EventLog。<br /><br />该文件复制服务可能会删除的文件* <path>*随时 \NtFrs_PreExisting___See_EventLog。 可以将文件保存被删除通过复制外出* <path> *\NtFrs_PreExisting___See_EventLog。 将文件复制到 c:\windows\sysvol\domain 可能导致名称冲突，如果文件已存在一些其他复制合作伙伴上。<br /><br />在某些情况下，文件复制服务可能复制文件从* <path>*插入 \NtFrs_PreExisting___See_EventLog * <path> *而不是从其他一些复制合作伙伴复制该文件。<br /><br />通过删除的文件，可以随时恢复空间* <path> *\NtFrs_PreExisting___See_EventLog。"|  
|**13508**|NtFrs|他文件复制服务时遇到问题实现从复制* \\\\ <Domain Controller FQDN> *到* <Domain Controller> *为* <path> *使用<br /><br />DNS name *\\\\<Domain Controller FQDN>*. FRS 将保留重新尝试收取费用。<br /><br />以下是一些你会看到此警告的原因。<br /><br />[1] FRS 无法正确解决了 DNS 名称* \\\\ <Domain Controller FQDN> *从这台计算机。<br /><br />[上未运行 2] FRS * \\\\ <Domain Controller FQDN> *。<br /><br />[在此副本 Active Directory 域服务 3] 拓扑信息尚未复制到所有域控制器。<br /><br />每个连接，修复该问题后您将看到指示建立连接的另一个事件日志消息后，将显示此事件日志消息。|  
|**13509**|NtFrs|复制服务已启用从复制* \\\\ <Domain Controller FQDN> *到* <Domain Controller> *为* <Path> *后重复重试。|  
|**13516**|NtFrs|该文件复制服务不会再导致计算机无法* <Domain Controller> *成为域控制器。 已成功初始化向系统卷和已通知向系统卷现在已作为 SYSVOL 共享准备 Netlogon 服务。<br /><br />键入"网络共享"若要检查 SYSVOL 共享。"|  
  
##### <a name="dfs-replication-event-log"></a>DFS 复制事件日志  
复制在 DFSR 服务非授权同步从合作伙伴。 复制来实现此目的通过删除 DFSR 数据库文件以及离开 SYSVOL 的内容不变，用作预植入的数据。 我们期望两次尝试进行同步。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**1004**|DFSR|DFS 复制服务已经启动。|  
|**1314**|DFSR|DFS 复制服务成功配置调试日志文件。<br /><br />其他信息：<br /><br />调试日志文件路径： C:\Windows\debug|  
|**6102**|DFSR|DFS 复制服务已成功注册 WMI 提供程序|  
|**1206**|DFSR|DFS 复制服务成功连接到域控制器 DC2.corp.contoso.com 访问配置的信息。|  
|**1210**|DFSR|DFS 复制服务成功设置传入复制请求 RPC 的聆听者。<br /><br />其他信息：<br /><br />端口： 0"|  
|**4614**|DFSR|DFS 复制服务在本地路径 C:\Windows\SYSVOL\domain 初始化 SYSVOL，并且正在等待执行初始复制。 它具有与其合作伙伴复制之前，复制的文件夹将保留在初始的同步状态中。 如果该服务器处于域控制器在升级过程，域控制器而不会向你推荐并且充当域控制器，直到解决此问题。 如果在初始的同步状态也是指定的合作伙伴或者遇到此服务器或同步合作伙伴共享冲突，则可以发生此问题。 如果此过程中发生事件 SYSVOL 迁移从文件复制服务 (FRS) DFS 复制到，更改不会复制，直到解决此问题。 这可能会导致该服务器变得与其他域控制器同步 SYSVOL 文件夹。<br /><br />其他信息：<br /><br />已复制的文件夹名称： SYSVOL 共享<br /><br />已复制的文件夹 ID:*<GUID>*<br /><br />复制组名称： 域系统音量<br /><br />复制组 ID:*<GUID>*<br /><br />成员 ID:*<GUID>*<br /><br />仅阅读： 0|  
|**4604**|DFSR|DFS 复制服务成功初始化在本地路径 C:\Windows\SYSVOL\domain SYSVOL 复制文件夹。 此成员已完成 SYSVOL 与合作伙伴 dc1.corp.contoso.com 的初始的同步。若要检查存在 SYSVOL 共享，打开 command prompt 窗口，然后键入""网络共享""。<br /><br />其他信息：<br /><br />已复制的文件夹名称： SYSVOL 共享<br /><br />已复制的文件夹 ID:*<GUID>*<br /><br />复制组名称： 域系统音量<br /><br />复制组 ID:*<GUID>*<br /><br />成员 ID:*<GUID>*<br /><br />同步合作伙伴：*<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>疑难解答虚拟化的域控制器安全还原  
  
### <a name="tools-for-troubleshooting"></a>以获取疑难解答工具  
  
#### <a name="logging-options"></a>登录选项  
内置的日志，则使用域控制器安全快照还原解决问题的最重要的工具。 所有这些日志已启用，并且配置为最大的详细级别，默认情况。  
  
|||  
|-|-|  
|**操作**|**日志**|  
|**创建快照**|-事件 viewer\Applications 和服务 logs\Microsoft\Windows\Hyper-V-工作|  
|**快照还原**|-事件 viewer\Applications 和服务 logs\Directory 服务<br />-事件 viewer\Windows logs\System<br />-事件 viewer\Windows logs\Application<br />-事件 viewer\Applications 和服务 logs\File 复制服务<br />-事件 viewer\Applications 和服务 logs\DFS 复制<br />-事件 viewer\Applications 和服务 logs\DNS<br />-事件 viewer\Applications 和服务 logs\Microsoft\Windows\Hyper-V-工作|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>用于域控制器配置疑难解答工具和命令  
若要解决无法通过日志介绍此内容的问题，请使用以下工具作为起始点：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   网络监视器 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>常规方法以获取疑难解答域控制器安全还原  
  
1.  是安全的快照还原预期的那样，但时遇到问题？  
  
    1.  检查目录服务事件日志  
  
        1.  是否有快照还原错误？  
  
        2.  是否有广告复制错误？  
  
    2.  检查系统事件日志  
  
        1.  是否有通信错误？  
  
        2.  是否有 AD 错误？  
  
2.  是安全的快照还原意外？  
  
    1.  若要确定谁或是什么原因导致回滚虚拟机监控程序审核日志检查一段  
  
    2.  请联系的虚拟机监控程序所有管理员，并询问它们移到谁回退 VM 无需通知  
  
3.  服务器实现 USN 回退保护并且不安全地进行还原？  
  
    1.  检查受支持的虚拟机监控程序或集成服务目录服务事件日志  
  
    2.  检查操作系统和验证运行 Windows Server 2012？  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>具体问题疑难解答  
  
#### <a name="events"></a>事件  
所有虚拟化域控制器安全快照还原到还原的域控制器 VM 目录服务事件日志写入事件。 应用程序、 系统、 文件复制服务，并 DFS 复制的事件日志，还可能包含用于恢复失败有用的疑难解答信息。  
  
下面是目录服务事件日志中的 Windows Server 2012 安全还原特定的事件。  
  
|||  
|-|-|  
|**事件 ID**|**2170**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|警告|  
|**消息**|已检测到代 ID 更改。<br /><br />缓存 DS （旧值） 在代 ID: 1%<br /><br />当前在虚拟机 （新值） 代 ID: %2<br /><br />在应用虚拟机快照、 虚拟机导入操作或之后实时迁移操作后，会发生代 ID 更改。 *<COMPUTERNAME>*将创建新的调用 ID 恢复域控制器。 不应使用虚拟机快照恢复虚拟化的域控制器。 受支持的方法，若要还原或回退 Active Directory 域服务数据库中的内容是使用 Active Directory 域服务感知备份应用程序进行系统状态备份还原。|  
|**笔记和分辨率**|如果需要的快照，这是一个成功事件。 如果没有，请检查超-V 工作事件日志，或与虚拟机监控程序管理员联系。|  
  
|||  
|-|-|  
|**事件 ID**|**2174**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|DC 是简虚拟域控制器克隆以及恢复虚拟的域控制器快照。|  
|**笔记和分辨率**|启动物理域控制器或不从快照还原的虚拟化的域控制器时预期的事件|  
  
|||  
|-|-|  
|**事件 ID**|**2181**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|由于虚拟机，正在还原到以前的状态中止事务。  在应用虚拟机快照、 虚拟机导入操作，或之后实时迁移操作后，将发生这种情况。|  
|**笔记和分辨率**|预计时还原快照。 交易跟踪 VM 代 ID 更改|  
  
|||  
|-|-|  
|**事件 ID**|**2185**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*停止用于 SYSVOL 文件夹复制该 FRS 或 DFSR 服务。<br /><br />服务名称: %1<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*必须初始化非授权本地 SYSVOL 副本上的还原。 停止使用 FRS 或 DFSR 服务复制 SYSVOL 文件夹和开头的相应的注册表项和值触发还原通过执行此操作。 重新启动 FRS 或 DFSR 服务时，将记录事件 2187年。|  
|**笔记和分辨率**|预计时还原快照。 此域控制器上的所有 SYSVOL 数据都替换合作伙伴 DC 的副本。|  
|**事件 ID**|2186|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*若要阻止用于复制 SYSVOL 文件夹 FRS 或 DFSR 服务的失败。<br /><br />服务名称: %1<br /><br />错误代码: %2<br /><br />错误消息: %3<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*必须初始化非授权本地 SYSVOL 副本上的还原。 这是通过停止 FRS 或 DFSR 复制服务用于复制 SYSVOL 文件夹，然后从它开始相应的注册表项和值触发还原。 *<COMPUTERNAME>*停止当前正在运行的服务失败，并且不能完成非授权恢复。 请手动执行非授权还原。|  
|**笔记和分辨率**|检查系统、 FRS 和 DFSR 事件日志中的详细信息。|  
  
|||  
|-|-|  
|**事件 ID**|**2187**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*开始使用复制 SYSVOL 文件夹 FRS 或 DFSR 服务。<br /><br />服务名称: %1<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*所需初始化非授权本地 SYSVOL 副本上的还原。 这是通过的停止使用 FRS 或 DFSR 服务复制 SYSVOL 文件夹和开头的相应的注册表项和触发还原的值。|  
|**笔记和分辨率**|预计时还原快照。 此域控制器上的所有 SYSVOL 数据都替换合作伙伴 DC 的副本。|  
  
|||  
|-|-|  
|**事件 ID**|**2188**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*无法启动用于复制 SYSVOL 文件夹 FRS 或 DFSR 服务。<br /><br />服务名称: %1<br /><br />错误代码: %2<br /><br />错误消息: %3<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*需要初始化非授权本地 SYSVOL 副本上的还原。 这是通过的停止使用 FRS 或 DFSR 服务复制 SYSVOL 和开头相应的注册表项和触发还原的值。 *<COMPUTERNAME>*无法启动用于复制 SYSVOL 文件夹 FRS 或 DFSR 服务，并且不能完成非授权恢复。 请执行非权威还原手动和重启该服务。|  
|**笔记和分辨率**|检查系统、 FRS 和 DFSR 事件日志中的详细信息。|  
  
|||  
|-|-|  
|**事件 ID**|**2189**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*设置以下的注册表值，以在非权威还原初始化 SYSVOL 副本：<br /><br />注册表项 %1<br /><br />注册表值： %2<br /><br />注册表值数据： %3<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*需要初始化非授权本地 SYSVOL 副本上的还原。 这是通过的停止使用 FRS 或 DFSR 服务复制 SYSVOL 文件夹和开头的相应的注册表项和触发还原的值。|  
|**笔记和分辨率**|预计时还原快照。 此域控制器上的所有 SYSVOL 数据都替换合作伙伴 DC 的副本。|  
  
|||  
|-|-|  
|**事件 ID**|**2190**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*设置以下的注册表值，以在非权威还原期间初始化 SYSVOL 副本失败：<br /><br />注册表项 %1<br /><br />注册表值： %2<br /><br />注册表值数据： %3<br /><br />错误代码: %4<br /><br />错误消息: %5<br /><br />Active Directory 检测到承载域控制器角色虚拟机，已还原到以前的状态。 *<COMPUTERNAME>*需要初始化非授权本地 SYSVOL 副本上的还原。 这是通过的停止使用 FRS 或 DFSR 服务复制 SYSVOL 文件夹和开头的相应的注册表项和触发还原的值。 *<COMPUTERNAME>*设置上述注册表值失败，并且不能完成非授权恢复。 请手动执行非授权还原。|  
|**笔记和分辨率**|检查应用和系统事件日志。 调查第三方应用程序可能会阻止注册表更新。|  
  
|||  
|-|-|  
|**事件 ID**|**2200**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*初始化复制以显示当前的域控制器。 复制完成后，将记录事件 2201年。|  
|**笔记和分辨率**|预计时还原快照。 标记广告的入站复制的开始。|  
  
|||  
|-|-|  
|**事件 ID**|**2201**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*已完成复制以显示当前的域控制器。|  
|**笔记和分辨率**|预计时还原快照。 标记广告的入站复制的结束。|  
  
|||  
|-|-|  
|**事件 ID**|**2202**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*复制失败，以使最新的域控制器。 下一次定期复制后，域控制器也会更新。|  
|**笔记和分辨率**|检查目录服务和系统事件日志。 使用 repadmin.exe 尝试强制复制并注意的任何失败。|  
  
|||  
|-|-|  
|**事件 ID**|**2204**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*已检测到更改的虚拟机代 id。 更改意味着虚拟域控制器，已到以前的状态中恢复。 *<COMPUTERNAME>*将执行以下操作保护针对可能数据分歧还原的域控制器，并保护创建的安全主体与 Sid 重复：<br /><br />创建新的调用 ID<br /><br />使失效当前 RID 池<br /><br />将在接下来的入站复制验证拥有的域控制器。 在此窗口域控制器举办 FSMO 角色，如果该角色将不可用。<br /><br />开始 SYSVOL 复制服务还原操作。<br /><br />启动复制以显示已还原的域控制器到最新状态。<br /><br />请求 RID 新池。|  
|**笔记和分辨率**|预计时还原快照。 这可以说明所有各种重置将安全恢复过程中发生的操作。|  
  
|||  
|-|-|  
|**事件 ID**|**2205**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*虚拟域控制器已还原到以前的状态后失效当前 RID 池。|  
|**笔记和分辨率**|预计时还原快照。 本地 RID 池必须域控制器有上移动的时间以及他们可能已签发销毁。|  
  
|||  
|-|-|  
|**事件 ID**|**2206**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*无法使当前 RID 池无效后虚拟域控制器已还原到以前的状态。<br /><br />其他数据：<br /><br />错误代码： %1<br /><br />错误值： %2|  
|**笔记和分辨率**|检查目录服务和系统事件日志。 验证可以从该服务器使用 Dcdiag.exe /test:ridmanager 达到不用主处于联机状态|  
  
|||  
|-|-|  
|**事件 ID**|**2207**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*无法还原后虚拟域控制器已还原到以前的状态。 重新启动到 DSRM 请求。 请检查以前事件的详细信息。|  
|**笔记和分辨率**|检查目录服务和系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2208**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|信息|  
|**消息**|*<COMPUTERNAME>*删除 DFSR 数据库非权威恢复期间初始化 SYSVOL 副本。|  
|**笔记和分辨率**|预计时还原快照。 这保证 DFSR 非授权同步 SYSVOL 从 DC 伙伴。 请注意，任何其他 DFSR 复制文件夹与 SYSVOL 相同的卷上还非-授权同步 （域控制器不建议将到主机自定义 DFSR 设置与 SYSVOL 相同的卷上）。|  
  
|||  
|-|-|  
|**事件 ID**|**2209**|  
|**源**|Microsoft 的 Windows ActiveDirectory_DomainService|  
|**严重性**|错误|  
|**消息**|*<COMPUTERNAME>*删除 DFSR 数据库失败。<br /><br />其他数据：<br /><br />错误代码： %1<br /><br />错误值： %2<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 *<COMPUTERNAME>*需要初始化非授权本地 SYSVOL 副本上的还原。 DFSR，这是通过停止 DFSR 服务、 删除 DFSR 数据库和重新启动该服务。 在重新启动 DFSR 时将重新生成数据库并开始初始同步。|  
|**笔记和分辨率**|检查 DFSR 事件日志。|  
  
#### <a name="error-messages"></a>错误消息  
有任何直接交互式错误失败的虚拟化的域控制器安全快照还原;在目录服务事件日志记录克隆的所有信息。 当然，关键复制或任何服务器广告错误体现为症状其他位置。  
  
#### <a name="known-issues-and-support-scenarios"></a>已知的问题并支持的方案  
[疑难解答域控制器安全还原常规方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore)通常足够大多数的问题进行疑难解答。  
  
|||  
|-|-|  
|**问题**|**无法在最近安全还原的域控制器上创建新的安全原则**|  
|**症状**|还原后快照，尝试与的域控制器失败创建一个新的安全主要 （用户、 计算机、 组）：<br /><br />错误 0x2010<br /><br />目录服务无法分配相关的标识符。|  
|**分辨率和笔记**|此问题是由的不用母版 FSMO 角色还原的计算机的过时知识。 如果后快照是拍摄，然后稍后还原，角色已移到此或其他域控制器，还原的域控制器不会在知识 RID 主机直到初始复制都已完成。<br /><br />若要解决此问题，允许完成广告复制到恢复的域控制器站。 如果仍不起作用，则验证所有域控制器都拥有同一哪个 DC 承载不用主机正确知识。|  
  
|||  
|-|-|  
|**问题**|**不共享 SYSVOL，向你推荐还原的域控制器**|  
|**症状**|还原快照之后, 一个或多个 Dc 执行不向你推荐不共享 sysvol，，不具有最新状态 SYSVOL 内容|  
|**分辨率和笔记**|没有正确的 DFSR 或 FRS.复制工作 SYSVOL 副本 DC 上游合作伙伴。 此问题无关安全还原，但很可能会清单作为安全还原的问题，因为在客户是意识到影响未还原的 Dc 其他复制问题|  
  
### <a name="advanced-troubleshooting"></a>高级故障排除  
此模块搜寻会学习通过使用的高级疑难解答*工作*作为示例、 对发生的事情的一些说明日志。 如果您已了解成功虚拟化的域控制器操作如下所示，失败变得更加您的环境中明显。 按它们的源的升序与提供这些日志*预期*复制的域控制器在每个日志与相关的事件。  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>还原复制 SYSVOL 使用 DFSR 域控制器  
  
##### <a name="directory-services-event-log"></a>目录服务事件日志  
目录服务日志包含大部分安全还原操作信息。 虚拟机监控程序更改 VM 代 ID 和 NTDS 服务笔记它，使无效 RID 池，然后更改调用 id。 新的 VM 代 ID 是设置和归巢的广告的数据服务器复制。 DFSR 服务已停止并且承载 SYSVOL 其数据库时会删除它，强制非授权同步站。 调整 USN 高水印。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**2170**|ActiveDirectory_DomainService|已检测到代 ID 更改。<br /><br />缓存 DS （旧值） 在代 ID:<br /><br />*<number>*<br /><br />当前在虚拟机 （新值） 代 ID:<br /><br />*<number>*<br /><br />在应用虚拟机快照、 虚拟机导入操作或之后实时迁移操作后，会发生代 ID 更改。 Active Directory 域服务将创建新的调用 ID 恢复域控制器。 不应使用虚拟机快照恢复虚拟化的域控制器。 受支持的方法，若要还原或回退 Active Directory 域服务数据库中的内容是使用 Active Directory 域服务感知备份应用程序进行系统状态备份还原。"|  
|**2181**|ActiveDirectory_DomainService|由于虚拟机，正在还原到以前的状态中止事务。  在应用虚拟机快照、 虚拟机导入操作，或之后实时迁移操作后，将发生这种情况。|  
|**2204**|ActiveDirectory_DomainService|Active Directory 域服务已检测到更改的虚拟机代 id。 更改意味着虚拟域控制器，已到以前的状态中恢复。 Active Directory 域服务将执行以下操作保护针对可能数据分歧还原的域控制器，并保护创建的安全主体与 Sid 重复：<br /><br />创建新的调用 ID<br /><br />使失效当前 RID 池<br /><br />将在接下来的入站复制验证拥有的域控制器。 在此窗口域控制器举办 FSMO 角色，如果该角色将不可用。<br /><br />开始 SYSVOL 复制服务还原操作。<br /><br />启动复制以显示已还原的域控制器到最新状态。<br /><br />请求新的 RID 池中。"|  
|**2181**|ActiveDirectory_DomainService|由于虚拟机，正在还原到以前的状态中止事务。  在应用虚拟机快照、 虚拟机导入操作，或之后实时迁移操作后，将发生这种情况。|  
|**1109**|ActiveDirectory_DomainService|此目录服务器 invocationID 属性已更改。 在创建备份的时间的最高更新序列号如下所示：<br /><br />InvocationID 属性 （旧值）：<br /><br />*<GUID>*<br /><br />InvocationID 属性 （新值）：<br /><br />*<GUID>*<br /><br />更新的序列号：<br /><br />*<number>*<br /><br />从备份的媒体还原目录服务器时更改 invocationID 配置提供可写的应用程序目录分区、 虚拟机快照在应用后，或实时迁移操作后虚拟机导入操作，已恢复。 不应使用虚拟机快照恢复虚拟化的域控制器。 还原或回退到受支持的方法 Active Directory 域服务数据库中的内容是使用 Active Directory 域感知服务的备份应用程序进行系统状态备份还原。"|  
|**2179**|ActiveDirectory_DomainService|已设置为以下参数域控制器计算机对象的 msDS GenerationId 属性：<br /><br />GenerationID 属性：<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 Active Directory 域服务初始化复制以显示当前的域控制器。 复制完成后，将记录事件 2201年。|  
|**2201**|ActiveDirectory_DomainService|Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 在完成操作 active Directory 域服务复制以显示当前的域控制器。|  
|**2185**|ActiveDirectory_DomainService|Active Directory 域服务停止用于复制 SYSVOL 文件夹 FRS 或 DFSR 服务。<br /><br />服务名称：<br /><br />DFSR<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 Active Directory 域服务必须初始化非授权本地 SYSVOL 副本上的还原。 停止使用 FRS 或 DFSR 服务复制 SYSVOL 文件夹和开头的相应的注册表项和值触发还原通过执行此操作。 2187 将记录事件 FRS 或 DFSR 服务重启。"|  
|**2208**|ActiveDirectory_DomainService|Active Directory 域服务中删除 DFSR 数据库非权威恢复期间初始化 SYSVOL 副本。<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 需要初始化非授权本地 SYSVOL 副本上的还原 active Directory 域服务。 DFSR，这是通过停止 DFSR 服务、 删除 DFSR 数据库和重新启动该服务。 在重新启动 DFSR 时将重新生成数据库并开始初始同步。"|  
|**2187**|ActiveDirectory_DomainService|Active Directory 域服务开始用于 SYSVOL 文件夹复制 FRS 或 DFSR 服务。<br /><br />服务名称：<br /><br />DFSR<br /><br />Active Directory 检测到虚拟机承载域控制器，已还原到以前的状态。 初始化非授权还原本地 SYSVOL 副本上的所需的 active Directory 域服务。 这是通过的停止使用 FRS 或 DFSR 服务复制 SYSVOL 文件夹和开头的相应的注册表项和触发还原的值。 "|  
|**1587**|ActiveDirectory_DomainService|此目录服务还原或已配置为举办应用程序目录分区。 因此，其复制身份已发生更改。 合作伙伴已请求使用我们的旧身份复制的更改。 已调整了起始的序列号。<br /><br />对应以下对象 GUID 已请求更改开始之前本地目录服务已还原从备份的媒体 USN USN 目标目录服务。<br /><br />对象 GUID:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />还原次 USN:<br /><br />*<number>*<br /><br />因此，已使用以下设置配置目标目录服务的最新的矢量。<br /><br />以前的数据库 GUID:<br /><br />*<GUID>*<br /><br />以前的对象 USN:<br /><br />*<number>*<br /><br />以前属性 USN:<br /><br />*<number>*<br /><br />新的数据库 GUID:<br /><br />*<GUID>*<br /><br />新的对象 USN:<br /><br />*<number>*<br /><br />新的属性 USN:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>系统事件日志  
系统事件日志笔记，可以将离线虚拟机联机或主机时间进行同步时出现的计算机时间。 使无效 RID 池和 DFSR 或 FRS 服务会重新启动。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**1**|内核常规|系统时间已更改为*？<now>* 从*< 快照时间月日期 >*。<br /><br />更改原因： 应用程序或系统组件时更改。|  
|**16654**|三千-目录-服务|帐户标识符 (Rid) 的池已失效。 在以下预期的情况下可能会导致该问题：<br /><br />1.从备份还原域控制器。<br /><br />2.从快照还原域控制器在虚拟机上运行。<br /><br />3.管理员手动已失效池。<br /><br />请参阅 https://go.microsoft.com/fwlink/?LinkId=226247 详细信息。|  
|**7036**|服务控制管理器|DFS 复制服务输入停止的状态。|  
|**7036**|服务控制管理器|DFS 复制服务输入正在运行的状态。|  
  
##### <a name="application-event-log"></a>应用程序事件日志  
应用程序事件日志笔记 DFSR 数据库停止和启动。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**103**|ESENT|DFSRs (1360) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db： 数据库引擎停止实例 (0)。<br /><br />脏关机： 0<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.141、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.000、 [10] 0.000、 [11] 介于 0.016、 [12] 0.000、 [13] 0.000、 [14] 0.000、 0.000 [15]。|  
|**102**|ESENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db： 数据库引擎 (6.02.8189.0000) 启动新实例 (0)。|  
|**105**|ESENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db： 数据库引擎启动新实例 (0)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.031、 [10] 0.000、 0.000 [11]。|  
|||DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db： 数据库引擎创建了一个新的数据库 (月 1 日，\\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 介于 0.016、 [4] 0.062、 [5] 0.000、 [6] 介于 0.016、 [7] 0.000、 [8] 0.000、 [9] 0.015、 [10] 0.000、 0.000 [11]。|  
  
##### <a name="dfs-replication-event-log"></a>DFS 复制事件日志  
DFSR 服务已停止并且时会删除它包含 SYSVOL 的数据库，强制非授权同步站。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**1006**|DFSR|将停止 DFS 复制服务。|  
|**1008**|DFSR|DFS 复制服务已停止。|  
|**1002**|DFSR|正在启动 DFS 复制服务。|  
|**1004**|DFSR|DFS 复制服务已经启动。|  
|**1314**|DFSR|DFS 复制服务成功配置调试日志文件。<br /><br />其他信息：<br /><br />调试日志文件路径： C:\Windows\debug|  
|**6102**|DFSR|DFS 复制服务已成功注册 WMI 提供程序。|  
|**1206**|DFSR|DFS 复制服务成功联系的域控制器* <domain controller FQDN> *访问配置的信息。|  
|**1210**|DFSR|DFS 复制服务成功设置传入复制请求 RPC 的聆听者。<br /><br />其他信息：<br /><br />端口： 0|  
|**4614**|DFSR|DFS 复制服务在本地路径 C:\Windows\SYSVOL\domain 初始化 SYSVOL，并且正在等待执行初始复制。 它具有与其合作伙伴复制之前，复制的文件夹将保留在初始的同步状态中。 如果该服务器处于域控制器在升级过程，域控制器而不会向你推荐并且充当域控制器，直到解决此问题。 如果在初始的同步状态也是指定的合作伙伴或者遇到此服务器或同步合作伙伴共享冲突，则可以发生此问题。 如果此过程中发生事件 SYSVOL 迁移从文件复制服务 (FRS) DFS 复制到，更改不会复制，直到解决此问题。 这可能会导致该服务器变得与其他域控制器同步 SYSVOL 文件夹。<br /><br />其他信息：<br /><br />已复制的文件夹名称： SYSVOL 共享<br /><br />已复制的文件夹 ID:*<GUID>*<br /><br />复制组名称： 域系统音量<br /><br />复制组 ID:*<GUID>*<br /><br />成员 ID:*<GUID>*<br /><br />仅阅读： 0|  
|**4604**|DFSR|DFS 复制服务成功初始化在本地路径 C:\Windows\SYSVOL\domain SYSVOL 复制文件夹。 此成员已完成 SYSVOL 与合作伙伴 dc1.corp.contoso.com 的初始的同步。若要检查存在 SYSVOL 共享，打开 command prompt 窗口，然后键入"网络共享"。<br /><br />其他信息：<br /><br />已复制的文件夹名称： SYSVOL 共享<br /><br />已复制的文件夹 ID:*<GUID>*<br /><br />复制组名称： 域系统音量<br /><br />复制组 ID:*<GUID>*<br /><br />成员 ID:*<GUID>*<br /><br />同步合作伙伴：*<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>还原复制 SYSVOL 使用 FRS 域控制器  
在此情况下而不是 DFSR 事件日志使用文件复制事件日志。 此外，应用程序事件日志写入不同 FRS 相关的事件。 否则，目录服务和系统事件日志消息通常都是相同，并在同一订购如上文所述。  
  
##### <a name="file-replication-service-event-log"></a>文件复制 Service 事件日志  
停止并重启 D2 BURFLAGS 值非授权同步 SYSVOL FRS 服务。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**13502**|NTFRS|复制服务已停止。|  
|**13503**|NTFRS|复制服务已停止。|  
|**13501**|NTFRS|在开始该文件复制服务|  
|**13512**|NTFRS|复制服务已检测到启用的磁盘写入缓存中包含目录 c:\windows\ntfrs\jet DC4 在计算机上的驱动器上。 电源驱动器中断和重要更新都将丢失时，文件复制服务可能不会恢复。|  
|**13565**|NTFRS|文件复制服务正在初始化向系统卷另一个域控制器中的数据。 计算机 DC4 不能成为域控制器，直到完成此过程。 然后将作为 SYSVOL 共享向系统卷。<br /><br />若要检查 SYSVOL 共享，在命令提示符下，键入：<br /><br />共享的网络<br /><br />当文件复制服务完成初始化过程时，将显示在 SYSVOL 共享。<br /><br />向系统卷初始化可能需要一些时间。 时间不依赖于系统音量，以及其他域控制器，域控制器之间复制间隔可用性中的数据量。"|  
|**13520**|NTFRS|该文件复制服务移动现有文件* <path> *到* <path> *\NtFrs_PreExisting___See_EventLog。<br /><br />该文件复制服务可能会删除的文件* <path>*随时 \NtFrs_PreExisting___See_EventLog。 可以将文件保存被删除通过复制外出* <path> *\NtFrs_PreExisting___See_EventLog。 复制到文件* <path> *如果文件已存在一些其他复制合作伙伴上可能会导致名称冲突。<br /><br />在某些情况下，文件复制服务可能复制文件从* <path>*插入 \NtFrs_PreExisting___See_EventLog * <path> *而不是从其他一些复制合作伙伴复制该文件。<br /><br />通过删除的文件，可以随时恢复空间* <path> *\NtFrs_PreExisting___See_EventLog。|  
|**13553**|NTFRS|文件复制服务成功以下副本集中到添加该计算机：<br /><br />"域系统卷 （SYSVOL 共享）"<br /><br />为该事件的相关信息如下所示：<br /><br />计算机 DNS 名为"*<domain controller FQDN>*"<br /><br />副本组成员名为"*<domain controller name>*"<br /><br />副本集根路径"*<path>*"<br /><br />副本分步目录路径是"* <path> * "<br /><br />将副本工作目录路径"*<path>*"|  
|**13554**|NTFRS|该文件复制服务成功添加连接到副本集中如下所示：<br /><br />"域系统卷 （SYSVOL 共享）"<br /><br />从站"*<partner domain controller FQDN>*"<br /><br />发送到"*<partner domain controller FQDN>*"<br /><br />详细信息可能会显示在后续事件日志消息。|  
|**13516**|NTFRS|该文件复制的服务不再阻止计算机 DC4 变得域控制器。 已成功初始化向系统卷和已通知向系统卷现在已作为 SYSVOL 共享准备 Netlogon 服务。<br /><br />键入"网络共享"若要检查 SYSVOL 共享。|  
  
##### <a name="application-event-log"></a>应用程序事件日志  
FRS 数据库停止和的启动方式、，是清除由于 D2 BURFLAGS 操作。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**消息**|  
|**327**|ESENT|ntfrs (1424) 数据库引擎分离数据库 (月 1 日，c:\windows\ntfrs\jet\ntfrs.jdb)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.015、 [3] 0.000、 [4] 0.000、 [5] 0.000、 [6] 0.516、 [7] 0.000、 [8] 0.000、 [9] 0.000、 [10] 0.000、 [11] 0.063、 0.000 [12]。<br /><br />恢复缓存： 0|  
|**103**|ESENT|ntfrs (1424) 数据库引擎停止实例 (0)。<br /><br />脏关机： 0<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.031、 [10] 0.000、 [11] 介于 0.016、 [12] 0.000、 [13] 0.000、 [14] 0.047、 0.000 [15]。|  
|**102**|ESENT|ntfrs (3000) 数据库引擎 (6.02.8189.0000) 启动新实例 (0)。|  
|**105**|ESENT|ntfrs (3000) 数据库引擎启动新实例 (0)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.062、 [10] 0.000、 0.141 [11]。|  
|**103**|ESENT|ntfrs (3000) 数据库引擎停止实例 (0)。<br /><br />脏关机： 0<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.000、 [10] 0.000、 [11] 0.000、 [12] 0.000、 [13] 0.015、 [14] 0.000、 0.000 [15]。|  
|**102**|ESENT|ntfrs (3000) 数据库引擎 (6.02.8189.0000) 启动新实例 (0)。|  
|**105**|ESENT|ntfrs (3000) 数据库引擎启动新实例 (0)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.078、 [10] 0.000、 0.109 [11]。|  
|**325**|ESENT|ntfrs (3000) 数据库引擎创建了一个新的数据库 (月 1 日，c:\windows\ntfrs\jet\ntfrs.jdb)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 介于 0.016、 [4] 介于 0.016、 [5] 0.000、 [6] 0.015、 [7] 0.000、 [8] 0.000、 [9] 0.078、 [10] 介于 0.016、 0.000 [11]。|  
|**103**|ESENT|ntfrs (3000) 数据库引擎停止实例 (0)。<br /><br />脏关机： 0<br /><br />内部计时序列: [1] 0.000、 [2] 0.000、 [3] 0.000、 [4] 0.000、 [5] 0.078、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.125、 [10] 介于 0.016、 [11] 0.000、 [12] 0.000、 [13] 0.000、 [14] 0.000、 0.000 [15]。|  
|**102**|ESENT|ntfrs (3000) 数据库引擎 (6.02.8189.0000) 启动新实例 (0)。|  
|**105**|ESENT|ntfrs (3000) 数据库引擎启动新实例 (0)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 介于 0.016、 [2] 0.000、 [3] 0.000、 [4] 0.094、 [5] 0.000、 [6] 0.000、 [7] 0.000、 [8] 0.000、 [9] 0.032、 [10] 0.000、 0.000 [11]。|  
|**326**|ESENT|ntfrs (3000) 数据库引擎附加数据库 (月 1 日，c:\windows\ntfrs\jet\ntfrs.jdb)。 (时间 = 0 秒)<br /><br />内部计时序列: [1] 0.000、 [2] 0.015、 [3] 0.000、 [4] 0.000、 [5] 介于 0.016、 [6] 0.015、 [7] 0.000、 [8] 0.000、 [9] 0.000、 [10] 0.000、 [11] 0.000、 0.000 [12]。<br /><br />保存的缓存： 1|  
  


