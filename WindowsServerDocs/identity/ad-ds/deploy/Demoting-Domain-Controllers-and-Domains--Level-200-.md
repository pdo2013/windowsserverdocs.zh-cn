---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: "降级域控制器和域 (级别 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c254a01da5534c1ddc673bc1e60382c166ddeda7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="demoting-domain-controllers-and-domains-level-200"></a>降级域控制器和域 (级别 200)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍如何删除广告 DS，使用服务器管理器或 Windows PowerShell。  
  
-   [广告 DS 删除流](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Workflow)  
  
-   [降级和角色删除 Windows PowerShell](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_PS)  
  
-   [降级](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Demote)  
  
## <a name="BKMK_Workflow"></a>广告 DS 删除流  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  
  
> [!CAUTION]  
> 升级到某个域控制器不支持，这将阻止服务器正常启动后，请删除 Dism.exe 或 DISM Windows PowerShell 模块的广告 DS 角色。  
>   
> 像服务器管理器或 ADDSDeployment 为 Windows PowerShell 模块，DISM 是原始 servicing 系统具有广告 DS 没有本身知道或其配置。 不要使用 Dism.exe 或 DISM Windows PowerShell 模块除非服务器不再域控制器卸载广告 DS 角色。  
  
## <a name="BKMK_PS"></a>降级和角色删除 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment 和 ServerManager Cmdlet**|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-确认<br /><br />***-凭据***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-强制<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|卸载-WindowsFeature/删除-WindowsFeature|***-名称***<br /><br />***-IncludeManagementTools***<br /><br />*重启*<br /><br />-删除<br /><br />-强制<br /><br />-计算机名称<br /><br />-凭据<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> **-凭据**参数为所需如果你未已登录作为企业管理员组（降级域中的最后一个 DC）的成员仅或域管理员组（降级副本 DC).The **-includemanagementtools**参数才需要你是否想要删除所有广告 DS 管理实用程序。  
  
## <a name="BKMK_Demote"></a>降级  
  
### <a name="remove-roles-and-features"></a>删除角色和功能  
Server 管理器中将提供两个接口删除 Active Directory 域服务角色：  
  
-   **管理**菜单主要仪表板上的使用**删除角色和功能**  
  
 ![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  
  
-   单击**广告 DS**或**所有服务器**导航窗格中。 向下滚动到**角色和功能**部分。 右键单击**Active Directory 域服务**中**角色和功能**列表，然后单击**删除角色或功能**。 跳过此接口**选择服务器**页面。  
  
 ![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  
  
ServerManager cmdlet**卸载 WindowsFeature**和**删除 WindowsFeature**阻止你删除广告 DS 角色，直到降级域控制器。  
  
### <a name="server-selection"></a>选择服务器  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  
  
**选择服务器**对话框，您可以选择从一台服务器之前将其添加到池，只要它是访问。 本地服务器运行服务器管理器是始终自动可用。  
  
### <a name="server-roles-and-features"></a>服务器角色和功能  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  
  
清除**Active Directory 域服务**复选框以降级域控制器;如果该服务器目前域控制器，这不会删除广告 DS 角色和改为切换到**验证结果**通过在优惠降级对话框。 否则，只需删除任何其他角色功能等二进制文件。  
  
-   如果你想要立即再次推广域控制器，不要删除任何其他广告 DS 相关角色或功能-例如 DNS、GPMC，或者 RSAT 工具。 删除其他角色和功能随着时间重新提升，服务器管理器重新安装这些功能，当你重新安装角色。  
  
-   如果你打算降级域控制器永久，删除不需要的广告 DS 角色和你自行决定的功能。 这要求清除这些的角色和功能所对应的复选框。  
  
    完整列表的广告 DS 相关的角色和功能包括：  
  
    -   Active DirectoryWindows PowerShell 功能的模块  
  
    -   广告 DS 和广告 LDS 工具功能  
  
    -   Active Directory 管理中心功能  
  
    -   广告 DS 单元和命令行工具功能  
  
    -   DNS 服务器  
  
    -   组策略管理控制台  
  
等效 ADDSDeployment 和 ServerManager Windows PowerShell cmdlet 是：  
  
```  
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
  
```  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  
  
### <a name="credentials"></a>凭据  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  
  
你在配置降级选项**凭据**页面。 提供以下列表中执行降级所需的凭据：  
  
-   降级额外的域控制器要求域管理员凭据。 选择**强制中删除此域控制器**将域控制器降级而不删除 Active Directory 对象域控制器元数据。  
  
    > [!WARNING]  
    > 不要选择此选项，除非域控制器无法联系其他域控制器，并且没有*合理无法*来解决该网络的问题。 强制的降级森林中的其余域控制器上保留 Active Directory 孤立元数据。 此外，所有已取消复制的更改在该域控制器，例如密码或新的用户帐户都将丢失永远停止营业。 孤立元数据是很大比例的 Microsoft 客户支持的情况下在广告 DS、 Exchange、 SQL，和其他软件的根本原因。  
    >   
    > 如果你强行降级域控制器，您*必须*手动立即执行清理元数据。 对于步骤，查看[干净向上服务器元数据](https://technet.microsoft.com/library/cc816907(WS.10).aspx)。  
  
   ![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
-   降级域中的最后一个域控制器需要企业管理员组成员资格，因为这会删除域本身 (如果树林中的最后一个域，这将删除森林)。 服务器管理器会通知你是否当前的域控制器在域中的最后一个域控制器。 选择**域中的最后一个域控制器**复选框以确认域控制器是在域中的最后一个域控制器。  
  
等效 ADDSDeployment Windows PowerShell 参数是：  
  
```  
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
  
```  
  
### <a name="warnings"></a>警告  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  
  
**警告**页面向你发送警报删除此域控制器可能出现的后果。 若要继续时，必须选择**继续删除**。  
  
> [!WARNING]  
> 如果你之前所选**强制中删除此域控制器**上**凭据**页面，然后**警告**页面显示托管由此域控制器的所有灵活单母版操作角色。 你*必须*捕获从另一个域控制器角色*立即*后降级此服务器。 占用域控制器上的详细信息，请参阅[占用操作母版角色](https://technet.microsoft.com/library/cc816779(WS.10).aspx)。  
  
此页上并没有等效 ADDSDeployment Windows PowerShell 参数。  
  
### <a name="removal-options"></a>删除选项  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  
  
**删除选项**页面显示时，具体取决于以前选择**域中的最后一个域控制器**上**凭据**页面。 此页面，可以配置可删除其他选项。 选择**忽略区域的最后一个 DNS 服务器**，**删除应用程序分区**，并**删除 DNS 委派**公开**下一步**按钮。  
  
如果适用于此域控制器，才会显示选项。 例如，如果该服务器未 DNS 委派然后该复选框不会显示。  
  
单击**更改**指定备用 DNS 管理的凭据。 单击**查看分区**以查看其他分区向导降级期间删除。 默认情况下，仅其他分区是域 DNS 和森林 DNS 区域。 其他所有分区都是非 Windows。  
  
等效 ADDSDeployment cmdlet 参数是：  
  
```  
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```  
  
### <a name="new-administrator-password"></a>新的管理员密码  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  
  
**新管理员密码**页面要求你提供密码内置本地计算机管理员帐户，降级完毕后计算机在某个域成员服务器或工作组计算机。  
  
**卸载 ADDSDomainController** cmdlet 和参数按照相同的默认值为服务器管理器中，如果未指定。  
  
**LocalAdministratorPassword**参数是特殊：  
  
-   如果*未指定*作为参数，然后 cmdlet 提示你输入并确认屏蔽的密码。 运行 cmdlet 交互时，这是首选的使用情况  
  
-   如果指定*值*，然后值必须安全字符串。 这不是运行 cmdlet 交互时首选的使用情况  
  
例如，你可以手动提示输入密码使用**读取主机**cmdlet 安全字符串用户提示  
  
```  
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> 以前的两个选项未确认密码，如使用格外小心：看不到密码  
  
虽然这是强烈建议不要，你还可以为转换清除文本变量，提供安全的字符串。 例如：  
  
```  
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
> [!WARNING]  
> 不建议提供或存储清除短的密码。 脚本中运行此命令或查找通过您的任何人都知道该计算机的本地管理员密码。 与该知识，它们将有权访问其所有数据，并且可以模拟服务器本身。  
  
### <a name="confirmation"></a>确认  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)  
  
**确认**页面显示计划的降级;页上未列出降级配置选项。 这是降级开始之前，将显示向导中的最后一页。 查看脚本按钮创建降级 Windows PowerShell 脚本。  
  
单击**降级**运行以下广告 DS 部署 cmdlet:  
  
```  
Uninstall-DomainController  
  
```  
  
使用可选**Whatif**具有参数**卸载 ADDSDomainController**和 cmdlet 查看配置的信息。 这使你查看 cmdlet 参数明确和隐式值。  
  
例如：  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)  
  
重启的提示是你要取消此操作，使用 ADDSDeployment Windows PowerShell 时的最后一次机会。 若要替代提示，请使用**-强制**或**确认：$false**参数。  
  
### <a name="demotion"></a>降级  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  
  
当**降级**页面显示时，域控制器配置开始，无法将停止或取消。 详细的操作显示在此页面上，并且日志写入：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
由于**卸载 AddsDomainController**和**卸载 WindowsFeature**仅有一项操作按、他们的最低要求参数了确认阶段此处显示。 按 enter 键启动不可撤销降级进程，并重新启动计算机。  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)  
  
若要自动接受重新引导提示，请使用**-强制**或**-确认：$false**与任何 ADDSDeployment Windows PowerShell cmdlet 参数。 若要防止服务器推广末尾自动重新启动，请使用**-norebootoncompletion: $false**参数。  
  
> [!WARNING]  
> 不建议重重新启动。 成员服务器必须重新启动才能正常工作。  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)  
  
下面是一种与其降至最低要求参数强行降级**-forceremoval**和**-demoteoperationmasterrole**。 **-凭据**参数不需要，因为作为企业管理员组成员的登录的用户：  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)  
  
以下是删除与其降至最低要求参数域中的最后一个域控制器它的一个示例**-lastdomaincontrollerindomain**和**-removeapplicationpartitions**:  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)  
  
如果你试图在降级服务器之前删除广告 DS 角色，Windows PowerShell 阻止有意错误：  
  
```  
Uninstall-WindowsFeature : An uninstallation prerequisite step failed duringthe removal of AD-Domain-Services, and uninstallation cannot continue.1. The domain controller needs to be demoted before the Active DirectoryDomain Services Role can be uninstalled.  
```  
  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)  
  
> [!IMPORTANT]  
> 你可以删除广告域服务角色二进制文件之前降级服务器后，必须重启计算机。  
  
### <a name="results"></a>结果  
![降级 DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)  
  
**结果**页面显示的成功或失败的升级和管理的任何重要信息。 10 秒钟后，域控制器将自动重新启动。  
  

