---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: "安装新的 Windows Server 2012 的 Active Directory 孩子或树域 (级别 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc0eecc44bbc5f7459f22aceb5ebe41cd61948b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>安装新的 Windows Server 2012 的 Active Directory 孩子或树域 (级别 200)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍了如何使用服务器管理器或 Windows PowerShell 现有的 Windows Server 2012 树林中添加孩子和树域。  
  
-   [孩子和树域工作流](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [孩子和树域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [部署](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>孩子和树域工作流  
下图说明 Active Directory 域服务配置过程，当你之前安装的广告 DS 角色，并且已经开始使用服务器管理器在现有的树林中创建新的域的 Active Directory 域服务配置向导。  
  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>孩子和树域 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|**Install-AddsDomain**|-SkipPreChecks<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-确认<br /><br />*-CreateDNSDelegation*<br /><br />***-凭据***<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*-DomainMode*<br /><br />***-DomainType***<br /><br />-强制<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-站点名*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> **-凭据**参数才需要不当前登录时作为企业管理员 group.The **-NewDomainNetBIOSName**参数是必需的如果你想要更改基于 DNS 域名称前缀自动生成的 15 个字符的名称，或者如果名称超出了 15 个字符。  
  
## <a name="BKMK_Deployment"></a>部署  
  
### <a name="deployment-configuration"></a>部署配置  
下面的屏幕截图显示了添加孩子域的选项：  
  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
下面的屏幕截图显示了添加树域的选项：  
  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
服务器管理器开始与每域控制器升级**部署配置**页面。 其他选项和必填的字段在此页和后续页面，具体取决于哪个部署操作你选择的更改。  
  
本主题将两个独立的操作：孩子的域升级和树域升级。 两种操作的唯一区别是你选择创建的域类型。 所有的其他步骤都相同之间的两个操作。  
  
-   若要创建新的孩子域，请单击**现有的树林中添加域**选择**孩子域**。 对于**父域名**中，键入或选择家长域的名称。 然后键入名称中的新域**新域名**框。 提供有效的、单标签孩子域名;该名称必须使用 DNS 域名称要求。  
  
-   若要创建现有林中树域，请单击**现有的树林中添加域**选择**树域**。 键入森林根域的名称，然后键入新的域的名称。 提供有效的完整根域名;该名称将不能单标记，并且必须使用 DNS 域名称要求。  
  
有关 DNS 名称的详细信息，请参阅[的计算机、域、网站和华丽绚烂命名 Active Directory 中的约定](https://support.microsoft.com/kb/909264)。  
  
服务器管理器 Active Directory 域服务配置向导会提示您输入凭据域如果你当前的凭据无法从域。 单击**更改**提供域凭据升级操作。  
  
部署配置 ADDSDeployment cmdlet 和参数是：  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
**域控制器选项**页上指定的新的域控制器域控制器选项。 配置域控制器选项包括**DNS 服务器**和**全球目录**;你无法将仅阅读域控制器配置为新的域中的第一个域控制器。  
  
Microsoft 建议所有域控制器都提供高可用性分布式环境中的 DNS 和 GC 服务。 GC 默认始终处于选中状态，并 DNS 选择默认情况下，如果当前域托管的基于 Start-of-Authority 查询，其域控制器上的已 DNS。 您还必须指定**域功能级别**。 默认功能级别 Windows Server 2012，并且你可以选择其他任何等于或大于当前森林功能级别的值。  
  
**域控制器选项**页面，还可以选择相应的 Active Directory 逻辑**站点名称**从森林配置。 默认情况下，选择最正确子网与该站点。 如果只有一个网站，它将自动选中。  
  
> [!IMPORTANT]  
> 如果该服务器不属于 Active Directory 网，并且多个 Active Directory 站点，选中任何和**下一步**按钮不可用，直到你选择站点从列表。  
  
指定**目录服务还原模式密码**必须遵守密码策略应用到服务器。 始终可以选择复杂的强密码或最好，密码。  
  
**域控制器选项**ADDSDeployment cmdlet 参数均是：  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> 网站的名称必须已经存在，当提供作为到值**站点名**参数。 **安装 AddsDomainController** cmdlet 不会创建网站的名称。 你可以使用**新 adreplicationsite** cmdlet 创建新的站点。  
  
**安装 ADDSDomainController** cmdlet 参数如果没有，请按照相同的默认值为服务器管理器。  
  
**SafeModeAdministratorPassword**特别是参数的操作：  
  
-   如果*未指定*作为参数，cmdlet 提示你输入并确认屏蔽的密码。 运行 cmdlet 交互时，这是首选的使用量。  
  
    例如，若要创建新的孩子域名为北美 Contoso.com 森林中的和会提示你输入并确认屏蔽的密码：  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
> 不建议提供或存储清除或模糊短的密码。 脚本中运行此命令或查找通过您的任何人都知道 DSRM 域控制器的密码。  有权访问文件的任何人都可以反转该模糊的密码。 与该知识，他们可以登录到 DC DSRM 以启动并最终模拟域控制器本身提升最大程度广告森林中的权限。 步骤，使用另一组**System.Security.Cryptography**文本的文件进行加密数据是建议，但利用范围。 最佳做法是完全避免密码存储。  
  
ADDSDeployment 模块提供跳过自动配置 DNS 客户端设置、转发器，以及根提示其他选项。 使用服务器管理器时，这是不配置。 此参数很重要，仅当你已经安装了之前配置域控制器 DNS 服务器服务：  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 选项和 DNS 委派凭据  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
**DNS 选项**页面使您能够提供用于委派备用 DNS 管理员凭据。  
  
当上选择 DNS 安装了现有的森林-在安装新的域**域控制器选项**页面-你不能配置任何的选项;委派将发生自动并不可挽回地。 你可以选择要提供权限才能更新该结构备用 DNS 管理凭据。  
  
**DNS 选项**ADDSDeployment Windows PowerShell 参数均是：  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
有关 DNS 委派的详细信息，请参阅[了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
### <a name="additional-options"></a>其他选项  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
**其他选项**页面显示的域 NetBIOS 名称，并使您可以将其重写。 默认情况下，NetBIOS 域名称匹配合法的域名上提供的最左端标签**部署配置**页面。 例如，如果提供 corp.contoso.com 合法的域名，默认 NetBIOS 域名是 CORP.  
  
如果名 15 字符或更少和不冲突用另一台 NetBIOS 名称、它是不变。 如果它会与其他 NetBIOS 名称有冲突，数字附加为的名称。 如果名多 15 字符，该向导将提供独特的、被截断的建议。 不论采取何种情况，该向导首先验证名称尚不在使用通过 WINS 查找和广播 NetBIOS。  
  
有关 DNS 名称的详细信息，请参阅[的计算机、域、网站和华丽绚烂命名 Active Directory 中的约定](https://support.microsoft.com/kb/909264)。  
  
**安装 AddsDomain**参数如果没有，请按照相同的默认值为服务器管理器。 **DomainNetBIOSName**是特殊的操作：  
  
1.  如果**NewDomainNetBIOSName** NetBIOS 域名称和中的单标签前缀域名未指定参数**域名**参数 15 字符或更少，则推广继续自动生成的名称。  
  
2.  如果**NewDomainNetBIOSName** NetBIOS 域名称和中的单标签前缀域名未指定参数**域名**参数为 16 个字符或的详细信息，然后升级失败。  
  
3.  如果**NewDomainNetBIOSName**参数指定 NetBIOS 域名为 15 字符或更少，然后升级继续使用该指定的名称。  
  
4.  如果**NewDomainNetBIOSName**参数指定 16 个字符的 NetBIOS 域名称或的详细信息，然后升级失败。  
  
**其他选项**ADDSDeployment cmdlet 参数是：  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>路径  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
**路径**页使你可以覆盖默认文件夹位置的广告 DS 数据库数据基本事务日志，，SYSVOL 共享。 始终处于子目录 %系统根 %的默认位置。  
  
**路径**ADDSDeployment cmdlet 参数均是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>查看选项并查看脚本  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
**回顾选项**页面，可以让你以验证你的设置并确保它们符合你的要求，开始安装之前。 这不是最后一次机会停止安装时使用服务器管理器。 这是只需继续配置之前，请确认你的设置的选项  
  
**回顾选项**页面中服务器管理器中还提供了一个可选**查看脚本**按钮来创建 Unicode 文本文件包含当前 ADDSDeployment 配置为单个 Windows PowerShell 脚本。 这使你为 Windows PowerShell 部署 studio 使用服务器管理器图形界面。 使用 Active Directory 域服务配置向导配置选项中, 导出配置，然后取消向导。  此过程中创建进一步修改或直接使用有效和语法正确的示例。 例如：  
  
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
> 服务器管理器通常会填满所有参数与值时升级，不依赖于默认设置（如它们可能会更改之间将来版本的 Windows 或 service pack）中。 是一个例外**-safemodeadministratorpassword**参数（这有意省略脚本中）。 若要强制确认提示，请运行 cmdlet 交互时忽略值。  
  
使用可选**Whatif**具有参数**安装 ADDSForest** cmdlet 查看配置的信息。 这使你查看的参数 cmdlet 明确和隐式值。  
  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件复选  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
**先决条件检查**是广告 DS 域配置中的新增功能。 此新阶段验证的服务器配置能够支持新的广告 DS 域。  
  
当安装新林根域，服务器管理器 Active Directory 域服务配置向导调用一系列序列化模块化测试。 这些测试向你发出警报建议的维修选项。 你可以根据需要多次运行测试。 域控制器过程所有先决条件测试才可继续通过。  
  
**先决条件检查**还面，如会影响较旧操作系统的安全更新的相关信息。  
  
特定的先决条件检查的详细信息，请参阅[先决条件检查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
你无法跳过**先决条件检查**时使用服务器管理器中，但你可以跳过此过程时使用使用下面的参数广告 DS 部署 cmdlet:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能会导致部分域控制器升级或损坏广告 DS 森林。  
  
单击**安装**开始域控制器升级过程中。 这是最后一次机会取消安装。 一旦开始后，你无法取消升级过程中。 在升级，无论推广结果的末尾，计算机重新启动将自动。  
  
### <a name="installation"></a>安装  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
当**安装**页面显示时，域控制器配置开始，无法将停止或取消。 详细的操作显示在此页面上，并且写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要安装使用 ADDSDeployment 模块新 Active Directory 域，使用以下 cmdlet:  
  
```  
Install-addsdomain  
```  
  
请参阅[孩子和树域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)必需和可选 arguments.The**安装 addsdomain** cmdlet 都仅有两个阶段（先决条件检查和安装）。 两个下图显示与的最低要求参数安装阶段**-domaintype**，**-newdomainname**，**-parentdomainname**，并**-凭据**。 注意如何操作，就像服务器管理器**安装 ADDSDomain**提醒你，升级将自动重新启动的服务器。  
  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
若要自动接受重新引导提示，请使用**-强制**或**-确认：$false**与任何 ADDSDeployment Windows PowerShell cmdlet 参数。 若要防止服务器推广末尾自动重新启动，请使用**-norebootoncompletion**参数。  
  
> [!WARNING]  
> 不建议重重新启动。 域控制器必须重新启动才能正常工作  
  
### <a name="results"></a>结果  
![安装新的广告孩子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**结果**页面显示的成功或失败的升级和管理的任何重要信息。 10 秒钟后，域控制器将自动重新启动。  
  

