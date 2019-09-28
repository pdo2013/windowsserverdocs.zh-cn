---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: 在现有域中安装副本 Windows Server 2012 域控制器（级别 200）
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5e72c18d3aa49774cf73d5365748e7bf20764b22
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390845"
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>在现有域中安装副本 Windows Server 2012 域控制器（级别 200）

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍使用服务器管理器或 Windows PowerShell 将现有林或域升级到 Windows Server 2012 的必要步骤。 它介绍如何将运行 Windows Server 2012 的域控制器添加到现有域。  
  
-   [升级和副本工作流](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [升级和副本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [部署](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>升级和副本工作流  
下图阐述了以下情况中的 Active Directory 域服务配置进程：你之前已安装 AD DS 角色，并且已使用服务器管理器启动 Active Directory 域服务配置向导以在现有域中创建新域控制器。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>升级和副本 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|Install-AddsDomainController|-Skipprechecks 不可<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-SiteName*<br /><br />*-ADPrepCredential*<br /><br />-ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-Force<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />-SiteName<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> 仅在你尚未作为 Enterprise Admins 和 Schema Admins 组（如果你要升级该林）或者 Domain Admins 组（如果你要将新 DC 添加到现有域）的成员登录时，需要 **-credential** 参数。  
  
## <a name="BKMK_Dep"></a>部署  
  
### <a name="deployment-configuration"></a>部署配置  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
服务器管理器从“部署配置” 页开始进行每个域控制器升级。 其余选项和必填字段在此页面和后续页面上会有所变化，这视所选部署操作而定。  
  
若要升级现有林或者将可写域控制器添加到现有域，请单击“向现有域添加域控制器” 并单击“选择” 以“指定此域的域信息”。 服务器管理器将根据需要提示你提供有效凭据。  
  
升级林需要包括 Windows Server 2012 中 Enterprise Admins 和 Schema Admins 组中组成员身份的凭据。 如果当前凭据没有足够权限或组成员身份，Active Directory 域服务配置向导将在稍后给出提示。  
  
在将域控制器添加到现有 Windows Server 2012 域以及添加到域控制器运行早期版本 Windows Server 的域之间，自动 Adprep 进程是唯一的操作区别。  
  
部署配置 ADDSDeployment cmdlet 和参数是：  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
某些测试在每个页面上执行，其中一些之后将作为离散的先决条件检查重复进行。 例如，如果选定的域不符合最小的功能级别，无需完成升级到先决条件检查的全过程，即可了解：  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
“域控制器选项”页面指定用于新域控制器的域控制器功能。 可配置的域控制器功能为“DNS 服务器”、“全局编录”和“只读域控制器”。 Microsoft 建议所有域控制器提供 DNS 和 GC 服务以实现分布式环境中的高可用性。 始终默认选中 GC；如果当前域托管的 DNS 已在其 DC 上（基于起始授权机构查询），则默认选中 DNS 服务器。 “域控制器选项”页还让你可以从林配置中选择相应的 Active Directory 逻辑“站点名称”。 默认情况下，将选择具有最合适子网的站点。 如果仅有一个站点，它将自动选择。  
  
> [!NOTE]  
> 如果该服务器不属于 Active Directory 子网，并且有多个 Active Directory 站点，则不选择任何内容且“下一步”按钮不可用，直到你从列表选择一个站点。  
  
指定的“目录服务还原模式密码”必须遵守应用到服务器的密码策略。 总是选择复杂强密码或首选密码。  
  
“域控制器选项” ADDSDeployment 参数是：  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 作为参数提供给 **-sitename** 时，此站点名称必须已存在。 **install-AddsDomainController** cmdlet 不创建站点。 可以使用 cmdlet **new-adreplicationsite** 创建新站点。  
  
**SafeModeAdministratorPassword** 参数的操作是特殊操作：  
  
-   如果 *未指定* 为参数，cmdlet 将提示你输入并确认掩蔽密码。 以交互方式运行 cmdlet 时，这是首选用法。  
  
    例如，要在 treyresearch.net 域中创建其他域控制器，并且收到输入并确认掩蔽的密码的提示：  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
    ```  
  
-   如果指定*值*，则该值必须是安全字符串。 以交互方式运行 cmdlet 时，这不是首选用法。  
  
例如，可通过使用 **Read-Host** cmdlet 提示用户提供安全字符串来手动提示输入密码：  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> 由于上个选项未确认密码，请务必十分谨慎：密码不可见。  
  
你还可以提供安全字符串作为转换的明文变量，尽管强烈不建议这样做。  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
最后，可以将模糊的密码存储在文件中，然后在以后重复使用它，而不会出现明文密码。 例如：  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> 不建议提供或存储清晰或模糊的文本密码。 任何在脚本中运行此命令或看到的人员都知道该域控制器的 DSRM 密码。  任何有权限访问该文件的人都可反转该模糊密码。 知道密码后，他们可以登录到以 DSRM 模式启动的 DC 并最终模拟域控制器本身，从而将他们的权限提升到 Active Directory 林中的最高级。 使用 **System.Security.Cryptography** 加密文本文件数据的一组其他步骤是可取的，但不在范围内。 最佳实践是完全避免密码存储。  
  
ADDSDeployment cmdlet 提供其他选项以跳过 DNS 客户端设置、转发器和根提示的自动配置。 使用服务器管理器时，不能跳过此配置选项。 仅当已在配置域控制器之前安装 DNS 服务器角色时，此参数才十分重要：  
  
```  
-SkipAutoConfigureDNS  
```  
  
“域控制器选项”页会警告你如果你的现有域控制器运行 Windows Server 2003，你将无法创建只读域控制器。 这是预期行为，而且你可以取消该警告。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 选项和 DNS 委派凭据  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
如果已选择“域控制器选项”页面上的“DNS 服务器”，并且如果指向允许 DNS 委派的区域，则“DNS 选项”页允许你配置 DNS 委派。 你可能需要提供作为“DNS Admins” 组成员的用户的备用凭据。  
  
“DNS 选项” ADDSDeployment cmdlet 参数是：  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
有关是否需要创建 DNS 委派的详细信息，请参阅 [了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
### <a name="additional-options"></a>其他选项  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
“其他选项” 页面提供用于将域控制器命名为复制源的配置选项，或者你可以将任何域控制器用作复制源。  
  
你还可以使用“从媒体安装 (IFM)”选项选择使用备份的媒体安装域控制器。 选中后，“从媒体安装” 复选框提供浏览器选项，而且你必须单击“验证” 以确保所提供的路径是有效的媒体。 仅从另一个现有 Windows Server 2012 计算机使用 Windows Server Backup 或 Ntdsutil.exe 创建由 IFM 选项使用的媒体；无法使用 Windows Server 2008 R2 或之前的操作系统为 Windows Server 2012 域控制器创建媒体。 有关 IFM 中的更改的详细信息，请参阅[简化管理附录](../../ad-ds/deploy/Simplified-Administration-Appendix.md)。 如果使用受 SYSKEY 保护的媒体，服务器管理器将在验证期间提示输入图像的密码。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
**其他选项** ADDSDeployment cmdlet 参数是：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>路径  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
“路径”页可以用于覆盖 AD DS 数据库、数据库事务日志和 SYSVOL 共享的默认文件夹位置。 默认位置始终位于 %systemroot% 的子目录中。  
  
Active Directory 路径 ADDSDeployment cmdlet 参数是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>准备选项  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
“准备选项” 页面提示你 AD DS 配置包括扩展架构 (forestprep) 和更新该域 (domainprep)。  仅当之前的 Windows Server 2012 域控制器安装未准备好林和域或者手动运行 Adprep.exe 时，你才会看到此页面。 例如，如果你将新域控制器添加到现有 Windows Server 2012 目录林根级域，Active Directory 域服务配置向导将取消此页面。  
  
当你单击“下一步”时，扩展架构和更新域操作不会发生。 这些事件仅在安装阶段发生。 此页面只是为了使你注意到稍后将在安装中发生的事件。  
  
此页面还验证当前用户凭据是 Schema Admin 和 Enterprise Admins 组的成员，因为你需要这些组的成员身份以扩展架构或准备域。 如果该页面通知你当前凭据未提供足够的权限，单击“更改”以提供足够的用户凭据。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
“其他选项”ADDSDeployment cmdlet 参数是：  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> 和之前版本的 Windows Server 一样，运行 Windows Server 2012 的域控制器的自动化域准备不运行 GPPREP。 为之前没有针对 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的所有域手动运行 **adprep.exe /gpprep** 。 你应当在域的历史记录中仅运行一次 GPPrep，而不是在每次升级时都运行。 Adprep.exe 不自动运行 /gpprep，因为它的操作可能导致 SYSVOL 文件夹中的所有文件和文件夹在所有域控制器上重新复制。  
>   
> 自动 RODCPrep 将在你升级域中第一个未分步的 RODC 时运行。 当你升级第一个可写 Windows Server 2012 域控制器时，它不会发生。 如果你计划部署只读域控制器，你仍然可以手动执行 **adprep.exe /rodcprep** 。  
  
### <a name="review-options-and-view-script"></a>审查选项和查看脚本  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
“审查” 选项 页可以用于验证设置并确保在开始安装前满足要求。 这不是停止使用服务器管理器安装的最后一次机会。 此页只是让你先查看和确认设置，然后再继续配置。  
  
服务器管理器中的“审查选项” 页还提供可选的“查看脚本” 按钮，可将包含当前 ADDSDeployment 配置的 Unicode 文本文件创建为一个 Windows PowerShell 脚本。 这样，你可以将服务器管理器图形界面用作 Windows PowerShell 部署工作台。 使用 Active Directory 域服务配置向导来配置选项、导出配置和取消向导。  此过程将创建有效且语法正确的示例，以便做进一步修改或直接使用。  
  
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
> 服务器管理器通常在升级时使用值填充所有参数，并且不依赖默认值（因为它们可能在 Windows 或服务包以后的版本之间发生更改）。 但该情况的一个例外是 **-safemodeadministratorpassword** 参数。 若要强制确认提示，请在以交互方式运行 cmdlet 时省略该值  
>   
> 将可选 **Whatif** 参数与 **Install-ADDSDomainController** cmdlet 一起使用以查看配置信息。 这使你可以查看 cmdlet 的参数的显式和隐式值。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件检查  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
“先决条件检查” 是 AD DS 域配置中的新功能。 此新阶段验证域和林是否能够支持新的 Windows Server 2012 域控制器。  
  
安装新的域控制器时，服务器管理器 Active Directory 域服务配置向导调用一系列序列化的模块化测试。 这些测试向你提出警告并提供建议的修复选项。 你可以根据需要多次运行测试。 域控制器进程在所有先决条件测试通过前无法继续。  
  
“先决条件检查”还显示相关的信息，例如影响较早版本的操作系统的安全性更改。  
  
有关特定的先决条件检查的详细信息，请参阅[先决条件检查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
使用服务器管理器时你无法绕过“先决条件检查” ，但是你可以在使用 AD DS 部署 cmdlet 时使用以下参数跳过该进程：  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能导致部分域控制器升级或 AD DS 林损坏。  
  
单击“安装”以开始域控制器升级进程。 这是取消安装的最后机会。 一旦开始升级过程，你无法取消它。 无论升级结果如何，计算机将在升级结束时自动重新启动。“先决条件检查” 页面会显示任何它在进程期间遇到的问题以及解决该问题的指南。  
  
### <a name="installation"></a>安装  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
当“安装” 页显示时，域控制器配置将开始，并且无法暂停或取消。 详细操作显示在此页面上并将写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log（如果服务器在工作组中）  
  
若要使用 ADDSDeployment 模块安装新的 Active Directory 林，请使用以下 cmdlet：  
  
```  
Install-addsdomaincontroller  
```  
  
有关必需和可选参数，请参阅[升级和副本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)。  
  
**Install-AddsDomainController** cmdlet 仅有两个阶段（先决条件检查和安装）。 下面两个图片显示安装阶段，并带有最少的所需参数 **-domainname** 和 **-credential**。 请注意 Adprep 操作如何作为将第一个 Windows Server 2012 域控制器添加到现有 Windows Server 2003 林的一部分自动进行。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
请注意 **Install-ADDSDomainController** 如何提醒你升级将自动重新启动服务器，就像服务器管理器一样。 若要自动接受重新启动提示，请将 **-force** 或 **-confirm:$false** 参数与任何 ADDSDeployment Windows PowerShell cmdlet 一起使用。 若要防止服务器在升级结束时自动重新启动，请使用 **-norebootoncompletion** 参数。  
  
> [!WARNING]  
> 不建议重写重新启动。 域控制器必须重新启动才能正常工作。  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
若要使用 Windows PowerShell 远程配置域控制器, 请将**install-addsdomaincontroller** cmdlet 包装在**调用-command** cmdlet*内*。 这需要使用大括号。  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
例如：  
  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> 有关安装和 Adprep 进程的工作原理的详细信息，请参阅 [Troubleshooting Domain Controller Deployment](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。  
  
### <a name="results"></a>结果  
![安装副本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
“结果” 页面显示升级是成功还是失败以及任何重要的管理信息。 如果成功，则域控制器将自动在 10 秒后重新启动。  
  
和之前版本的 Windows Server 一样，运行 Windows Server 2012 的域控制器的自动化域准备不运行 GPPREP。 为之前没有针对 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的所有域手动运行 **adprep.exe /gpprep** 。 你应当在域的历史记录中仅运行一次 GPPrep，而不是在每次升级时都运行。 Adprep.exe 不自动运行 /gpprep，因为它的操作可能导致 SYSVOL 文件夹中的所有文件和文件夹在所有域控制器上重新复制。  
  

