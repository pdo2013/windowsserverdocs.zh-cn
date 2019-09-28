---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: 安装新的 Windows Server 2012 Active Directory 子域或树域（级别 200）
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d0944377739f43ea5d9b8d0d9c94c13e9f18985f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390894"
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>安装新的 Windows Server 2012 Active Directory 子域或树域（级别 200）

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍如何使用服务器管理器或 Windows PowerShell 将子域和树域添加到现有 Windows Server 2012 林。  
  
-   [子工作流和树域工作流](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [子域和树域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [部署](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>子工作流和树域工作流  
当你之前已安装 AD DS 角色，并且已使用服务器管理器启动 Active Directory 域服务配置向导以在现有林中创建新域时，下图说明了此情况下的 Active Directory 域服务配置过程。  
  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>子域和树域 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|**安装-Install-addsdomain**|-Skipprechecks 不可<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*-DomainMode*<br /><br />***-DomainType***<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SiteName*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> 仅在当前未作为 Enterprise Admins 组成员登录时需要 **-credential** 参数。如果你希望更改根据 DNS 域名前缀自动生成的 15 个字符的名称，或者名称超过 15 个字符，则需要 **-NewDomainNetBIOSName** 参数。  
  
## <a name="BKMK_Deployment"></a>部署  
  
### <a name="deployment-configuration"></a>部署配置  
下面的屏幕截图显示了用于添加子域的选项：  
  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
下面的屏幕截图显示了用于添加树域的选项：  
  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
服务器管理器从“部署配置” 页开始进行每个域控制器升级。 其余选项和必填字段在此页面和后续页面上会有所变化，这视所选部署操作而定。  
  
本主题结合了两个独立的操作：字域升级和树域升级。 两种操作唯一的区别是你选择创建的域类型。 两种操作的所有其他步骤都相同。  
  
-   要创建新子域，单击“将域添加到现有林”并选择“子域”。 对于“父域名称”，键入或选择父域的名称。 然后在“新域名”框中键入新域的名称。 提供有效的、单标签子域名；该名称必须使用 DNS 域名要求。  
  
-   要在现有林内创建树域，单击“将域添加到现有林”并选择“树域”。 键入目录林根级域的名称，然后键入新域的名称。 提供有效的完全限定的根域名；该名称不能是单标记，必须使用 DNS 域名要求。  
  
有关 DNS 名称的详细信息，请参阅 [Active Directory 中用于计算机、域、站点和 OU 的命名约定](https://support.microsoft.com/kb/909264)。  
  
如果你当前的凭据不是来自该域，服务器管理器 Active Directory 域服务配置向导将提示你提供域凭据。 单击“更改”为升级操作提供域凭据。  
  
部署配置 ADDSDeployment cmdlet 和参数是：  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
“域控制器选项” 页指定新域控制器的域控制器选项。 可配置的域控制器选项包括 **“DNS 服务器”** 和 **“全局目录”** ；你不能将只读域控制器配置为新域的第一个域控制器。  
  
Microsoft 建议所有域控制器提供 DNS 和 GC 服务以实现分布式环境中的高可用性。 始终默认选中 GC；如果当前域托管的 DNS 已在其 DC 上（基于起始授权机构查询），则默认选中 DNS。 你还必须指定“域功能级别”。 默认功能级别是 Windows Server 2012，而且你可以选择等于或大于当前林功能级别的任何值。  
  
“域控制器选项”页还让你可以从林配置中选择相应的 Active Directory 逻辑“站点名称”。 默认情况下，选择带有最合适的子网的站点。 如果仅有一个站点，则自动选择它。  
  
> [!IMPORTANT]  
> 如果该服务器不属于 Active Directory 子网，并且有多个 Active Directory 站点，则不选择任何内容且“下一步”按钮不可用，直到你从列表选择一个站点。  
  
指定的“目录服务还原模式密码”必须遵守应用到服务器的密码策略。 总是选择复杂强密码或首选密码。  
  
“域控制器选项”ADDSDeployment cmdlet 参数是：  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> 当站点名称作为值提供到 **sitename** 参数时，该名称必须已经存在。 **install-AddsDomainController** cmdlet 不创建站点名称。 你可以使用 **new-adreplicationsite** cmdlet 创建新站点。  
  
**Install-ADDSDomainController** cmdlet 参数使用与服务器管理器相同的默认值（如果未指定）。  
  
**SafeModeAdministratorPassword** 参数的操作是特殊操作：  
  
-   如果 *未指定* 为参数，cmdlet 将提示你输入并确认掩蔽密码。 以交互方式运行 cmdlet 时，这是首选用法。  
  
    例如，要在 Contoso.com 林中创建名为 NorthAmerica 的新子域，并且收到输入并确认掩蔽密码的提示。  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
> 不建议提供或存储清晰或模糊的文本密码。 任何在脚本中运行此命令或看到的人员都知道该域控制器的 DSRM 密码。  任何有权限访问该文件的人都可反转该模糊密码。 知道密码后，他们可以登录到以 DSRM 模式启动的 DC 并最终模拟域控制器本身，从而将他们的权限提升到 AD 林中的最高级。 使用 **System.Security.Cryptography** 加密文本文件数据的一组其他步骤是可取的，但不在范围内。 最佳实践是完全避免密码存储。  
  
ADDSDeployment 模块提供其他选项以跳过 DNS 客户端设置、转发器和根提示的自动配置。 这不可在使用服务器管理器时进行配置。 仅当你已在配置域控制器之前安装 DNS 服务器服务时，此参数才十分重要：  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 选项和 DNS 委派凭据  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
“DNS 选项” 页使你可以提供备用 DNS 管理员凭据以用于委派。  
  
在现有林中（你已在其中在“域控制器选项”页上选择了 DNS 安装）安装新域时，你无法配置任何选项；委派将自动进行，且不可撤销。 你可以选择提供备用 DNS 管理凭据，这些凭据具有更新该结构的权限。  
  
“DNS 选项” ADDSDeployment Windows PowerShell 参数是：  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
有关 DNS 委派的详细信息，请参阅 [了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
### <a name="additional-options"></a>其他选项  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
“其他选项”页显示该域的 NetBIOS 名称并使你可以重写它。 默认情况下，NetBIOS 域名与“部署配置” 页上提供的完全限定的域名最左边的标签相匹配。 例如，如果你提供了 corp.contoso.com 的完全限定的域名，则默认 NetBIOS 域名是 CORP。  
  
如果名称是 15 个字符或更少且与另一个 NetBIOS 名称不冲突，则它保持不变。 如果它与另一个 NetBIOS 名称发生冲突，则将数字附加到该名称。 如果该名称多于 15 个字符，则向导提供一个唯一的截断的建议名称。 在任一情况下，向导首先将通过 WINS 查找和 NetBIOS 广播验证该名称尚未被使用。  
  
有关 DNS 名称的详细信息，请参阅 [Active Directory 中用于计算机、域、站点和 OU 的命名约定](https://support.microsoft.com/kb/909264)。  
  
**Install-AddsDomain** 参数使用与服务器管理器相同的默认值（如果未指定）。 “DomainNetBIOSName”操作是特殊操作：  
  
1.  如果未使用 NetBIOS 域名指定“NewDomainNetBIOSName”参数且“DomainName”参数中的单标签前缀域名等于或少于 15 个字符，则升级将使用自动生成的名称继续运行。  
  
2.  如果未使用 NetBIOS 域名指定 **NewDomainNetBIOSName** 参数且 **DomainName** 参数中的单标签前缀域名等于或多于 16 个字符，则升级失败。  
  
3.  如果使用等于或少于 15 个字符的 NetBIOS 域名指定 **NewDomainNetBIOSName** 参数，则升级将使用指定的名称继续运行。  
  
4.  如果使用等于或多于 16 个字符的 NetBIOS 域名指定 **NewDomainNetBIOSName** 参数，则升级失败。  
  
“其他选项” ADDSDeployment cmdlet 参数是：  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>路径  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
“路径” 页使你可以覆盖 AD DS 数据库、数据库事务日志和 SYSVOL 共享的默认文件夹位置。 默认位置始终位于 %systemroot% 的子目录中。  
  
**路径** ADDSDeployment cmdlet 参数是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>审查选项和查看脚本  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
“审查选项” 页面使你可以在开始安装前验证设置并确保它们符合你的要求。 在使用服务器管理器时，这并不是停止安装的最后机会。 这只是一个在继续配置前确认设置的选项  
  
服务器管理器中的“审查选项” 页还提供可选的“查看脚本” 按钮，可将包含当前 ADDSDeployment 配置的 Unicode 文本文件创建为一个 Windows PowerShell 脚本。 这样，你可以将服务器管理器图形界面用作 Windows PowerShell 部署工作台。 使用 Active Directory 域服务配置向导来配置选项、导出配置和取消向导。  此过程将创建有效且语法正确的示例，以便做进一步修改或直接使用。 例如：  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomain `  
-NoGlobalCatalog:$false `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainType "ChildDomain" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NewDomainName "research" `  
-NewDomainNetBIOSName "RESEARCH" `  
-ParentDomainName "corp.contoso.com" `  
-Norebootoncompletion:$false `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> 服务器管理器通常在升级时使用值填充所有参数，并且不依赖默认值（因为它们可能在 Windows 或服务包以后的版本之间发生更改）。 但该情况的一个例外是 **-safemodeadministratorpassword** 参数（有意从脚本中省略）。 若要强制执行确认提示，则在以交互方式运行 cmdlet 时省略该值。  
  
将可选的 **Whatif** 参数与 **Install-ADDSForest** cmdlet 一起使用以查看配置信息。 这使你可以查看 cmdlet 的参数的显式和隐式值。  
  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件检查  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
“先决条件检查” 是 AD DS 域配置中的新功能。 此新阶段验证服务配置是否能够支持新的 AD DS 域。  
  
在安装新目录林根级域时，服务器管理器 Active Directory 域服务配置向导将调用一系列序列化的模块化测试。 这些测试向你提出警告并提供建议的修复选项。 你可以根据需要多次运行测试。 域控制器进程在所有先决条件测试通过前无法继续。  
  
“先决条件检查”还显示相关的信息，例如影响较早版本的操作系统的安全性更改。  
  
有关特定先决条件检查的详细信息，请参阅 [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
使用服务器管理器时你无法绕过“先决条件检查” ，但是你可以在使用 AD DS 部署 cmdlet 时使用以下参数跳过该进程：  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能导致部分域控制器升级或 AD DS 林损坏。  
  
单击“安装”以开始域控制器升级进程。 这是取消安装的最后机会。 一旦开始升级过程，你无法取消它。 无论升级结果如何，计算机将在升级结束时自动重新启动。  
  
### <a name="installation"></a>安装  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
当“安装” 页显示时，域控制器配置将开始，并且无法暂停或取消。 详细操作显示在此页面上并将写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要使用 ADDSDeployment 模块安装新的 Active Directory 域，请使用以下 cmdlet：  
  
```  
Install-addsdomain  
```  
  
有关所需的参数和可选参数，请参阅[子域和树域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)。**Install-addsdomain** cmdlet 仅有两个阶段（先决条件检查和安装）。 下面的两个图显示安装阶段，并带有最少的所需参数 **-domaintype**、 **-newdomainname**、 **-parentdomainname** 和 **-credential**。 请注意 **Install-ADDSDomain** 如何提醒你升级将自动重新启动服务器，就像服务器管理器一样。  
  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
若要自动接受重新启动提示，请将 **-force** 或 **-confirm:$false** 参数与任何 ADDSDeployment Windows PowerShell cmdlet 一起使用。 若要防止服务器在升级结束时自动重新启动，请使用 **-norebootoncompletion** 参数。  
  
> [!WARNING]  
> 不建议重写重新启动。 域控制器必须重新启动才能正常工作  
  
### <a name="results"></a>结果  
![安装新的 AD 子项](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
“结果” 页面显示升级是成功还是失败以及任何重要的管理信息。 域控制器将在 10 秒后自动重新启动。  
  

