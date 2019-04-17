---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: "安装 Windows Server 2012 的 Active Directory Read-Only 域控制器 (RODC) (级别 200)"
description: "本主题介绍如何创建分阶段的 RODC 帐户，然后将该帐户的服务器附加 RODC 在安装过程。 本主题也说明如何安装 RODC 无需执行的分步的安装。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 78281ca845f79955aaa25aa45394284c59e639cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>安装 Windows Server 2012 的 Active Directory Read-Only 域控制器 (RODC) (级别 200)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍如何创建分阶段的 RODC 帐户，然后将该帐户的服务器附加 RODC 在安装过程。 本主题也说明如何安装 RODC 无需执行的分步的安装。  
  
## <a name="stage-rodc-workflow"></a>阶段 RODC 工作流  
分阶段阅读分为两个独立的阶段仅域控制器 (RODC) 安装适用：  
  
1.  临时未占用的计算机帐户  
  
2.  在升级过程中连接到该帐户的 RODC  
  
下图说明 Active Directory 域服务 Read-Only 暂存的进程，创建帐户时所在空白 RODC 计算机在域中使用 Active Directory 管理中心 (Dsac.exe) 域控制器。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>阶段 RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|Add-addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-域名***<br /><br />***-站点名***<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />***-凭据***<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />-ReplicationSourceDC|  
  
> [!NOTE]  
> **-凭据**参数才需要如果您不已登录的域管理员组成员。  
  
## <a name="attach-rodc-workflow"></a>附加 RODC 工作流  
下图所示的 Active Directory 域服务配置过程中，在你已安装 DS AD 作用，你转移 RODC 帐户，并开始**推广此域控制器服务器**使用现有域，将其连接到计算机分阶段的帐户创建新 RODC 服务器管理器。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>附加 RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|Install-AddsDomaincontroller|-SkipPreChecks<br /><br />***-域名***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-凭据***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-LogPath*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> **-凭据**参数才需要如果您不已登录的域管理员组成员。  
  
## <a name="staging"></a>临时  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
你执行仅阅读域控制器计算机帐户转移操作方法是打开 Active Directory 管理中心 (**Dsac.exe**)。 单击导航窗格中的域的名称。 双击**域控制器**管理列表中。 单击**预创建 Read-only 域控制器帐户**任务窗格中。  
  
有关 Active Directory 管理中心的详细信息，请参阅[高级广告 DS 管理使用 Active Directory 管理中心 & #40;级别 200 & #41;](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)和查看[Active Directory 管理中心：入门](https://technet.microsoft.com/library/dd560651(WS.10).aspx)。  
  
如果你有创建仅阅读域控制器的体验，你将发现安装向导具有相同的图形界面可以看到使用较旧的 Active Directory 用户和计算机贴靠的 Windows Server 2008 从时，并使用相同的代码，其中包括导出所使用的过时 dcpromo 的人参与文件格式中的配置。  
  
Windows Server 2012 引入新的 ADDSDeployment cmdlet 到阶段 RODC 计算机帐户，但向导不 cmdlet 使用它的操作。 以下部分中显示等效 cmdlet 参数以使每个更容易理解与关联的信息。  
  
**预创建 Read-only 域控制器帐户**Active Directory 管理中心任务窗格中的链接相当于 ADDSDeployment Windows PowerShell cmdlet:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>欢迎使用  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
**欢迎 Active Directory 域服务安装向导**对话框拥有一个名为的选项**高级模式安装使用**。 选择此选项，然后单击**下一步**显示密码复制策略选项。 清除此选项可用于复制密码的策略选项（这是进一步详细讨论在此部分中的更高版本）的默认值。  
  
### <a name="network-credentials"></a>网络的凭据  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
在域名选项**网络的凭据**对话框默认情况下显示的域的 Active Directory 管理中心目标。 默认情况下使用你当前的凭据。 如果不包含会员域管理员组中，单击**备用凭据**，并单击**设置**向导提供用户名和密码即域管理员的成员。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-credential <pscredential>  
```  
  
请记住，临时系统是直接从 Windows Server 2008 R2 端口，并不提供新的 Adprep 功能。 如果你计划部署分阶段的 RODC 帐户，必须首先部署未分阶段的 RODC 域中，以便自动 rodcprep 操作运行时，或手动运行 adprep.exe /rodcprep 第一。  
  
否则，你将收到错误"你将无法只读域控制器安装此域中，因为"adprep /rodcprep"尚未运行"。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>指定计算机名称  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
**指定计算机名称**对话框要求你输入单标签**计算机名称**不存在的域控制器。 域控制器，您将配置并稍后连接到此帐户必须具有相同的名称，或者升级操作不会检测分阶段的帐户。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>选择一个站点  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
**选择某一站点**对话框显示有关当前森林 Active Directory 站点的列表。 分阶段只读域控制器操作要求你单个站点从列表中选择。 RODC 使用此信息来配置分区中创建它各自的对象并本身加入正确的站点时部署后首次启动电脑。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>其他域控制器选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
**其他域控制器选项**对话框，您可以指定域控制器，包含以运行**DNS 服务器**和一**全球目录**。 Microsoft 建议仅阅读域控制器提供 DNS 和 GC 服务，以便同时安装默认设置。RODC 角色是一个企图分支机构方案宽的区域网络不可用的位置，以及不这些 DNS 和全球目录服务，branch 中的计算机不能够使用广告 DS 资源和功能。  
  
**只读域控制器 (RODC)**预选择的选项，并且无法禁用。 等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> 默认情况下，**-NoGlobalCatalog**价值是，这意味着如果不指定参数，域控制器将是全球目录服务器 $false。  
  
### <a name="specify-the-password-replication-policy"></a>指定密码复制策略  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
**指定密码复制策略**对话框，您可以修改默认允许缓存自己的密码只读该域控制器上的帐户的列表。 在列表中使用配置的帐户**拒绝**或不是在他们的密码不缓存（隐式）列表。 资源或通过 Active Directory 提供的功能，无法访问不允许 RODC 上的缓存密码和无法连接并验证可写的域控制器的帐户。  
  
> [!IMPORTANT]  
> 该向导将显示此对话框仅是否您选择**使用高级模式安装**欢迎屏幕上的复选框。 如果你清除该复选框，该向导使用以下默认组和值：  
>   
> -   管理员-拒绝  
> -   服务器运营商的拒绝  
> -   备份运营商的拒绝  
> -   帐户运营商的拒绝  
> -   拒绝 RODC 密码复制组-拒绝  
> -   允许 RODC 密码复制组-允许  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>委托 RODC 安装和管理  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
**RODC 安装委派和管理**对话框，您可以配置用户或组包含允许服务器附加到 RODC 计算机帐户的用户。 单击**设置**浏览用户或组域。 用户或组此对话框提升本地管理权限为 RODC 中指定。 指定的用户或指定的组成员可以执行的使用权限相当于计算机的管理员组 RODC 上的操作。 它们是*不*域管理员或域内置管理员组中的成员。  
  
使用此选项，而无需域管理员组授予 branch 管理员会员委派 branch office 管理。 不需要委派 RODC 管理。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>摘要  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
**摘要**对话框可以让你以确认你的设置。 这是最后停止安装之前向导将分阶段的帐户创建的机会。 单击**下一步**当你准备创建分阶段的 RODC 计算机帐户。  单击**导出设置**保存答案文件过时 dcpromo 人参与文件格式。  
  
### <a name="creation"></a>创建  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
**Active Directory 域服务安装向导**Active Directory 中创建分阶段只读域控制器。 启动后，你无法取消此操作。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
使用以下 cmdlet 暂存使用 ADDSDeployment Windows PowerShell 模块的只读域控制器计算机帐户：  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
请参阅[阶段 RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS)必需和可选参数。  
  
因为**添加 addsreadonlydomaincontrolleraccount**仅有一项操作（先决条件检查和安装）的两个阶段，使用下面的屏幕截图显示的最低要求参数了安装阶段。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
阶段 RODC 操作 Active Directory 中创建 RODC 计算机帐户。 显示 Active Directory 管理中心**域控制器类型**作为**未占用域控制器帐户**。 此域控制器类型指示分阶段的 RODC 帐户已准备好要向其附加为阅读仅域控制器服务器。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> Active Directory 管理中心不再需要为只读域控制器计算机帐户吸附服务器。 使用服务器管理器和 Active Directory 域服务配置向导或 ADDSDeployment Windows PowerShell 模块 cmdlet**安装 AddsDomainController**才能连接到它的新 RODC 转移帐户。 步骤都类似于向现有的域中，有分阶段的 RODC 计算机帐户包含配置选项决定转移 RODC 计算机帐户次例外添加新的可写的域控制器。  
  
## <a name="attaching"></a>连接  
  
### <a name="deployment-configuration"></a>部署配置  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
服务器管理器开始与每域控制器升级**部署配置**页面。 其他选项和必填的字段在此页和后续页面，具体取决于哪个部署操作你选择的更改。  
  
若要添加到现有的域只读域控制器，请选择**添加到现有的域的域控制器**单击**选择**到按钮**指定此域域信息**。 服务器管理器会自动将提示你输入有效的凭据，或者你可以单击**更改**。  
  
附加 RODC 需要在 Windows Server 2012 域管理员组中的成员。 Active Directory 域服务配置向导你稍后当提示你当前的凭据没有足够的权限或组成员。  
  
**部署配置**ADDSDeployment Windows PowerShell cmdlet 和参数是：  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
**域控制器选项**页面显示的域控制器新的域控制器的选项。 在加载时此页面，Active Directory 域服务配置向导将现有的域控制器，以便占用帐户检查发送 LDAP 查询。 如果查询发现未占用的域控制器计算机帐户共享同名的当前的计算机，然后向导阅读的页面顶部将显示一条通知消息"**目录中存在目标服务器的名称匹配的 RODC 预先创建的帐户。选择是否使用此现有 RODC 帐户或重新安装此域控制器**。" 该向导将使用**使用现有 RODC 帐户**作为默认配置。  
  
> [!IMPORTANT]  
> 你可以使用**重新安装此域控制器**选项时，域控制器遭到物理问题，无法返回到功能。 这可以节省时间配置更换域控制器，通过离开域控制器计算机帐户时，并对象 Active Directory 的元数据。 安装在新计算机*同名*，并将其作为域控制器在域中提升。 **重新安装此域控制器**选项将不可用，如果你从 Active Directory（元数据清理）中删除了域控制器对象元数据。  
  
附加到 RODC 计算机帐户服务器时，不能配置域控制器选项。 当您创建分阶段的 RODC 计算机帐户，您将配置域控制器选项。  
  
指定**目录服务还原模式密码**必须遵守密码策略应用到服务器。 始终可以选择复杂的强密码或最好，密码。  
  
**域控制器选项**ADDSDeployment Windows PowerShell 参数均是：  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 网站的名称必须已经存在，当提供作为的参数**-站点名**。 **安装 AddsDomainController** cmdlet 不会创建网站的名称。 你可以使用 cmdlet**新 adreplicationsite**创建新的站点。  
  
**安装 ADDSDomainController**参数如果没有，请按照相同的默认值为服务器管理器。  
  
**SafeModeAdministratorPassword**特别是参数的操作：  
  
-   如果*未指定*作为参数，cmdlet 提示你输入并确认屏蔽的密码。 运行 cmdlet 交互时，这是首选的使用量。  
  
    例如，在 corp.contoso.com 中创建新 RODC，力求尽可能系统提示您输入并确认屏蔽的密码：  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="additional-options"></a>其他选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
**其他选项**页面提供配置选项命名为复制源，域控制器，也可以使用任何域控制器为复制源。  
  
你还可以选择要域控制器使用安装媒体使用安装媒体 (IFM) 选项从备份。 **安装媒体的**复选框提供选中一次浏览选项，你必须单击**验证**以确保所提供的路径是有效的媒体。 从另一台现有 Windows Server 2012 计算机仅; IFM 选项通过使用媒体创建与 Windows Server 备份或 Ntdsutil.exe 你无法使用 Windows Server 2008 R2 或以前的操作系统创建媒体的 Windows Server 2012 域控制器。 有关 IFM 中的更改的详细信息，请参阅[Ntdsutil.exe 安装媒体更改从](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)。 如果使用 SYSKEY 受保护的媒体、服务器管理器会提示图像的密码在验证过程。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
**其他选项**ADDSDeployment cmdlet 参数均是：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>路径  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
**路径**页使你可以覆盖默认文件夹位置的广告 DS 数据库数据库事务日志中，并 SYSVOL 共享。 始终处于子目录 %系统根 %的默认位置。 **路径**ADDSDeployment cmdlet 参数均是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>查看选项并查看脚本  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
**回顾选项**页面，可以让你以验证你的设置并确保它们符合你的要求，开始安装之前。 这不是最后一次机会停止安装使用服务器管理器。 此页只可以进行检查，配置在继续之前，请确认你的设置。 **回顾选项**页面中服务器管理器中还提供了一个可选**查看脚本**按钮来创建 Unicode 文本文件包含当前 ADDSDeployment 配置为单个 Windows PowerShell 脚本。 这使你为 Windows PowerShell 部署 studio 使用服务器管理器图形界面。 使用 Active Directory 域服务配置向导配置选项中, 导出配置，然后取消向导。 此过程中创建进一步修改或直接使用有效和语法正确的示例。 例如：  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "corp.contoso.com" `  
-LogPath "C:\Windows\NTDS" `  
-SYSVOLPath "C:\Windows\SYSVOL" `  
-UseExistingAccount:$true `  
-Norebootoncompletion:$false  
-Force:$true  
  
```  
  
> [!NOTE]  
> 服务器管理器通常会填满所有参数与值时升级，不依赖于默认设置（如它们可能会更改之间将来版本的 Windows 或 service pack）中。 是一个例外**-safemodeadministratorpassword**参数。 若要强制确认提示忽略值运行 cmdlet 交互时  
  
使用可选**Whatif**具有参数**安装 ADDSDomainController** cmdlet 查看配置的信息。 这使你查看的参数 cmdlet 明确和隐式值。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件复选  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
**先决条件检查**是广告 DS 域配置中的新增功能。 此新阶段验证的服务器配置能够支持新的广告 DS 森林。  
  
当安装新林根域，服务器管理器 Active Directory 域服务配置向导调用一系列序列化模块化测试。 这些测试向你发出警报建议的维修选项。 你可以根据需要多次运行测试。 域控制器安装过程所有先决条件测试才可继续通过。  
  
**先决条件检查**还面，如会影响较旧操作系统的安全更新的相关信息。 先决条件检查的详细信息，请参阅[先决条件检查](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
你无法跳过**先决条件检查**时使用服务器管理器中，但你可以跳过此过程时使用使用下面的参数广告 DS 部署 cmdlet:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能会导致部分域控制器升级或损坏广告 DS 森林。  
  
单击**安装**开始域控制器升级过程中。 这是最后一次机会取消安装。 一旦开始后，你无法取消升级过程中。 在升级，无论推广结果的末尾，计算机重新启动将自动。  
  
### <a name="installation"></a>安装  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
当安装页面显示时，域控制器配置开始，无法暂停或取消。 详细的操作显示在此页面上，并且写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要安装使用 ADDSDeployment 模块新 Active Directory 森林，使用以下 cmdlet:  
  
```  
Install-addsdomaincontroller  
  
```  
  
请参阅[附加 RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS)必需和可选参数。  
  
**安装 addsdomaincontroller** cmdlet 都仅有两个阶段（先决条件检查和安装）。 两个下图显示与的最低要求参数安装阶段**-域名**，**-useexistingaccount**，并**-凭据**。 注意如何操作，就像服务器管理器**安装 ADDSDomainController**提醒你，升级将自动重新启动 server:  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
若要自动接受重新引导提示，请使用**-强制**或**-确认：$false**与任何 ADDSDeployment Windows PowerShell cmdlet 参数。 若要防止服务器推广末尾自动重新启动，请使用**-norebootoncompletion**参数。  
  
> [!WARNING]  
> 不建议重重新启动。 域控制器必须重新启动才能正常工作。  
  
### <a name="results"></a>结果  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**结果**页面显示的成功或失败的升级和管理的任何重要信息。 10 秒钟后，域控制器将自动重新启动。  
  
## <a name="rodc-without-staging-workflow"></a>不临时工作流 RODC  
下图说明 Active Directory 域服务配置过程中，当你之前安装的广告 DS 角色，并且已经开始使用现有的 Windows Server 2012 域中创建新的非转移只读域控制器服务器管理器 Active Directory 域服务配置向导。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>不临时 Windows PowerShell RODC  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-域名***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-站点名***<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-凭据***<br /><br />*-CriticalReplicationOnly*<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> **-凭据**参数才需要如果您不已登录的域管理员组成员。  
  
## <a name="rodc-without-staging-deployment"></a>不临时部署 RODC  
  
### <a name="deployment-configuration"></a>部署配置  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
服务器管理器开始与每域控制器升级**部署配置**页面。 其他选项和必填的字段在此页和后续页面，具体取决于哪个部署操作你选择的更改。  
  
若要添加到现有的 Windows Server 2012 域未分阶段只读域控制器，请选择**添加到现有的域的域控制器**单击**选择**到按钮**指定此域域信息**。 服务器管理器会自动将提示你输入有效的凭据，或者你可以单击**更改**。  
  
附加 RODC 需要在 Windows Server 2012 域管理员组中的成员。 Active Directory 域服务配置向导你稍后当提示你当前的凭据没有足够的权限或组成员。  
  
**部署配置**ADDSDeployment Windows PowerShell cmdlet 和参数是：  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
**域控制器选项**页上指定的新的域控制器域控制器功能。 配置域控制器功能**DNS 服务器**，**全球目录**，并**只读域控制器**。 Microsoft 建议所有域控制器都提供高可用性分布式环境中的 DNS 和 GC 服务。 GC 默认始终处于选中状态，并 DNS 服务器选择默认情况下，如果当前域主机 DNS 其域控制器上的已开始的颁发机构查询为基础。  
  
**域控制器选项**页面，还可以选择相应的 Active Directory 逻辑**站点名称**从森林配置。 默认情况下，选择最正确子网与该站点。 如果只有一个网站，它将自动选择该站点。  
  
> [!IMPORTANT]  
> 如果该服务器不属于 Active Directory 网，并且多个 Active Directory 站点，选中任何和**下一步**按钮不可用，直到你选择站点从列表。  
  
指定**目录服务还原模式密码**必须遵守密码策略应用到服务器。 始终可以选择复杂的强密码或最好 passphrase.The**域控制器选项**ADDSDeployment Windows PowerShell 参数均是：  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 网站的名称必须已经存在，当提供作为的参数**-站点名**。 **安装 AddsDomainController** cmdlet 不会创建网站的名称。 你可以使用 cmdlet**新 adreplicationsite**创建新的站点。  
  
**安装 ADDSDomainController**参数如果没有，请按照相同的默认值为服务器管理器。  
  
**SafeModeAdministratorPassword**特别是参数的操作：  
  
-   如果*未指定*作为参数，cmdlet 提示你输入并确认屏蔽的密码。 运行 cmdlet 交互时，这是首选的使用量。  
  
    例如，在 corp.contoso.com 中创建新 RODC，力求尽可能系统提示您输入并确认屏蔽的密码：  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="rodc-options"></a>RODC 选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
**RODC 选项**页面使您要修改的设置：  
  
-   委派的管理员帐户  
  
-   可以将密码复制到 RODC 的帐户  
  
-   复制到 RODC 密码拒绝帐户  
  
委派的管理员帐户获得 RODC 到本地管理权限。 这些用户可以使用本地计算机管理员组等效权限操作。  他们并不域管理员或域内置管理员组中的成员。 此选项可用于委派分支机构管理不提供了域管理权限。 配置管理委派不是必需的。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
资源或通过 Active Directory 提供的功能，无法访问不允许 RODC 上的缓存密码和无法连接并验证可写的域控制器的帐户。  
  
> [!IMPORTANT]  
> 如果不会修改，使用默认组和设置：  
>   
> -   管理员-拒绝  
> -   服务器运营商的拒绝  
> -   备份运营商的拒绝  
> -   帐户运营商的拒绝  
> -   拒绝 RODC 密码复制组-拒绝  
> -   允许 RODC 密码复制组-允许  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>其他选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
**其他选项**页面提供配置选项命名为复制源，域控制器，也可以使用任何域控制器为复制源。  
  
你还可以选择要域控制器使用安装媒体使用安装媒体 (IFM) 选项从备份。 **安装媒体的**复选框提供选中一次浏览选项，你必须单击**验证**以确保所提供的路径是有效的媒体。 从另一台现有 Windows Server 2012 计算机仅; IFM 选项通过使用媒体创建与 Windows Server 备份或 Ntdsutil.exe 你无法使用 Windows Server 2008 R2 或以前的操作系统创建媒体的 Windows Server 2012 域控制器。  附录 IFM 中的更改提供了详细信息。 如果使用 SYSKEY 受保护的媒体、服务器管理器会提示图像的密码在验证过程。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
其他选项 ADDSDeployment cmdlet 参数是：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>路径  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
**路径**页使你可以覆盖默认文件夹位置的广告 DS 数据库数据库事务日志中，并 SYSVOL 共享。 始终处于子目录 %系统根 %的默认位置。 **路径**ADDSDeployment cmdlet 参数均是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>准备选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
**准备选项**页面向你发送警报广告 DS 配置包括扩展架构 (forestprep) 和更新域（域）。 森林或域不已准备好以前的 Windows Server 2012 域控制器安装或通过手动运行 Adprep.exe 时，你只看到此页面。 例如，Active Directory 域服务配置向导取消此页面，如果您添加到现有的 Windows Server 2012 森林根域的新副本域控制器。  
  
在你单击扩展架构和更新域不发生**下一步**。 这些事件发生仅在安装阶段。 此页面只需将显示有关将更高版本中安装发生的事件感知。  
  
此页还验证的当前用户凭据成员的方案管理员和企业版管理员组中，您可以根据需要扩展架构或准备域这些组中的成员。 单击**更改**提供足够的用户的凭据，如果页通知你当前的凭据不会提供足够的权限。  
  
其他选项 ADDSDeployment cmdlet 参数是：  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> 与以前版本的 Windows Server，Windows Server 2012 自动的域准备不运行 GPPREP。 运行**adprep.exe /gpprep**手动为所有之前未 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的域。 在某个域，不能与每次升级的历史记录，应运行一次只能 GPPrep。 不能运行 Adprep.exe /gpprep 自动因为其操作可能导致在 SYSVOL 文件夹中，重新复制所有域控制器上的所有文件和文件夹。  
>   
> 自动 RODCPrep 运行提升某个域中的第一个未分阶段的 RODC 时。 它不会触发推广第一个可写的 Windows Server 2012 域控制器。 你也仍然手动可以运行**adprep.exe /rodcprep**如果你计划部署只读域控制器。  
  
### <a name="review-options-and-view-script"></a>查看选项并查看脚本  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
**回顾选项**页面，可以让你以验证你的设置并确保它们符合你的要求，开始安装之前。 这不是最后一次机会停止安装使用服务器管理器。 此页只可以进行检查，配置在继续之前，请确认你的设置。  
  
**回顾选项**页面中服务器管理器中还提供了一个可选**查看脚本**按钮来创建 Unicode 文本文件包含当前 ADDSDeployment 配置为单个 Windows PowerShell 脚本。 这使你为 Windows PowerShell 部署 studio 使用服务器管理器图形界面。 使用 Active Directory 域服务配置向导配置选项中, 导出配置，然后取消向导。 此过程中创建进一步修改或直接使用有效和语法正确的示例。 例如：  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-AllowPasswordReplicationAccountName @("CORP\Allowed RODC Password Replication Group", "CORP\Chicago RODC Admins", "CORP\Chicago RODC Users and Computers") `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DelegatedAdministratorAccountName "CORP\Chicago RODC Admins" `  
-DenyPasswordReplicationAccountName @("BUILTIN\Administrators", "BUILTIN\Server Operators", "BUILTIN\Backup Operators", "BUILTIN\Account Operators", "CORP\Denied RODC Password Replication Group") `  
-DomainName "corp.contoso.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-ReadOnlyReplica:$true `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> 服务器管理器通常会填满所有参数与值时升级，不依赖于默认设置（如它们可能会更改之间将来版本的 Windows 或 service pack）中。 是一个例外**-safemodeadministratorpassword**参数。 若要强制确认提示，请运行 cmdlet 交互时忽略值。  
  
使用与 Install-ADDSDomainController cmdlet 可选 Whatif 参数查看配置的信息。 这使你查看的参数 cmdlet 明确和隐式值。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件复选  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
**先决条件检查**是广告 DS 域配置中的新增功能。 此新阶段验证的服务器配置能够支持新的广告 DS 森林。  
  
当安装新林根域，服务器管理器 Active Directory 域服务配置向导调用一系列序列化模块化测试。 这些测试向你发出警报建议的维修选项。 你可以根据需要多次运行测试。 域控制器过程所有先决条件测试才可继续通过。  
  
**先决条件检查**还面，如会影响较旧操作系统的安全更新的相关信息。  
  
你无法跳过**先决条件检查**时使用服务器管理器中，但你可以跳过此过程时使用使用下面的参数广告 DS 部署 cmdlet:  
  
```  
-skipprechecks  
  
```  
  
单击**安装**开始域控制器升级过程中。 这是最后一次机会取消安装。 一旦开始后，你无法取消升级过程中。 在升级，无论推广结果的末尾，计算机重新启动将自动。  
  
### <a name="installation"></a>安装  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
当**安装**页面显示时，域控制器配置开始，无法将停止或取消。 详细的操作显示在此页面上，并且写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要安装使用 ADDSDeployment 模块新 Active Directory 森林，使用以下 cmdlet:  
  
```  
Install-addsdomaincontroller  
  
```  
  
请参阅**ADDSDeployment Cmdlet**表格中的此部分中所需和可选参数 begininng。  
  
**安装 addsdomaincontroller** cmdlet 都仅有两个阶段（先决条件检查和安装）。 两个下图显示与的最低要求参数安装阶段**-域名**，**-readonlyreplica**，**-站点名**，并**-凭据**。 注意如何操作，就像服务器管理器**安装 ADDSDomainController**提醒你，升级将自动重新启动 server:  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
若要自动接受重新引导提示，请使用**-强制**或**-确认：$false**与任何 ADDSDeployment Windows PowerShell cmdlet 参数。 若要防止服务器推广末尾自动重新启动，请使用**-norebootoncompletion**参数。  
  
> [!WARNING]  
> 不建议重重新启动。 域控制器必须重新启动才能正常工作。 如果注销域控制器，无法交互式登录后，直到重新启动它。  
  
### <a name="results"></a>结果  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
**结果**页面显示的成功或失败的升级和管理的任何重要信息。 10 秒钟后，域控制器将自动重新启动。  
  

