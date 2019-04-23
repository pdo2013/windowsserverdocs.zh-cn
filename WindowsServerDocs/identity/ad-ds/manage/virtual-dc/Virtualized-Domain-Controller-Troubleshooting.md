---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: 虚拟化域控制器疑难解答
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ca58d36599be76e4d196d91f298b9841884e7a36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863658"
---
# <a name="virtualized-domain-controller-troubleshooting"></a>虚拟化域控制器疑难解答

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题提供有关为虚拟化域控制器功能进行故障排除的详细方法。  
  
-   [故障排除虚拟化域控制器克隆](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [虚拟化的域控制器安全还原进行故障排除](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>简介  
提高故障排除技巧的最重要的方式是建立测试实验室并严格检查有用的常规方案。 如果你遇到错误，它们将更为浅显易懂，因为之后你将具有域控制器升级工作原理的牢固基础。 这也使你可以培养分析和网络分析的技能。 它适用于所有分布式系统技术，并非仅适用于虚拟化域控制器部署。  
  
用于域控制器配置高级故障排除的要素如下：  
  
1.  与注重细节相结合的线性分析。  
  
2.  了解网络捕获分析  
  
3.  了解内置日志  
  
第一个和第二个要素超出了本主题范围，但可对第三个要素进行详细说明。 虚拟化域控制器故障排除需要一个合乎逻辑的线性方法。 其关键在于使用提供的数据处理问题，而且仅当你已用完提供的输出和记录时，才借助复杂的工具和分析。  
  
## <a name="BKMK_TshootVDCCloning"></a>故障排除虚拟化域控制器克隆  
本部分包含：  
  
-   [故障排除工具](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [日志记录选项](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [常规方法进行故障排除域控制器克隆](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [服务器核心和事件日志](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [故障排除的特定问题](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
虚拟化域控制器克隆的故障排除策略遵循以下常规格式：  
  
![虚拟 dc 进行故障排除](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>故障排除工具  
  
#### <a name="BKMK_LoggingOptions"></a>日志记录选项  
内置日志是对域控制器克隆问题进行故障排除的最重要工具。 默认情况下，所有这些日志都处于启用状态并配置为最大详细级别。  
  
|||  
|-|-|  
|**Operation**|**Log**|  
|**克隆**|-事件 viewer\Windows logs\System<br />-事件 viewer\Applications and services logs\Directory 服务<br />-   %systemroot%\debug\dcpromo.log|  
|**升级**|-   %systemroot%\debug\dcpromo.log<br />-事件 viewer\Applications and services logs\Directory 服务<br />-事件 viewer\Windows logs\System<br />-事件 viewer\Applications and services logs\File 复制服务<br />-事件 viewer\Applications and services logs\DFS 复制|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>用于对域控制器配置进行故障排除的工具和命令  
要解决日志未说明的问题，请从使用以下工具开始：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>常规方法进行故障排除域控制器克隆  
  
1.  VM 是否启动进入 DS 修复模式 (DSRM)？ 它指示有必要进行故障排除。 若要在 DSRM 中登录，请使用 **.\Administrator** 帐户并指定 DSRM 密码。  
  
    1.  检查 Dcpromo.log。  
  
        1.  初始克隆步骤成功完成，但域控制器升级失败？  
  
        2.  错误指示本地域控制器的问题还是 AD DS 环境的问题（例如，从 PDC 模拟器中返回的错误）？  
  
    2.  检查系统和目录服务事件日志、dccloneconfig.xml 和 CustomDCCloneAllowList.xml  
  
        1.  不兼容的应用程序是否需要位于 CustomDCCloneAllowList.xml 允许列表中？  
  
        2.  Dccloneconfig.xml 中的 IP 地址或计算机名称是否重复或无效？  
  
        3.  Dccloneconfig.xml 中的 Active Directory 站点是否无效？  
  
        4.  是否未在 Dccloningconfig.xml 中设置 IP 地址且没有可用的 DHCP 服务器？  
  
        5.  PDC 模拟器是否处于联机状态且可以通过 RPC 协议验证？  
  
        6.  域控制器是否为“可克隆的域控制器”组的成员？ 是否在该组的域根上设置**允许 DC 创建其自身的克隆**权限？  
  
        7.  Dccloneconfig.xml 文件是否包含阻止正确分析的语法错误？  
  
        8.  是否支持虚拟机监控程序？  
  
        9. 克隆成功开始后，域控制器升级是否失败？  
  
        10. 是否超过自动生成的域控制器名称的最大数量 （9999）？  
  
        11. 是否复制了 MAC 地址？  
  
2.  克隆与源 DC 的主机名是否相同？  
  
    1.  允许的某个位置中是否存在 Dccloneconfig.xml 文件？  
  
3.  VM 启动进入正常模式且克隆完成，但域控制器无法正常运作？  
  
    1.  首先检查是否在克隆上更改了主机名。 如果主机名不同，则至少克隆已部分完成。  
  
    2.  域控制器具有来自 dccloneconfig.xml 的源域控制器的重复 IP 地址，但在克隆期间源域控制器处于脱机状态？  
  
    3.  如果域控制器正在进行播发，则将该问题作为不进行克隆也会遇到的任何普通后升级问题来进行处理。  
  
    4.  如果域控制器未进行播发，则检查目录服务、系统、应用程序、文件复制和 DFS 复制的事件日志以查找后升级错误。  
  
#### <a name="disabling-dsrm-boot"></a>禁用 DSRM 启动  
由于任何错误引导进入 DSRM 后，请诊断失败的原因，而且如果 dcpromo.log 没有表明无法重试克隆，则修复失败原因并重置 DSRM 标志。 下次重新启动时，失败的克隆不会自主返回到正常模式；为了再次尝试克隆，你必须删除 DS 还原模式启动标志。 要求以提升的管理员身份运行所有这些步骤。  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>使用 Msconfig.exe 删除 DSRM  
若要使用 GUI 关闭 DSRM 启动，请使用系统配置工具：  
  
1.  运行 msconfig.exe  
  
2.  在**启动**选项卡的**启动选项**下，取消选择**安全启动**（启用 **Active Directory 修复**选项时已选择它）  
  
3.  得到提示后点击“确定”并重启  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>使用 Bcdedit.exe 删除 DSRM  
若要从命令行关闭 DSRM 启动，请使用启动配置数据存储区编辑器：  
  
1.  打开 CMD 提示符并运行：  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  使用以下方法重新启动计算机：  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe 还可在 Windows PowerShell 控制台中使用。 其中的命令为：  
>   
> Bcdedit.exe /deletevalue safeboot  
>   
> Restart-computer  
  
### <a name="BKMK_ServerCoreEvents"></a>服务器核心和事件日志  
事件日志中包含许多有关虚拟化域控制器克隆操作的有用信息。 默认情况下，Windows Server 2012 计算机安装是服务器核心安装，这意味着不存在图形界面，因此无法运行本地事件查看器管理单元。  
  
要在运行服务器核心安装的服务器上查看事件日志：  
  
-   在本地运行 Wevtutil.exe 工具  
  
-   在本地运行 PowerShell cmdlet Get-WinEvent  
  
-   如果已启用"远程事件日志管理"组 （或等效端口） 以允许入站的通信的 Windows 高级防火墙规则，你可以管理使用 Eventvwr.exe、 wevtutil.exe 或 Get-winevent 远程事件日志。 在 Windows PowerShell 3.0 中，也可以使用 NETSH.exe、组策略或新的 Set-NetFirewallRule cmdlet 在服务器核心安装上完成该任务。  
  
> [!WARNING]  
> 当图形 shell 位于 DSRM 中时，请勿尝试将它添加回计算机。 在安全模式或 DSRM 中，Windows 服务堆栈 (CBS) 无法正确运行。 在 DSRM 中添加功能或角色的尝试将无法完成，而且会导致计算机处于不稳定状态，直到它正常启动。 因为 DSRM 中虚拟化域控制器克隆无法正常启动，而且在大多数情况下不应正常启动，所以无法安全地添加图形 shell。 不支持执行此操作，可能会导致你的服务器无法使用。  
  
### <a name="BKMK_SpecificProblems"></a>故障排除的特定问题  
  
#### <a name="events"></a>事件  
所有虚拟化域控制器克隆事件都将写入克隆域控制器 VM 的目录服务事件日志。 应用程序、文件复制服务和 DFS 复制事件日志可能还包含失败克隆的有用疑难解答信息。 PDC 模拟器上的事件日志中可能提供了 RPC 调用 PDC 模拟器期间的故障。  
  
下面是目录服务事件日志中特定于 Windows Server 2012 克隆的事件，以及用于错误的注释和建议解决方案。  
  
##### <a name="directory-services-event-log"></a>目录服务事件日志  
  
|||  
|-|-|  
|**事件 ID**|**2160**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|本地*<COMPUTERNAME>* 已找到虚拟域控制器克隆配置文件。<br /><br />虚拟域控制器克隆配置文件位于：%1<br /><br />虚拟域控制器克隆配置文件的存在表示本地虚拟域控制器是另一个虚拟域控制器的克隆。 *<COMPUTERNAME>* 将开始克隆自身。|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。 在 DSA 工作目录 %systemroot%\ntds 和任何本地或可移动磁盘的根目录中检查 dcclconeconfig.xml 文件。|  
  
|||  
|-|-|  
|**事件 ID**|**2161**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|本地*<COMPUTERNAME>* 未找到虚拟域控制器克隆配置文件。 本地计算机不是克隆的 DC。|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。 在 DSA 工作目录 %systemroot%\ntds 和任何本地或可移动磁盘的根目录中检查 dcclconeconfig.xml 文件。|  
  
|||  
|-|-|  
|**事件 ID**|**2162**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|虚拟域控制器克隆失败。<br /><br />有关与虚拟域控制器克隆尝试相对应的错误的详细信息，请检查系统事件日志和 %systemroot%\debug\dcpromo.log 中记录的事件。<br /><br />错误代码：%1|  
|**说明和解决方法**|按照消息说明，此错误为 catchall。|  
  
|||  
|-|-|  
|**事件 ID**|**2163**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|已启动 DsRoleSvc 服务以克隆本地虚拟域控制器。|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。 在 DSA 工作目录 %systemroot%\ntds 和任何本地或可移动磁盘的根目录中检查 dcclconeconfig.xml 文件。|  
  
|||  
|-|-|  
|**事件 ID**|**2164**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法启动 DsRoleSvc 服务以克隆本地虚拟域控制器。|  
|**说明和解决方法**|检查 DS 角色服务器服务 (DsRoleSvc) 的服务设置，并确保将其启动类型设置为手动。 验证没有任何第三方程序正在阻止此服务的启动。|  
  
|||  
|-|-|  
|**事件 ID**|**2165**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法在本地虚拟域控制器克隆期间启动线程。<br /><br />错误代码：%1<br /><br />错误消息：%2<br /><br />线程名称：%3|  
|**说明和解决方法**|请联系 Microsoft 产品支持|  
  
|||  
|-|-|  
|**事件 ID**|**2166**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 需要 RPCSS 服务以开始重新启动进入 DSRM。 等待 RPCSS 初始化到运行状态失败。<br /><br />错误代码：%1|  
|**说明和解决方法**|检查 RPC 服务器服务 (Rpcss) 的系统事件日志和服务设置|  
  
|||  
|-|-|  
|**事件 ID**|**2167**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法初始化虚拟域控制器信息。 有关详细信息，请参阅之前的事件日志条目。<br /><br />其他数据<br /><br />故障代码：%1|  
|**说明和解决方法**|按照消息说明，此错误为 catchall。|  
  
|||  
|-|-|  
|**事件 ID**|**2168**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />此 DC 在受支持的虚拟机监控程序上运行。 检测到 VM 生成 ID。<br /><br />VM 生成 ID 的当前值：%1|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2169**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|未检测到 VM 生成 ID。 DC 托管在不支持 VM 生成 ID 的物理计算机、Hyper-V 的下级版本或者 VM 监控程序上。<br /><br />其他数据<br /><br />检查 VM 生成 ID 时返回的错误代码：%1|  
|**说明和解决方法**|如果不打算克隆，则这是一个成功事件。 否则，请检查系统事件日志并查看虚拟机监控程序产品支持文档。|  
  
|||  
|-|-|  
|**事件 ID**|**2170**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|警告|  
|**Message**|已检测到生成 ID 更改。<br /><br />DS 中缓存的生成 ID（旧值）：%1<br /><br />VM 中的当前生成 ID（新值）：%2<br /><br />生成 ID 将在应用虚拟机快照之后、在虚拟机导入操作之后或在实时迁移操作之后发生更改。 *<COMPUTERNAME>* 将创建一个新的调用 ID 以恢复域控制器。 不应使用虚拟机快照还原虚拟化域控制器。 支持用于还原或回滚 Active Directory 域服务数据库中的内容的方法是：还原使用 Active Directory 域服务感知备份应用程序制作的系统状态备份。|  
|**说明和解决方法**|如果打算克隆，则这是一个成功事件。 否则，请检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2171**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|未检测到生成 ID 更改。<br /><br />DS 中缓存的生成 ID（旧值）：%1<br /><br />VM 中的当前生成 ID（新值）：%2|  
|**说明和解决方法**|如果不打算克隆，则这是一个成功事件，而且在虚拟化 DC 每次重新启动时应该可以见到。 否则，请检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2172**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|读取域控制器的计算机对象的 msDS-GenerationId 属性。<br /><br />msDS-GenerationId 属性值：%1|  
|**说明和解决方法**|如果打算克隆，则这是一个成功事件。 否则，请检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2173**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|无法读取域控制器的计算机对象的 msDS-GenerationId 属性。 这可能是由于数据库事务失败或本地数据库中不存在生成 ID 所致。 在 dcpromo 之后的第一次重新启动过程中不存在 msDS-GenerationId，或者该 DC 并非虚拟域控制器。<br /><br />其他数据<br /><br />故障代码：%1|  
|**说明和解决方法**|如果打算克隆，则这是一个成功事件，而且它是完成克隆后的第一次 VM 重新启动。 还可以在非虚拟域控制器上忽略它。 否则，请检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2174**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|该 DC 既不是虚拟域控制器克隆，也不是已还原虚拟域控制器快照。|  
|**说明和解决方法**|如果不打算克隆，则这是一个成功事件。 否则，请检查系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2175**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|虚拟域控制器克隆配置文件存在于不受支持的平台上。|  
|**说明和解决方法**|当找到 dccloneconfig.xml 但无法找到 VM 生成 ID 时，会发生这种情况，例如，在不支持 VM 生成 ID 的物理计算机上或虚拟机监控程序上找到 dccloneconfig.xml 文件。|  
  
|||  
|-|-|  
|**事件 ID**|**2176**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|重命名的虚拟域控制器克隆配置文件。<br /><br />其他数据<br /><br />旧文件名：%1<br /><br />新文件名：%2|  
|**说明和解决方法**|启动源 VM 备份时预期将重命名，因为尚未更改 VM 生成 ID。 这会阻止源域控制器的克隆尝试。|  
  
|||  
|-|-|  
|**事件 ID**|**2177**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|重命名虚拟域控制器克隆配置文件失败。<br /><br />其他数据<br /><br />文件名：%1<br /><br />故障代码：%2 %3|  
|**说明和解决方法**|启动源 VM 备份时预期将尝试重命名，因为尚未更改 VM 生成 ID。 这会阻止源域控制器的克隆尝试。 手动重命名该文件，并调查可能会阻止文件重命名的已安装的第三方产品。|  
  
|||  
|-|-|  
|**事件 ID**|**2178**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|已检测到虚拟域控制器克隆配置文件，但尚未更改 VM 生成 ID。 本地 DC 是克隆源 DC。 重命名克隆配置文件。|  
|**说明和解决方法**|预期在启动源 VM 备份时发生，因为尚未更改 VM 生成 ID。 这会阻止源域控制器的克隆尝试。|  
  
|||  
|-|-|  
|**事件 ID**|**2179**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|域控制器的计算机对象的 msDS-GenerationId 属性已设置为以下参数：<br /><br />GenerationID 属性：%1|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2180**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|警告|  
|**Message**|无法设置域控制器的计算机对象的 msDS-GenerationId 属性。<br /><br />其他数据<br /><br />故障代码：%1|  
|**说明和解决方法**|检查系统事件日志和 Dcpromo.log。 在 MS TechNet、MS 知识库和 MS 博客中查找特定错误以确定其常规含义，然后根据这些结果进行故障排除。|  
  
|||  
|-|-|  
|**事件 ID**|**2182**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|内部事件：目录服务被要求克隆远程 DSA：|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2183**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|内部事件： *<COMPUTERNAME>* 完成了克隆远程目录系统代理请求。<br /><br />原始 DC 名称：%3<br /><br />请求克隆 DC 名称：%4<br /><br />请求克隆 DC 站点：%5<br /><br />其他数据<br /><br />错误值：%1 %2|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2184**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法为克隆的 DC 创建域控制器帐户。<br /><br />原始 DC 名称：%1<br /><br />克隆的 DC 的允许数量：%2<br /><br />可以生成的克隆域控制器帐户的数量上限 *<COMPUTERNAME>* 超出了。|  
|**说明和解决方法**|根据命名约定，如果不降级域控制器，则单一源域控制器名称仅可自动生成 9999 次。 使用 XML 中的 <computername> 元素从以不同方式命名的 DC 中生成新的唯一的名称或克隆。|  
  
|||  
|-|-|  
|**事件 ID**|**2191**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 设置以下注册表值以禁用 DNS 更新。<br /><br />注册表项：%1<br /><br />注册表值：%2<br /><br />注册表值数据：%3<br /><br />在克隆过程中，本地计算机在短时间内可能会与克隆源计算机具有相同的计算机名称。 在此期间将禁用 DNS A 和 AAAA 记录注册，因此客户端无法将请求发送到正在进行克隆的本地计算机。 完成克隆后，克隆进程将再次启用 DNS 更新。|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2192**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法设置以下注册表值以禁用 DNS 更新。<br /><br />注册表项：%1<br /><br />注册表值：%2<br /><br />注册表值数据：%3<br /><br />错误代码：%4<br /><br />错误消息：%5<br /><br />在克隆过程中，本地计算机在短时间内可能会与克隆源计算机具有相同的计算机名称。 在此期间将禁用 DNS A 和 AAAA 记录注册，因此客户端无法将请求发送到正在进行克隆的本地计算机。|  
|**说明和解决方法**|检查应用程序和系统事件日志。 调查可能会阻止注册表更新的第三方应用程序。|  
  
|||  
|-|-|  
|**事件 ID**|**2193**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 设置以下注册表值以启用 DNS 更新。<br /><br />注册表项：%1<br /><br />注册表值：%2<br /><br />注册表值数据：%3<br /><br />在克隆过程中，本地计算机在短时间内可能会与克隆源计算机具有相同的计算机名称。 在此期间将禁用 DNS A 和 AAAA 记录注册，因此客户端无法将请求发送到正在进行克隆的本地计算机。|  
|**说明和解决方法**|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|**事件 ID**|**2194**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法设置以下注册表值以启用 DNS 更新。<br /><br />注册表项：%1<br /><br />注册表值：%2<br /><br />注册表值数据：%3<br /><br />错误代码：%4<br /><br />错误消息：%5<br /><br />在克隆过程中，本地计算机在短时间内可能会与克隆源计算机具有相同的计算机名称。 在此期间将禁用 DNS A 和 AAAA 记录注册，因此客户端无法将请求发送到正在进行克隆的本地计算机。|  
|**说明和解决方法**|检查应用程序和系统事件日志。 调查可能会阻止注册表更新的第三方应用程序。|  
  
|||  
|-|-|  
|**事件 ID**|**2195**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|无法设置 DSRM 启动。<br /><br />错误代码：%1<br /><br />错误消息：%2<br /><br />当虚拟域控制器克隆失败，或虚拟域控制器克隆配置文件在不受支持的虚拟机监控程序上显示时，本地计算机将重新启动进入 DSRM，以进行故障排除。 设置 DSRM 启动失败。|  
|**说明和解决方法**|检查应用程序和系统事件日志。 调查可能会阻止注册表更新的第三方应用程序。|  
  
|||  
|-|-|  
|**事件 ID**|**2196**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|无法启用关机权限。<br /><br />错误代码：%1<br /><br />错误消息：%2<br /><br />当虚拟域控制器克隆失败，或虚拟域控制器克隆配置文件在不受支持的虚拟机监控程序上显示时，本地计算机将重新启动进入 DSRM，以进行故障排除。 启用关机权限失败。|  
|**说明和解决方法**|检查应用程序和系统事件日志。 调查可能会阻止使用权限的第三方应用程序。|  
  
|||  
|-|-|  
|**事件 ID**|**2197**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|无法启动系统关闭。<br /><br />错误代码：%1<br /><br />错误消息：%2<br /><br />当虚拟域控制器克隆失败，或虚拟域控制器克隆配置文件在不受支持的虚拟机监控程序上显示时，本地计算机将重新启动进入 DSRM，以进行故障排除。 启动系统关闭失败。|  
|**说明和解决方法**|检查应用程序和系统事件日志。 调查可能会阻止使用权限的第三方应用程序。|  
  
|||  
|-|-|  
|**事件 ID**|**2198**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法创建或修改以下克隆的 DC 对象。<br /><br />其他数据：<br /><br />对象：<br /><br />%1<br /><br />错误值：%2<br /><br />%3|  
|**说明和解决方法**|在 MS TechNet、MS 知识库和 MS 博客中查找特定错误以确定其常规含义，然后根据这些结果进行故障排除。|  
  
|||  
|-|-|  
|**事件 ID**|**2199**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 未能创建以下克隆的 DC 对象，因为该对象已存在。<br /><br />其他数据：<br /><br />源 DC：<br /><br />%1<br /><br />对象：<br /><br />%2|  
|**说明和解决方法**|验证 dccloneconfig.xml 未指定现有的域控制器，或者已在多个克隆上使用 dccloneconfig.xml 的副本，而没有编辑名称。 如果仍不希望发生此冲突，则请确定哪位管理员对其进行了升级；请联系他们以讨论是否应降级现有的域控制器、是否应清理现有的域控制器元数据，以及克隆是否应使用不同的名称。|  
  
|||  
|-|-|  
|**事件 ID**|**2203**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|最后一个虚拟域控制器克隆失败。 这是自此之后的首次重新启动，因此这应该是克隆的重试。 但是，既不存在虚拟域控制器克隆配置文件，也未检测到 VM 生成 ID 发生更改。 启动进入 DSRM。<br /><br />最后一个虚拟域控制器克隆失败：%1<br /><br />存在虚拟域控制器克隆配置文件：%2<br /><br />检测到虚拟机生成 ID 发生更改：%3|  
|**说明和解决方法**|如果之前克隆失败，则预期出现该现象，因为 dccloneconfig.xml 缺失或无效|  
  
|||  
|-|-|  
|事件 ID|2210|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|<COMPUTERNAME> 无法为克隆域控制器创建对象。<br /><br />其他数据：<br /><br />克隆 ID：%6<br /><br />克隆域控制器名称：%1<br /><br />重试循环：%2<br /><br />异常值：%3<br /><br />错误值：%4<br /><br />DSID：%5|  
|注释和解析|有关克隆失败原因的详细信息，请查看系统和目录服务事件日志以及 dcpromo.log。|  
  
|||  
|-|-|  
|事件 ID|2211|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 已为克隆域控制器创建对象。<br /><br />其他数据：<br /><br />克隆 ID：%3<br /><br />克隆域控制器名称：%1<br /><br />重试循环：%2|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2212|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 已开始为克隆域控制器创建对象。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />克隆名称：%2<br /><br />克隆站点：%3<br /><br />克隆 RODC：%4|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2213|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 为只读域控制器克隆创建了一个新的 KrbTgt 对象。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />新 KrbTgt 对象 Guid：%2|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2214|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 将为克隆域控制器创建计算机对象。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />原始域控制器：%2<br /><br />克隆域控制器：%3|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2215|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 将在以下站点中添加克隆域控制器。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />站点：%2|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2216|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 将为克隆域控制器创建服务器容器。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />服务器容器：%2|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2217|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 将为克隆域控制器创建服务器对象。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />服务器对象：%2|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2218|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 将为克隆域控制器创建 NTDS 设置。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />对象：%2|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2219|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 将为克隆只读域控制器创建连接对象。<br /><br />其他数据：<br /><br />克隆 ID：%1|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2220|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 将为克隆只读域控制器创建 SYSVOL 对象。<br /><br />其他数据：<br /><br />克隆 ID：%1|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2221|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|<COMPUTERNAME> 无法为克隆的域控制器生成随机密码。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />克隆域控制器名称：%2<br /><br />错误：%3 %4|  
|注释和解析|检查系统事件日志，以获取无法创建计算机帐户密码原因的详细信息。|  
  
|||  
|-|-|  
|事件 ID|2222|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|<COMPUTERNAME> 无法为克隆域控制器设置密码。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />克隆域控制器名称：%2<br /><br />错误：%3 %4|  
|注释和解析|检查系统事件日志，以获取有关无法设置计算机帐户密码原因的详细信息。|  
  
|||  
|-|-|  
|事件 ID|2223|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|<COMPUTERNAME> 成功地为克隆的域控制器设置计算机帐户密码。<br /><br />其他数据：<br /><br />克隆 ID：%1<br /><br />克隆域控制器名称：%2<br /><br />重试总次数：%3|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2224|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|虚拟域控制器克隆失败。 在克隆的计算机上存在下列 %1 个托管服务帐户：<br /><br />%2<br /><br />为成功克隆，必须删除所有托管服务帐户。 可以使用 Remove-ADComputerServiceAccount PowerShell cmdlet 完成此操作。|  
|注释和解析|预期在使用独立 MSA（非组 MSA）时发生。 请*不要*遵循事件建议删除帐户，因为它编写有误。 使用 Uninstall-adserviceaccount – [ https://technet.microsoft.com/library/hh852310 ](https://technet.microsoft.com/library/hh852310)。<br /><br />在 Windows Server 2012 中，已将独立 MSA（在 Windows Server 2008 R2 中首次发布）替换为组 MSA (gMSA)。 GMSA 支持克隆。|  
  
|||  
|-|-|  
|事件 ID|2225|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|信息性|  
|消息|已从本地域控制器中成功删除以下安全主体的缓存密钥：<br /><br />%1<br /><br />克隆只读域控制器之后，将在克隆的域控制器上删除之前在克隆的源只读域控制器上缓存的密钥。|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|2226|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|无法从本地域控制器中删除以下安全主体的缓存密钥：<br /><br />%1<br /><br />错误：%2 (%3)<br /><br />克隆只读域控制器之后，需要在克隆上删除之前在克隆的源只读域控制器上缓存的密钥，以便减少攻击者可从被盗用的或泄露的克隆获取这些凭据的风险。 如果安全主体是一个高特权帐户，并且应使它免受该风险，请使用 rootDSE 操作 rODCPurgeAccount 在本地域控制器上手动清除其密钥。|  
|注释和解析|检查系统和目录服务事件日志以获取详细信息。|  
  
|||  
|-|-|  
|事件 ID|2227|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|在尝试从本地域控制器中删除缓存的密钥时，会引发异常。<br /><br />其他数据：<br /><br />异常值：%1<br /><br />错误值：%2<br /><br />DSID：%3<br /><br />克隆只读域控制器之后，需要在克隆上删除之前在克隆的源只读域控制器上缓存的密钥，以便减少攻击者可从被盗用的或泄露的克隆获取这些凭据的风险。 如果其中任何一个安全主体是高特权帐户，并且应使它免受该风险，请使用 rootDSE 操作 rODCPurgeAccount 在本地域控制器上手动清除其密钥。|  
|注释和解析|检查系统和目录服务事件日志以获取详细信息。|  
  
|||  
|-|-|  
|事件 ID|2228|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|严重性|错误|  
|消息|此域控制器的 Active Directory 数据库中的 VM 生成 ID 与此虚拟机的当前值不同。 但是找不到虚拟域控制器克隆配置文件 (DCCloneConfig.xml)，因此未尝试执行域控制器克隆操作。 如果要执行域控制器克隆操作，请确保在任一支持的位置中提供 DCCloneConfig.xml。 此外，此域控制器的 IP 地址与另一个域控制器的 IP 地址发生冲突。 要确保不会发生服务中断，已将域控制器配置为启动进入 DSRM。<br /><br />其他数据：<br /><br />重复的 IP 地址：%1|  
|注释和解析|如果可能，此保护机制将停止重复的域控制器（例如，使用 DHCP 时它不会这样做）。 添加一个有效的 DcCloneConfig.xml 文件，删除 DSRM 标志，并重新尝试克隆|  
  
|||  
|-|-|  
|事件 ID|29218|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆失败。 无法完成克隆操作，而且已将克隆的域控制器重新启动进入目录服务还原模式 (DSRM)。<br /><br />有关与虚拟域控制器克隆尝试相对应的错误，以及是否可重新使用此克隆映像的详细信息，请检查之前记录的事件和 %systemroot%\debug\dcpromo.log。<br /><br />如果一个或多个日志条目指示无法重试克隆进程，则必须安全地销毁该映像。 否则，你可以修复错误、清除 DSRM 启动标志并正常重新启动；在重新启动时，将重试克隆操作。|  
|注释和解析|有关克隆失败原因的详细信息，请查看系统和目录服务事件日志以及 dcpromo.log。|  
  
|||  
|-|-|  
|事件 ID|29219|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|信息性|  
|消息|虚拟域控制器克隆成功。|  
|注释和解析|这是一个成功事件，仅当意外发生时才成为问题。|  
  
|||  
|-|-|  
|事件 ID|29248|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆无法获取 Winlogon 通知。 返回的错误代码为 %1 (%2)。<br /><br />有关此错误的详细信息，请在 %systemroot%\debug\dcpromo.log 中查看与虚拟域控制器克隆尝试相对应的错误。|  
|注释和解析|请联系 Microsoft 产品支持|  
  
|||  
|-|-|  
|事件 ID|29249|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆无法分析虚拟域控制器配置文件。<br /><br />返回的 HRESULT 代码为 %1。<br /><br />配置文件为：%2<br /><br />请在配置文件中修复这些错误，然后重试克隆操作。<br /><br />有关此错误的详细信息，请参阅 %systemroot%\debug\dcpromo.log。|  
|注释和解析|检查 dclconeconfig.xml 文件中的使用 XML 编辑器的语法错误以及 DCCloneConfigSchema.xsd 架构文件。|  
  
|||  
|-|-|  
|事件 ID|29250|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆失败。 当前在克隆的虚拟域控制器上启用的一些软件或服务在用于虚拟域控制器克隆的允许应用程序列表中不存在。<br /><br />下面是缺少的条目：<br /><br />%2<br /><br />%1（如果存在）用作定义的包含列表。<br /><br />如果安装了无法克隆的应用程序，则无法完成克隆操作。<br /><br />请运行 Active Directory PowerShell Cmdlet Get-ADDCCloningExcludedApplicationList，以检查安装在克隆的计算机上但未包含在允许列表中的应用程序，然后将其添加到允许列表中（如果它们与虚拟域控制器克隆兼容）。 如果任何应用程序与虚拟域控制器克隆不兼容，请先卸载这些应用程序，然后再重试克隆操作。<br /><br />虚拟域控制器克隆过程按以下搜索顺序搜索允许的应用程序列表文件 (CustomDCCloneAllowList.xml)；将使用找到的第一个文件并忽略所有其他文件：<br /><br />1.注册表值名称：HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2.DSA 工作目录文件夹所在的相同目录<br /><br />3. %windir%\NTDS<br /><br />4.可移动读/写媒体（按驱动器根目录的驱动器号顺序排列）|  
|注释和解析|按照消息说明进行操作|  
  
|||  
|-|-|  
|事件 ID|29251|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆无法重置克隆计算机的 IP 地址。<br /><br />返回的错误代码为 %1 (%2)。<br /><br />虚拟域控制器配置文件中网络配置部分的配置问题可能会导致此错误。<br /><br />有关与虚拟域控制器克隆尝试期间 IP 地址重置相对应的错误的详细信息，请参阅 %systemroot%\debug\dcpromo.log。<br /><br />重置克隆的计算机上的计算机 IP 地址的详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=208030|  
|注释和解析|验证 dccloneconfig.xml 中设置的 IP 信息有效且未与原始源计算机重复。|  
  
|||  
|-|-|  
|事件 ID|29253|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆失败。 克隆域控制器无法找到克隆计算机的克隆计算机主域中的主域控制器 (PDC) 操作主机。<br /><br />返回的错误代码为 %1 (%2)。<br /><br />请验证克隆计算机主域中的主域控制器分配给了实时域控制器、处于联机状态并正常运作。 验证克隆的计算机可通过所需端口和协议与主域控制器进行 LDAP/RPC 连接。|  
|注释和解析|验证已设置克隆的域控制器 IP 和 DNS 信息。 使用 Dcdiag.exe /test: locatorcheck 验证 pdce 是否处于联机状态，使用 Nltest.exe /server:*<PDCE>* /dclist:*<domain>* rpc，从同时 PDCE 获取网络捕获克隆失败和分析流量。|  
  
|||  
|-|-|  
|事件 ID|29254|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆无法绑定到主域控制器 %1。<br /><br />返回的错误代码为 %2 (%3)。<br /><br />请验证主域控制器 %1 处于联机状态且可正常运作。 验证克隆的计算机可通过所需端口和协议与主域控制器进行 LDAP/RPC 连接。|  
|注释和解析|验证已设置克隆的域控制器 IP 和 DNS 信息。 使用 Dcdiag.exe /test: locatorcheck 验证 pdce 是否处于联机状态，使用 Nltest.exe /server:*<PDCE>* /dclist:*<domain>* rpc，从同时 PDCE 获取网络捕获克隆失败和分析流量。|  
  
|||  
|-|-|  
|事件 ID|29255|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆失败。<br /><br />在主域控制器 %1 上尝试创建被克隆的映像所需的对象时返回错误 %2 (%3)。<br /><br />请验证克隆的域控制器是否有权克隆自身。 在主域控制器 %1 上的目录服务事件日志中查看相关事件。|  
|注释和解析|在 MS TechNet、MS 知识库和 MS 博客中查找特定错误以确定其典型含义，并根据这些结果进行故障排除。|  
  
|||  
|-|-|  
|事件 ID|29256|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|尝试设置“启动到目录服务还原模式”标志失败，错误代码为 %1。<br /><br />有关错误的详细信息，请参阅 %systemroot%\debug\dcpromo.log。|  
|注释和解析|检查目录服务日志和 dcpromo.log 以获取详细信息。 检查应用程序和系统事件日志。 调查可能会阻止使用权限的第三方应用程序。|  
  
|||  
|-|-|  
|事件 ID|29257|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|已完成虚拟域控制器克隆。 尝试重新启动计算机失败，错误代码 %1。<br /><br />请重新启动计算机以完成克隆操作。|  
|注释和解析|检查应用程序和系统事件日志。 调查可能会阻止使用权限的第三方应用程序。|  
  
|||  
|-|-|  
|事件 ID|29264|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|尝试清除“启动到目录服务还原模式”标志失败，错误代码 %1。<br /><br />有关错误的详细信息，请参阅 %systemroot%\debug\dcpromo.log。|  
|注释和解析|检查目录服务日志和 dcpromo.log 以获取详细信息。 检查应用程序和系统事件日志。 调查可能会阻止使用权限的第三方应用程序。|  
  
|||  
|-|-|  
|事件 ID|29265|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|信息性|  
|消息|虚拟域控制器克隆成功。 已将虚拟域控制器克隆配置文件 %1 重命名为 %2。|  
|注释和解析|不适用，这是一个成功事件。|  
  
|||  
|-|-|  
|事件 ID|29266|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆成功。 尝试重命名虚拟域控制器克隆配置文件 %1 失败，错误代码 %2 (%3)。|  
|注释和解析|手动重命名 dccloneconfig.xml 文件。|  
  
|||  
|-|-|  
|事件 ID|29267|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|严重性|错误|  
|消息|虚拟域控制器克隆无法检查虚拟域控制器克隆允许应用程序列表。<br /><br />返回的错误代码为 %1 (%2)。<br /><br />克隆允许列表文件中的语法错误可能会导致此错误（当前正在检查的文件是：%3）。 有关此错误的详细信息，请参阅 %systemroot%\debug\dcpromo.log。|  
|注释和解析|按照事件说明进行操作|  
  
##### <a name="error-messages"></a>错误消息  
失败的虚拟化域控制器克隆没有直接的交互式错误；所有的克隆信息都记录在系统和目录服务日志中，域控制器升级记录在 dcpromo.log 中。 但是，如果服务器启动进入 DS 还原模式，请立即进行调查，因为升级或克隆已失败。  
  
克隆失败请首先检查 dcpromo.log。 随后可能需要查看目录服务和系统日志以进行进一步诊断，具体取决于列出的故障。  
  
#### <a name="known-issues-and-support-scenarios"></a>已知的问题和支持方案  
以下是 Windows Server 2012 开发过程中的常见问题。 所有这些问题都是“特意设计的”，它们具有有效的解决方法或具有更适合的技术以在第一时间避免它们。 有些问题可能会在 Windows Server 2012 的更高版本中解决。  
  
|||  
|-|-|  
|**问题**|**克隆失败，DSRM**|  
|**症状**|克隆启动进入目录服务还原模式|  
|**解析和注释**|验证遵循的来自部署虚拟化域控制器部分和[故障排除域控制器克隆的常规方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)部分的所有步骤<br /><br />在知识库 2742844 中进行了描述。|  
  
|||  
|-|-|  
|**问题**|**使用 DHCP 克隆时额外的 IP 租约**|  
|**症状**|在成功克隆 DC 并使用 DHCP 后，克隆首次启动将使用一个 DHCP 租约。 然后，当服务器重命名并作为 DC 重启时，它将使用第二个 DHCP 租约。 由于没有释放第一个 IP 地址，你最终将使用“虚拟”租约|  
|**解析和注释**|手动删除 DHCP 中未使用的地址租约或允许它正常过期。 在知识库 2742836 中进行了描述。|  
  
|||  
|-|-|  
|**问题**|**克隆失败并启动到 DSRM 后很长的延迟**|  
|**症状**|克隆看起来会在“域控制器克隆已完成 X%”处暂停 8 到 15 分钟。 此后，克隆失败并启动到 DSRM。|  
|**解析和注释**|克隆的计算机无法从 DHCP 或 SLAAC 获取动态 IP 地址、正在使用一个重复的 IP 地址，或者无法找到 PDC。 由克隆执行的多个重试尝试会导致延迟。 解决网络问题以允许克隆。<br /><br />在知识库 2742844 中进行了描述。|  
  
|||  
|-|-|  
|**问题**|**克隆不会重新创建所有服务主体名称**|  
|**症状**|如果一组*三部分*服务主体名称 (SPN) 包括带有一个端口的 NetBIOS 名称和不带端口但其他部分完全相同的 NetBIOS 名称，则不会使用新计算机名重新创建非端口条目。 例如：<br /><br />customspn/DC1:200/app1 INVALID USE OF SYMBOLS *该名称使用新计算机名重新创建*<br /><br />customspn/DC1/app1 INVALID USE OF SYMBOLS *不会使用新计算机名重新创建此条目*<br /><br />将重新创建完全限定的名称并重新创建没有三个部分的 SPN，而不考虑端口。 例如，以下名称将在克隆上成功重新创建：<br /><br />customspn/DC1:202 INVALID USE OF SYMBOLS *这是重新创建的名称*<br /><br />customspn/DC1 INVALID USE OF SYMBOLS *这是重新创建的名称*<br /><br />customspn/DC1.corp.contoso.com:202 INVALID USE OF SYMBOLS *这是重新创建的名称*<br /><br />customspn/DC1.corp.contoso.com INVALID USE OF SYMBOLS *这是重新创建的名称*|  
|**解析和注释**|这是 Windows 中（并非仅在克隆中）域控制器重命名过程的限制。 在任何情况下，重命名逻辑都不处理三部分的 SPN。 大多数已包含的 Windows 服务都不受此影响，因为它们将根据需要重新创建任何缺少的 SPN。 其他应用程序可能需要手动输入 SPN 来解决该问题。<br /><br />在知识库 2742874 中进行了描述。|  
  
|||  
|-|-|  
|**问题**|**克隆失败，启动进入 DSRM，一般网络错误**|  
|**症状**|克隆启动进入目录服务修复模式。 存在一般网络错误。|  
|解析和注释|确保新的克隆不包含由源域控制器分配的重复静态 MAC 地址；通过在源虚拟机和克隆虚拟机的虚拟机监控程序主机上运行此命令，你可以看出 VM 是否使用了静态 MAC 地址：<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />将 MAC 地址更改为唯一的静态地址，或切换到使用动态 MAC 地址。<br /><br />在知识库 2742844 中进行了描述|  
  
|||  
|-|-|  
|**问题**|**克隆失败，作为源 DC 的副本启动进入 DSRM**|  
|**症状**|新克隆在没有进行克隆的情况下启动。 未重命名 dccloneconfig.xml，而且服务器以 DS 还原模式启动。 目录服务事件日志显示错误 2164<br /><br />*<COMPUTERNAME>* 无法启动 DsRoleSvc 服务以克隆本地虚拟域控制器。|  
|**解析和注释**|检查 DS 角色服务器服务 (DsRoleSvc) 的服务设置，并确保将其启动类型设置为手动。 验证没有任何第三方程序正在阻止此服务的启动。<br /><br />有关如何在确保更新获取已复制出站的同时回收此辅助 DC 的详细信息，请参阅 Microsoft 知识库文章 2742970。|  
  
|||  
|-|-|  
|**问题**|**克隆失败，启动进入 DSRM，错误 8610**|  
|**症状**|克隆启动进入目录服务还原模式。 Dcpromo.log 显示 8610 错误（即 ERROR_DS_ROLE_NOT_VERIFIED 8610 或 0x21A2）|  
|**解析和注释**|如果可检测到 PDC，但它尚未执行足够的副本以允许自身扮演该角色，则将发生这种情况。 例如，已启动克隆且另一个管理员将 PDCE FSMO 角色移动到新的 DC。<br /><br />在知识库 2742916 中进行了描述。|  
  
|||  
|-|-|  
|**问题**|**克隆失败，启动进入 DSRM，一般网络错误**|  
|**症状**|克隆启动进入目录服务还原模式。 存在一般网络错误。|  
|**解析和注释**|确保新的克隆不包含由源域控制器分配的重复静态 MAC 地址；通过在源虚拟机和克隆虚拟机的 Hyper-V 主机上运行此命令，你可以看出 VM 是否使用了静态 MAC 地址：<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />将 MAC 地址更改为唯一的静态地址，或切换到使用动态 MAC 地址。<br /><br />在知识库 2742844 中进行了描述。|  
  
|||  
|-|-|  
|**问题**|**克隆失败，启动进入 DSRM**|  
|**症状**|克隆启动进入目录服务修复模式|  
|**解析和注释**|确保 dccloneconfig.xml 包含架构定义（请参阅 sampledccloneconfig.xml 第 2 行）：<br /><br />**<d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig">**<br /><br />在知识库 2742844 中进行了描述|  
  
|||  
|-|-|  
|问题|**无登录服务器是可用的错误登录到 DSRM**|  
|**症状**|克隆启动进入目录服务修复模式。 尝试登录并收到错误消息：<br /><br />**目前有无登录服务器可以用来处理登录请求**|  
|**解析和注释**|确保使用 DSRM 管理员帐户而不是域帐户进行登录。 使用向左键，然后键入用户名：<br /><br />**.\administrator**<br /><br />在知识库 2742908 中进行了描述|  
  
|||  
|-|-|  
|**问题**|**克隆源失败并启动到 DSRM，错误**|  
|**症状**|在克隆过程中，出现错误 8437“无法在 PDC 上创建克隆 DC 对象”(0x20f5)|  
|**解析和注释**|在 DCCloneConfig.xml 中，已将重复的计算机名称设置为源 DC 或现有 DC。 计算机名称还需遵循 NetBIOS 计算机名格式（15 个字符或更少，非 FQDN）。<br /><br />通过设置唯一有效的名称来修复 dccloneconfig.xml 文件。<br /><br />在知识库 2742959 中进行了描述|  
  
|||  
|-|-|  
|**问题**|**New-addccloneconfigfile 错误"索引已超出范围"**|  
|**症状**|运行 new-addccloneconfigfile cmdlet 时将收到错误：<br /><br />索引已超出范围。 必须为非负数且小于集合的大小。|  
|**解析和注释**|必须在提升为管理员身份的 Windows PowerShell 控制台中运行 cmdlet。 计算机上缺少本地管理员组成员身份将导致此错误。<br /><br />在知识库 2742927 中进行了描述|  
  
|||  
|-|-|  
|**问题**|**克隆失败，复制 DC**|  
|**症状**|克隆启动而不进行克隆，复制现有源 DC|  
|**解析和注释**|已复制并启动计算机，但计算机任何受支持的位置中都不包含 DcCloneConfig.xml 文件，而且也没有包含源域控制器的重复 IP 地址。 必须正确地删除 DC 以避免数据丢失。<br /><br />在知识库 2742970 中进行了描述。|  
  
|||  
|-|-|  
|**问题**|**New-addccloneconfigfile 将失败，并在服务器不是运行错误检查源域控制器是否可克隆域控制器组的成员，是否 GC 不可用时。**|  
|**症状**|在运行 New-ADDCCloneConfigFile 以创建 dccloneconfig.xml 文件时，将收到错误：<br /><br />代码-服务器不可操作|  
|**解析和注释**|验证从服务器（你在其中运行 New-addccloneconfigfile）到 GC 的连接，并验证已将“可克隆域控制器”组中源域控制器的成员身份复制到该 GC。<br /><br />在 GC 或 DC 最近可能处于脱机状态的情况下，将运行以下命令以作为刷新 DC 定位符缓存的一种方法：<br /><br />代码-nltest /dsgetdc: /GC /FORCE|  
  
### <a name="advanced-troubleshooting"></a>高级疑难解答  
此模块旨在通过将*工作*日志作为示例使用并说明所发生的情况来教授高级疑难解答。 如果你了解成功的虚拟化域控制器操作是什么样子，那么在你的环境中失败会变得很明显。 这些日志都由其源给出，在每个日志中，与克隆的域控制器相关的*预期*事件按升序进行排序（即使它们是警告和错误）。  
  
#### <a name="cloning-a-domain-controller"></a>克隆域控制器  
在本例中，克隆域控制器使用 DHCP 获取 IP 地址、使用 FRS 或 DFSR（请根据需要参阅相应的日志）复制 SYSVOL、是全局编录并且使用空白 dccloneconfig.xml 文件。  
  
##### <a name="directory-services-event-log"></a>目录服务事件日志  
目录服务日志包含大部分基于事件的克隆操作信息。 虚拟机监控程序更改 VM 生成 ID，NTDS 服务对它进行注释，然后使 RID 池无效并更改调用 ID。 已设置新的 VM 生成 ID 且服务器将复制 Active Directory 数据入站。 停止 DFSR 服务并删除其包含 SYSVOL 的数据库，强制执行非权威同步入站。 调整 USN 高水印。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**2160**|ActiveDirectory_DomainService|本地 Active Directory 域服务已找到虚拟域控制器克隆配置文件。<br /><br />虚拟域控制器克隆配置文件位于：<br /><br />*<path>* \DCCloneConfig.xml<br /><br />虚拟域控制器克隆配置文件的存在表示本地虚拟域控制器是另一个虚拟域控制器的克隆。 Active Directory 域服务将开始克隆其本身。|  
|**2191**|ActiveDirectory_DomainService|Active Directory 域服务将设置以下注册表值以禁用 DNS 更新。<br /><br />注册表项：<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />注册表值：<br /><br />UseDynamicDns<br /><br />注册表值数据：<br /><br />0<br /><br />在克隆过程中，本地计算机在短时间内可能会与克隆源计算机具有相同的计算机名称。 在此期间将禁用 DNS A 和 AAAA 记录注册，因此客户端无法将请求发送到正在进行克隆的本地计算机。 完成克隆后，克隆进程将再次启用 DNS 更新。|  
|**2191**|ActiveDirectory_DomainService|Active Directory 域服务将设置以下注册表值以禁用 DNS 更新。<br /><br />注册表项：<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />注册表值：<br /><br />RegistrationEnabled<br /><br />注册表值数据：<br /><br />0<br /><br />在克隆过程中，本地计算机在短时间内可能会与克隆源计算机具有相同的计算机名称。 在此期间将禁用 DNS A 和 AAAA 记录注册，因此客户端无法将请求发送到正在进行克隆的本地计算机。 完成克隆后，克隆进程将再次启用 DNS 更新。<br /><br />"信息 2012 年 2 月 7 日 3:12:49 PM Microsoft Windows ActiveDirectory_DomainService 2191 内部配置"Active Directory 域服务设置以下注册表值以禁用 DNS 更新。<br /><br />注册表项：<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />注册表值：<br /><br />DisableDynamicUpdate<br /><br />注册表值数据：<br /><br />1<br /><br />在克隆过程中，本地计算机在短时间内可能会与克隆源计算机具有相同的计算机名称。 在此期间将禁用 DNS A 和 AAAA 记录注册，因此客户端无法将请求发送到正在进行克隆的本地计算机。 完成克隆后，克隆进程将再次启用 DNS 更新。|  
|**2172**|ActiveDirectory_DomainService|读取域控制器的计算机对象的 msDS-GenerationId 属性。<br /><br />msDS-GenerationId 属性值：<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|已检测到生成 ID 更改。<br /><br />在 DS 中缓存的生成 ID（旧值）：<br /><br />*<Number>*<br /><br />VM 中的当前生成 ID（新值）：<br /><br />*<Number>*<br /><br />生成 ID 将在应用虚拟机快照之后、在虚拟机导入操作之后或在实时迁移操作之后发生更改。 Active Directory 域服务将创建一个新的调用 ID 以恢复域控制器。 不应使用虚拟机快照还原虚拟化域控制器。 支持用于还原或回滚 Active Directory 域服务数据库中的内容的方法是：还原使用 Active Directory 域服务感知备份应用程序制作的系统状态备份。|  
|**1109**|ActiveDirectory_DomainService|已更改此目录服务器的 invocationID 属性。 在创建备份时，最高更新序列号如下所示：<br /><br />InvocationID 属性（旧值）：<br /><br />*<GUID>*<br /><br />InvocationID 属性（新值）：<br /><br />*<GUID>*<br /><br />更新序列号：<br /><br />*<Number>*<br /><br />在以下情况中，将更改 InvocationID：目录服务器从备份媒体还原，将其配置为托管可写应用程序目录分区，并且在应用虚拟机快照、虚拟机导入操作或实时迁移操作之后已恢复目录服务器。 不应使用虚拟机快照还原虚拟化域控制器。 支持用于还原或回滚 Active Directory 域服务数据库中的内容的方法是：还原使用 Active Directory 域服务感知备份应用程序制作的系统状态备份。|  
|**1000**|ActiveDirectory_DomainService|Microsoft Active Directory 域服务启动完成。|  
|**1394**|ActiveDirectory_DomainService|已清除阻止 Active Directory 域服务数据库更新的所有问题。 成功完成 Active Directory 域服务数据库的新的更新。 已重启 Net Logon 服务|  
|**2163**|ActiveDirectory_DomainService|已启动 DsRoleSvc 服务以克隆本地虚拟域控制器。|  
|**326**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎已附加数据库 (1, C:\Windows\NTDS\ntds.dit)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.016、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000。<br /><br />已保存的缓存：1|  
|**103**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎已停止实例 (0)。<br /><br />异常关闭：0<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.032、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.000、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。|  
|**102**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎 (6.02.8225.0000) 正在启动新实例 (0)。|  
|**105**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎已启动新实例 (0)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.016、[2] 0.000、[3] 0.015、[4] 0.078、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.046、[10] 0.000、[11] 0.000。|  
|**1004**|ActiveDirectory_DomainService|已成功关闭 Active Directory 域服务。|  
|**102**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎 (6.02.8225.0000) 正在启动新实例 (0)。|  
|**326**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎已附加数据库 (1, C:\Windows\NTDS\ntds.dit)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.015、[3] 0.016、[4] 0.000、[5] 0.031、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000。<br /><br />已保存的缓存：1|  
|**105**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎已启动新实例 (0)。 （时间 = 1 秒）<br /><br />内部计时序列：[1] 0.031、[2] 0.000、[3] 0.000、[4] 0.391、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.000。|  
|**1109**|ActiveDirectory_DomainService|已更改此目录服务器的 invocationID 属性。 在创建备份时，最高更新序列号如下所示：<br /><br />InvocationID 属性（旧值）：<br /><br />*<GUID>*<br /><br />InvocationID 属性（新值）：<br /><br />*<GUID>*<br /><br />更新序列号：<br /><br />*<Number>*<br /><br />在以下情况中，将更改 InvocationID：目录服务器从备份媒体还原，将其配置为托管可写应用程序目录分区，并且在应用虚拟机快照、虚拟机导入操作或实时迁移操作之后已恢复目录服务器。 不应使用虚拟机快照还原虚拟化域控制器。 支持用于还原或回滚 Active Directory 域服务数据库中的内容的方法是：还原使用 Active Directory 域服务感知备份应用程序制作的系统状态备份。|  
|**1168**|ActiveDirectory_DomainService|内部错误：Active Directory 域服务出错。<br /><br />其他数据<br /><br />错误值（十进制）：<br /><br />2<br /><br />错误值（十六进制）：<br /><br />2<br /><br />内部 ID：<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|将此域控制器升级为全局编录的操作会延迟以下间隔时间。<br /><br />间隔（分钟）：<br /><br />5<br /><br />此延迟是必须的，以便在播发全局编录之前准备好所要求的目录分区。 在注册表中，你可以指定目录系统代理在本地域控制器升级为全局编录之前等待的秒数。 要了解全局编录延迟播发注册表值，请参阅资源工具包分发系统指南|  
|**103**|NTDS ISAM|NTDS (536) NTDSA：数据库引擎已停止实例 (0)。<br /><br />异常关闭：0<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.047、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.016、[10] 0.000、[11] 0.000、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。|  
|**1004**|ActiveDirectory_DomainService|已成功关闭 Active Directory 域服务。|  
|**1539**|ActiveDirectory_DomainService|Active Directory 域服务无法禁用以下硬盘上基于软件的磁盘写入高速缓存。<br /><br />硬盘：<br /><br />c:<br /><br />系统失败时数据可能丢失|  
|**2179**|ActiveDirectory_DomainService|域控制器的计算机对象的 msDS-GenerationId 属性已设置为以下参数：<br /><br />GenerationID 属性：<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|无法读取域控制器的计算机对象的 msDS-GenerationId 属性。 这可能是由于数据库事务失败或本地数据库中不存在生成 ID 所致。 在 dcpromo 之后的第一次重新启动过程中不存在 msDS-GenerationId，或者该 DC 并非虚拟域控制器。<br /><br />其他数据<br /><br />故障代码：<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Microsoft Active Directory 域服务启动完成，版本 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|已清除阻止 Active Directory 域服务数据库更新的所有问题。 成功完成 Active Directory 域服务数据库的新的更新。 已重启 Net Logon 服务。|  
|**1128**|ActiveDirectory_DomainService|1128 知识一致性检查“已从以下源目录服务到本地目录服务创建复制连接。<br /><br />源目录服务：<br /><br />CN = NTDS 设置，*<Domain Controller DN>*<br /><br />本地目录服务：<br /><br />CN = NTDS 设置， *<Domain Controller DN>*<br /><br />其他数据<br /><br />原因代码：<br /><br />0x2<br /><br />创建点内部 ID：<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|源目录服务已优化由目标目录服务显示的更新序列号 (USN)。 源和目标目录服务具有通用的复制伙伴。 目标目录服务使用通用复制伙伴保持最新状态，而且使用此伙伴的备份安装了源目录服务。<br /><br />目标目录服务 ID：<br /><br />*<GUID> (<FQDN>)*<br /><br />通用目录服务 ID：<br /><br />*<GUID>*<br /><br />通用属性 USN：<br /><br />*<Number>*<br /><br />因此，已使用以下设置配置目标目录服务的最新程度矢量。<br /><br />上一个对象 USN：<br /><br />0<br /><br />上一个属性 USN：<br /><br />0<br /><br />数据库 GUID：<br /><br />*<GUID>*<br /><br />对象 USN：<br /><br />*<Number>*<br /><br />属性 USN：<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>系统事件日志  
克隆操作的后续指示位于系统事件日志中。 因为虚拟机监控程序告诉来宾计算机，它已克隆或从快照还原，所以域控制器将立即使其 RID 池失效，以避免之后复制安全主体。 随着克隆继续进行，将出现各种预期的操作和消息，主要是围绕启动和停止服务，以及由此导致的某些预期错误。 完成后，系统事件日志将记录整体克隆成功。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**16654**|Directory-Services-SAM|帐户标识符 (RID) 池已失效。 在以下预期情况下，可能会出现该问题：<br /><br />1.通过备份还原域控制器。<br /><br />2.通过快照还原在虚拟机上运行的域控制器。<br /><br />3.管理员已手动使池失效|  
|**7036**|服务控制管理器|“Active Directory 域服务”服务已进入运行状态。|  
|**7036**|服务控制管理器|“Kerberos 密钥发行中心”服务已进入运行状态。|  
|**3096**|Netlogon|找不到此域的主域控制器。|  
|**7036**|服务控制管理器|“安全帐户管理器”服务已进入运行状态。|  
|**7036**|服务控制管理器|“服务器”服务已进入运行状态。|  
|**7036**|服务控制管理器|“Netlogon”服务已进入运行状态。|  
|**7036**|服务控制管理器|“Active Directory Web 服务”服务已进入运行状态。|  
|**7036**|服务控制管理器|“DFS 复制”服务已进入运行状态。|  
|**7036**|服务控制管理器|“文件复制服务”服务已进入运行状态。|  
|**14533**|Microsoft-Windows-DfsSvc|DFS 已构建完所有命名空间。|  
|**14531**|Microsoft-Windows-DfsSvc|DFS 服务器已完成初始化。|  
|**7036**|服务控制管理器|“DFS 命名空间”服务已进入运行状态。|  
|**7023**|服务控制管理器|“站点间消息”服务已终止，并出现以下错误：<br /><br />指定的服务器无法执行所请求的操作。|  
|**7036**|服务控制管理器|“站点间消息”服务已进入停止状态。|  
|**5806**|Netlogon|已手动禁用此域控制器上的动态更新。<br /><br />USER ACTION<br /><br />重新配置此域控制器以使用动态更新或手动将 DNS 记录从文件“%SystemRoot%\System32\Config\Netlogon.dns”添加到 DNS 数据库。|  
|**16651**|Directory-Services-SAM|新帐户标识符池的请求失败。 将重试该操作，直到请求成功。 错误在于<br /><br />所请求的 FSMO 操作失败。 无法联系当前 FSMO 所有者。|  
|**7036**|服务控制管理器|“DNS 服务器”服务已进入运行状态。|  
|**7036**|服务控制管理器|“DS 角色服务器”服务已进入运行状态。|  
|**7036**|服务控制管理器|“Netlogon”服务已进入停止状态。|  
|**7036**|服务控制管理器|“文件复制服务”服务已进入停止状态。|  
|**7036**|服务控制管理器|“Kerberos 密钥发行中心”服务已进入停止状态。|  
|**7036**|服务控制管理器|“DNS 服务器”服务已进入停止状态。|  
|**7036**|服务控制管理器|“Active Directory 域服务”服务已进入停止状态。|  
|**7036**|服务控制管理器|“Netlogon”服务已进入运行状态。|  
|**7040**|服务控制管理器|已将“Active Directory 域服务”服务的启动类型从自动启动更改为禁用。|  
|**7036**|服务控制管理器|“Netlogon”服务已进入停止状态。|  
|**7036**|服务控制管理器|“文件复制服务”服务已进入运行状态。|  
|**29219**|DirectoryServices-DSROLE-Server|虚拟域控制器克隆成功。|  
|**29223**|DirectoryServices-DSROLE-Server|现在此服务器是一个域控制器。|  
|**29265**|DirectoryServices-DSROLE-Server|虚拟域控制器克隆成功。 已将虚拟域控制器克隆配置文件 C:\Windows\NTDS\DCCloneConfig.xml 重命名为 C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml。|  
|**1074**|User32|进程 C:\Windows\system32\lsass.exe (DC2) 已代表用户 NT AUTHORITY\SYSTEM 初始化计算机 DC2 的重启，原因如下：操作系统：重新配置（已计划）<br /><br />原因代码：0x80020004<br /><br />关机类型：重启<br /><br />注释：“|  
  
##### <a name="dcpromolog"></a>DCPROMO.LOG  
Dcpromo.log 包含目录服务事件日志中未描述的克隆的实际升级部分。 因为该日志不提供事件日志条目提供的描述级别，所以模块的本部分包含了额外注释。  
  
升级过程意味着克隆启动，将清除 DC 的当前配置并使用现有的 AD 数据库将其重新升级（类似于 IFM 升级），然后 DC 将复制 AD 和 SYSVOL 的入站更改增量，最后克隆完成。  
  
> [!NOTE]  
> 出于可读性考虑，已通过删除日期列修改了此模块中的日志。  
  
> [!NOTE]  
> 有关 dcpromo.log 的进一步说明，请参阅“了解 Windows Server 2012 中的 AD DS 简化管理及其疑难解答”。  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   开始基于克隆的升级  
  
-   设置目录服务还原模式标志，以使服务器无法如同原始克隆一样正常启动备份，并引起命名或目录服务冲突  
  
-   更新目录服务事件日志  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   停止 NetLogon 服务，以使域控制器不进行播发  
  
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
  
-   检查 dccloneconfig.xml 文件中特定于管理员的自定义。  
  
-   在此示例情况下，它是一个空白文件，因此将自动生成所有设置并需要网络中的自动 IP 寻址  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   验证安装的服务或程序都是 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 的一部分  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   由于管理员未指定 IP 信息，请在网络适配器上启用 DHCP  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   找到 PDC 模拟器  
  
-   设置克隆的站点（在此情况下将自动生成）  
  
-   设置克隆的名称（在此情况下将自动生成）  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   创建新的克隆计算机对象  
  
-   重命名克隆以匹配新的名称  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   根据之前的 dccloneconfig.xml 或自动生成规则提供升级设置  
  
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
  
-   停止并配置所有与 AD DS 相关的服务（NTDS、NTFRS/DFSR、KDC、DNS）  
  
> [!NOTE]  
> 在此方案中，预期需要较长时间关闭 DNS 服务，因为它使用了在停止 NTDS 服务之前便已不再可用的 AD 集成区域 – 请参阅模块中本部分稍后所述的 DNS 事件。  
  
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
  
-   强制 NT5DS (NTP) 与另一个域控制器（通常为 PDCE）进行时间同步  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   联系保留克隆的源域控制器帐户的域控制器  
  
-   刷新任何现有的 Kerberos 票证  
  
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
  
-   停止 NetLogon 服务并设置其启动类型  
  
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
  
-   将 DFSR/NTFRS 服务配置为自动运行  
  
-   删除其现有数据库文件，以在服务下次启动时强制执行 SYSVOL 的非权威同步  
  
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
  
-   使用现有 NTDS 数据库文件启动升级过程  
  
-   联系 RID 主机  
  
> [!NOTE]  
> AD DS 服务实际并非安装在此处，这是日志中的旧检测  
  
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
  
-   更改存在于源计算机数据库中的现有调用 ID  
  
-   为此克隆创建新的 NTDS 设置对象  
  
-   在伙伴域控制器的 AD 对象增量中进行复制  
  
> [!NOTE]  
> 即使将所有对象都列为已复制，这也是只是为更新归类所需的元数据。 克隆的 NTDS 数据库中所有未更改的对象都已存在，并且不需要再次复制，如同使用基于 IFM 的升级一样。  
  
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
  
-   根据需要将任何缺少的更新填充到 GC 分区  
  
-   完成此升级的关键 AD DS 部分  
  
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
  
-   完成 SYSVOL 的入站复制  
  
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
  
-   启用客户端 DNS 注册  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   运行由 DefaultDCCloneAllowList.xml <SysprepInformation> 元素指定的 SYSPREP 模块。  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   克隆升级完成  
  
-   删除 DSRM 启动标志，以使服务器下次可以正常启动  
  
-   重命名 dccloneconfig.xml，以便下次启动时不再读取它  
  
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
发生克隆时，NTDS.DIT 数据库处于脱机状态的时间将延长。 ADWS 服务将为此记录至少一个事件。 克隆完成后，将启动 ADWS 服务，请注意目前尚不存在有效的计算机证书（存在与否取决于你的环境是否使用自动注册部署 Microsoft PKI），然后将启动新域控制器的实例。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**1202**|ADWS 实例事件|现在此计算机托管了指定的目录实例，但 Active Directory Web 服务无法处理它。 Active Directory Web 服务将定期重试此操作。<br /><br />目录实例：NTDS<br /><br />目录实例 LDAP 端口：389<br /><br />目录实例 SSL 端口：636|  
|**1000**|ADWS 实例事件|正在启动 Active Directory Web 服务|  
|**1008**|ADWS 实例事件|Active Directory Web 服务已成功减少其安全特权|  
|**1100**|ADWS 实例事件|已加载 Active Directory Web 服务配置文件的 <appsettings> 部分中指定的值，未出现错误。|  
|**1400**|ADWS 实例事件|ADWS 证书事件 Active Directory Web 服务找不到具有指定证书名称的服务器证书。 使用 SSL/TLS 连接需要证书。 若要使用 SSL/TLS 连接，请验证计算机上安装了来自可信任证书颁发机构 (CA) 的有效服务器身份验证证书。<br /><br />证书名称： *<Server FQDN>*|  
|**1100**|ADWS 实例事件|已加载 Active Directory Web 服务配置文件的 <appsettings> 部分中指定的值，未出现错误。|  
|**1200**|ADWS 实例事件|目前，Active Directory Web 服务在处理指定的目录实例。<br /><br />目录实例：NTDS<br /><br />目录实例 LDAP 端口：389<br /><br />目录实例 SSL 端口：636|  
  
##### <a name="dns-server-event-log"></a>DNS 服务器事件日志  
因为 AD DS 数据库处于脱机状态时 DNS 服务仍在运行，所以发生克隆时，DNS 服务将发生短暂的预期中断。 若使用 Active Directory 集成的 DNS，则会发生这种情况，但若使用标准主要 DNS 或辅助 DNS，则不会发生这种情况。 这些错误将多次记录。 克隆完成后，正常情况下，DNS 将恢复联机状态。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**4013**|DNS-Server-Service|DNS 服务器正在等待 Active Directory 域服务 (AD DS) 显示已完成目录的初始同步的信号。 在初始同步完成之前，DNS 服务器服务将无法启动，因为可能未将关键 DNS 数据复制到此域控制器上。 如果 AD DS 事件日志中的事件指示 DNS 名称解析出现问题，则请考虑将此域中另一台 DNS 服务器的 IP 地址添加到这台计算机“Internet 协议”属性的 DNS 服务器列表中。 每隔两分钟记录一次此事件，直到 AD DS 显示已成功完成初始同步的信号为止。|  
|**4015**|DNS-Server-Service|DNS 服务器遇到了来自 Active Directory 的关键错误。 检查 Active Directory 是否正常运作。 扩展的错误调试信息（可能为空）是 """"。 事件数据包含了错误。|  
|**4000**|DNS-Server-Service|DNS 服务器无法打开 Active Directory。  此 DNS 服务器配置为获取并使用该区域的目录中的信息，并且如果没有该信息将无法加载该区域。  检查 Active Directory 是否正常运作，并重新加载该区域。 事件数据即为错误代码。|  
|**4013**|DNS-Server-Service|DNS 服务器正在等待 Active Directory 域服务 (AD DS) 显示已完成目录的初始同步的信号。 在初始同步完成之前，DNS 服务器服务将无法启动，因为可能未将关键 DNS 数据复制到此域控制器上。 如果 AD DS 事件日志中的事件指示 DNS 名称解析出现问题，则请考虑将此域中另一台 DNS 服务器的 IP 地址添加到这台计算机“Internet 协议”属性的 DNS 服务器列表中。 每隔两分钟记录一次此事件，直到 AD DS 显示已成功完成初始同步的信号为止。|  
|**2**|DNS-Server-Service|已启动 DNS 服务器。|  
|**4**|DNS-Server-Service|DNS 服务器已完成区域的背景加载。 现在所有区域都可以进行 DNS 更新和区域传送，其单独的区域配置也允许这样。|  
  
##### <a name="file-replication-service-event-log"></a>文件复制服务事件日志  
在克隆过程中，文件复制服务将从合作伙伴进行非权威同步。 克隆通过完成此操作删除 NTFRS 数据库文件并保持 SYSVOL 的内容不变，以便用作预植入的数据。 两次尝试同步是预期行为。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**13562**|NtFrs|下面是轮询域控制器 DC2.root.fabrikam.com 的 FRS 副本集配置信息时，文件复制服务遇到的警告和错误的摘要。<br /><br />无法绑定到域控制器。 将在下一个轮询周期重试|  
|**13502**|NtFrs|文件复制服务正在停止。|  
|**13565**|NtFrs|文件复制服务使用另一域控制器的数据初始化系统卷。 在完成此过程之前，计算机 DC2 将无法成为域控制器。 然后，系统卷将共享为 SYSVOL。<br /><br />若要检查 SYSVOL 共享，请在命令提示符下键入：<br /><br />net share<br /><br />文件复制服务完成初始化过程后，将显示 SYSVOL 共享。<br /><br />系统卷的初始化过程可能需要一些时间。 所需时间取决于系统卷中的数据量、其他域控制器的可用性和域控制器之间的复制间隔。|  
|**13501**|NtFrs|文件复制服务正在启动|  
|**13502**|NtFrs|文件复制服务正在停止。|  
|**13503**|NtFrs|文件复制服务已停止。|  
|**13565**|NtFrs|文件复制服务使用另一域控制器的数据初始化系统卷。 在完成此过程之前，计算机 DC2 将无法成为域控制器。 然后，系统卷将共享为 SYSVOL。<br /><br />若要检查 SYSVOL 共享，请在命令提示符下键入：<br /><br />net share<br /><br />文件复制服务完成初始化过程后，将显示 SYSVOL 共享。<br /><br />系统卷的初始化过程可能需要一些时间。 所需时间取决于系统卷中的数据量、其他域控制器的可用性和域控制器之间的复制间隔。|  
|**13501**|NtFrs|文件复制服务正在启动。|  
|**13553**|NtFrs|文件复制服务已成功地将此计算机添加到以下副本集：<br /><br />“DOMAIN SYSTEM VOLUME (SYSVOL SHARE)”<br /><br />与此事件相关的信息如下所示：<br /><br />计算机 DNS 名称是  *<Domain Controller FQDN>*<br /><br />副本集成员名称是 *<Domain Controller>*<br /><br />副本集根路径是 *<path>*<br /><br />复本分段目录路径是 *<path>*<br /><br />复本工作目录路径是 *<path>*|  
|**13520**|NtFrs|文件复制服务中移动已存在的文件<path>到*<path>* \NtFrs_PreExisting___See_EventLog。<br /><br />文件复制服务可能会删除中的文件 *<path>* 随时 \NtFrs_PreExisting___See_EventLog。 可以通过复制出被删除保存文件*<path>* \NtFrs_PreExisting___See_EventLog。 如果文件在其他复制伙伴中已经存在，将文件复制到 c:\windows\sysvol\domain 可能会引起名称冲突。<br /><br />在某些情况下，文件复制服务可能会将文件从*<path>* \NtFrs_PreExisting___See_EventLog 到*<path>* 而不是从某个其他复制文件复制伙伴。<br /><br />通过删除中的文件，可以随时恢复空间*<path>* \NtFrs_PreExisting___See_EventLog。"|  
|**13508**|NtFrs|文件复制服务时遇到问题，启用从复制*\\ \\ <Domain Controller FQDN>* 到*<Domain Controller>* 为 *<path>* 使用<br /><br />DNS 名称*\\ \\ <Domain Controller FQDN>*。 FRS 会不断重试。<br /><br />以下是一些你将看到此警告的原因。<br /><br />[1] FRS 不能正确解析 DNS 名称*\\ \\ <Domain Controller FQDN>* 从这台计算机。<br /><br />[2] FRS 未运行*\\ \\ <Domain Controller FQDN>*。<br /><br />[3] 尚未将此副本的 Active Directory 域服务中的拓扑信息复制到所有域控制器。<br /><br />每次连接都将显示一次该事件日志消息，解决该问题后，你将看到另一个事件日志消息，指示已建立连接。|  
|**13509**|NtFrs|文件复制服务已启用复制*\\ \\ <Domain Controller FQDN>* 到*<Domain Controller>* 为*<Path>* 后多次重试。|  
|**13516**|NtFrs|文件复制服务不再阻止计算机*<Domain Controller>* 成为域控制器。 已成功初始化系统卷，并已通知 Netlogon 服务现在可以将系统卷共享为 SYSVOL。<br /><br />键入“net share”检查 SYSVOL 共享。|  
  
##### <a name="dfs-replication-event-log"></a>DFS 复制事件日志  
在克隆过程中，DFSR 服务将从伙伴进行非权威同步。 克隆通过该方法实现此功能：删除 DFSR 数据库文件并保持 SYSVOL 的内容不变，以便用作预植入的数据。 两次尝试同步是预期行为。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**1004**|DFSR|DFS 复制服务已启动。|  
|**1314**|DFSR|DFS 复制服务已成功配置调试日志文件。<br /><br />其他信息：<br /><br />调试日志文件路径：C:\Windows\debug|  
|**6102**|DFSR|DFS 复制服务已成功注册 WMI 提供程序|  
|**1206**|DFSR|DFS 复制服务已成功联系域控制器 DC2.corp.contoso.com 以访问配置信息。|  
|**1210**|DFSR|DFS 复制服务为传入的复制请求成功地设置了 RPC 侦听程序。<br /><br />其他信息：<br /><br />端口：0"|  
|**4614**|DFSR|DFS 复制服务在本地路径 C:\Windows\SYSVOL\domain 中初始化了 SYSVOL，并且正在等待执行初始复制。 所复制的文件夹将保留在初始同步状态，直到它已通过其伙伴进行复制。 如果该服务器正在升级到域控制器，则在解决此问题之前，该域控制器将不会进行播发，并且不会发挥如同域控制器的作用。 如果指定的伙伴也同样处于初始同步状态，或者如果在此服务器或同步伙伴遇到共享冲突，则可能发生这种情况。 如果在从文件复制服务 (FRS) 到 DFS 复制的 SYSVOL 迁移过程中遇到此事件，则在解决此问题之前，将不会复制更改。 这可能会导致此服务器上的 SYSVOL 文件夹与其他域控制器不同步。<br /><br />其他信息：<br /><br />已复制的文件夹名称：SYSVOL 共享<br /><br />已复制的文件夹 ID: *<GUID>*<br /><br />复制组名称：域系统卷<br /><br />复制组 ID: *<GUID>*<br /><br />成员 ID: *<GUID>*<br /><br />只读：0|  
|**4604**|DFSR|DFS 复制服务已成功初始化位于本地路径 C:\Windows\SYSVOL\domain 上的 SYSVOL 已复制文件夹。 此成员已完成 SYSVOL 和合作伙伴 dc1.corp.contoso.com 的初始同步。  若要检查 SYSVOL 共享是否存在，请打开命令提示符窗口，然后键入“net share”。<br /><br />其他信息：<br /><br />已复制的文件夹名称：SYSVOL 共享<br /><br />已复制的文件夹 ID: *<GUID>*<br /><br />复制组名称：域系统卷<br /><br />复制组 ID: *<GUID>*<br /><br />成员 ID: *<GUID>*<br /><br />同步伙伴： *<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>虚拟化的域控制器安全还原进行故障排除  
  
### <a name="tools-for-troubleshooting"></a>故障排除工具  
  
#### <a name="logging-options"></a>日志记录选项  
内置的日志是用于解决域控制器安全快照还原问题的最重要工具。 默认情况下，所有这些日志都处于启用状态并配置为最大详细级别。  
  
|||  
|-|-|  
|**Operation**|**Log**|  
|**创建快照**|-事件 viewer\Applications and services logs\Microsoft\Windows\Hyper-V-辅助角色|  
|**快照还原**|-事件 viewer\Applications and services logs\Directory 服务<br />-事件 viewer\Windows logs\System<br />-事件 viewer\Windows 日志 \ 应用程序<br />-事件 viewer\Applications and services logs\File 复制服务<br />-事件 viewer\Applications and services logs\DFS 复制<br />-事件 viewer\Applications and services logs\DNS<br />-事件 viewer\Applications and services logs\Microsoft\Windows\Hyper-V-辅助角色|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>用于对域控制器配置进行故障排除的工具和命令  
要解决日志未说明的问题，请从使用以下工具开始：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>故障排除域控制器安全还原的常规方法  
  
1.  虽然安全快照还原是预期操作，但遇到问题？  
  
    1.  检查目录服务事件日志  
  
        1.  是否存在快照还原错误？  
  
        2.  是否存在 AD 复制错误？  
  
    2.  检查系统事件日志  
  
        1.  是否存在通信错误？  
  
        2.  是否存在 AD 错误？  
  
2.  是否意外地执行安全快照还原？  
  
    1.  检查虚拟机监控程序审核日志以确定导致回滚的人员或原因  
  
    2.  联系虚拟机监控程序的所有管理员并询问他们是谁在没有进行通知的情况下回滚了虚拟机  
  
3.  服务器是否实现 USN 回滚保护但并未安全地还原？  
  
    1.  检查目录服务事件日志中不受支持的虚拟机监控程序或集成服务  
  
    2.  是否检查了操作系统并验证了正在运行的 Windows Server 2012？  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>故障排除的特定问题  
  
#### <a name="events"></a>事件  
所有虚拟化域控制器安全快照还原事件都将写入已还原域控制器 VM 的目录服务事件日志中。 应用程序、系统、文件复制服务和 DFS 复制事件日志可能还包含已失败还原的有用疑难解答信息。  
  
下面是目录服务事件日志中特定于 Windows Server 2012 安全还原的事件。  
  
|||  
|-|-|  
|**事件 ID**|**2170**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|警告|  
|**Message**|已检测到生成 ID 更改。<br /><br />DS 中缓存的生成 ID（旧值）：%1<br /><br />VM 中的当前生成 ID（新值）：%2<br /><br />生成 ID 将在应用虚拟机快照之后、在虚拟机导入操作之后或在实时迁移操作之后发生更改。 *<COMPUTERNAME>* 将创建一个新的调用 ID 以恢复域控制器。 不应使用虚拟机快照还原虚拟化域控制器。 支持用于还原或回滚 Active Directory 域服务数据库中的内容的方法是：还原使用 Active Directory 域服务感知备份应用程序制作的系统状态备份。|  
|**说明和解决方法**|如果快照为预期行为，则这是一个成功事件。 如果不是，则请检查 Hyper-V-Worker 事件日志，或联系虚拟机监控程序管理员。|  
  
|||  
|-|-|  
|**事件 ID**|**2174**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|该 DC 既不是虚拟域控制器克隆，也不是已还原虚拟域控制器快照。|  
|**说明和解决方法**|当启动物理域控制器或虚拟化域控制器未从快照还原时，这是预期事件|  
  
|||  
|-|-|  
|**事件 ID**|**2181**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|因为虚拟机还原到以前的状态，所以中止了事务。  在应用虚拟机快照后、虚拟机导入操作后或者实时迁移操作后，将发生这种情况。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 事务将跟踪 VM 生成 ID 更改|  
  
|||  
|-|-|  
|**事件 ID**|**2185**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 已停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务。<br /><br />服务名称：%1<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 必须初始化本地 SYSVOL 副本上的非权威还原。 执行该操作的方法是：停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务，然后使用相应的注册表项和值启动它来触发还原。 重启 FRS 或 DFSR 服务时，将会记录事件 2187。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 此域控制器上的所有 SYSVOL 数据都将替换为伙伴 DC 的副本。|  
|**事件 ID**|2186|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务。<br /><br />服务名称：%1<br /><br />错误代码：%2<br /><br />错误消息：%3<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 必须初始化本地 SYSVOL 副本上的非权威还原。 完成该操作的方法是：停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 复制服务，然后使用相应的注册表项和值启动它来触发还原。 *<COMPUTERNAME>* 无法停止当前正在运行的服务，无法完成非权威还原。 请手动执行非权威还原。|  
|**说明和解决方法**|检查系统、FRS 和 DFSR 事件日志以获取详细信息。|  
  
|||  
|-|-|  
|**事件 ID**|**2187**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 已启动用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务。<br /><br />服务名称：%1<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 需要初始化本地 SYSVOL 副本上的非权威还原。 完成该操作的方法是：停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务，然后使用相应的注册表项和值启动它来触发还原。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 此域控制器上的所有 SYSVOL 数据都将替换为伙伴 DC 的副本。|  
  
|||  
|-|-|  
|**事件 ID**|**2188**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法启动用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务。<br /><br />服务名称：%1<br /><br />错误代码：%2<br /><br />错误消息：%3<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 需要初始化本地 SYSVOL 副本上的非权威还原。 完成该操作的方法是：停止用于复制 SYSVOL 的 FRS 或 DFSR 服务，然后使用相应的注册表项和值启动它来触发还原。 *<COMPUTERNAME>* 无法启动用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务，无法完成非权威还原。 请手动执行非权威还原，然后重启该服务。|  
|**说明和解决方法**|检查系统、FRS 和 DFSR 事件日志以获取详细信息。|  
  
|||  
|-|-|  
|**事件 ID**|**2189**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 设置以下注册表值以非权威还原期间初始化 SYSVOL 副本：<br /><br />注册表项：%1<br /><br />注册表值：%2<br /><br />注册表值数据：%3<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 需要初始化本地 SYSVOL 副本上的非权威还原。 完成该操作的方法是：停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务，然后使用相应的注册表项和值启动它来触发还原。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 此域控制器上的所有 SYSVOL 数据都将替换为伙伴 DC 的副本。|  
  
|||  
|-|-|  
|**事件 ID**|**2190**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法设置以下注册表值以非权威还原期间初始化 SYSVOL 副本：<br /><br />注册表项：%1<br /><br />注册表值：%2<br /><br />注册表值数据：%3<br /><br />错误代码：%4<br /><br />错误消息：%5<br /><br />Active Directory 检测到托管域控制器角色的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 需要初始化本地 SYSVOL 副本上的非权威还原。 完成该操作的方法是：停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务，然后使用相应的注册表项和值启动它来触发还原。 *<COMPUTERNAME>* 无法设置以上注册表值，无法完成非权威还原。 请手动执行非权威还原。|  
|**说明和解决方法**|检查应用程序和系统事件日志。 调查可能会阻止注册表更新的第三方应用程序。|  
  
|||  
|-|-|  
|**事件 ID**|**2200**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 初始化复制以将当前的域控制器。 完成复制后，将会记录事件 2201。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 标记入站 AD 复制的开始。|  
  
|||  
|-|-|  
|**事件 ID**|**2201**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 已完成复制，以将当前域控制器。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 标记入站 AD 复制的结束。|  
  
|||  
|-|-|  
|**事件 ID**|**2202**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 无法的复制以将域控制器保持最新状态。 在下一次定期复制后，将更新域控制器。|  
|**说明和解决方法**|检查目录服务和系统事件日志。 使用 repadmin.exe 尝试强制执行复制，并记录任何失败。|  
  
|||  
|-|-|  
|**事件 ID**|**2204**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 已检测到虚拟机生成 ID 的更改。 更改意味着虚拟域控制器已还原为以前的状态。 *<COMPUTERNAME>* 将执行以下操作，以保护已还原的域控制器针对可能的数据分歧，以及保护具有重复 Sid 的安全主体的创建：<br /><br />创建新的调用 ID<br /><br />使当前 RID 池无效<br /><br />在下次入站复制时，将验证 FSMO 角色的所有权。 在此窗口期间，如果域控制器保留 FSMO 角色，则该角色将不可用。<br /><br />启动 SYSVOL 复制服务还原操作。<br /><br />启动复制以使已还原的域控制器保持最新状态。<br /><br />请求新的 RID 池。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 这说明了将作为安全还原过程一部分进行的各种不同的重置操作。|  
  
|||  
|-|-|  
|**事件 ID**|**2205**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 使当前 RID 池失效后虚拟域控制器恢复到以前的状态。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 必须销毁本地 RID 池，因为域控制器已按时间顺序查看，并且可能已进行签发。|  
  
|||  
|-|-|  
|**事件 ID**|**2206**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法使当前 RID 池失效后虚拟域控制器恢复到以前的状态。<br /><br />其他数据：<br /><br />错误代码：%1<br /><br />错误值：%2|  
|**说明和解决方法**|检查目录服务和系统事件日志。 验证可使用 Dcdiag.exe /test:ridmanager 从该服务器访问处于联机状态的 RID 主机|  
  
|||  
|-|-|  
|**事件 ID**|**2207**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 还原虚拟域控制器恢复到以前的状态后失败。 已请求重新启动到 DSRM。 请检查之前的事件，以获取详细信息。|  
|**说明和解决方法**|检查目录服务和系统事件日志。|  
  
|||  
|-|-|  
|**事件 ID**|**2208**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|信息性|  
|**Message**|*<COMPUTERNAME>* 删除了 DFSR 数据库，以非权威还原期间初始化 SYSVOL 副本。|  
|**说明和解决方法**|当还原快照时，这是预期事件。 这可确保 DFSR 从伙伴 DC 非权威地同步 SYSVOL。 请注意，与 SYSVOL 位于相同卷上的任何其他 DFSR 已复制文件夹也将进行非权威同步（在与 SYSVOL 相同的卷上，不建议使用域控制器托管自定义 DFSR 集）。|  
  
|||  
|-|-|  
|**事件 ID**|**2209**|  
|**源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|错误|  
|**Message**|*<COMPUTERNAME>* 无法删除 DFSR 数据库。<br /><br />其他数据：<br /><br />错误代码：%1<br /><br />错误值：%2<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 *<COMPUTERNAME>* 需要初始化本地 SYSVOL 副本上的非权威还原。 对于 DFSR 而言，这可以通过停止 DFSR 服务、删除 DFSR 数据库并重启该服务来完成。 在重启时，DFSR 将重建数据库并启动初始同步。|  
|**说明和解决方法**|检查 DFSR 事件日志。|  
  
#### <a name="error-messages"></a>错误消息  
失败的虚拟化域控制器安全快照还原不存在任何直接的交互式错误；所有克隆信息都记录在目录服务事件日志中。 当然，所有关键复制或服务器播发错误都在其他位置将其自身显示为症状。  
  
#### <a name="known-issues-and-support-scenarios"></a>已知的问题和支持方案  
[故障排除域控制器安全还原的常规方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore)通常足够用于解决大多数问题。  
  
|||  
|-|-|  
|**问题**|**无法在最近安全还原的域控制器上创建新的安全主体**|  
|**症状**|在还原快照之后，在该域控制器上新建安全主体（用户、计算机、组）的尝试失败，并显示：<br /><br />错误 0x2010<br /><br />目录服务无法分配相对标识符。|  
|**解析和注释**|此问题是由已还原计算机对 RID 主机 FSMO 角色的过时知识引起的。 如果在拍摄完快照并还原快照后，该角色移动到这个或另一个域控制器，则在初始复制完成之前，已还原的域控制器将不会获得关于 RID 主机的知识。<br /><br />若要解决此问题，请允许 AD 复制对已还原域控制器执行完入站操作。 如果仍无法正常工作，验证所有域控制器都正确地知道哪个 DC 托管 RID 主机。|  
  
|||  
|-|-|  
|**问题**|**还原的域控制器执行不共享 SYSVOL，无法进行播发**|  
|**症状**|在还原快照后，一个或多个 DC 无法进行播发、无法共享 sysvol，并且不包含最新的 SYSVOL 内容|  
|**解析和注释**|DC 的上游伙伴没有用于正确复制 DFSR 或 FRS 的可用 SYSVOL 副本。 此问题与安全还原无关，但很可能会表现为安全还原问题，因为客户没有意识到影响未还原的 DC 的其他复制问题|  
  
### <a name="advanced-troubleshooting"></a>高级疑难解答  
此模块旨在通过将*工作*日志作为示例使用并说明所发生的情况来教授高级疑难解答。 如果你了解成功的虚拟化域控制器操作是什么样子，那么在你的环境中失败会变得很明显。 这些日志都由源给出，在每个日志中，与克隆的域控制器相关的*预期*事件按升序进行排序。  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>还原使用 DFSR 复制 SYSVOL 的域控制器  
  
##### <a name="directory-services-event-log"></a>目录服务事件日志  
目录服务事件日志包含大部分的安全还原操作信息。 虚拟机监控程序更改 VM 生成 ID，NTDS 服务对它进行注释，然后使 RID 池无效并更改调用 ID。 已设置新的 VM 生成 ID 且服务器将复制 AD 数据入站。 停止 DFSR 服务并删除其包含 SYSVOL 的数据库，强制执行非权威同步入站。 调整 USN 高水印。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**2170**|ActiveDirectory_DomainService|已检测到生成 ID 更改。<br /><br />在 DS 中缓存的生成 ID（旧值）：<br /><br />*<number>*<br /><br />VM 中的当前生成 ID（新值）：<br /><br />*<number>*<br /><br />生成 ID 将在应用虚拟机快照之后、在虚拟机导入操作之后或在实时迁移操作之后发生更改。 Active Directory 域服务将创建一个新的调用 ID 以恢复域控制器。 不应使用虚拟机快照还原虚拟化域控制器。 支持用于还原或回滚 Active Directory 域服务数据库中的内容的方法是：还原使用 Active Directory 域服务感知备份应用程序制作的系统状态备份。”|  
|**2181**|ActiveDirectory_DomainService|因为虚拟机还原到以前的状态，所以中止了事务。  在应用虚拟机快照后、虚拟机导入操作后或者实时迁移操作后，将发生这种情况。|  
|**2204**|ActiveDirectory_DomainService|Active Directory 域服务已检测到 VM 生成 ID 的更改。 更改意味着虚拟域控制器已还原为以前的状态。 Active Directory 域服务将执行以下操作，以针对可能的数据分歧保护已还原的域控制器，以及保护具有重复 SID 的安全主体的创建：<br /><br />创建新的调用 ID<br /><br />使当前 RID 池无效<br /><br />在下次入站复制时，将验证 FSMO 角色的所有权。 在此窗口期间，如果域控制器保留 FSMO 角色，则该角色将不可用。<br /><br />启动 SYSVOL 复制服务还原操作。<br /><br />启动复制以使已还原的域控制器保持最新状态。<br /><br />请求新的 RID 池。|  
|**2181**|ActiveDirectory_DomainService|因为虚拟机还原到以前的状态，所以中止了事务。  在应用虚拟机快照后、虚拟机导入操作后或者实时迁移操作后，将发生这种情况。|  
|**1109**|ActiveDirectory_DomainService|已更改此目录服务器的 invocationID 属性。 在创建备份时，最高更新序列号如下所示：<br /><br />InvocationID 属性（旧值）：<br /><br />*<GUID>*<br /><br />InvocationID 属性（新值）：<br /><br />*<GUID>*<br /><br />更新序列号：<br /><br />*<number>*<br /><br />在以下情况中，将更改 InvocationID：目录服务器从备份媒体还原，将其配置为托管可写应用程序目录分区，并且在应用虚拟机快照、虚拟机导入操作或实时迁移操作之后已恢复目录服务器。 不应使用虚拟机快照还原虚拟化域控制器。 支持用于还原或回滚 Active Directory 域服务数据库中的内容的方法是：还原使用 Active Directory 域服务感知备份应用程序制作的系统状态备份。”|  
|**2179**|ActiveDirectory_DomainService|域控制器的计算机对象的 msDS-GenerationId 属性已设置为以下参数：<br /><br />GenerationID 属性：<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 Active Directory 域服务初始化复制，以将域控制器保持最新状态。 完成复制后，将会记录事件 2201。|  
|**2201**|ActiveDirectory_DomainService|Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 Active Directory 域服务已完成复制，以将域控制器保持最新状态。|  
|**2185**|ActiveDirectory_DomainService|Active Directory 域服务已停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务。<br /><br />服务名称：<br /><br />DFSR<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 Active Directory 域服务必须在本地 SYSVOL 副本上初始化非权威还原。 执行该操作的方法是：停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务，然后使用相应的注册表项和值启动它来触发还原。 重新启动 FRS 或 DFSR 服务后，将会记录事件 2187。|  
|**2208**|ActiveDirectory_DomainService|在非权威恢复期间，Active Directory 域服务删除了 DFSR 数据库，以初始化 SYSVOL 副本。<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 Active Directory 域服务需要在本地 SYSVOL 副本上初始化非权威还原。 对于 DFSR 而言，这可以通过停止 DFSR 服务、删除 DFSR 数据库并重启该服务来完成。 在重新启动 DFSR 将重建数据库并启动初始同步。"|  
|**2187**|ActiveDirectory_DomainService|Active Directory 域服务已启动用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务。<br /><br />服务名称：<br /><br />DFSR<br /><br />Active Directory 检测到托管域控制器的虚拟机已恢复为以前的状态。 Active Directory 域服务需要在本地 SYSVOL 副本上初始化非权威还原。 完成该操作的方法是：停止用于复制 SYSVOL 文件夹的 FRS 或 DFSR 服务，然后使用相应的注册表项和值启动它来触发还原。 "|  
|**1587**|ActiveDirectory_DomainService|此目录服务已经还原或配置为承载应用程序目录分区。 因此，其复制标识已经更改。 伙伴已经请求使用旧标识进行复制更改。 开始次序号已经调整。<br /><br />与下列对象 GUID 相应的目标目录服务已经请求开始于某 USN 的更改，该 USN 先于本地目录服务从备份媒体还原的 USN。<br /><br />对象 GUID：<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />还原时的 USN：<br /><br />*<number>*<br /><br />因此，已使用以下设置配置目标目录服务的最新程度矢量。<br /><br />上一个数据库 GUID：<br /><br />*<GUID>*<br /><br />上一个对象 USN：<br /><br />*<number>*<br /><br />上一个属性 USN：<br /><br />*<number>*<br /><br />新数据库 GUID：<br /><br />*<GUID>*<br /><br />新对象 USN：<br /><br />*<number>*<br /><br />新属性 USN：<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>系统事件日志  
系统事件日志记录将脱机的虚拟机恢复到联机状态并与主机时间同步时所显示的计算机时间。 RID 池将无效，而且 DFSR 或 FRS 服务将重启。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**1**|Kernel-General|系统时间已更改为 *？<now>* 从 *< 快照时间/日期 >*。<br /><br />更改原因：一个应用程序或系统组件更改了时间。|  
|**16654**|Directory-Services-SAM|帐户标识符 (RID) 池已失效。 在以下预期情况下，可能会出现该问题：<br /><br />1.通过备份还原域控制器。<br /><br />2.通过快照还原在虚拟机上运行的域控制器。<br /><br />3.管理员已手动使池失效。<br /><br />有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=226247。|  
|**7036**|服务控制管理器|“DFS 复制”服务已进入停止状态。|  
|**7036**|服务控制管理器|“DFS 复制”服务已进入运行状态。|  
  
##### <a name="application-event-log"></a>应用程序事件日志  
应用程序事件日志记录 DFSR 数据库停止和启动。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**103**|ESENT|Dfsr (1360) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db:数据库引擎已停止实例 (0)。<br /><br />异常关闭：0<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.141、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.016、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。|  
|**102**|ESENT|Dfsr (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db:数据库引擎 (6.02.8189.0000) 正在启动新实例 (0)。|  
|**105**|ESENT|Dfsr (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db:数据库引擎已启动新实例 (0)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.000。|  
|||Dfsr (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db:数据库引擎创建新的数据库 (1， \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.016、[4] 0.062、[5] 0.000、[6] 0.016、[7] 0.000、[8] 0.000、[9] 0.015、[10] 0.000、[11] 0.000。|  
  
##### <a name="dfs-replication-event-log"></a>DFS 复制事件日志  
停止 DFSR 服务并删除包含 SYSVOL 的数据库，这会强制执行非权威同步入站。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**1006**|DFSR|DFS 复制服务正在停止。|  
|**1008**|DFSR|DFS 复制服务已停止。|  
|**1002**|DFSR|DFS 复制服务正在启动。|  
|**1004**|DFSR|DFS 复制服务已启动。|  
|**1314**|DFSR|DFS 复制服务已成功配置调试日志文件。<br /><br />其他信息：<br /><br />调试日志文件路径：C:\Windows\debug|  
|**6102**|DFSR|DFS 复制服务已成功注册 WMI 提供程序。|  
|**1206**|DFSR|DFS 复制服务已成功联系的域控制器*<domain controller FQDN>* 以访问配置信息。|  
|**1210**|DFSR|DFS 复制服务为传入的复制请求成功地设置了 RPC 侦听程序。<br /><br />其他信息：<br /><br />端口：0|  
|**4614**|DFSR|DFS 复制服务在本地路径 C:\Windows\SYSVOL\domain 中初始化了 SYSVOL，并且正在等待执行初始复制。 所复制的文件夹将保留在初始同步状态，直到它已通过其伙伴进行复制。 如果该服务器正在升级到域控制器，则在解决此问题之前，该域控制器将不会进行播发，并且不会发挥如同域控制器的作用。 如果指定的伙伴也同样处于初始同步状态，或者如果在此服务器或同步伙伴遇到共享冲突，则可能发生这种情况。 如果在从文件复制服务 (FRS) 到 DFS 复制的 SYSVOL 迁移过程中遇到此事件，则在解决此问题之前，将不会复制更改。 这可能会导致此服务器上的 SYSVOL 文件夹与其他域控制器不同步。<br /><br />其他信息：<br /><br />已复制的文件夹名称：SYSVOL 共享<br /><br />已复制的文件夹 ID: *<GUID>*<br /><br />复制组名称：域系统卷<br /><br />复制组 ID: *<GUID>*<br /><br />成员 ID: *<GUID>*<br /><br />只读：0|  
|**4604**|DFSR|DFS 复制服务已成功初始化位于本地路径 C:\Windows\SYSVOL\domain 上的 SYSVOL 已复制文件夹。 此成员已完成 SYSVOL 和合作伙伴 dc1.corp.contoso.com 的初始同步。  若要检查 SYSVOL 共享是否存在，请打开命令提示符窗口，然后键入“net share”。<br /><br />其他信息：<br /><br />已复制的文件夹名称：SYSVOL 共享<br /><br />已复制的文件夹 ID: *<GUID>*<br /><br />复制组名称：域系统卷<br /><br />复制组 ID: *<GUID>*<br /><br />成员 ID: *<GUID>*<br /><br />同步伙伴： *<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>还原使用 FRS 复制 SYSVOL 的域控制器  
在这种情况下，将使用文件复制事件日志而不是 DFSR 事件日志。 应用程序事件日志还将写入其他与 FRS 相关的事件。 否则，一般情况下目录服务和系统事件日志消息是一样的，并采用相同的顺序（如上所述）。  
  
##### <a name="file-replication-service-event-log"></a>文件复制服务事件日志  
停止并使用 D2 BURFLAGS 值重启 FRS 服务以非权威地同步 SYSVOL。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**13502**|NTFRS|文件复制服务正在停止。|  
|**13503**|NTFRS|文件复制服务已停止。|  
|**13501**|NTFRS|文件复制服务正在启动|  
|**13512**|NTFRS|文件复制服务在计算机 DC4 上包含目录 c:\windows\ntfrs\jet 的驱动器中检测到一个已启用的磁盘写入高速缓存。 当驱动器的电源中断并且关键更新丢失时，文件复制服务可能无法恢复。|  
|**13565**|NTFRS|文件复制服务使用另一域控制器的数据初始化系统卷。 在完成此过程之前，计算机 DC4 将无法成为域控制器。 然后，系统卷将共享为 SYSVOL。<br /><br />若要检查 SYSVOL 共享，请在命令提示符下键入：<br /><br />net share<br /><br />文件复制服务完成初始化过程后，将显示 SYSVOL 共享。<br /><br />系统卷的初始化过程可能需要一些时间。 所需时间取决于系统卷中的数据量、其他域控制器的可用性和域控制器之间的复制间隔。|  
|**13520**|NTFRS|文件复制服务中移动已存在的文件*<path>* 到*<path>* \NtFrs_PreExisting___See_EventLog。<br /><br />文件复制服务可能会删除中的文件 *<path>* 随时 \NtFrs_PreExisting___See_EventLog。 可以通过复制出被删除保存文件*<path>* \NtFrs_PreExisting___See_EventLog。 将文件复制到*<path>* 如果其他复制伙伴上已存在的文件可能会导致名称冲突。<br /><br />在某些情况下，文件复制服务可能会将文件从*<path>* \NtFrs_PreExisting___See_EventLog 到*<path>* 而不是从某个其他复制文件复制伙伴。<br /><br />通过删除中的文件，可以随时恢复空间*<path>* \NtFrs_PreExisting___See_EventLog。|  
|**13553**|NTFRS|文件复制服务已成功地将此计算机添加到以下副本集：<br /><br />“DOMAIN SYSTEM VOLUME (SYSVOL SHARE)”<br /><br />与此事件相关的信息如下所示：<br /><br />计算机 DNS 名称是"*<domain controller FQDN>*"<br /><br />副本集成员名称是"*<domain controller name>*"<br /><br />副本集根路径是"*<path>*"<br /><br />复本分段目录路径是"*<path>* "<br /><br />复本工作目录路径是"*<path>*"|  
|**13554**|NTFRS|文件复制服务已将以下所示连接成功添加到副本集中：<br /><br />“DOMAIN SYSTEM VOLUME (SYSVOL SHARE)”<br /><br />从入站"*<partner domain controller FQDN>*"<br /><br />出站到"*<partner domain controller FQDN>*"<br /><br />后续的事件日志消息可能会显示详细信息。|  
|**13516**|NTFRS|文件复制服务不再阻止计算机 DC4 成为域控制器。 已成功初始化系统卷，并已通知 Netlogon 服务现在可以将系统卷共享为 SYSVOL。<br /><br />键入“net share”检查 SYSVOL 共享。|  
  
##### <a name="application-event-log"></a>应用程序事件日志  
FRS 数据库停止并启动，并因为 D2 BURFLAGS 操作而清除。  
  
||||  
|-|-|-|  
|**事件 ID**|**源**|**Message**|  
|**327**|ESENT|ntfrs (1424) 数据库引擎已分离数据库 (1, c:\windows\ntfrs\jet\ntfrs.jdb)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.015、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.516、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.063、[12] 0.000。<br /><br />恢复的缓存：0|  
|**103**|ESENT|ntfrs (1424) 数据库引擎已停止实例 (0)。<br /><br />异常关闭：0<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.016、[12] 0.000、[13] 0.000、[14] 0.047、[15] 0.000。|  
|**102**|ESENT|ntfrs (3000) 数据库引擎 (6.02.8189.0000) 正在启动新实例 (0)。|  
|**105**|ESENT|ntfrs (3000) 数据库引擎已启动新实例 (0)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.062、[10] 0.000、[11] 0.141。|  
|**103**|ESENT|ntfrs (3000) 数据库引擎已停止实例 (0)。<br /><br />异常关闭：0<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000、[13] 0.015、[14] 0.000、[15] 0.000。|  
|**102**|ESENT|ntfrs (3000) 数据库引擎 (6.02.8189.0000) 正在启动新实例 (0)。|  
|**105**|ESENT|ntfrs (3000) 数据库引擎已启动新实例 (0)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.078、[10] 0.000、[11] 0.109。|  
|**325**|ESENT|ntfrs (3000) 数据库引擎已创建新数据库 (1, c:\windows\ntfrs\jet\ntfrs.jdb)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.016、[4] 0.016、[5] 0.000、[6] 0.015、[7] 0.000、[8] 0.000、[9] 0.078、[10] 0.016、[11] 0.000。|  
|**103**|ESENT|ntfrs (3000) 数据库引擎已停止实例 (0)。<br /><br />异常关闭：0<br /><br />内部计时序列：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.078、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.125、[10] 0.016、[11] 0.000、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。|  
|**102**|ESENT|ntfrs (3000) 数据库引擎 (6.02.8189.0000) 正在启动新实例 (0)。|  
|**105**|ESENT|ntfrs (3000) 数据库引擎已启动新实例 (0)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.016、[2] 0.000、[3] 0.000、[4] 0.094、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.032、[10] 0.000、[11] 0.000。|  
|**326**|ESENT|ntfrs (3000) 数据库引擎已附加数据库 (1, c:\windows\ntfrs\jet\ntfrs.jdb)。 （时间 = 0 秒）<br /><br />内部计时序列：[1] 0.000、[2] 0.015、[3] 0.000、[4] 0.000、[5] 0.016、[6] 0.015、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000。<br /><br />已保存的缓存：1|  
  


