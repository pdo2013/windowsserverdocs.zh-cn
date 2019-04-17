---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: "解决了域控制器部署"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 254209b0da6a7bc0cd5f3880e14a7e5b16822cdd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-domain-controller-deployment"></a>解决了域控制器部署

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍详细的方法，疑难解答，域控制器配置和部署。  
  
-   [故障排除简介](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Intro)  
  
-   [故障排除的选项](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Options)  
  
## <a name="BKMK_Intro"></a>故障排除简介  
![故障排除](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  
  
## <a name="BKMK_Options"></a>故障排除的选项  
  
### <a name="logging-options"></a>登录选项  
内置的日志，则使用域控制器升级和降级解决问题的最重要的工具。 所有这些日志已启用，并且默认情况下配置最大的详细级别。  
  
|阶段|日志|  
|---------|-------|  
|服务器管理器或 ADDSDeployment Windows PowerShell 操作|-%systemroot%\debug\dcpromoui.log<br /><br />-%systemroot%\debug\dcpromoui*.log|  
|域控制器安装/提升|-%systemroot%\debug\dcpromo.log<br /><br />-%systemroot%\debug\dcpromo*.log<br /><br />-事件 viewer\Windows logs\System<br /><br />-事件 viewer\Windows logs\Application<br /><br />-事件 viewer\Applications 和服务 logs\Directory 服务<br /><br />-事件 viewer\Applications 和服务 logs\File 复制服务<br /><br />-事件 viewer\Applications 和服务 logs\DFS 复制|  
|森林或域升级|-%systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|服务器管理器 ADDSDeployment Windows PowerShell 部署引擎|-事件 viewer\Applications 和服务 logs\ Microsoft \Windows\DirectoryServices-Deployment\Operational|  
|Windows 维护服务|-%systemroot%\Logs\CBS\\*<br /><br />-%systemroot%\servicing\sessions\sessions.xml<br /><br />-%systemroot%\winsxs\poqexec.log<br /><br />-%systemroot%\winsxs\pending.xml|  
  
### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>用于域控制器配置疑难解答工具和命令  
若要解决无法通过日志介绍此内容的问题，请使用以下工具作为起始点：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx)，任务管理器和 MSInfo32.exe  
  
-   [网络监视器 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865)（或第三方网络捕获和分析工具）  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>常规方法以获取疑难解答域控制器配置  
  
1.  简单语法问题是否导致错误？  
  
    1.  所做的那样你键入了错误，或忘记提供 ADDSDeployment Windows PowerShell 参数？ 例如，如果使用 ADDSDeployment Windows PowerShell，是否忘了添加所需的参数**-域名**具有有效的名称？  
  
    2.  检查 Windows PowerShell 主机输出仔细查看完全它失败的原因分析提供的命令行。  
  
2.  错误是否先决条件失败？  
  
    1.  现在，由先决条件检查不允许许多用来与致命推广结果中显示的错误。  
  
    2.  先决条件错误的文本仔细检查，他们提供解决大多数问题，需要指南与它们控股的方案。  
  
3.  是否在升级，因此严重错误？  
  
    1.  仔细检查结果：许多错误都有例如错误的密码、网络名称分辨率，或者离线状态下的关键域控制器简单说明。  
  
    2.  检查一段 Dcpromoui.log 和 dcpromo.log 的输出中，在显示的错误然后工作后反汇编从它们以查看发生故障的原因的迹象。  
  
        1.  始终比较到工作示例日志  
  
        2.  仅当结果指示扩展架构或准备森林或域的问题，请检查错误 ADPrep 的日志。  
  
        3.  仅当 Dcpromoui.log 缺少详细信息，或者随意结束由于未处理的异常配置过程中，请检查 DirectoryServices 部署事件日志中的错误。  
  
    3.  检查目录服务、系统和应用程序事件日志中的其他配置问题的指示方向移动即可。 通常情况下，域控制器升级是只需会影响所有分布式的系统其他网络错误配置的症状。  
  
    4.  使用 dcdiag.exe 和 repadmin.exe 以验证整体森林健康和指示可能会阻止进一步域控制器升级的细微错误配置的信息。  
  
    5.  使用 AutoRuns.exe、任务管理器或 MSinfo32.exe 检查计算机存在可能会影响的第三方软件。  
  
        1.  删除第三方软件（不只是禁用该软件;，但仍可加载驱动程序）。  
  
    6.  推广以及复制合作伙伴域控制器和分析与个双面网络捕获升级过程中的无法正常工作的计算机上安装网络监视器 3.4。  
  
        1.  这与你的工作实验环境，以了解健康推广看起来像和了发生故障。  
  
        2.  此时，错误很可能会与森林对象、非默认安全更改或网络，并且此新域控制器在 DNS、防火墙、主机入侵保护软件、或其他的外部因素错误配置的受害者。  
  
### <a name="troubleshooting-specific-problems"></a>具体问题疑难解答  
  
#### <a name="events-and-error-messages"></a>事件和错误消息  
域控制器升级和始终降级返回结尾的操作以及与不同的大多数程序中，而非退货零成功，请执行代码。 若要查看结尾的域控制器配置代码，你有多个选项：  
  
1.  当使用服务器管理器，检查自动重新启动前的 10 秒钟推广结果。  
  
2.  当使用 ADDSDeployment Windows PowerShell，检查自动重新启动前的 10 秒钟推广结果。 或者，选择不在完成的自动重新启动。 你应当添加**格式列表**管道以使更轻松地阅读输出。 例如：  
  
    ```  
    Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  
  
    ```  
  
    先决条件验证和验证中的错误不要继续保持重启，以便在所有的情况下中可见。 例如：  
  
  ![故障排除](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  
  
3.  在任何方案中，检查 dcpromo.log 和 dcpromoui.log。  
  
    > [!NOTE]  
    > 下面列出了错误的一些不再可能在更高版本操作系统的操作系统和域控制器配置更改由于。 新的 ADDSDeployment Windows PowerShell 代码还会阻止某些错误，但 dcpromo.exe//unattend 不;这是切换到 ADDSDeployment Windows PowerShell 过时 DCPromo 来自你当前自动化所有有说服力的另一个原因。  
  
升级和降级返回以下成功邮件的代码。  
  
|错误代码|说明|注意|  
|--------------|---------------|--------|  
|1|退出成功|你仍必须重新启动，删除了自动重启标志此只需笔记|  
|2|退出时，成功，需要重新启动||  
|3|退出时，成功，与非关键失败|通常看到时退回 DNS 委派警告。 如果没有配置 DNS 委派，使用：<br /><br />-creatednsdelegation: $false|  
|4|退出时，成功，与非关键失败，需要重新启动|通常看到时退回 DNS 委派警告。 如果没有配置 DNS 委派，使用：<br /><br />-creatednsdelegation: $false|  
  
升级和降级返回以下故障邮件的代码。 另外，还有，可能需要扩展的错误消息。始终整个错误仔细阅读，而不仅仅是数字部分。  
  
|错误代码|说明|推荐的分辨率|  
|--------------|---------------|------------------------|  
|11|已运行的域控制器升级|不要运行比域控制器在同一时间为相同的目标计算机的升级的一个实例|  
|12|用户必须是管理员|作为的内置管理员进行分组，并确保你具有 UAC，提升成员登录|  
|13|在安装证书颁发机构|你无法降级此域控制器，因为这种鼠类还证书颁发机构。 请勿不删除 CA 之前，如果它发出仔细库存其用法的证书，删除角色，将导致中断。 不要域控制器上运行 Ca|  
|14|安全启动的方式运行|启动到正常模式服务器|  
|15|角色更改正在进行，或需要重新启动|你必须重新启动之前推广服务器（因以前的配置更改）|  
|16|错误平台上运行|*不可能会收到此错误*|  
|17|没有 NTFS 5 驱动器存在|此错误不能在 Windows Server 2012、至少需要 %系统驱动器 %ntfs 格式|  
|18|在 windir 没有足够的空间|使用 cleanmgr.exe %系统驱动器 %卷上释放空间|  
|19|名称更改挂起，需要重新启动|重新启动服务器|  
|20|是语法无效的计算机名称|具有有效的名称将计算机重命名|  
|21|此域控制器保存域控制器、是 GC、和/或是一个 DNS 服务器|添加**-demoteoperationmasterrole**时使用**-forceremoval**。|  
|22|TCP/IP 需要安装或不起作用|验证计算机了 TCP/IP 配置，绑定，并且能够正常工作|  
|23|需要先配置 DNS 客户端|设置主 DNS 服务器时到某个域中添加新的域控制器|  
|24|提供的凭据是无效或缺失需要的元素|验证你的用户名和密码是正确|  
|25|找不到指定域的域控制器|验证 DNS 客户端设置，规则防火墙|  
|26|无法从森林读取域列表|验证 DNS 客户端设置、LDAP 功能、防火墙规则|  
|27|缺少域名|指定域升级或降级时|  
|28|错误域名|当提升选择不同的有效 DNS 域名称|  
|29|家长域不存在|验证时创建一个新孩子域或树域指定的家长域|  
|30|森林中并非域|验证域名提供|  
|31|孩子域中已存在|指定不同域名|  
|32|错误 NetBIOS 域名|指定有效 NetBIOS 域名|  
|33|IFM 文件路径无效|验证你的安装介质文件夹路径|  
|34|IFM 数据库已损坏|使用正确安装从媒体此操作系统和角色（类型都相同的域控制器-RODC 而不是 RWDC 相同的操作系统版本）|  
|35|缺少 SYSKEY|从媒体安装已加密并且你必须提供有效的 SYSKEY 使用它|  
|37|路径 NTDS 数据库或其日志无效|更改为解决的 ntfs、不映射的驱动器或 UNC 路径路径数据库和日志|  
|38|音量没有足够的空间用于 NTDS 数据库或日志|释放空间使用 cleanmgr.exe 增加磁盘空间，手动通过其他地方移动不必要的数据来清除空间|  
|39|路径 SYSVOL 无效|更改为解决的 ntfs、不映射的驱动器或 UNC 路径路径 SYSVOL 文件夹|  
|40|无效的网站的名称|提供存在站点名称|  
|41|需要指定密码的安全模式|提供 DSRM 帐户的密码，无法空白无论密码策略如何配置|  
|42|安全模式密码不符合条件（仅提升）|提供符合密码策略配置的规则 DSRM 帐户密码|  
|43|管理员密码不符合条件（仅降级）|提供符合密码策略的本地管理员帐户的密码配置规则|  
|44|林指定的名称无效。|指定有效森林根 DNS 域名称|  
|45|与指定的名称树林中已存在|选择不同的森林根 DNS 域名称|  
|46|树指定的名称无效。|指定有效树 DNS 域名称|  
|47|树中指定的名称已存在|选择不同的树 DNS 域名称|  
|48|森林结构无法容纳树名称|选择不同的树 DNS 域名称|  
|49|指定的域不存在|验证你键入的域名|  
|50|降，在最后一个域控制器检测到即使不可，或指定最后一个域控制器，但它不是|不指定**域中的最后一个域控制器**(**-lastdomaincontrollerindomain**) 除非它是如此。 使用**-ignorelastdcindomainmismatch**覆盖如果这是第真正最后域控制器，并且虚拟域控制器元数据|  
|51|存在此域控制器上的应用分区|指定到**删除应用程序分区**(**-removeapplicationpartitions**)|  
|52|缺少必要的命令行参数（，即答案文件必须指定命令行上）|*仅 dcpromo//unattend，它将不再支持与看到。 请参阅较旧的文档*|  
|53|推广/降级失败，必须重新启动计算机才能清理|检查扩展的错误和日志|  
|54|升级月降级失败|检查扩展的错误和日志|  
|55|推广降级用户已取消|检查扩展的错误和日志|  
|56|推广降级用户已取消，必须重新启动计算机清理|检查扩展的错误和日志|  
|58|必须在 RODC 升级过程中指定站点名称|你必须为 RODC 指定站点，它不会自动检测类似 RWDC|  
|59|在降，此域控制器是它的一个区域的最后一个 DNS 服务器|这是指定**域中的最后一个 DNS 服务器**或使用**-ignorelastdnsserverfordomain**|  
|60|运行 Windows Server 2008 或更高版本的域控制器必须以便推广 RODC 的域中|推广至少一个 Windows Server 2008 或更高版本的模型可写的域控制器|  
|61|你无法安装与现有不宿主 DNS 域中的 DNS Active Directory 域服务|*不能收到此错误*|  
|62|没有 [DCInstall] 部分答案文件。|*仅 dcpromo//unattend，它将不再支持与看到。 请参阅较旧的文档。*|  
|63|Windows server 2003 下面是功能级别的森林|至少提升到森林功能级别 Windows Server 2003 本机。 Windows 2000 和 Windows NT 4.0 不再受支持的操作系统|  
|64|促销失败，因为组件二进制检测失败|安装广告 DS 角色|  
|65|促销失败，因为失败组件二进制安装|安装广告 DS 角色|  
|66|促销失败，因为操作系统检测失败|检查扩展的错误和日志;服务器无法返回操作系统版本。 它是可能计算机将需要重新安装，因为其整体的运行状况高度可疑|  
|68|复制合作伙伴无效。|使用 repadmin.exe 或**获取-ADReplication\ *** Windows PowerShell 验证合作伙伴域控制器健康|  
|69|所需的端口已在另一个应用程序通过使用|使用**netstat.exe anob**定位错误赋予保留广告 DS 端口的进程|  
|70|必须 GC 森林根域控制器。|*仅 dcpromo//unattend，它将不再支持与看到。 请参阅较旧的文档*|  
|71|DNS 服务器已安装|不指定安装 DNS (**-installDNS**) 如果已安装 DNS 服务|  
|72|计算机运行的非管理员模式中远程桌面服务|你无法推广此域控制器，因为它也是 RDS 服务器为两个以上管理员用户配置。 不要的移除 RDS 仔细库存其用法-如果正在使用的应用程序或最终用户、删除之前将导致中断|  
|73|指定的森林功能级别无效。|指定有效森林功能级别|  
|74|指定的域功能级别无效。|指定有效的域的功能级别|  
|75|无法确定默认密码复制策略。|验证 RODC 密码复制策略存在，并且可以访问|  
|76|指定复制/非复制安全组是无效|验证有效的域和用户帐户中指定密码复制策略时键入|  
|77|指定的参数无效|检查扩展的错误和日志|  
|78|若要检查 Active Directory 森林而失败|检查扩展的错误和日志|  
|79|不能升级 RODC，因为尚未执行 rodcprep|使用 Windows Server 2012 准备林或使用**adprep.exe /rodcprep**|  
|80|尚未执行域|使用 Windows Server 2012 准备域，或者使用**adprep.exe /domainprep**|  
|81|尚未执行 Forestprep|使用 Windows Server 2012 准备林或使用**adprep.exe /forestprep 成功**|  
|82|森林架构不匹配|使用 Windows Server 2012 准备林或使用**adprep.exe /forestprep 成功**|  
|83|不受支持的 SKU|*不可能会收到此错误*|  
|84|无法检测到域控制器帐户|验证现有域控制器已经设置了正确的用户帐户控制属性。|  
|85|无法选择阶段 2 域控制器帐户|如果你指定"使用现有帐户"，但找到任一没有帐户，或者在帐户查找期间时出现错误返回。 请确保你提供了正确的 RODC 转移帐户|  
|86|需要运行阶段 2 提升|返回如果推广额外的域控制器，但存在现有帐户，并且未指定"允许重新安装"|  
|87|域控制器冲突类型的帐户存在|重命名升级，如果不是试图附加到未占用的域控制器之前计算机。 你必须将其附加到未占用的域控制器帐户使用**-useexistingaccount**和正确仅读或写参数，具体取决于帐户类型|  
|88|指定的服务器管理员无效|指定 RODC 管理员委派无效的帐户。 验证有效的用户或组指定的帐户|  
|89|指定域去掉的主机处于离线状态。|使用**netdom.exe 查询 fsmo**检测 RID 主机。 联机，使其在可以访问您提升了域控制器|  
|90|域名主机处于离线状态。|使用**netdom.exe 查询 fsmo**检测域名主机。 联机，使其在可以访问您提升了域控制器|  
|91|未检测到进程是否 wow64|*不能收到此错误不再操作系统都是 64 位*|  
|92|不受支持 Wow64 过程|*不能收到此错误不再操作系统都是 64 位*|  
|93|域控制器服务非说服力降级没有运行|启动广告 DS 服务|  
|94|本地管理员密码不符合要求：为空或不需要|提供非空密码，并确保本地密码策略需要密码|  
|95|不能降级最后一个 Windows Server 2008 或更高版本的域控制器动态 Rodc 其中存在的域中|所有 Windows Server 2008 或更高版本可写的域控制器才能降级必须先将降级所有 Rodc|  
|96|无法卸载 DS 二进制文件|*仅 dcpromo//unattend，它将不再支持与看到。 请参阅较旧的文档*|  
|97|森林功能级别版本高于孩子域操作系统|提供孩子域功能同一或高于森林功能级别|  
|98|正在组件二进制安装/卸载。|*仅 dcpromo//unattend，它将不再支持与看到。 请参阅较旧的文档*|  
|99|森林功能级别电量过低（错误是 Windows Server 2012 仅）|至少提升到森林功能级别 Windows Server 2003 本机。 Windows 2000 和 Windows NT 4.0 不再受支持的操作系统|  
|100|域功能级别电量过低（错误是 Windows Server 2012 仅）|至少提升到域功能级别 Windows Server 2003 本机。 Windows 2000 和 Windows NT 4.0 不再受支持的操作系统|  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>/ 可能已知问题以及支持的方案  
下面是在 Windows Server 2012 开发过程中看到的常见问题。 所有这些问题会"通过设计"，并且具有有效的解决方法或更适合用于首先避免这些的技术。 这些行为的许多都是相同在 Windows Server 2008 R2 和较旧操作系统，但广告 DS 部署重写带来增强的敏感度的问题。  
  
|问题|降级域控制器离开 DNS 运行了没有区域|  
|---------|-----------------------------------------------------------------|  
|症状|服务器仍对 DNS 请求的响应，但没有区域的信息|  
|分辨率和笔记|删除广告 DS 角色时, 还删除了 DNS 服务器角色或设置为已禁用 DNS 服务器服务。 请记住指向比本身另一台服务器 DNS 客户端。 如果使用的 Windows PowerShell，运行以下命令降级服务器后：<br /><br />代码-卸载 windowsfeature dns<br /><br />或<br /><br />代码-组服务 dns-启动类型禁用<br />停止服务 dns|  
  
|问题|升级到现有单标签域 Windows Server 2012 不配置 updatetopleveldomain = 1 或 allowsinglelabeldnsdomain = 1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|症状|不会发生的记录动态 DNS 注册|  
|分辨率和笔记|将这些值使用 Netlogon 和 DNS 组策略设置。 Microsoft 开始阻止单标签域创建了 Windows Server 2008;你可以使用 ADMT 或域重命名工具若要更改对批准 DNS 域结构。|  
  
|问题|如果有预先创建、未占用 RODC 帐户域中的最后一个域控制器降级无法正常工作|  
|---------|------------------------------------------------------------------------------------------------------------|  
|症状|降级失败并消息：<br /><br />**Dcpromo.General.54**<br /><br />Active Directory 域服务找不到另一个 Active Directory 域控制器传输目录分区 CN 中的其余数据 = 方案，CN = 配置，直流 = corp，直流 = contoso，直流 = com。<br /><br />"指定的域名称的格式无效。"|  
|分辨率和笔记|删除任何剩余预在降级某个域中之前, 创建了 RODC 帐户使用**Dsa.msc**或**Ntdsutil.exe 元数据清理**。|  
  
|问题|自动的林和域准备不运行 GPPREP|  
|---------|---------------------------------------------------------------|  
|症状|组策略，结果设置的策略 (RSOP) 规划模式，跨域计划功能需要现有 GP 系统更新的文件和 Active Directory 的权限。 没有 Gpprep，不能使用 RSOP 规划跨域。|  
|分辨率和笔记|运行**adprep.exe /gpprep**手动为所有之前未 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的域。 管理员应某个域，不能与每次升级的历史记录中运行一次只能 GPPrep。 它不是运行通过自动 adprep，因为如果你已拥有设置足够自定义的权限，这将导致重新复制所有域控制器上的所有 SYSVOL 内容。|  
  
|问题|安装媒体中的无法正常工作时指向 unc 验证|  
|---------|------------------------------------------------------------------|  
|症状|返回错误：<br /><br />代码-无法验证 media 路径。 使用"2"参数呼叫"GetDatabaseInfo"例外。 无效的文件夹。|  
|分辨率和笔记|你必须 IFM 文件存储在本地磁盘，而不是远程 UNC 路径。 此有意阻止阻止部分服务器推广，由于网络中断。|  
  
|问题|显示两次在域控制器升级过程中的 DNS 委派警告|  
|---------|-------------------------------------------------------------------------|  
|症状|返回警告*两次*提升使用 ADDSDeployment Windows PowerShell 时：<br /><br />代码-"无法创建一个委派此 DNS 服务器，因为权威父区域找不到或不运行 Windows DNS 服务器。 如果你要集成现有 DNS 基础结构，应家长区域，以确保可靠名称分辨率从之外域中手动创建委派给此 DNS 服务器。 否则，不不需要任何操作。"|  
|分辨率和笔记|忽略。 ADDSDeployment Windows PowerShell 显示警告首先期间先决条件检查，然后再次期间域控制器配置。 如果你不希望配置 DNS 委派，使用参数：<br /><br />代码--creatednsdelegation: $false<br /><br />不要*不*取消该消息以跳先决条件检查|  
  
|问题|配置过程指定 UPN 或非域凭据返回误导错误|  
|---------|--------------------------------------------------------------------------------------------|  
|症状|服务器管理器返回错误：<br /><br />代码-呼叫"DNSOption"与"6"参数的例外<br /><br />ADDSDeployment Windows PowerShell 返回错误：<br /><br />代码-的用户权限验证失败。 你必须提供属于该用户帐户的域的名称。|  
|分辨率和笔记|确保你提供有效的域的形式的凭据**域名 \**。|  
  
|问题|删除使用 Dism.exe DirectoryServices 域控制器角色，导致无法启动服务器|  
|---------|---------------------------------------------------------------------------------------------------|  
|症状|如果使用 Dism.exe 去广告 DS 角色在完美降级域控制器之前，不会再服务器正常启动，并显示错误：<br /><br />代码-状态：0x000000000<br />信息：意外发生错误。|  
|分辨率和笔记|启动会进入目录服务修复模式使用*Shift + F8*。 添加广告 DS 角色重新，然后强制降级域控制器。 或者，从备份中还原系统状态。 请不要将 Dism.exe 用于广告 DS 角色删除;该实用程序也不知道的域控制器。|  
  
|问题|设置 Win2012 到的目录林时，安装新林失败|  
|---------|--------------------------------------------------------------------|  
|症状|使用 ADDSDeployment Windows PowerShell 推广返回错误：<br /><br />代码-Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />先决条件域控制器推广验证失败。 指定的域功能级别无效。|  
|分辨率和笔记|未指定不 Win2012 森林功能模式*也*指定 Win2012 域功能模式。 以下是可没有错误示例：<br /><br />代码--目录林 Win2012-domainmode Win2012]|  
  
|||  
|-|-|  
|问题|单击验证在安装从媒体选择区域看起来不执行任何操作|  
|症状|当你指定 IFM 文件夹的路径时，请单击**验证**按钮永远不会返回一条消息，或者出现做任何事情。|  
|分辨率和笔记|**验证**按钮仅返回错误，如果有问题。 否则，可使**下一步**按钮可选的如果你已提供 IFM 路径。 必须单击**验证**继续如果选择了 IFM。|  
  
|||  
|-|-|  
|问题|降级与服务器管理器中不提供完成前的反馈。|  
|症状|当使用服务器管理器中删除广告 DS 角色和降级域控制器，没有持续反馈提供，直到降级完毕或无法正常工作。|  
|分辨率和笔记|这是限制服务器管理器。 反馈、使用 ADDSDeployment Windows PowerShell cmdlet:<br /><br />代码-Uninstall-addsdomaincontroller|  
  
|||  
|-|-|  
|问题|安装媒体验证从不会检测 RODC 介质提供可写的域控制器，反之亦然。|  
|症状|提升了新的域控制器使用 IFM 和-例如 RODC 可写的域控制器或 RWDC 介质 rodc-到 IFM 提供媒体不正确时验证按钮将不会返回出现错误。 更高版本，升级将失败，错误：<br /><br />代码-时出错尝试此计算机配置为域控制器。 <br />Install-From-Media 提升 Read-Only DC 无法启动，因为不允许指定的源数据库。 仅从其他 Rodc 数据库可用于 RODC IFM 提升。|  
|分辨率和笔记|验证只验证 IFM 整体完整性。 请提供到服务器的错误 IFM 类型。 在你尝试使用正确的媒体再次的升级之前，请重新启动的服务器。|  
  
|||  
|-|-|  
|问题|升级到预创建的计算机帐户 RODC 无法正常工作|  
|症状|当使用 ADDSDeployment Windows PowerShell 推广分阶段的计算机帐户与新 RODC、收到错误：<br /><br />代码-参数设置不能解决使用指定名为参数。    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|分辨率和笔记|请提供已经定义 RODC 预先创建帐户上的已参数。 其中包括：<br /><br />代码--readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-站点名<br />-installdns|  
  
|||  
|-|-|  
|问题|取消选择/选择"自动重新启动目标的每个服务器如有必要"无法执行任何操作|  
|症状|如果选择（或不选择）服务器管理器选项**必要时自动重新启动目标的每个服务器**whendemoting 域控制器角色删除通过，服务器始终重新启动后，无论选择。|  
|分辨率和笔记|这是有意。 降级过程会重新启动无论为此设置的服务器。|  
  
|||  
|-|-|  
|问题|Dcpromo.log 显示"[错误] 服务器文件失败 2 上的安全设置"|  
|症状|域控制器降级完成未出现问题，但 dcpromo 日志检查显示错误：<br /><br />2 失败代码-[错误] 设置服务器文件安全|  
|分辨率和笔记|忽略，可以预期和修饰错误。|  
  
|||  
|-|-|  
|问题|先决条件 adprep 检查失败，错误"无法执行 Exchange 架构冲突检查"|  
|症状|在尝试将 Windows Server 2012 域控制器提升到现有 Windows Server 2003，Windows Server 2008 或 Windows Server 2008 R2 的森林先决条件检查失败，错误：<br /><br />代码-的先决条件广告准备验证失败。 无法为域执行 Exchange 架构冲突检查*<domain name>* (例外：RPC 服务器不可用)<br /><br />Adprep.log 显示错误：<br /><br />代码-Adprep 无法数据从检索服务器*<domain controller>*<br /><br />通过 Windows 管理 Instrumentation (WMI)。|  
|分辨率和笔记|新的域控制器无法访问 WMI 通过对现有的域控制器 DCOM/RPC 协议。 到目前为止，已三个原因：<br /><br />-向现有的域控制器防火墙规则阻止访问<br /><br />网络服务帐户是在"登录即服务"中缺少 (SeServiceLogonRight) 现有的域控制器上的权限<br /><br />-NTLM 禁用域控制器上使用的安全策略，述[引入这限制的种身份验证](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  
  
|||  
|-|-|  
|问题|创建新的广告 DS 森林始终显示 DNS 警告|  
|症状|当创建新的广告 DS 森林并创建新的域控制器上的 DNS 区域为自身，你始终可以收到条警告消息：<br /><br />代码的错误检测到 DNS 配置中。 <br />使用该计算机 DNS 服务器无响应超时间隔内。<br />(错误代码 0x000005B4"ERROR_TIMEOUT")|  
|分辨率和笔记|忽略。 指向现有 DNS 服务器和区域而适用的情况下，此警告是有意根域中的新林，在第一个该域控制器上。|  
  
|||  
|-|-|  
|问题|Windows PowerShell-whatif 参数返回正确 DNS 服务器信息|  
|症状|如果你使用**-whatif**参数配置域控制器隐式或明确时**-installdns: $true**，所生成的输出显示：<br /><br />代码-"DNS 服务器：否"|  
|分辨率和笔记|忽略。 安装并配置无误 DNS。|  
  
|||  
|-|-|  
|问题|升级后, 登录失败，"没有足够的存储空间来处理此命令"|  
|症状|推广新的域控制器，然后注销并尝试登录的交互方式后，你会收到错误消息：<br /><br />代码-没有足够的存储空间来处理此命令|  
|分辨率和笔记|域控制器未在重新启动之前，可能原因错误升级后，或者是因为指定 ADDSDeployment Windows PowerShell 参数**-norebootoncompletion**。 重新启动的域控制器。|  
  
|||  
|-|-|  
|问题|下一步按钮不可用域控制器选项页面上|  
|症状|即使你已设置密码，**下一步**按钮上**域控制器选项**页中服务器管理器中不可用。 中列出的任何站点**站点名称**菜单。|  
|分辨率和笔记|你有多个广告 DS 站点和至少一个缺少子网;此将来域控制器属于一个这些个子网。 你必须手动从网站名称下拉菜单中选择子网。 你还应该查看使用 DSSITE.MSC 或使用以下的 Windows PowerShell 命令，以找到所有站点缺少子网：<br /><br />代码-获取 adreplicationsite-筛选 *-属性子网 & #124;在哪里对象 {！$ 的 _.subnets eq"\ *"} & #124;格式表名称|  
  
|||  
|-|-|  
|问题|推广或降级失败，消息"无法启动服务"|  
|症状|如果你尝试升级，降级，或复制的域控制器你收到错误：<br /><br />代码-服务无法启动，或者因为它是已禁用，或者它有与之关联的设备没有启动"(0x80070422)<br /><br />该错误可能是交互式的一场活动，如 dcpromoui.log 或 dcpromo.log 日志写入或|  
|分辨率和笔记|禁用 DS 角色服务器服务 (DsRoleSvc)。 默认情况下，此服务广告 DS 角色安装期间安装并将设置为手动启动类型。 不要禁用此服务。 将其返回到手册设置，并允许 DS 角色操作，以启动和停止它的需求。 此问题是设计。|  
  
|||  
|-|-|  
|问题|服务器管理器仍将警告你需要推广 DC|  
|症状|如果你提升过时的 dcpromo.exe//unattend 使用域控制器或升级到 Windows Server 2012 的地方中的现有 Windows Server 2008 R2 域控制器，服务器管理器仍然显示部署后配置任务**推广此域控制器服务器**。|  
|分辨率和笔记|单击部署后警告链接，该消息将公益会消失。 这就是修饰和预期。|  
  
|||  
|-|-|  
|问题|缺少角色安装服务器管理器部署脚本|  
|症状|如果您提升使用服务器管理器的域控制器，并保存 Windows PowerShell 部署脚本，它不包含角色安装 cmdlet 和参数 (安装 windowsfeature-命名广告域服务 includemanagementtools)。 不角色，不能配置 DC。|  
|分辨率和笔记|手动添加该 cmdlet 以及参数任何脚本。 这种情况预计和设计。|  
  
|||  
|-|-|  
|问题|未命名 PS1 服务器管理器部署脚本|  
|症状|如果提升使用服务器管理器的域控制器，保存 Windows PowerShell 部署脚本随机的临时名称，而不是 PS1 文件命名该文件。|  
|分辨率和笔记|手动重命名该文件。 这种情况预计和设计。|  
  
|问题|Dcpromo//unattend 允许不受支持的功能级别|  
|-|-|  
|症状|如果你提升了域控制器 dcpromo//unattend 使用下面的示例 answer 文件：<br /><br />代码-<br /><br />[DCInstall]<br />新域 = 森林<br /><br />ReplicaOrNewDomain = 域<br /><br />NewDomainDNSName = corp.contoso.com<br /><br />SafeModeAdminPassword =Safepassword@6<br /><br />DomainNetbiosName = corp<br /><br />DNSOnNetwork = 是<br /><br />AutoConfigDNS = 是<br /><br />RebootOnSuccess = NoAndNoPromptEither<br /><br />RebootOnCompletion = 无<br /><br />*DomainLevel = 0*<br /><br />*ForestLevel = 0*<br /><br />升级将失败，以下中 dcpromoui.log 错误：<br /><br />代码-dcpromoui EA4.5B8 0089 13:31:50.783 Enter CArgumentsSpec::ValidateArgument DomainLevel<br /><br />Dcpromoui EA4.5B8 008A 13:31:50.783 DomainLevel 的值为 0<br /><br />Dcpromoui EA4.5B8 008B 13:31:50.783 退出代码是 77<br /><br />Dcpromoui EA4.5B8 008 C 13:31:50.783 指定参数无效。<br /><br />Dcpromoui EA4.5B8 008 D 13:31:50.783 结束日志<br /><br />Dcpromoui EA4.5B8 0032 13:31:50.830 退出代码是 77<br /><br />级别 0 为 Windows 2000，Windows Server 2012 不支持。|  
|分辨率和笔记|不要使用过时的 dcpromo//unattend 并了解，它允许你指定无效设置的更高版本失败。 这种情况预计和设计。|  

|问题|推广创建 NTDS 设置对象，在"挂起"无法完成|  
|-|-|  
|症状|推广副本 DC 或 RODC，如果到达"创建 NTDS 设置对象"永远不会继续或完成升级。 日志停止也更新。|  
|分辨率和笔记|这是一个已知的问题所致内置的本地管理员帐户凭据提供匹配到内置域管理员帐户的密码。 这将不是错误，而是会无限期等待（准循环）酷睿设置 engine 中向下导致失败。 这正常-但不需要的行为。<br /><br />若要解决服务器：<br /><br />1.重新启动它。<br /><br />1.广告中, 删除该服务器成员计算机帐户（不尚未会 DC 帐户）<br /><br />1.服务器上强制 disjoin 它域中<br /><br />1.该在服务器上，删除广告 DS 角色。<br /><br />1.重新启动<br /><br />1.重新添加广告 DS 角色和 reattempt 升级，确保你始终提供***domain\admin***格式化凭据到 DC 推广并不只是内置的本地管理员帐户|  
