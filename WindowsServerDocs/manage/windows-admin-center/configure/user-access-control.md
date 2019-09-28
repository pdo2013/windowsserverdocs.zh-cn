---
title: 配置用户访问控制和权限
description: 了解如何使用 Active Directory 或 Azure AD （Project Honolulu）配置用户访问控制和权限
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 20b311e9330880c2b26e2494aabe27bb04891868
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407034"
---
# <a name="configure-user-access-control-and-permissions"></a>配置用户访问控制和权限

> 适用于：Windows Admin Center、Windows Admin Center 预览版

如果尚未这样做，请先熟悉[Windows 管理中心中的 "用户访问控制" 选项](../plan/user-access-options.md)

> [!NOTE]
> Windows 管理中心中的基于组的访问在工作组环境中不受支持，或跨不可信域。

## <a name="gateway-access-role-definitions"></a>网关访问角色定义

有两个角色可用于访问 Windows 管理中心网关服务：

**网关用户**可以连接到 Windows 管理中心网关服务来通过该网关管理服务器，但不能更改访问权限，也不能更改用于向网关进行身份验证的身份验证机制。

**网关管理员**可以配置谁获取访问权限，以及用户如何向网关进行身份验证。 仅网关管理员可以在 Windows 管理中心中查看和配置访问设置。 网关计算机上的本地管理员始终为 Windows 管理中心网关服务的管理员。

> [!NOTE]
> 对网关的访问权限并不意味着可以访问网关可见的托管服务器。 若要管理目标服务器，连接用户必须使用具有管理访问权限的凭据（通过其传递的 Windows 凭据或使用 "**管理方式**" 操作在 Windows 管理中心会话中提供的凭据）目标服务器。

## <a name="active-directory-or-local-machine-groups"></a>Active Directory 或本地计算机组

默认情况下，Active Directory 或本地计算机组用于控制网关访问。 如果有 Active Directory 域，则可以在 Windows 管理中心界面中管理网关用户和管理员访问权限。

在 "**用户**" 选项卡上，你可以控制谁可以作为网关用户访问 Windows 管理中心。 默认情况下，如果未指定安全组，则访问网关 URL 的任何用户都可以访问。 将一个或多个安全组添加到 "用户" 列表后，对这些组的成员的访问将受到限制。

如果未在你的环境中使用 Active Directory 域，则访问权限由 Windows 管理中心网关计算机上的 @no__t 0 和 @no__t 本地组控制。

### <a name="smartcard-authentication"></a>智能卡身份验证

可以通过为基于智能卡的安全组指定其他_所需_组来强制执行**智能卡身份验证**。 添加基于智能卡的安全组后，如果用户是 "用户" 列表中包含的任何安全组和智能卡组的成员，则该用户只能访问 Windows 管理中心服务。

在 "**管理员**" 选项卡上，可以控制谁可以访问 Windows 管理中心。 计算机上的本地管理员组将始终具有完全权限的管理员访问权限，并且不能从列表中删除。 通过添加安全组，您可以授予这些组的成员更改 Windows 管理中心网关设置的权限。 管理员列表支持智能卡身份验证，其方式与用户列表相同：安全组和智能卡组的和条件。

## <a name="azure-active-directory"></a>Azure Active Directory

如果你的组织使用 Azure Active Directory （Azure AD），则可以通过要求 Azure AD 身份验证来访问网关，选择向 Windows 管理中心添加**额外**的安全层。 为了访问 Windows 管理中心，用户的**Windows 帐户**还必须具有对网关服务器的访问权限（即使使用 Azure AD 身份验证）。 当你使用 Azure AD 时，你将从 Azure 门户而不是从 Windows 管理中心 UI 中管理 Windows 管理中心用户和管理员访问权限。

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>在启用 Azure AD 身份验证时访问 Windows 管理中心

根据所使用的浏览器，某些访问配置了 Azure AD 身份验证的 Windows 管理中心的用户将在**浏览器**中收到额外的提示，用户需要为计算机提供其 Windows 帐户凭据已安装 Windows 管理中心。 输入该信息后，用户将获得额外的 Azure Active Directory 身份验证提示，该提示需要 Azure 帐户的凭据，该帐户在 Azure 中的 Azure AD 应用程序中已获访问权限。

> [!NOTE]
> 如果用户的 Windows 帐户在网关计算机上具有**管理员权限**，则系统不会提示用户进行 Azure AD 身份验证。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>为 Windows 管理中心预览配置 Azure Active Directory 身份验证

转到 "Windows 管理中心**设置** > **访问**"，并使用切换开关打开 "使用 Azure Active Directory 将安全层添加到网关"。 如果尚未向 Azure 注册网关，则会指导您此时执行此操作。

默认情况下，Azure AD 租户的所有成员都具有对 Windows 管理中心网关服务的用户访问权限。 只有网关计算机上的本地管理员才有权访问 Windows 管理中心的网关。 请注意，网关计算机上的本地管理员权限不受限制-本地管理员可以执行任何操作，无论 Azure AD 是否用于身份验证。

如果要向特定 Azure AD 用户或组网关用户或网关管理员授予对 Windows 管理中心服务的访问权限，则必须执行以下操作：

1.  使用 "访问设置" 中提供的超链接，在 Azure 门户中，使用 "访问权限" Azure AD 应用程序。 注意仅当启用 Azure Active Directory 身份验证时，此超链接才可用。 
    -   你还可以在 Azure 门户中找到你的应用程序，方法是转到**Azure Active Directory**@no__t 1 个**企业应用程序** > **所有应用程序**并搜索**WindowsAdminCenter** （Azure AD 应用程序将命名为WindowsAdminCenter-<gateway name>）。 如果未收到任何搜索结果，请确保 "**显示**" 设置为 "**所有应用程序**"，将 "**应用程序状态**" 设置为 "**任意**"，然后单击 "应用"，然后尝试搜索。 找到应用程序后，请访问 "**用户和组**"
2.  在 "属性" 选项卡中，将 "**用户分配**" 设置为 "是"。
    完成此操作后，只有在 "**用户和组**" 选项卡中列出的成员才能够访问 Windows 管理中心网关。
3.  在 "用户和组" 选项卡中，选择 "**添加用户**"。 必须为每个添加的用户/组分配网关用户或网关管理员角色。

启用 Azure AD 身份验证后，网关服务将重新启动，你必须刷新浏览器。 你随时可以在 Azure 门户中更新 SME Azure AD 应用程序的用户访问权限。

当用户尝试访问 Windows 管理中心网关 URL 时，系统将提示用户使用其 Azure Active Directory 的标识进行登录。 请记住，用户还必须是网关服务器上的本地用户的成员才能访问 Windows 管理中心。

用户和管理员可以在 Windows 管理中心设置的 "**帐户**" 选项卡中查看其当前登录的帐户以及此 Azure AD 帐户的注销。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>为 Windows 管理中心配置 Azure Active Directory 身份验证

[若要设置 Azure AD 身份验证，你必须首先向 Azure 注册网关](azure-integration.md)（你只需为 Windows 管理中心网关执行此操作一次）。 此步骤将创建一个 Azure AD 应用程序，你可以从中管理网关用户和网关管理员访问权限。

如果要向特定 Azure AD 用户或组网关用户或网关管理员授予对 Windows 管理中心服务的访问权限，则必须执行以下操作：

1.  请在 Azure 门户中找到你的 SME Azure AD 应用程序。 
    -   单击 "**更改访问控制**"，然后从 Windows 管理中心访问设置中选择**Azure Active Directory**时，可以使用 UI 中提供的超链接来访问 Azure 门户中的 Azure AD 应用程序。 单击 "保存" 并选择 "Azure AD 作为访问控制标识提供者时，此超链接也在访问设置中提供。
    -   你还可以在 Azure 门户中找到你的应用程序，方法是转到**Azure Active Directory**@no__t 1 个**企业应用程序** > **所有应用程序**并搜索**SME** （将 Azure AD 应用命名为 sme-6）。 如果未收到任何搜索结果，请确保 "**显示**" 设置为 "**所有应用程序**"，将 "**应用程序状态**" 设置为 "**任意**"，然后单击 "应用"，然后尝试搜索。 找到应用程序后，请访问 "**用户和组**"
2.  在 "属性" 选项卡中，将 "**用户分配**" 设置为 "是"。
    完成此操作后，只有在 "**用户和组**" 选项卡中列出的成员才能够访问 Windows 管理中心网关。
3.  在 "用户和组" 选项卡中，选择 "**添加用户**"。 必须为每个添加的用户/组分配网关用户或网关管理员角色。

在 "**更改访问控制**" 窗格中保存 Azure AD 访问控制后，网关服务将重新启动，你必须刷新浏览器。 你可以随时更新 Azure 门户中 Windows 管理中心 Azure AD 应用程序的用户访问权限。 

当用户尝试访问 Windows 管理中心网关 URL 时，系统将提示用户使用其 Azure Active Directory 的标识进行登录。 请记住，用户还必须是网关服务器上的本地用户的成员才能访问 Windows 管理中心。 

使用 Windows 管理中心的 " **Azure** " 选项卡的 "常规设置"，用户和管理员可以查看其当前登录的帐户，以及此 Azure AD 帐户的注销。

### <a name="conditional-access-and-multi-factor-authentication"></a>条件性访问和多重身份验证

使用 Azure AD 作为额外的安全层来控制对 Windows 管理中心网关的访问的好处之一是，你可以利用 Azure AD 的强大安全功能，例如条件访问和多重身份验证。 

[详细了解如何使用 Azure Active Directory 配置条件访问。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>配置单一登录

**在 Windows Server 上部署为服务时的单一登录**

在 Windows 10 上安装 Windows 管理中心时，可以使用单一登录。 但是，如果你要在 Windows Server 上使用 Windows 管理中心，则需要在你的环境中设置某种形式的 Kerberos 委派，然后才能使用单一登录。 委派会将网关计算机配置为受信任，以委托给目标节点。 

若要在环境中配置[基于资源的约束委派](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview)，请运行以下 PowerShell cmdlet。 （请注意，这需要运行 Windows Server 2012 或更高版本的域控制器）。

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

在此示例中，Windows 管理中心网关安装在服务器**WindowsAdminCenterGW**上，目标节点名称为**ManagedNode**。

若要删除此关系，请运行以下 cmdlet：

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>基于角色的访问控制

使用基于角色的访问控制，您可以为用户提供对计算机的有限访问权限，而不是使其完全成为本地管理员。
[阅读有关基于角色的访问控制和可用角色的详细信息。](../plan/user-access-options.md#role-based-access-control)

设置 RBAC 包括2个步骤：在目标计算机上启用支持，并将用户分配到相关角色。

> [!TIP]
> 请确保对配置基于角色的访问控制的计算机具有本地管理员特权。

### <a name="apply-role-based-access-control-to-a-single-machine"></a>将基于角色的访问控制应用于单台计算机

单计算机部署模型非常适合只需几台计算机进行管理的简单环境。
配置具有基于角色的访问控制支持的计算机将导致以下更改：

-   包含 Windows 管理中心所需功能的 PowerShell 模块将安装在你的系统驱动器上 `C:\Program Files\WindowsPowerShell\Modules` 下。 所有模块都将从**Microsoft**
-   所需状态配置将运行一次性配置，以便在计算机上配置足够的管理终结点，名为 " **Microsoft**"。 此终结点定义 Windows 管理中心使用的3个角色，并将在用户连接到它时作为临时本地管理员运行。
-   3将创建新的本地组，以控制为哪些用户分配了哪些角色的访问权限：
    -   Windows 管理中心管理员
    -   Windows 管理中心 Hyper-v 管理员
    -   Windows 管理中心读者

若要在一台计算机上启用对基于角色的访问控制的支持，请执行以下步骤：

1.  使用在目标计算机上具有本地管理员权限的帐户，打开 Windows 管理中心并连接到要使用基于角色的访问控制进行配置的计算机。
2.  在**概述**工具上，单击 "**设置**"  > **基于角色的访问控制**。
3.  单击页面底部的 "**应用**"，以在目标计算机上启用基于角色的访问控制支持。 应用程序过程涉及到复制 PowerShell 脚本和在目标计算机上调用配置（使用 PowerShell Desired State Configuration）。 最长可能需要10分钟才能完成。 这会暂时断开 Windows 管理中心、PowerShell 和 WMI 用户的连接。
4.  刷新页面以检查基于角色的访问控制的状态。 准备就绪后，状态将更改为 "已**应用**"。

应用配置后，可以将用户分配到角色：

1.  打开 "**本地用户和组**" 工具，并导航到 "**组**" 选项卡。
2.  选择**Windows 管理中心**的 "读者" 组。
3.  在底部的*详细信息*窗格中，单击 "**添加用户**"，然后输入用户或安全组的名称，该用户或安全组应通过 Windows 管理中心对服务器具有只读访问权限。 用户和组可以来自本地计算机或 Active Directory 域。
4.  为**Windows 管理中心 Hyper-v 管理员**和**windows 管理中心管理员**组重复步骤2-3。

你还可以通过使用 "[受限制的组" 策略设置](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29)配置组策略对象，在域中一致地填充这些组。

### <a name="apply-role-based-access-control-to-multiple-machines"></a>将基于角色的访问控制应用于多台计算机

在大型企业部署中，可以使用现有的自动化工具将基于角色的访问控制功能从 Windows 管理中心网关下载到计算机。
配置包设计为与 PowerShell Desired State Configuration 一起使用，但你可以对其进行调整，使其适用于你的首选自动化解决方案。

#### <a name="download-the-role-based-access-control-configuration"></a>下载基于角色的访问控制配置

若要下载基于角色的访问控制配置包，你需要有权访问 Windows 管理中心和 PowerShell 提示符。

如果在 Windows Server 上以服务模式运行 Windows 管理中心网关，请使用以下命令下载配置包。
请确保用适用于你的环境的正确的网关地址更新网关地址。

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

如果在 Windows 10 计算机上运行 Windows 管理中心网关，请改为运行以下命令：

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

展开 zip 存档后，会看到以下文件夹结构：

- InstallJeaFeatures
- JustEnoughAdministration （目录）
- 模块（目录）
    - @No__t-0 （目录）
    - WindowsAdminCenter. Jea （目录）

若要在节点上配置基于角色的访问控制支持，需要执行以下操作：

1.  将 JustEnoughAdministration @no__t、和 WindowsAdminCenter 模块复制到目标计算机上的 PowerShell 模块目录中。 通常，此位置位于 @no__t 0。
2.  更新**InstallJeaFeature**文件以匹配 RBAC 终结点所需的配置。
3.  运行 InstallJeaFeature 以编译 DSC 资源。
4.  将 DSC 配置部署到所有计算机，以应用配置。

以下部分介绍如何使用 PowerShell 远程处理执行此操作。

#### <a name="deploy-on-multiple-machines"></a>在多台计算机上部署

若要将下载的配置部署到多台计算机上，需要更新**InstallJeaFeatures**脚本，以便为你的环境添加适当的安全组，将文件复制到每台计算机，并调用配置脚本。
您可以使用您喜欢的自动化工具来实现此目的，但本文将着重介绍纯基于 PowerShell 的方法。

默认情况下，配置脚本将在计算机上创建本地安全组，以控制对每个角色的访问。
这适用于加入工作组和加入域的计算机，但如果要在仅限域的环境中进行部署，则可能希望直接将域安全组与每个角色关联。
若要更新配置以使用域安全组，请打开**InstallJeaFeatures**并进行以下更改：

1.  从文件中删除 3**组**资源：
    1.  "组 MS-Readers-组"
    2.  "Group MS-Hyper-v-Administrators 组"
    3.  "组 MS-管理员-组"
2.  从 JeaEndpoint **DependsOn**属性中删除3组资源
    1.  "[Group] MS-Readers-组"
    2.  "[Group] MS-Hyper-v-管理员-组"
    3.  "[组] MS-Administrators-组"
3.  将 JeaEndpoint **RoleDefinitions**属性中的组名称更改为所需的安全组。 例如，如果你有一个安全组*CONTOSO\MyTrustedAdmins* ，该安全组应分配给 Windows 管理中心管理员角色的访问权限，请将 `'$env:COMPUTERNAME\Windows Admin Center Administrators'` 更改 `'CONTOSO\MyTrustedAdmins'`。 需要更新的三个字符串是：
    1.  ' $env： COMPUTERNAME\Windows 管理中心管理员
    2.  ' $env： COMPUTERNAME\Windows 管理中心
    3.  ' $env： COMPUTERNAME\Windows 管理中心读者 '

> [!NOTE]
> 请确保为每个角色使用唯一的安全组。 如果将同一安全组分配给多个角色，则配置将失败。

接下来，在**InstallJeaFeatures**文件的末尾，将以下 PowerShell 行添加到脚本底部：

```powershell
Copy-Item "$PSScriptRoot\JustEnoughAdministration" "$env:ProgramFiles\WindowsPowerShell\Modules" -Recurse -Force
$ConfigData = @{
    AllNodes = @()
    ModuleBasePath = @{
        Source = "$PSScriptRoot\Modules"
        Destination = "$env:ProgramFiles\WindowsPowerShell\Modules"
    }
}
InstallJeaFeature -ConfigurationData $ConfigData | Out-Null
Start-DscConfiguration -Path "$PSScriptRoot\InstallJeaFeature" -JobName "Installing JEA for Windows Admin Center" -Force
```

最后，可以将包含模块、DSC 资源和配置的文件夹复制到每个目标节点并运行**InstallJeaFeature**脚本。
若要从管理工作站远程执行此操作，可以运行以下命令：

```powershell
$ComputersToConfigure = 'MyServer01', 'MyServer02'

$ComputersToConfigure | ForEach-Object {
    $session = New-PSSession -ComputerName $_ -ErrorAction Stop
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC\JustEnoughAdministration\" -Destination "$env:ProgramFiles\WindowsPowerShell\Modules\" -ToSession $session -Recurse -Force
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC" -Destination "$env:TEMP\WindowsAdminCenter_RBAC" -ToSession $session -Recurse -Force
    Invoke-Command -Session $session -ScriptBlock { Import-Module JustEnoughAdministration; & "$env:TEMP\WindowsAdminCenter_RBAC\InstallJeaFeature.ps1" } -AsJob
    Disconnect-PSSession $session
}
```
