---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: "将副本 Windows Server 2012 域控制器安装在现有的域 (级别 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e3151a8beee2870ecc747a64241df9d562ad4cc2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>将副本 Windows Server 2012 域控制器安装在现有的域 (级别 200)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍所必需的现有森林或域升级到 Windows Server 2012、服务器管理器或 Windows PowerShell 使用的步骤。 介绍了如何添加到现有的域中运行 Windows Server 2012 的域控制器。  
  
-   [升级和副本工作流](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [升级和副本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [部署](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>升级和副本工作流  
下图说明 Active Directory 域服务配置过程，当你之前安装的广告 DS 角色，并且已经开始使用现有的域中创建新的域控制器服务器管理器 Active Directory 域服务配置向导。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>升级和副本 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-域名***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-站点名*<br /><br />*-ADPrepCredential*<br /><br />-ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-确认<br /><br />*-CreateDNSDelegation*<br /><br />***-凭据***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-强制<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />-站点名<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> **-凭据**参数才需要如果你未已登录作为企业管理员和方案管理员组（如果你有升级森林）或域管理员组成员（如果你要添加到现有的域的新 DC）。  
  
## <a name="BKMK_Dep"></a>部署  
  
### <a name="deployment-configuration"></a>部署配置  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
服务器管理器开始与每域控制器升级**部署配置**页面。 其他选项和必填的字段在此页和后续页面，具体取决于哪个部署操作你选择的更改。  
  
若要升级了现有的森林或添加到现有的域可写的域控制器，请单击**添加到现有的域的域控制器**单击**选择**到**指定此域域信息**。 服务器管理器会提示你输入有效的凭据根据需要。  
  
升级森林要求包含在 Windows Server 2012 的企业管理员和方案管理员组中的组成员的凭据。 Active Directory 域服务配置向导你稍后当提示你当前的凭据没有足够的权限或组成员。  
  
自动 Adprep 过程是添加到现有的 Windows Server 2012 域和运行更早版本的 Windows Server 域控制器域域控制器仅运营差异。  
  
部署配置 ADDSDeployment cmdlet 和参数是：  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
在每个页面，其中一些重复后面部分尽可能离散先决条件检查执行某些测试。 例如，如果所选的域不满足最低功能的级别，你无需一直通过升级前往先决条件的检查，以找出：  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
**域控制器选项**页上指定的新的域控制器域控制器功能。 配置域控制器功能**DNS 服务器**，**全球目录**，并**只读域控制器**。 Microsoft 建议所有域控制器都提供高可用性分布式环境中的 DNS 和 GC 服务。 GC 默认始终处于选中状态，并 DNS 服务器选择默认情况下，如果当前域主机 DNS 其域控制器上的已开始的颁发机构查询为基础。 **域控制器选项**页面，还可以选择相应的 Active Directory 逻辑**站点名称**从森林配置。 默认情况下，选择最正确子网与该站点。 如果只有一个网站，它将自动选中。  
  
> [!NOTE]  
> 如果该服务器不属于 Active Directory 网，并且多个 Active Directory 站点，选中任何和**下一步**按钮不可用，直到你选择站点从列表。  
  
指定**目录服务还原模式密码**必须遵守密码策略应用到服务器。 始终可以选择复杂的强密码或最好，密码。  
  
**域控制器选项**ADDSDeployment 参数均是：  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 网站的名称必须已经存在，当提供作为的参数**-站点名**。 **安装 AddsDomainController** cmdlet 不会创建的站点。 你可以使用 cmdlet**新 adreplicationsite**创建新的站点。  
  
**SafeModeAdministratorPassword**特别是参数的操作：  
  
-   如果*未指定*作为参数，cmdlet 提示你输入并确认屏蔽的密码。 运行 cmdlet 交互时，这是首选的使用量。  
  
    例如，在 treyresearch.net 域中创建额外的域控制器，力求尽可能系统提示您输入并确认屏蔽的密码：  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
    ```  
  
-   如果指定*值*的值必须安全字符串。 运行 cmdlet 交互时，这不是首选的使用量。  
  
例如，你可以手动提示输入密码使用**读取主机**cmdlet 安全字符串用户提示：  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> 上一个选项没有确认密码，如使用格外小心：的密码不可见。  
  
虽然这是强烈建议不要，你还可以为转换清除文本变量，提供安全的字符串。  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
最后，无法将模糊的密码存储在某个文件，然后重用它更高版本，而清晰的文本密码会显示。 例如：  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> 不建议提供或存储清除或模糊短的密码。 脚本中运行此命令或查找通过您的任何人都知道 DSRM 域控制器的密码。  有权访问文件的任何人都可以反转该模糊的密码。 与该知识，他们可以登录到 DC DSRM 以启动并最终模拟域控制器本身提升最大程度 Active Directory 森林中的权限。 步骤，使用另一组**System.Security.Cryptography**文本的文件进行加密数据是建议，但利用范围。 最佳做法是完全避免密码存储。  
  
ADDSDeployment cmdlet 提供其他选项，可跳过自动配置 DNS 客户端设置、转发器，以及根提示。 使用服务器管理器时，无法跳过此配置选项。 此参数很重要，仅当你安装 DNS 服务器前配置域控制器角色：  
  
```  
-SkipAutoConfigureDNS  
```  
  
**域控制器选项**页面发出警告，如果你现有的域控制器运行 Windows Server 2003，不能创建阅读仅域控制器。 这预期的那样，并且可以消除警告。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 选项和 DNS 委派凭据  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
**DNS 选项**页面使您能够配置 DNS 委派，如果你选择**DNS 服务器**选项卡上*域控制器选项*页面和如果指向的区域 DNS 委派允许的位置。 你可能需要提供其他成员的用户的凭据**DNS 管理员**组。  
  
**DNS 选项**ADDSDeployment cmdlet 参数均是：  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
你需要创建 DNS 委派的详细信息，请参阅[了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
### <a name="additional-options"></a>其他选项  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
**其他选项**页面提供了命名为复制源，域控制器配置选项，也可以使用任何域控制器为复制源。  
  
你还可以选择要域控制器使用安装媒体使用安装媒体 (IFM) 选项从备份。 **安装媒体的**复选框提供选中一次浏览选项，你必须单击**验证**以确保所提供的路径是有效的媒体。 从另一台现有 Windows Server 2012 计算机仅; IFM 选项通过使用媒体创建与 Windows Server 备份或 Ntdsutil.exe 你无法使用 Windows Server 2008 R2 或以前的操作系统创建媒体的 Windows Server 2012 域控制器。 有关 IFM 中的更改的详细信息，请参阅[简体中文管理附录](../../ad-ds/deploy/Simplified-Administration-Appendix.md)。 如果使用 SYSKEY 受保护的媒体、服务器管理器会提示图像的密码在验证过程。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
**其他选项**ADDSDeployment cmdlet 参数均是：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>路径  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
**路径**页使你可以覆盖默认文件夹位置的广告 DS 数据库数据库事务日志中，并 SYSVOL 共享。 始终处于子目录 %系统根 %的默认位置。  
  
Active Directory 路径 ADDSDeployment cmdlet 参数均是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>准备选项  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
**准备选项**页面向你发送警报广告 DS 配置包括扩展架构 (forestprep) 和更新域（域）。  森林和域有尚未准备好以前的 Windows Server 2012 域控制器安装或通过手动运行 Adprep.exe 时，你只看到此页面。 例如，Active Directory 域服务配置向导取消此页面，如果你添加到现有的 Windows Server 2012 森林根域新的域控制器。  
  
在你单击扩展架构和更新域不发生**下一步**。 这些事件发生仅在安装阶段。 此页面只需将显示有关将更高版本中安装发生的事件感知。  
  
此页还验证的当前用户凭据成员的方案管理员和企业版管理员组中，您可以根据需要扩展架构或准备域这些组中的成员。 单击**更改**提供足够的用户的凭据，如果页通知你当前的凭据不会提供足够的权限。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
其他选项 ADDSDeployment cmdlet 参数是：  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> 作为与以前版本的 Windows Server，为运行 Windows Server 2012 的域控制器自动的域准备不运行 GPPREP。 运行**adprep.exe /gpprep**手动为所有之前未 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的域。 在某个域，不能与每次升级的历史记录，应运行一次只能 GPPrep。 不能运行 Adprep.exe /gpprep 自动因为其操作可能导致在 SYSVOL 文件夹中，重新复制所有域控制器上的所有文件和文件夹。  
>   
> 自动 RODCPrep 运行提升某个域中的第一个未分阶段的 RODC 时。 它不会触发推广第一个可写的 Windows Server 2012 域控制器。 你也仍然手动可以**adprep.exe /rodcprep**如果你计划部署只读域控制器。  
  
### <a name="review-options-and-view-script"></a>查看选项并查看脚本  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
**回顾选项**页面，可以让你以验证你的设置并确保它们符合你的要求，开始安装之前。 这不是最后一次机会停止安装使用服务器管理器。 此页只可以进行检查，配置在继续之前，请确认你的设置。  
  
**回顾选项**页面中服务器管理器中还提供了一个可选**查看脚本**按钮来创建 Unicode 文本文件包含当前 ADDSDeployment 配置为单个 Windows PowerShell 脚本。 这使你为 Windows PowerShell 部署 studio 使用服务器管理器图形界面。 使用 Active Directory 域服务配置向导配置选项中, 导出配置，然后取消向导。  此过程中创建进一步修改或直接使用有效和语法正确的示例。  
  
例如：  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "root.fabrikam.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> 服务器管理器通常会填满所有参数与值时升级，不依赖于默认设置（如它们可能会更改之间将来版本的 Windows 或 service pack）中。 是一个例外**-safemodeadministratorpassword**参数。 若要强制确认提示忽略值运行 cmdlet 交互时  
>   
> 使用可选**Whatif**具有参数**安装 ADDSDomainController** cmdlet 查看配置的信息。 这使你查看的参数 cmdlet 明确和隐式值。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件复选  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
**先决条件检查**是广告 DS 域配置中的新增功能。 此新阶段验证域和林可以支持新的 Windows Server 2012 域控制器。  
  
当安装新的域控制器，服务器管理器 Active Directory 域服务配置向导调用一系列序列化模块化测试。 这些测试向你发出警报建议的维修选项。 你可以根据需要多次运行测试。 域控制器过程所有先决条件测试才可继续通过。  
  
**先决条件检查**还面，如会影响较旧操作系统的安全更新的相关信息。  
  
有关特定的先决条件检查的详细信息，请参阅[先决条件检查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
你无法跳过**先决条件检查**时使用服务器管理器中，但你可以跳过此过程时使用使用下面的参数广告 DS 部署 cmdlet:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能会导致部分域控制器升级或损坏广告 DS 森林。  
  
单击**安装**开始域控制器升级过程中。 这是最后一次机会取消安装。 一旦开始后，你无法取消升级过程中。 计算机将自动重新启动提升，无论推广 results.The**先决条件检查**页面显示该过程，用于解决该问题的指南期间遇到任何问题。  
  
### <a name="installation"></a>安装  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
当**安装**页面显示时，域控制器配置开始，无法将停止或取消。 详细的操作显示在此页面上，并且写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log（如果服务器工作组中）  
  
若要安装使用 ADDSDeployment 模块新 Active Directory 森林，使用以下 cmdlet:  
  
```  
Install-addsdomaincontroller  
```  
  
请参阅[升级和副本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)必需和可选参数。  
  
**安装 AddsDomainController** cmdlet 都仅有两个阶段（先决条件检查和安装）。 两个下图显示与的最低要求参数安装阶段**-域名**和**-凭据**。 注意如何 Adprep 操作将自动添加到现有的 Windows Server 2003 森林的第一个 Windows Server 2012 域控制器的一部分：  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
注意如何操作，就像服务器管理器**安装 ADDSDomainController**提醒你，升级将自动重新启动的服务器。 若要自动接受重新引导提示，请使用**-强制**或**-确认：$false**与任何 ADDSDeployment Windows PowerShell cmdlet 参数。 若要防止服务器推广末尾自动重新启动，请使用**-norebootoncompletion**参数。  
  
> [!WARNING]  
> 不建议重重新启动。 域控制器必须重新启动才能正常工作。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
若要配置远程使用 Windows PowerShell 域控制器，换行**安装 adddomaincontroller** cmdlet*内*的**调用命令**cmdlet。 这就需要使用花括号。  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
例如：  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> 有关如何安装并 Adprep 处理工作原理的详细信息，请参阅[疑难解答域控制器部署](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。  
  
### <a name="results"></a>结果  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**结果**页面显示的成功或失败的升级和管理的任何重要信息。 如果成功，域控制器将自动 10 秒钟后重新启动。  
  
作为与以前版本的 Windows Server，为运行 Windows server 2012 的域控制器自动的域准备不运行 GPPREP。 运行**adprep.exe /gpprep**手动为所有之前未 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的域。 在某个域，不能与每次升级的历史记录，应运行一次只能 GPPrep。 不能运行 Adprep.exe /gpprep 自动因为其操作可能导致在 SYSVOL 文件夹中，重新复制所有域控制器上的所有文件和文件夹。  
  

