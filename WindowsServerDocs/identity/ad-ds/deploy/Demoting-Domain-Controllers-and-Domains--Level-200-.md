---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: 降级域控制器和域（级别 200）
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e3f320b67196a2400ebedbaeaf0a5b59969400e8
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588095"
---
# <a name="demoting-domain-controllers-and-domains"></a>降级域控制器和域

>适用于：Windows Server

本主题介绍如何使用服务器管理器或 Windows PowerShell 删除 AD DS。
  
## <a name="ad-ds-removal-workflow"></a>AD DS 删除工作流

![AD DS 删除工作流图表](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  

> [!CAUTION]
> 不支持在升级到域控制器后使用 Dism.exe 或 Windows PowerShell DISM 模块删除 AD DS 角色，并且将阻止服务器正常启动。
>
> 和服务器管理器或 Windows PowerShell 的 ADDSDeployment 模块不同，DISM 是本机服务系统，它没有继承关于 AD DS 或其配置的知识。 不要使用 Dism.exe 或 Windows PowerShell DISM 模块卸载 AD DS 角色，除非服务器不再是域控制器。

## <a name="demotion-and-role-removal-with-powershell"></a>通过 PowerShell 降级和角色删除

|||  
|-|-|  
|**ADDSDeployment 和 ServerManager Cmdlet**|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|卸载-Install-addsdomaincontroller|-Skipprechecks 不可<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirm<br /><br />***-Credential***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Force<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Uninstall-WindowsFeature/Remove-WindowsFeature|***-Name***<br /><br />***-IncludeManagementTools***<br /><br />*-重新启动*<br /><br />-Remove<br /><br />-Force<br /><br />-ComputerName<br /><br />-Credential<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> 仅在你尚未作为 Enterprise Admins 组（降级域中的最后一个 DC）或 Domain Admins 组（降级副本 DC）的成员登录时，才需要 **-credential** 参数。仅在你希望删除所有 AD DS 管理实用程序时，才需要 **-includemanagementtools** 参数。  
  
## <a name="demote"></a>降级  
  
### <a name="remove-roles-and-features"></a>删除角色和功能

服务器管理器提供两个接口以删除 Active Directory 域服务角色：  
  
* 主仪表板上的“管理” 菜单，使用“删除角色和功能”  

   ![服务器管理器-删除角色和功能](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  

* 单击导航窗格上的“AD DS”或“所有服务器”。 向下滚动到“角色和功能”部分。 右键单击“角色和功能”列表中的“Active Directory 域服务”并单击“删除角色或功能”。 此接口跳过“服务器选择”页面。  

   ![服务器管理器-所有服务器-删除角色和功能](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  

ServerManager cmdlet **Uninstall 和 Uninstall**会阻止你删除 AD DS**角色，直到**降级域控制器。
  
### <a name="server-selection"></a>服务器选择

![删除角色和功能向导选择目标服务器](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  

“服务器选择”对话框使你可以从之前添加到池的服务器中选择一个（只要它可访问）。 运行服务器管理器的本地服务器始终自动可用。  

### <a name="server-roles-and-features"></a>服务器角色和功能

![删除角色和功能向导-选择要删除的角色](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  

如果服务器当前是域控制器，清除“Active Directory 域服务”复选框以降级域控制器，此操作不删除 AD DS 角色，而是切换到提供降级功能的“验证结果”对话框。 否则，它会像任何其他角色功能一样删除二进制文件。  

* 如果你希望立即再次升级域控制器，不要删除任何其他 AD DS 相关的角色或功能（例如 DNS、GPMC 或 RSAT 工具）。 删除其他角色和功能将增加重新升级的时间，因为当你重新安装角色时，服务器管理器会重新安装这些功能。  
* 如果你希望永久降级域控制器，根据你自己的判断删除不需要的 AD DS 角色和功能。 这需要清除这些角色和功能对应的复选框。  

   AD DS 相关的角色和功能的完整列表包括：  
  
   * 用于 Windows PowerShell 功能的 Active Directory 模块  
   * AD DS 和 AD LDS 工具功能  
   * Active Directory 管理中心功能  
   * AD DS 管理单元和命令行工具功能  
   * DNS 服务器  
   * 组策略管理控制台  
  
等效的 ADDSDeployment 和 ServerManager Windows PowerShell cmdlet 如下：  
  
```
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
```

![删除角色和功能向导-确认对话框](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  

![删除角色和功能向导-验证](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  

### <a name="credentials"></a>凭据

![Active Directory 域服务配置向导-凭据选择](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  

你可在“凭据” 页上配置降级选项。 从以下列表提供执行降级所需的凭据：  

* 降级其他域控制器需要 Domain Admin 凭据。 选择 **"强制删除此域控制器**" 将降级域控制器，且不会从 Active Directory 删除域控制器对象的元数据。  

   > [!WARNING]  
   > 如果域控制器可以联系其他域控制器，则不要选择此选项，而且*还没有任何合理的方法*可解决这种网络问题。 强制降级会将 Active Directory 中已丢弃的元数据保留在林中的其余域控制器上。 此外，该域控制器上所有未复制的更改（如密码或新用户帐户）都将永久丢失。 已丢弃的元数据是 AD DS、Exchange、SQL 和其他软件的大部分 Microsoft 客户支持案例的根本原因。  
   >
   > 如果强制降级域控制器，*必须*立即手动执行元数据清理。 有关步骤，请查看 [清理服务器元数据](ad-ds-metadata-cleanup.md)。  

   ![Active Directory 域服务配置向导-凭据强制删除](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
* 降级域中的最后一个域控制器需要 Enterprise Admins 组的成员身份，因为这将删除域本身（如果是林中的最后一个域，这将删除林）。 服务器管理器将通知当前域控制器是否是域中的最后一个域控制器。 选中“域中最后一个域控制器”复选框以确认该域控制器是域中最后一个域控制器。  

等效 ADDSDeployment Windows PowerShell 参数是：  

```
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
```

### <a name="warnings"></a>警告

![Active Directory 域服务配置向导-凭据 FSMO 角色影响](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  

“警告” 页面向你警示删除此域控制器可能出现的后果。 若要继续，则必须选中“继续删除”。

> [!WARNING]  
> 如果你之前选中了“凭据” 页面上的“强制删除此域控制器” ，那么 **警告** 页面将显示此域控制器托管的所有灵活单主机操作角色。 你 *必须* 在降级此服务器后 *立即* 从另一个域控制器占用这些角色。 有关占用 FSMO 角色的详细信息，请参阅 [占用操作主机角色](https://technet.microsoft.com/library/cc816779(WS.10).aspx)。

此页面没有等效的 ADDSDeployment Windows PowerShell 参数。

### <a name="removal-options"></a>删除选项

![Active Directory 域服务配置向导-凭据删除 DNS 和应用程序分区](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  

根据之前选择“凭据”页面上的“域中的最后一个域控制器”，将出现“删除选项”页。 此页面使你可以配置其他删除选项。 选择 "**忽略区域的最后一个 DNS 服务器**"、"**删除应用程序分区**" 和 "**删除 DNS 委派**" 以启用 "**下一步**" 按钮。

该选项仅在适用于此域控制器时出现。 例如，如果此服务器没有 DNS 委派，则该复选框将不会显示。

单击“更改”指定备用 DNS 管理凭据。 单击“查看分区”以查看向导在降级期间删除的其他分区。 默认情况下，仅有的其他分区是域 DNS 和林 DNS 区域。 所有其他分区都是非 Windows 分区。

等效的 ADDSDeployment cmdlet 参数是：  

```
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```

### <a name="new-administrator-password"></a>新建管理员密码

![Active Directory 域服务配置向导-凭据新的管理员密码](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  

当降级完成且计算机成为域成员服务器或工作组计算机后，"**新建管理员密码**" 页将要求你提供内置本地计算机的管理员帐户的密码。

**Uninstall-ADDSDomainController** cmdlet 和参数遵循与服务器管理器相同的默认值（如果未指定）。

**LocalAdministratorPassword** 参数是特殊参数：

* 如果 *未指定* 为参数，则 cmdlet 将提示你输入并确认掩蔽的密码。 以交互方式运行 cmdlet 时，这是首选用法。
* 如果 *已使用值*指定，那么该值必须是一个安全字符串。 以交互方式运行 cmdlet 时，这不是首选用法。

例如，你可以通过使用**读取主机**cmdlet 来提示用户输入安全字符串来手动提示输入密码。

```
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> 由于前两个选项不确认密码，因此请谨慎使用：密码不可见。

你还可以提供安全字符串作为转换的明文变量，尽管强烈不建议这样做。 例如：

```
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

> [!WARNING]
> 不建议提供或存储明文密码。 任何在脚本中运行此命令或在你背后偷看的人都知道该计算机的本地管理员密码。 知道密码后，他们可以访问其所有数据并模拟服务器本身。

### <a name="confirmation"></a>确认

![Active Directory 域服务配置向导-查看选项](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)

“确认” 页面将显示计划的降级；该页面不会列出降级配置选项。 这是向导在降级开始前显示的最后一个页面。 “查看脚本”按钮将创建一个 Windows PowerShell 降级脚本。

单击“降级”以运行以下 AD DS 部署 cmdlet：

```
Uninstall-ADDSDomainController
```

将可选 **Whatif** 参数与 **Uninstall-ADDSDomainController** 和 cmdlet 一起使用以查看配置信息。 这使你可以查看 cmdlet 的参数的显式值和隐式值。

例如：

![PowerShell 卸载-Install-addsdomaincontroller 示例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)

使用 ADDSDeployment Windows PowerShell 时，重新启动的提示是你取消此操作的最后机会。 若要覆盖该提示，请使用 **-force** 或 **confirm:$false** 参数。  

### <a name="demotion"></a>降级

![Active Directory 域服务配置向导-正在降级](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  

当“降级”页面显示时，域控制器配置将开始，且无法暂停或取消。 详细的操作在此页面上显示并将写入日志：  

* %systemroot%\debug\dcpromo.log
* %systemroot%\debug\dcpromoui.log

由于**install-addsdomaincontroller**和**uninstall**只包含一个操作各，因此，它们会在确认阶段中显示，并带有最少的所需参数。 按 ENTER 将启动不可撤销的降级过程，并重新启动计算机。

![PowerShell 卸载-Install-addsdomaincontroller 示例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)

![PowerShell Uninstall-从示例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)

若要自动接受重新启动提示，请将 **-force** 或 **-confirm:$false** 参数与任何 ADDSDeployment Windows PowerShell cmdlet 一起使用。 若要防止在升级结束时自动重新启动，请使用 **-norebootoncompletion:$false** 参数。

> [!WARNING]
> 不建议重写重新启动。 成员服务器必须重新启动才能正常工作。

![PowerShell 卸载-Install-addsdomaincontroller Force 示例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)

以下是在所需的 **-forceremoval** 和 **-demoteoperationmasterrole**参数最少的情况下强制降级的示例。 无需 **-credential** 参数，因为用户以 Enterprise Admins 组成员身份登录：

![PowerShell 卸载-Install-addsdomaincontroller 示例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)

以下是删除域中最后一个域控制器的示例，并带有最少的所需参数“-lastdomaincontrollerindomain” 和“-removeapplicationpartitions”。

![PowerShell Install-addsdomaincontroller-LastDomainControllerInDomain 示例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)

如果你尝试在降级服务器之前删除 AD DS 角色，则 Windows PowerShell 会阻止你发生错误：

![卸载 AD 域服务过程中卸载先决条件步骤失败，无法继续卸载。 1. 需要对域控制器进行降级，然后才能卸载 Active DirectoryDomain Services 角色。](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)

> [!IMPORTANT]
> 你必须在降级服务器之前重新启动计算机，然后才能删除 AD-Domain-Services 角色二进制文件。

### <a name="results"></a>结果

![删除 AD DS 后，你将被注销警告](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)

“结果” 页面显示升级是成功还是失败以及任何重要的管理信息。 域控制器将在 10 秒后自动重新启动。
