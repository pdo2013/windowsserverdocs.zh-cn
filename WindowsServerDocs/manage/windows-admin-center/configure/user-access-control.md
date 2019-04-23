---
title: 配置用户访问控制和权限
description: 了解如何配置用户访问控制和使用 Active Directory 或 Azure AD (项目 Honolulu) 的权限
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850568"
---
# <a name="configure-user-access-control-and-permissions"></a>配置用户访问控制和权限

>适用于：Windows Admin Center，Windows Admin Center 预览版

如果尚不具备，自己应熟悉[Windows Admin Center 中的用户访问控制选项](../plan/user-access-options.md)

>[!NOTE]
> 在工作组环境中或跨不受信任的域不支持 Windows Admin Center 中的组基于访问权限。

## <a name="gateway-access-role-definitions"></a>网关访问的角色定义

有两个角色的 Windows Admin Center 网关服务的访问权限：

**网关用户**可以连接到 Windows Admin Center 网关服务来管理服务器通过该网关，但它们不能更改访问权限，也不用于对网关进行身份验证的身份验证机制。

**网关管理员**可以配置用户获取访问权限以及如何向网关的用户进行身份验证。 仅网关管理员可以查看和在 Windows Admin Center 中配置访问设置。 网关计算机上的本地管理员始终是 Windows Admin Center 网关服务的管理员。

> [!NOTE]
> 到网关的访问权限并不意味着对可见的托管服务器访问网关。 若要管理的目标服务器，连接的用户必须使用凭据 (通过其传递通过 Windows 凭据或使用在 Windows Admin Center 会话中提供的凭据**作为管理**操作) 具有到该目标服务器的管理访问权限。

## <a name="active-directory-or-local-machine-groups"></a>Active Directory 或本地计算机组

默认情况下，使用 Active Directory 或本地计算机组来控制网关的访问。 如果您有一个 Active Directory 域，你可以管理网关用户和管理员从 Windows Admin Center 界面中访问。

上**用户**可以控制可以访问 Windows Admin Center 作为网关用户的选项卡。 默认情况下，如果未指定安全组，访问网关 URL 的任何用户具有访问权限。 一旦将一个或多个安全组添加到用户列表，访问权限仅限于这些组的成员。

如果不在你的环境中使用 Active Directory 域，由控制访问```Users```和```Administrators```Windows Admin Center 网关计算机上的本地组。

### <a name="smartcard-authentication"></a>智能卡身份验证

您可以强制执行**智能卡身份验证**通过指定附加_必需_组的基于智能卡的安全组。 添加基于智能卡的安全组后，用户仅可以访问 Windows Admin Center 服务，如果他们是任何安全组的成员，并且包括在用户列表中的智能卡组。

上**管理员**可以控制可以访问网关管理员为 Windows Admin Center 的选项卡。 计算机上的本地管理员组将始终具有完全管理员访问权限，并且不能从列表中删除。 通过添加安全组，则为提供的这些组的权限的成员可以更改 Windows Admin Center 网关设置。 管理员列表支持智能卡身份验证的用户列表的方式相同： 与安全组和智能卡组的 AND 条件。

## <a name="azure-active-directory"></a>Azure Active Directory

如果你的组织使用 Azure Active Directory (Azure AD)，您可以选择添加**其他**到 Windows Admin Center 通过要求 Azure AD 身份验证访问网关的安全层。 若要访问 Windows Admin Center，用户的**Windows 帐户**（即使使用 Azure AD 身份验证），还必须有权访问网关服务器。 当你使用 Azure AD 时，你将管理 Windows Admin Center 用户和管理员访问权限从 Azure 门户中，而不是从 Windows Admin Center UI。

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>启用 Azure AD 身份验证后访问 Windows Admin Center

具体取决于使用的浏览器，配置 Azure AD 身份验证访问 Windows Admin Center 某些用户将收到的额外提示**从浏览器**他们需要提供有关其 Windows 帐户凭据在其安装 Windows Admin Center 的计算机。 输入该信息后，用户将获得更多的 Azure Active Directory 身份验证提示，它将需要 Azure 帐户，已被授予在 Azure 中的 Azure AD 应用程序中的访问权限的凭据。

> [!NOTE]
> Windows 用户帐户具有**管理员权限**网关上机不会提示其 Azure AD 身份验证。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>配置 Windows Admin Center 预览版的 Azure Active Directory 身份验证

转到 Windows Admin Center**设置** > **访问**并使用切换开关来开启"使用 Azure Active Directory 的安全层添加到网关"。 如果尚未注册到 Azure 网关，我们将帮助您为此，在这一次。

默认情况下，Azure AD 租户的所有成员都具有用户访问 Windows Admin Center 网关服务。 只有在网关计算机上的本地管理员才拥有 Windows Admin Center 网关的管理员访问权限。 请注意，在网关计算机上的本地管理员权限不能进行限制，即本地管理员可以执行任何操作而不考虑是否将 Azure AD 用于身份验证。

如果想要授予特定的 Azure AD 用户或组网关的用户或网关管理员访问权限的 Windows Admin Center 服务，必须执行以下操作：

1.  通过使用访问设置中提供的超链接，请转到 Windows Admin Center Azure AD 应用程序在 Azure 门户中。 请注意当启用 Azure Active Directory 身份验证时，此超链接才可用。 
    -   此外可以找到你的应用程序在 Azure 门户中通过转到**Azure Active Directory** > **企业应用程序** > **的所有应用程序**并搜索**WindowsAdminCenter** (Azure AD 应用程序将被命名为 WindowsAdminCenter-<gateway name>)。 如果没有获得任何搜索结果，请确保**显示**设置为**的所有应用程序**，**应用程序状态**设置为**任何**并单击应用然后尝试搜索。 一旦找到应用程序，请转到**用户和组**
2.  在属性选项卡中设置**需要进行用户分配**为是。
    完成连接后，只有成员列入**用户和组**选项卡将能够访问 Windows Admin Center 网关。
3.  在用户和组选项卡中，选择**添加用户**。 必须分配网关用户或每个用户/组添加的网关的管理员角色。

一旦启用 Azure AD 身份验证，则网关服务重新启动并且您必须刷新浏览器。 您可以更新 SME Azure AD 应用程序在 Azure 门户中在任何时候用户访问权限。

将提示用户在尝试访问 Windows Admin Center 网关 URL 时使用其 Azure Active Directory 身份进行登录。 请记住，用户还必须在网关服务器的本地用户访问 Windows Admin Center 的成员。

用户和管理员可以查看其当前登录的帐户和从此 Azure AD 帐户以及注销**帐户**Windows Admin Center 设置选项卡。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>配置 Windows Admin Center 的 Azure Active Directory 身份验证

[若要设置 Azure AD 身份验证，必须先注册你的网关与 Azure](azure-integration.md) （您只需执行一次此 Windows Admin Center 网关）。 此步骤将创建可用于管理网关用户网关的管理员访问权限的 Azure AD 应用程序。

如果想要授予特定的 Azure AD 用户或组网关的用户或网关管理员访问权限的 Windows Admin Center 服务，必须执行以下操作：

1.  请转到你在 Azure 门户中的 SME Azure AD 应用程序。 
    -   当您单击**更改访问控制**，然后选择**Azure Active Directory**从 Windows Admin Center 访问设置，您可以使用在 UI 中提供的超链接来访问你的 Azure AD在 Azure 门户中的应用程序。 此超链接也位于访问设置后单击保存，并已选择 Azure AD 作为标识提供者的访问控制。
    -   此外可以找到你的应用程序在 Azure 门户中通过转到**Azure Active Directory** > **企业应用程序** > **的所有应用程序**并搜索**SME** (将名为 Azure AD 应用程序 SME-<gateway>)。 如果没有获得任何搜索结果，请确保**显示**设置为**的所有应用程序**，**应用程序状态**设置为**任何**并单击应用然后尝试搜索。 一旦找到应用程序，请转到**用户和组**
2.  在属性选项卡中设置**需要进行用户分配**为是。
    完成连接后，只有成员列入**用户和组**选项卡将能够访问 Windows Admin Center 网关。
3.  在用户和组选项卡中，选择**添加用户**。 必须分配网关用户或每个用户/组添加的网关的管理员角色。

一旦保存 Azure AD 中的访问控制**更改访问控制**窗格中，网关服务将重新启动，必须刷新浏览器。 您可以更新 Windows Admin Center Azure AD 应用程序在 Azure 门户中在任何时候用户访问权限。 

将提示用户在尝试访问 Windows Admin Center 网关 URL 时使用其 Azure Active Directory 身份进行登录。 请记住，用户还必须在网关服务器的本地用户访问 Windows Admin Center 的成员。 

使用**Azure**卡中的 Windows Admin Center 常规设置、 用户和管理员可以查看其当前登录的帐户和此 Azure AD 帐户以及注销。

### <a name="conditional-access-and-multi-factor-authentication"></a>条件性访问和多重身份验证

使用 Azure AD 作为额外层来控制对 Windows Admin Center 网关访问的优势之一是安全的你可以利用 Azure AD 的功能强大的安全功能，如条件性访问和多重身份验证。 

[了解有关使用 Azure Active Directory 中配置条件性访问的详细信息。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>配置单一登录

**单一登录时作为 Windows 服务器上的服务部署**

在 Windows 10 上安装 Windows Admin Center 时，已准备好使用单一登录。 如果您将在 Windows Server 上使用 Windows Admin Center，但是，您需要设置某种形式的环境中的 Kerberos 委派，然后才能使用单一登录。 委派将网关计算机配置为受信任委派到目标节点。 

若要配置[基于资源的约束委派](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1)在环境中，运行以下 PowerShell cmdlet。 （要注意，这要求域控制器运行 Windows Server 2012 或更高版本）。

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

在此示例中，在服务器上安装 Windows Admin Center 网关**WindowsAdminCenterGW**，并且目标节点名称为**ManagedNode**。

若要删除此关系，请运行以下 cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>基于角色的访问控制

基于角色的访问控制，可向用户提供受限访问权限的计算机而不是使其完整的本地管理员。
[详细了解基于角色的访问控制和可用的角色。](../plan/user-access-options.md#role-based-access-control)

设置 RBAC 包含 2 个步骤： 目标计算机上启动支持，并将用户分配到相关的角色。

> [!TIP]
> 请确保在计算机上具有本地管理员权限的配置支持基于角色的访问控制。

### <a name="apply-role-based-access-control-to-a-single-machine"></a>将基于角色的访问控制应用到一台计算机

在单台计算机部署模型非常适合仅几个要管理的计算机的简单环境。
使用支持基于角色的访问控制配置一台计算机将导致以下更改：
-   在系统驱动器中，将在安装 PowerShell 模块与函数所需的 Windows Admin Center `C:\Program Files\WindowsPowerShell\Modules`。 所有模块将从都开始**Microsoft.Sme**
-   Desired State Configuration 将运行一次性配置名为的机上配置的 Just Enough Administration 的终结点**Microsoft.Sme.PowerShell**。 此终结点定义使用 Windows Admin Center 的 3 个角色，并在用户连接到它时将作为临时的本地管理员运行。
-   控制哪些用户分配到了哪些角色访问权限，将创建 3 个新的本地组：
    -   Windows Admin Center 管理员
    -   Windows Admin Center 的 HYPER-V 管理员
    -   Windows Admin Center 读取器

若要启用对一台计算机上的基于角色的访问控制的支持，请按照下列步骤：

1.  打开 Windows Admin Center 并连接到你想要使用基于角色的访问控制使用具有目标计算机上的本地管理员权限的帐户配置的计算机。
2.  上**概述**工具中，单击**设置** > **基于角色的访问控制**。
3.  单击**应用**底部的页后，可以启用对目标计算机上的基于角色的访问控制的支持。 应用程序进程涉及在目标计算机上复制 PowerShell 脚本和调用 （使用 PowerShell Desired State Configuration） 的配置。 可能需要 10 分钟才能完成，并且将导致重新启动 WinRM。 这会暂时断开连接 Windows Admin Center、 PowerShell 和 WMI 的用户。
4.  刷新页面以检查基于角色的访问控制的状态。 用于准备就绪时，状态将更改为**应用**。

配置应用后，可以将用户分配角色：

1.  打开**本地用户和组**工具，然后导航到**组**选项卡。
2.  选择**Windows Admin Center 读者**组。
3.  在中*详细信息*窗格的底部，单击**添加用户**，然后输入应通过 Windows Admin Center 具有到服务器的只读访问权限的用户或安全组的名称。 用户和组可以来自本地计算机或你的 Active Directory 域。
4.  重复步骤 2-3 **Windows Admin Center HYPER-V 管理员**并**Windows Admin Center 管理员**组。

您还可以填充这些组一致地在你的域之间通过配置组策略对象与[受限的组策略设置](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29)。

### <a name="apply-role-based-access-control-to-multiple-machines"></a>将基于角色的访问控制应用到多台计算机

在大型企业部署中，可以使用现有的自动化工具将推送的基于角色的访问控制功能到你的计算机通过从 Windows Admin Center 网关下载配置包。
配置包设计用于 PowerShell Desired State Configuration，但您也可以修改它以使用您首选的自动化解决方案。

#### <a name="download-the-role-based-access-control-configuration"></a>下载基于角色的访问控制配置

若要下载的基于角色的访问控制配置包，您将需要有权访问 Windows Admin Center 和 PowerShell 提示符。

如果在服务模式下运行 Windows Admin Center 网关在 Windows Server 上，使用以下命令来下载配置包。
请确保使用正确的订阅为您的环境更新网关地址。

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

如果在 Windows 10 计算机上运行 Windows Admin Center 网关，请改为运行以下命令：

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

在展开 zip 存档时，会看到以下文件夹结构：
- InstallJeaFeatures.ps1
- JustEnoughAdministration （目录）
- 模块 （目录）
    - Microsoft.SME。\* （目录）
    - WindowsAdminCenter.Jea (directory)

若要配置对节点上的基于角色的访问控制的支持，需要执行以下操作：
1.  将复制 JustEnoughAdministration，Microsoft.SME。\*，和 WindowsAdminCenter.Jea 模块到目标计算机上的 PowerShell 模块目录。 通常情况下，该文件夹位于`C:\Program Files\WindowsPowerShell\Modules`。
2.  更新**InstallJeaFeature.ps1**文件以匹配的 RBAC 终结点所需的配置。
3.  运行 InstallJeaFeature.ps1 来编译 DSC 资源。
4.  将 DSC 配置部署到所有计算机以应用配置。

以下部分介绍如何执行此操作使用 PowerShell 远程处理。

#### <a name="deploy-on-multiple-machines"></a>在多个计算机上部署

若要部署到多台计算机上下载的配置，你将需要更新**InstallJeaFeatures.ps1**脚本以包括你的环境的适当的安全组，将文件复制到每台计算机，然后调用配置脚本。
可以使用首选的自动化工具来实现此目的，但本文将重点介绍纯基于 PowerShell 的方法。

默认情况下，将配置脚本创建本地安全组来控制对每个角色的访问在计算机上。
这是适用于工作组和域已加入的计算机，但如果你正在部署在仅限域的环境中，您可能希望直接将与每个角色关联的域安全组。
若要更新配置为使用域安全组，打开**InstallJeaFeatures.ps1**并进行以下更改：

1.  删除第三**组**文件中的资源：
    1.  "组 MS 读取器组"
    2.  "Group MS-Hyper-V-Administrators-Group"
    3.  "组 MS 管理员组"
2.  3 个组资源移除 JeaEndpoint **DependsOn**属性
    1.  "[Group]MS-Readers-Group"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group]MS-Administrators-Group"
3.  更改组名称在 JeaEndpoint **RoleDefinitions**到所需的安全组的属性。 例如，如果您有一个安全组*CONTOSO\MyTrustedAdmins* ，应为 Windows Admin Center 管理员角色，更改分配访问权限`'$env:COMPUTERNAME\Windows Admin Center Administrators'`到`'CONTOSO\MyTrustedAdmins'`。 你需要更新的三个字符串是：
    1.  $env: COMPUTERNAME\Windows 管理员中心管理员 '
    2.  $env: COMPUTERNAME\Windows 管理员中心的 HYPER-V 管理员 '
    3.  $env: COMPUTERNAME\Windows 管理员中心读者

> [!NOTE]
> 请确保为每个角色使用独特的安全组。 如果同一安全组分配到多个角色，配置将失败。

接下来，在末尾**InstallJeaFeatures.ps1**文件中，将 PowerShell 的以下行添加到脚本底部：

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

最后，您可以复制包含模块、 DSC 资源和配置到每个目标节点的文件夹，然后运行**InstallJeaFeature.ps1**脚本。
若要远程执行此操作从管理工作站，可以运行以下命令：

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
