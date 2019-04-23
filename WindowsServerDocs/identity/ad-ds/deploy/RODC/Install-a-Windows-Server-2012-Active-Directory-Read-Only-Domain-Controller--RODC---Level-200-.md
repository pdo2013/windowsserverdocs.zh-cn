---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: 安装 Windows Server 2012 Active Directory 只读域控制器 (RODC)（级别 200）
description: 本主题介绍如何创建分步的 RODC 帐户，然后在 RODC 安装期间将服务器附加到该帐户。 本主题还说明了如何在不执行分步安装的情况下安装 RODC。
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c8d34d7b35f3cd5209fd6096f69b16162229bc3a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863078"
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>安装 Windows Server 2012 Active Directory 只读域控制器 (RODC)（级别 200）

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍如何创建分步的 RODC 帐户，然后在 RODC 安装期间将服务器附加到该帐户。 本主题还说明了如何在不执行分步安装的情况下安装 RODC。  
  
## <a name="stage-rodc-workflow"></a>分步 RODC 工作流  
分步的只读域控制器 (RODC) 安装包括两个独立的阶段：  
  
1.  分步创建未占用的计算机帐户  
  
2.  在升级期间将 RODC 附加到该帐户  
  
下图阐释了 Active Directory 域服务只读域控制器分步过程，在此过程中你使用 Active Directory 管理中心 (Dsac.exe) 在域中创建空白 RODC 计算机帐户。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>分步 RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|Add-addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-DomainName***<br /><br />***-SiteName***<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />***-Credential***<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />-ReplicationSourceDC|  
  
> [!NOTE]  
> 仅当你尚未作为 Domain Admins 组成员登录时，才需要 **-credential** 参数。  
  
## <a name="attach-rodc-workflow"></a>附加 RODC 工作流  
下图阐释了 Active Directory 域服务配置过程，你已在其中安装了 AD DS 角色、分步创建 RODC 帐户，并使用服务器管理器开始了“将此服务器升级到域控制器”以在现有域中创建新的 RODC，并将其附加到分步的计算机帐户。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>附加 RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|Install-AddsDomaincontroller|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-LogPath*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> 仅当你尚未作为 Domain Admins 组成员登录时，才需要 **-credential** 参数。  
  
## <a name="staging"></a>分步  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
通过打开 Active Directory 管理中心 (**Dsac.exe**)，你可以执行只读域控制器的计算机帐户的分步操作。 单击导航窗格中的域名。 双击管理列表中的“域控制器”  。 单击任务窗格中的“预创建只读域控制器帐户”。  
  
有关 Active Directory 管理中心的详细信息，请参阅[高级的 AD DS 管理使用 Active Directory Administrative Center&#40;级别 200&#41; ](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)并查看[Active Directory管理中心：入门](https://technet.microsoft.com/library/dd560651(WS.10).aspx)。  
  
如果你有创建只读域控制器的经验，你将发现安装向导的图形界面与在使用 Windows Server 2008 中较早版本 Active Directory 用户和计算机管理单元时看到的相同，并且使用相同的代码，这包括采用由过时的 dcpromo 使用的无人参与文件格式导出配置。  
  
Windows Server 2012 引入了新的 ADDSDeployment cmdlet 以分步创建 RODC 计算机帐户，但是向导不将该 cmdlet 用于其操作。 以下部分显示等效的 cmdlet 和参数，以便使每种方法的相关信息更简单易懂。  
  
**预创建只读域控制器帐户**Active Directory 管理中心内的任务窗格中的链接，相当于 ADDSDeployment Windows PowerShell cmdlet:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>欢迎  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
**欢迎使用 Active Directory 域服务安装向导**对话框有一个名为“使用高级模式安装”的选项。 选择此选项并单击“下一步”以显示密码复制策略选项。 清除此选项以使用密码复制策略选项的默认值（本部分稍后将进一步详细讨论）。  
  
### <a name="network-credentials"></a>网络凭据  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
“网络凭据”  对话框中的域名选项显示 Active Directory 管理中心默认针对的域。 默认情况下，使用你的当前凭据。 如果它们不包括 Domain Admins 组的成员身份，请单击“备用凭据” ，并单击“设置”  以向向导提供作为 Domain Admins 组成员的用户名和密码。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-credential <pscredential>  
```  
  
请记住，分步系统是来自 Windows Server 2008 R2 的直接端口，并且不提供新的 Adprep 功能。 如果你计划部署分步 RODC 帐户，你必须首先在该域中部署未分步的 RODC 以运行自动 rodcprep 操作，或者首先手动运行 adprep.exe /rodcprep。  
  
否则，你将收到错误“你将无法在此域中安装只读域控制器，因为‘adprep /rodcprep’尚未运行”。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>指定计算机名称  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
“指定计算机名称”  对话框需要你输入不存在的域控制器的单标签“计算机名称”  。 你之后配置并附加到此帐户的域控制器必须具有相同的名称，否则升级操作将不会检测出分步的帐户。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>选择一个站点  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
“选择一个站点”  对话框显示当前林的 Active Directory 站点列表。 分步只读域控制器操作需要你从该列表中选择单个站点。 RODC 使用此信息在配置分区中创建其 NTDS 设置对象，并在部署后首次启动时将其自身加入正确的站点中。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>其他域控制器选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
“其他域控制器选项”  对话框使你可以指定域控制器包括作为“DNS 服务器”  运行和作为“全局编录” 运行。 Microsoft 建议只读域控制器提供 DNS 和 GC 服务，因此将默认安装两者；RODC 角色的一个意图是分支机构方案，其中广域网可能不可用，而且在没有 DNS 和全局编录服务的情况下，分支机构中的计算机将无法使用 AD DS 资源和功能。  
  
“只读域控制器 (RODC)”选项是预选中的选项，并且无法禁用。 等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> 默认情况下 **-NoGlobalCatalog**值是 $false，这意味着如果未指定参数，域控制器将是全局编录服务器。  
  
### <a name="specify-the-password-replication-policy"></a>指定密码复制策略  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
“指定密码复制策略”  对话框使你可以修改允许在此只读域控制器上缓存密码的帐户的默认列表。 此列表中使用“Deny”  配置的帐户或不在此列表中的帐户（隐式）不会缓存它们的密码。 不被允许在 RODC 上缓存密码并且无法连接并身份验证到可写域控制器的帐户无法访问 Active Directory 提供的资源或功能。  
  
> [!IMPORTANT]  
> 仅当你选择欢迎屏幕上的“使用高级模式安装”  复选框时，向导显示此对话框。 如果你清除此复选框，则向导将使用以下默认组和值：  
>   
> -   管理员 - Deny  
> -   服务器操作员 - Deny  
> -   备份操作员 - Deny  
> -   帐户操作员 - Deny  
> -   拒绝的 RODC 密码复制组 - Deny  
> -   允许的 RODC 密码复制组 - Allow  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>RODC 安装和管理的委派  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
“RODC 安装和管理的委派”对话框使你可以配置允许将服务器附加到 RODC 计算机帐户的用户或包含用户的组。 单击“设置”以浏览用户或组的域。 此对话框中指定的用户或组获取 RODC 的本地管理权限。 指定的用户或指定组的成员可以执行使用等效于计算机的 Administrators 组的权限在 RODC 上的操作。 他们 *不是* Domain Admins 或域内置 Administrators 组的成员。  
  
使用此选项委派分支机构管理，而无需授予 Domain Admins 组分支管理员成员身份。 不需要委派 RODC 管理。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>总结  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
“摘要”  对话框使你可以确认你的设置。 这是在向导创建分步帐户前停止安装的最后机会。 当你准备好创建分步 RODC 计算机帐户时，单击“下一步”。  单击“导出设置”  以使用过时的 dcpromo 无人参与文件格式保存应答文件。  
  
### <a name="creation"></a>创建  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
“Active Directory 域服务安装向导”在 Active Directory 中创建分步只读域控制器。 你无法在其开始后取消此操作。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
使用以下 cmdlet 通过 ADDSDeployment Windows PowerShell 模块分步创建只读域控制器计算机帐户：  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
有关必需和可选参数，请参阅[分步 RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS)。  
  
由于 **Add-addsreadonlydomaincontrolleraccount** 仅有一个包含两个阶段（先决条件检查和安装）的操作，因此以下屏幕截图显示带有最少必需参数的安装阶段。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
分步 RODC 操作在 Active Directory 中创建 RODC 计算机帐户。 Active Directory 管理中心将“域控制器类型”显示为“未占用域控制器帐户”。 此域控制器类型显示分步 RODC 帐户已经为服务器附加为只读域控制器做好准备。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> Active Directory 管理中心不再需要将服务器附加到只读域控制器计算机帐户。 使用服务器管理员和 Active Directory 域服务配置向导或者 ADDSDeployment Windows PowerShell 模块 cmdlet **Install-AddsDomainController** 将新的 RODC 附加到其分步帐户。 这些步骤与将新的可写域控制器添加到现有域类似，例外之处是分步 RODC 计算机帐户包含的配置选项在你分步创建 RODC 计算机帐户时已经决定。  
  
## <a name="attaching"></a>附加  
  
### <a name="deployment-configuration"></a>部署配置  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
服务器管理器从“部署配置”  页开始进行每个域控制器升级。 其余选项和必填字段在此页面和后续页面上会有所变化，这视所选部署操作而定。  
  
若要将只读域控制器添加到现有域，选择“向现有域添加域控制器”并单击“选择”按钮以“指定此域的域信息”。 服务器管理器自动提示你提供有效的凭据，或者你可以单击“更改” 。  
  
附加 RODC 需要 Windows Server 2012 中 Domain Admins 组的成员身份。 如果当前凭据没有足够权限或组成员身份，Active Directory 域服务配置向导将在稍后给出提示。  
  
**部署配置** ADDSDeployment Windows PowerShell cmdlet 和参数是：  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
“域控制器选项”页面显示用于新域控制器的域控制器选项。 当此页面加载时，Active Directory 域服务配置向导向现有域控制器发送 LDAP 查询以检查未占用的帐户。 如果查询将查找未占用的域控制器共享相同的名称与当前计算机的计算机帐户，则该向导将在页面顶部显示一条信息性消息"**与名称匹配的预创建的 RODC 帐户目标的服务器存在的目录中。选择是否要使用此现有 RODC 帐户还是重新安装此域控制器**。" 向导使用“使用现有 RODC 帐户”作为默认配置。  
  
> [!IMPORTANT]  
> 当域控制器遇到物理问题且无法恢复运行时，你可以使用“重新安装此域控制器”  选项。 在配置替换域控制器时，通过在 Active Directory 中保留域控制器计算机帐户和对象元数据，这可以节省时间。 使用 *相同名称*安装新计算机并将其升级为域中的域控制器。 **重新安装此域控制器**选项将不可用，如果从 Active Directory （元数据清理） 删除域控制器对象的元数据。  
  
将服务器附加到 RODC 计算机帐户时，你无法配置域控制器选项。 将在创建分步 RODC 计算机帐户时配置域控制器选项。  
  
指定的“目录服务还原模式密码”必须遵守应用到服务器的密码策略。 总是选择复杂强密码或首选密码。  
  
**域控制器选项** ADDSDeployment Windows PowerShell 参数是：  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 作为参数提供给 **-sitename** 时，此站点名称必须已存在。 **install-AddsDomainController** cmdlet 不创建站点名称。 可以使用 cmdlet **new-adreplicationsite** 创建新站点。  
  
**Install-ADDSDomainController** 参数使用与服务器管理器相同的默认值（如果未指定）。  
  
**SafeModeAdministratorPassword** 参数的操作是特殊操作：  
  
-   如果 *未指定* 为参数，cmdlet 将提示你输入并确认掩蔽密码。 以交互方式运行 cmdlet 时，这是首选用法。  
  
    例如，要在 corp.contoso.com 中创建新的 RODC，并且收到输入并确认掩蔽密码的提示：  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="additional-options"></a>其他选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
“其他选项”  页面提供用于将域控制器命名为复制源的配置选项，或者你可以将任何域控制器用作复制源。  
  
你还可以使用“从媒体安装 (IFM)”选项选择使用备份的媒体安装域控制器。 选中后，“从媒体安装”  复选框提供浏览器选项，而且你必须单击“验证”  以确保所提供的路径是有效的媒体。 仅从另一个现有 Windows Server 2012 计算机使用 Windows Server Backup 或 Ntdsutil.exe 创建由 IFM 选项使用的媒体；无法使用 Windows Server 2008 R2 或之前的操作系统为 Windows Server 2012 域控制器创建媒体。 有关 IFM 中的更改的详细信息，请参阅 [Ntdsutil.exe 从媒体安装更改](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)。 如果使用受 SYSKEY 保护的媒体，服务器管理器将在验证期间提示输入图像的密码。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
**其他选项** ADDSDeployment cmdlet 参数是：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>路径  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
“路径”页可以用于覆盖 AD DS 数据库、数据库事务日志和 SYSVOL 共享的默认文件夹位置。 默认位置始终位于 %systemroot% 的子目录中。 **路径** ADDSDeployment cmdlet 参数是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>审查选项和查看脚本  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
“审查” 选项  页可以用于验证设置并确保在开始安装前满足要求。 这不是停止使用服务器管理器安装的最后一次机会。 此页只是让你先查看和确认设置，然后再继续配置。 服务器管理器中的“审查选项”  页还提供可选的“查看脚本”  按钮，可将包含当前 ADDSDeployment 配置的 Unicode 文本文件创建为一个 Windows PowerShell 脚本。 这样，你可以将服务器管理器图形界面用作 Windows PowerShell 部署工作台。 使用 Active Directory 域服务配置向导来配置选项、导出配置和取消向导。 此过程将创建有效且语法正确的示例，以便做进一步修改或直接使用。 例如：  
  
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
> 服务器管理器通常在升级时使用值填充所有参数，并且不依赖默认值（因为它们可能在 Windows 或服务包以后的版本之间发生更改）。 但该情况的一个例外是 **-safemodeadministratorpassword** 参数。 若要强制确认提示，请在以交互方式运行 cmdlet 时省略该值  
  
将可选 **Whatif** 参数与 **Install-ADDSDomainController** cmdlet 一起使用以查看配置信息。 这使你可以查看 cmdlet 的参数的显式和隐式值。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件检查  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
“先决条件检查”  是 AD DS 域配置中的新功能。 此新阶段验证服务器配置是否能够支持新的 AD DS 林。  
  
在安装新目录林根级域时，服务器管理器 Active Directory 域服务配置向导将调用一系列序列化的模块化测试。 这些测试向你提出警告并提供建议的修复选项。 你可以根据需要多次运行测试。 域控制器安装进程在所有先决条件测试通过前无法继续。  
  
“先决条件检查”还显示相关的信息，例如影响较早版本的操作系统的安全性更改。 有关先决条件检查的详细信息，请参阅 [Prerequisite Checking](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
使用服务器管理器时你无法绕过“先决条件检查”  ，但是你可以在使用 AD DS 部署 cmdlet 时使用以下参数跳过该进程：  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能导致部分域控制器升级或 AD DS 林损坏。  
  
单击“安装”以开始域控制器升级进程。 这是取消安装的最后机会。 一旦开始升级过程，你无法取消它。 无论升级结果如何，计算机将在升级结束时自动重新启动。  
  
### <a name="installation"></a>安装  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
当“安装”页显示时，域控制器配置将开始，并且无法暂停或取消。 详细操作显示在此页面上并将写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要使用 ADDSDeployment 模块安装新的 Active Directory 林，请使用以下 cmdlet：  
  
```  
Install-addsdomaincontroller  
  
```  
  
请参阅 [Attach RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) 以了解必需和可选参数。  
  
**Install-addsdomaincontroller** cmdlet 仅有两个阶段（先决条件检查和安装）。 下面两个图片显示安装阶段，并带有最少的所需参数 **-domainname**、 **-useexistingaccount**和 **-credential**。 请注意 **Install-ADDSDomainController** 如何提醒你升级将自动重新启动服务器，就像服务器管理器一样：  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
若要自动接受重新启动提示，请将 **-force** 或 **-confirm:$false** 参数与任何 ADDSDeployment Windows PowerShell cmdlet 一起使用。 若要防止服务器在升级结束时自动重新启动，请使用 **-norebootoncompletion** 参数。  
  
> [!WARNING]  
> 不建议重写重新启动。 域控制器必须重新启动才能正常工作。  
  
### <a name="results"></a>结果  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
“结果”  页面显示升级是成功还是失败以及任何重要的管理信息。 域控制器将在 10 秒后自动重新启动。  
  
## <a name="rodc-without-staging-workflow"></a>没有分步工作流的 RODC  
下图说明了以下情况中的 Active Directory 域服务配置过程：你以前安装了 AD DS 角色，并已使用服务器管理器启动 Active Directory 域服务配置向导，以在现有 Windows Server 2012 域中创建新的非分步只读域控制器。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>没有分步 Windows PowerShell 的 RODC  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-SiteName***<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-CriticalReplicationOnly*<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> 仅当你尚未作为 Domain Admins 组成员登录时，才需要 **-credential** 参数。  
  
## <a name="rodc-without-staging-deployment"></a>没有分步部署的 RODC  
  
### <a name="deployment-configuration"></a>部署配置  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
服务器管理器从“部署配置”  页开始进行每个域控制器升级。 其余选项和必填字段在此页面和后续页面上会有所变化，这视所选部署操作而定。  
  
若要将未分步的只读域控制器添加到现有 Windows Server 2012 域中，选择“向现有域添加域控制器”并单击“选择”按钮以“指定此域的域信息”。 服务器管理器自动提示你提供有效的凭据，或者你可以单击“更改” 。  
  
附加 RODC 需要 Windows Server 2012 中 Domain Admins 组的成员身份。 如果当前凭据没有足够权限或组成员身份，Active Directory 域服务配置向导将在稍后给出提示。  
  
**部署配置** ADDSDeployment Windows PowerShell cmdlet 和参数是：  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>域控制器选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
“域控制器选项”页面指定用于新域控制器的域控制器功能。 可配置的域控制器功能为“DNS 服务器”、“全局编录”和“只读域控制器”。 Microsoft 建议所有域控制器提供 DNS 和 GC 服务以实现分布式环境中的高可用性。 始终默认选中 GC；如果当前域托管的 DNS 已在其 DC 上（基于起始授权机构查询），则默认选中 DNS 服务器。  
  
“域控制器选项”页还让你可以从林配置中选择相应的 Active Directory 逻辑“站点名称”。 默认情况下，将选择具有最合适子网的站点。 如果只有一个站点，将自动选择该站点。  
  
> [!IMPORTANT]  
> 如果该服务器不属于 Active Directory 子网，并且有多个 Active Directory 站点，则不选择任何内容且“下一步”按钮不可用，直到你从列表选择一个站点。  
  
指定的“目录服务还原模式密码”必须遵守应用到服务器的密码策略。 总是选择复杂的强密码（或首选密码）。 **域控制器选项** ADDSDeployment Windows PowerShell 参数是：  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 作为参数提供给 **-sitename** 时，此站点名称必须已存在。 **install-AddsDomainController** cmdlet 不创建站点名称。 可以使用 cmdlet **new-adreplicationsite** 创建新站点。  
  
**Install-ADDSDomainController** 参数使用与服务器管理器相同的默认值（如果未指定）。  
  
**SafeModeAdministratorPassword** 参数的操作是特殊操作：  
  
-   如果 *未指定* 为参数，cmdlet 将提示你输入并确认掩蔽密码。 以交互方式运行 cmdlet 时，这是首选用法。  
  
    例如，要在 corp.contoso.com 中创建新的 RODC，并且收到输入并确认掩蔽密码的提示：  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="rodc-options"></a>RODC 选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
“RODC 选项”页支持你修改设置：  
  
-   委派的管理员帐户  
  
-   允许将密码复制到 RODC 的帐户  
  
-   拒绝将密码复制到 RODC 的帐户  
  
委派的管理员帐户获得 RODC 的本地管理权限。 这些用户可以使用等效于本地计算机的 Administrators 组的权限运行。  他们不是 Domain Admins 或域内置管理员组的成员。 在不指派域管理权限的情况下委派分支机构管理时，这样选择很有用。 不需要配置管理委派。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
不被允许在 RODC 上缓存密码并且无法连接并身份验证到可写域控制器的帐户无法访问 Active Directory 提供的资源或功能。  
  
> [!IMPORTANT]  
> 如果未修改，则使用默认组和设置：  
>   
> -   管理员 - Deny  
> -   服务器操作员 - Deny  
> -   备份操作员 - Deny  
> -   帐户操作员 - Deny  
> -   拒绝的 RODC 密码复制组 - Deny  
> -   允许的 RODC 密码复制组 - Allow  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>其他选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
“其他选项”  页面提供用于将域控制器命名为复制源的配置选项，或者你可以将任何域控制器用作复制源。  
  
你还可以使用“从媒体安装 (IFM)”选项选择使用备份的媒体安装域控制器。 选中后，“从媒体安装”  复选框提供浏览器选项，而且你必须单击“验证”  以确保所提供的路径是有效的媒体。 仅从另一个现有 Windows Server 2012 计算机使用 Windows Server Backup 或 Ntdsutil.exe 创建由 IFM 选项使用的媒体；无法使用 Windows Server 2008 R2 或之前的操作系统为 Windows Server 2012 域控制器创建媒体。  附录提供了有关 IFM 中的更改的详细信息。 如果使用受 SYSKEY 保护的媒体，服务器管理器将在验证期间提示输入图像的密码。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
其他选项 ADDSDeployment cmdlet 参数是：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>路径  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
“路径”页可以用于覆盖 AD DS 数据库、数据库事务日志和 SYSVOL 共享的默认文件夹位置。 默认位置始终位于 %systemroot% 的子目录中。 **路径** ADDSDeployment cmdlet 参数是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>准备选项  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
“准备选项”  页面提示你 AD DS 配置包括扩展架构 (forestprep) 和更新该域 (domainprep)。 仅当之前的 Windows Server 2012 域控制器安装未准备好林或域或者手动运行 Adprep.exe 时，你才会看到此页面。 例如，如果你将新副本域控制器添加到现有 Windows Server 2012 目录林根级域，Active Directory 域服务配置向导将取消此页面。  
  
当你单击“下一步” 时，扩展架构和更新域操作不会发生。 这些事件仅在安装阶段发生。 此页面只是为了使你注意到稍后将在安装中发生的事件。  
  
此页面还验证当前用户凭据是 Schema Admin 和 Enterprise Admins 组的成员，因为你需要这些组的成员身份以扩展架构或准备域。 如果该页面通知你当前凭据未提供足够的权限，单击“更改”以提供足够的用户凭据。  
  
“其他选项”ADDSDeployment cmdlet 参数是：  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> 与之前版本的 Windows Server，Windows Server 2012 的自动化域准备不运行 GPPREP。 为之前没有针对 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的所有域手动运行 **adprep.exe /gpprep** 。 你应当在域的历史记录中仅运行一次 GPPrep，而不是在每次升级时都运行。 Adprep.exe 不自动运行 /gpprep，因为它的操作可能导致 SYSVOL 文件夹中的所有文件和文件夹在所有域控制器上重新复制。  
>   
> 自动 RODCPrep 将在你升级域中第一个未分步的 RODC 时运行。 当你升级第一个可写 Windows Server 2012 域控制器时，它不会发生。 如果你计划部署只读域控制器，你也仍然可以手动运行 **adprep.exe /rodcprep**。  
  
### <a name="review-options-and-view-script"></a>审查选项和查看脚本  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
“审查” 选项  页可以用于验证设置并确保在开始安装前满足要求。 这不是停止使用服务器管理器安装的最后一次机会。 此页只是让你先查看和确认设置，然后再继续配置。  
  
服务器管理器中的“审查选项”  页还提供可选的“查看脚本”  按钮，可将包含当前 ADDSDeployment 配置的 Unicode 文本文件创建为一个 Windows PowerShell 脚本。 这样，你可以将服务器管理器图形界面用作 Windows PowerShell 部署工作台。 使用 Active Directory 域服务配置向导来配置选项、导出配置和取消向导。 此过程将创建有效且语法正确的示例，以便做进一步修改或直接使用。 例如：  
  
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
> 服务器管理器通常在升级时使用值填充所有参数，并且不依赖默认值（因为它们可能在 Windows 或服务包以后的版本之间发生更改）。 但该情况的一个例外是 **-safemodeadministratorpassword** 参数。 若要强制执行确认提示，则在以交互方式运行 cmdlet 时省略该值。  
  
使用带有 Install-ADDSDomainController cmdlet 的 Whatif 参数查看配置信息。 这使你可以查看 cmdlet 的参数的显式和隐式值。  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>先决条件检查  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
“先决条件检查”  是 AD DS 域配置中的新功能。 此新阶段验证服务器配置是否能够支持新的 AD DS 林。  
  
在安装新目录林根级域时，服务器管理器 Active Directory 域服务配置向导将调用一系列序列化的模块化测试。 这些测试向你提出警告并提供建议的修复选项。 你可以根据需要多次运行测试。 域控制器进程在所有先决条件测试通过前无法继续。  
  
“先决条件检查”还显示相关的信息，例如影响较早版本的操作系统的安全性更改。  
  
使用服务器管理器时你无法绕过“先决条件检查”  ，但是你可以在使用 AD DS 部署 cmdlet 时使用以下参数跳过该进程：  
  
```  
-skipprechecks  
  
```  
  
单击“安装”以开始域控制器升级进程。 这是取消安装的最后机会。 一旦开始升级过程，你无法取消它。 无论升级结果如何，计算机将在升级结束时自动重新启动。  
  
### <a name="installation"></a>安装  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
当“安装”  页显示时，域控制器配置将开始，并且无法暂停或取消。 详细操作显示在此页面上并将写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要使用 ADDSDeployment 模块安装新的 Active Directory 林，请使用以下 cmdlet：  
  
```  
Install-addsdomaincontroller  
  
```  
  
有关必需和可选参数，请参阅此部分开头的“ADDSDeployment Cmdlet”表。  
  
**Install-addsdomaincontroller** cmdlet 仅有两个阶段（先决条件检查和安装）。 下面的两个图显示安装阶段，并带有最少的所需参数 **-domainname**、 **-readonlyreplica**、 **-sitename**和 **-credential**。 请注意 **Install-ADDSDomainController** 如何提醒你升级将自动重新启动服务器，就像服务器管理器一样：  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
若要自动接受重新启动提示，请将 **-force** 或 **-confirm:$false** 参数与任何 ADDSDeployment Windows PowerShell cmdlet 一起使用。 若要防止服务器在升级结束时自动重新启动，请使用 **-norebootoncompletion** 参数。  
  
> [!WARNING]  
> 不建议重写重新启动。 域控制器必须重新启动才能正常工作。 如果注销域控制器，则你无法以交互方式重新登录，直到重新启动它。  
  
### <a name="results"></a>结果  
![安装 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
“结果”  页面显示升级是成功还是失败以及任何重要的管理信息。 域控制器将在 10 秒后自动重新启动。  
  

