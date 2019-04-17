---
title: 配置用户访问控制和权限
description: 了解如何配置用户访问控制和权限使用 Active Directory 或 Azure AD (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2019
ms.locfileid: "9152042"
---
# 配置用户访问控制和权限

>适用于：Windows Admin Center、Windows Admin Center 预览版

如果你尚未这样做，使自己熟悉[Windows Admin Center 中的用户访问控制选项](../plan/user-access-options.md)

>[!NOTE]
> 在工作组环境中或通过不受信任的域不支持 Windows Admin Center 中的基于组访问。

## 网关访问角色定义

有两个角色的访问权限的 Windows Admin Center 网关服务：

**网关用户**可以连接到 Windows Admin Center 网关服务来管理服务器通过该网关，但不是能更改的访问权限，也不用于到网关身份验证的身份验证机制。

**网关管理员**可以配置谁可以访问权限以及如何对网关的用户进行身份验证。 只有网关管理员可以查看和配置 Windows Admin Center 中的访问权限设置。 网关计算机上的本地管理员始终是 Windows Admin Center 网关服务的管理员。

> [!NOTE]
> 访问网关并不意味着由网关到托管服务器可见的访问权限。 若要管理目标服务器，连接的用户必须使用凭据 （无论是通过其传递到 Windows 凭据或使用**作为管理**操作的 Windows Admin Center 会话中提供的凭据） 具有管理访问权限为该目标服务器。

## Active Directory 或本地计算机组

默认情况下，使用 Active Directory 或本地计算机组用于控制网关的访问。 如果你有 Active Directory 域，你可以管理网关用户和管理员在 Windows Admin Center 界面中从访问。

在**用户**选项卡上，你可以控制谁可以访问 Windows Admin Center 作为网关用户。 默认情况下，并且如果你未指定安全组，任何用户访问的网关 URL 访问。 一旦用户列表中添加一个或多个安全组，访问被限制到这些组的成员。

如果你不在你的环境中使用 Active Directory 域，由控制访问```Users```和```Administrators```Windows Admin Center 网关计算机上的本地组。

### 智能卡身份验证

你可以通过指定其他_所需_组基于智能卡的安全组，在强制执行**智能卡身份验证**。 一旦你已添加基于智能卡的安全组，用户只能访问 Windows Admin Center 服务如果它们是任何安全组的成员和智能卡组包含在用户列表。

**管理员**选项卡上，你可以控制谁可以访问 Windows Admin Center 作为网关管理员。 在计算机上的本地管理员组将始终具有完整的管理员访问权限，并且无法从列表中删除。 通过添加安全组，可以将这些组权限的成员，若要更改 Windows Admin Center 网关设置。 管理员列表支持智能卡身份验证的用户列表相同的方式： 为安全组和智能卡组的 AND 条件。

## Azure Active Directory

如果你的组织使用 Azure Active Directory (Azure AD)，你可以选择通过需要 Azure AD 身份验证才能访问该网关添加到 Windows Admin Center 的一层**额外**的安全。 若要访问 Windows Admin Center，用户的**Windows 帐户**还必须访问网关服务器 （即使使用 Azure AD 身份验证）。 当你使用 Azure AD 时，你将管理 Windows Admin Center 用户和管理员访问权限，从 Azure 门户中，而不是从 Windows Admin Center UI。

### 当启用了 Azure AD 身份验证访问 Windows Admin Center

具体取决于使用的浏览器，使用配置的 Azure AD 身份验证的访问 Windows Admin Center 将收到其他提示**从浏览器**所需提供其 Windows 某些用户帐户凭据的计算机上安装 Windows Admin Center。 输入该信息后, 的用户将收到其他的 Azure Active Directory 的身份验证提示，这需要 Azure 帐户已授予访问权限在 Azure 中的 Azure AD 应用程序中的凭据。

> [!NOTE]
> 用户的 Windows 帐户具有**管理员权限**的网关计算机会在 Azure AD 身份验证不会提示用户。

### 配置 Windows Admin Center 预览版的 Azure Active Directory 身份验证

转到 Windows Admin Center**设置** > **访问**和使用切换开关，打开"使用 Azure Active Directory 以添加到网关的一层安全性"。 如果你尚未注册到 Azure 的网关，你将指导执行该操作这一次。

默认情况下，Azure AD 租户中的所有成员都具有用户对 Windows Admin Center 网关服务的访问。 只有本地管理员网关计算机上的具有管理员访问 Windows Admin Center 网关。 请注意，网关计算机上的本地管理员权限不能是受限-本地管理员可以执行任何操作而不考虑是否使用 Azure AD 进行身份验证。

如果你希望为特定的 Azure AD 用户或组网关用户或对 Windows Admin Center 服务的网关管理员访问权限，你必须执行以下操作：

1.  通过使用访问设置中提供的超链接，请转到你的 Azure 门户中的 Windows Admin Center Azure AD 应用程序。 请注意启用 Azure Active Directory 身份验证时，此超链接才可用。 
    -   你还可以找到你的应用程序在 Azure 门户中通过转到**Azure Active Directory** > **企业应用程序** > **所有应用程序**和搜索**WindowsAdminCenter** （Azure AD 应用程序将被命名为WindowsAdminCenter-<gateway name>)。 如果你不获取任何搜索结果，确保**显示**设置为**所有应用程序**，**应用程序状态**设置为**任何**并单击应用，然后尝试搜索。 一旦找到应用程序，请转到**用户和组**
2.  在属性选项卡中，将设置为是的**用户分配所需**。
    完成此操作后，只有在**用户和组**选项卡中列出的成员将能够访问 Windows Admin Center 网关。
3.  在用户和组选项卡中，选择**添加用户**。 你必须分配网关用户或每个用户/组添加的网关管理员角色。

一旦你打开 Azure AD 身份验证，网关服务重启，你必须刷新浏览器。 你可以更新的 Azure 门户中的 SME Azure AD 应用程序在任何时间的用户访问权限。

将提示用户在尝试访问 Windows Admin Center 网关 URL 时使用其 Azure Active Directory 身份登录。 请记住，用户还必须要访问 Windows Admin Center 网关服务器上的本地用户的成员。

用户和管理员可以查看其当前登录的帐户和以及注销时此 Azure AD 的帐户从**帐户**选项卡的 Windows Admin Center 设置。

### 配置 Windows Admin center 的 Azure Active Directory 身份验证

[若要设置 Azure AD 身份验证，必须首先注册你使用 Azure 的网关](azure-integration.md)（你只需执行一次此 Windows Admin Center 网关）。 此步骤创建 Azure AD 应用程序，你可以从中管理网关用户和网关的管理员访问权限。

如果你希望为特定的 Azure AD 用户或组网关用户或对 Windows Admin Center 服务的网关管理员访问权限，你必须执行以下操作：

1.  转到 Azure 门户中 SME Azure AD 应用程序。 
    -   当你单击**更改访问控制**，然后选择 Windows Admin Center 访问设置中的**Azure Active Directory**时，你可以使用在 UI 中提供的超链接访问你的 Azure 门户中的 Azure AD 应用程序。 单击保存并选择 Azure AD 与访问控制标识提供程序后，此超链接还是访问设置中可用。
    -   你还可以找到你的应用程序在 Azure 门户中通过转到**Azure Active Directory** > **企业应用程序** > **所有应用程序**和搜索**SME** (Azure AD 应用程序将被命名为 SME-<gateway>)。 如果你不获取任何搜索结果，确保**显示**设置为**所有应用程序**，**应用程序状态**设置为**任何**并单击应用，然后尝试搜索。 一旦找到应用程序，请转到**用户和组**
2.  在属性选项卡中，将设置为是的**用户分配所需**。
    完成此操作后，只有在**用户和组**选项卡中列出的成员将能够访问 Windows Admin Center 网关。
3.  在用户和组选项卡中，选择**添加用户**。 你必须分配网关用户或每个用户/组添加的网关管理员角色。

一旦你保存的 Azure AD 访问控制在**更改访问控制**窗格中，在网关服务重新启动，你必须刷新浏览器。 你可以更新的 Azure 门户中的 Windows Admin Center Azure AD 应用程序在任何时间的用户访问权限。 

将提示用户在尝试访问 Windows Admin Center 网关 URL 时使用其 Azure Active Directory 身份登录。 请记住，用户还必须要访问 Windows Admin Center 网关服务器上的本地用户的成员。 

使用 Windows Admin Center 常规设置的**Azure**选项卡，用户和管理员可以查看其当前登录的帐户和此 Azure AD 帐户以及注销。

### 条件访问和多重身份验证

使用 Azure AD 为一层额外来控制访问 Windows Admin Center 网关的好处之一是安全性的，你可以利用 Azure AD 的功能强大的安全功能，如条件访问和多重身份验证。 

[了解有关配置与 Azure Active Directory 条件访问。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## 配置单一登录

**单一登录时作为 Windows Server 上的服务部署**

Windows 10 上安装 Windows Admin Center 时，它已准备好使用单一登录。 如果你要将 Windows Server 上使用 Windows Admin Center，但是，你需要设置某种形式的 Kerberos 委派你的环境中，然后才能使用单一登录。 委派为受信任，才能委派到目标节点来配置网关计算机。 

若要配置你的环境中的[基于资源的约束委派](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1)，运行以下 PowerShell cmdlet。 （不知道这需要在运行 Windows Server 2012 域控制器或更高版本）。

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

在此示例中，服务器**WindowsAdminCenterGW**上, 安装 Windows Admin Center 网关和目标节点名称是**ManagedNode**。

若要删除的这种关系，运行以下 cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## 基于角色的访问控制

基于角色的访问控制可以向用户提供对而不是使其完整本地管理员的计算机的有限访问。
[阅读有关基于角色的访问控制和可用的角色的详细信息。](../plan/user-access-options.md#role-based-access-control)

设置 RBAC 包含 2 步： 目标计算机上启动支持并将用户分配给相关的角色。

> [!TIP]
> 请确保在计算机上具有本地管理员权限的配置基于角色的访问控制的支持。

### 将基于角色的访问控制应用到一台计算机

单个计算机部署模型非常适合于仅几台计算机来管理与简单环境。
对基于角色的访问控制的支持配置计算机将导致以下更改：
-   与 Windows Admin Center 所需的功能的 PowerShell 模块将安装在系统驱动器，在`C:\Program Files\WindowsPowerShell\Modules`。 所有模块将都开头**Microsoft.Sme**
-   所需的状态配置将运行一次性配置，以在名为**Microsoft.Sme.PowerShell**计算机上配置的 Just Enough Administration 终结点。 此终结点定义使用通过 Windows Admin Center 的 3 个角色，并将作为临时的本地管理员运行，当用户连接到它。
-   控制哪些用户分配的访问权限的角色，将创建 3 个新的本地组：
    -   Windows Admin Center 管理员
    -   Windows Admin Center HYPER-V 管理员
    -   Windows Admin Center 阅读器

若要启用对在单台计算机上的基于角色的访问控制的支持，请按照下列步骤：

1.  打开 Windows Admin Center 并连接到你想要使用基于角色的访问控制使用具有在目标计算机上的本地管理员权限的帐户配置的计算机。
2.  在**概述**工具中，单击**设置** > **基于角色的访问控制**。
3.  单击**应用**页面底部以启用对基于角色的访问控制的目标计算机上的支持。 应用程序过程包括在目标计算机上复制的 PowerShell 脚本和调用 （使用 PowerShell Desired State Configuration） 的配置。 它可能需要 10 分钟才能完成，并将导致 WinRM 重启。 这将临时断开 Windows Admin Center、 PowerShell 和 WMI 的用户。
4.  刷新页面，以检查基于角色的访问控制的状态。 可以使用时，状态将更改为**应用**。

一旦应用配置时，可以将用户分配角色：

1.  打开**本地用户和组**工具，然后导航到**组**选项卡。
2.  选择**Windows Admin Center 阅读器**组。
3.  在底部的*详细信息*窗格中，单击**添加用户**，然后输入应通过 Windows Admin Center 具有到服务器的只读访问权限的用户或安全组的名称。 用户和组可以来自本地计算机或 Active Directory 域。
4.  在**Windows Admin Center HYPER-V 管理员**和**Windows Admin Center 管理员**组重复步骤 2-3。

你还可以填充这些组一致地跨域通过使用[受限的组策略设置](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29)配置组策略对象。

### 将基于角色的访问控制应用到多台计算机

在大型企业部署中，你可以使用现有的自动化工具推送基于角色的访问控制功能至你的计算机从 Windows Admin Center 网关下载配置包的。
配置包设计用于 PowerShell Desired State Configuration，但你也可以修改其使用首选的自动化解决方案。

#### 下载基于角色的访问控制配置

若要下载在基于角色的访问控制配置包，你将需要有权访问 Windows Admin Center 和 PowerShell 提示符。

如果你运行的 Windows Admin Center 网关服务模式在 Windows Server 上，使用以下命令来下载配置包。
请务必更新你的环境的正确一个网关地址。

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

如果你运行的 Windows Admin Center 网关的 Windows 10 计算机上，请改为运行以下命令：

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

在展开的 zip 存档时，你将看到以下文件夹结构：
- InstallJeaFeatures.ps1
- JustEnoughAdministration （目录）
- 模块 （目录）
    - Microsoft.SME.\* （目录）
    - WindowsAdminCenter.Jea （目录）

若要配置的节点上的基于角色的访问控制的支持，你需要执行以下操作：
1.  将 JustEnoughAdministration、 Microsoft.SME.\* 和 WindowsAdminCenter.Jea 模块复制到目标计算机上的 PowerShell 模块目录。 通常，这是位于`C:\Program Files\WindowsPowerShell\Modules`。
2.  更新**InstallJeaFeature.ps1**文件以匹配 RBAC 终结点你所需的配置。
3.  运行 InstallJeaFeature.ps1 编译 DSC 资源。
4.  将你的 DSC 配置部署到所有计算机将配置应用。

以下部分介绍了如何使用 PowerShell 远程处理执行此操作。

#### 在多台计算机上部署

若要部署下载到多台计算机上的配置，你将需要更新**InstallJeaFeatures.ps1**脚本以包括你的环境适当的安全组将文件复制到每台计算机，并调用配置脚本。
你可以使用首选的自动化工具来完成此任务，但是本文重点使用纯基于 PowerShell 的方法。

默认情况下，配置脚本将用于控制对每个角色访问在计算机上创建本地安全组。
这是适用于工作组，域加入的计算机，但如果你可能想为直接在仅域环境中部署每个角色相关联的域安全组。
若要更新配置，以使用域安全组，打开**InstallJeaFeatures.ps1**并进行以下更改：

1.  从文件中删除的 3 个**组**资源：
    1.  "组 MS 阅读器组"
    2.  "组 MS 超-V-管理员的组"
    3.  "组 MS 管理员组"
2.  从 JeaEndpoint**取决于**属性中移除的 3 个组资源
    1.  "[组] MS 阅读器组"
    2.  "[组] MS 超-V-管理员的组"
    3.  "[组] MS 管理员组"
3.  将 JeaEndpoint **RoleDefinitions**属性中的组名称更改为你所需的安全组。 例如，如果你有一个安全组*CONTOSO\MyTrustedAdmins*应到了 Windows Admin Center 管理员角色分配的访问权限，更改`'$env:COMPUTERNAME\Windows Admin Center Administrators'`到`'CONTOSO\MyTrustedAdmins'`。 你需要更新的三个字符串都：
    1.  $env: COMPUTERNAME\Windows 管理员中心管理员的
    2.  $env: COMPUTERNAME\Windows 管理员中心 HYPER-V 管理员的
    3.  $env: COMPUTERNAME\Windows 管理员中心读者

> [!NOTE]
> 请务必使用为每个角色的唯一安全组。 如果相同的安全组分配给多个角色，配置将失败。

接下来，在**InstallJeaFeatures.ps1**文件末尾，添加以下行的 PowerShell 脚本的底部：

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

最后，你可以复制包含模块、 DSC 资源和配置到每个目标节点的文件夹，并运行**InstallJeaFeature.ps1**脚本。
若要执行此操作远程从管理员工作站，可以运行以下命令：

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
