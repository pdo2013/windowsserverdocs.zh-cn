---
title: 管理主机保护者服务
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 41912c90beacbb0c0c285ea4da8305c1afdf2a51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386540"
---
# <a name="managing-the-host-guardian-service"></a>管理主机保护者服务

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

主机保护者服务（HGS）是受保护的构造解决方案的成为。
它负责确保将构造中的 Hyper-v 主机称为宿主或企业，并运行受信任的软件，并管理用于启动受防护 Vm 的密钥。
当租户决定信任你托管其受防护的 Vm 时，它们会将其信任置于主机保护者服务的配置和管理中。
因此，在管理主机保护者服务时必须遵循最佳做法，以确保受保护的构造的安全性、可用性和可靠性。
以下各节中的指南介绍了适用于 HGS 的管理员最常见的操作问题。

## <a name="limiting-admin-access-to-hgs"></a>限制对 HGS 的管理员访问权限
由于 HGS 的安全敏感性，确保其管理员是组织的高度可信成员，并且最好与构造资源的管理员区分开来，这一点很重要。
此外，建议您仅使用安全通信协议（例如通过 HTTPS 的 WinRM）从安全工作站管理 HGS。

### <a name="separation-of-duties"></a>职责分离
设置 HGS 时，可以选择仅为 HGS 创建独立的 Active Directory 林，或将 HGS 加入到现有的受信任域中。
此决定以及在组织中分配管理员的角色决定了 HGS 的信任边界。
谁有权访问 HGS，无论是直接作为管理员还是作为其他人（如 Active Directory）的管理员，可以影响 HGS，都可以控制受保护的结构。
HGS 管理员选择有权运行受防护的 Vm 的 Hyper-v 主机，并管理启动受防护的 Vm 所需的证书。
有权访问 HGS 的攻击者或恶意管理员可以利用此功能来授权受攻击的主机运行受防护的 Vm，通过删除密钥材料等来发起拒绝服务攻击。

为避免这种风险，*强烈*建议您限制 hgs 的管理员（包括 hgs 加入到的域）和 hyper-v 环境之间的重叠。
通过确保没有一个管理员可以访问这两个系统，攻击者需要从两个人中泄露2个不同的帐户，以完成其任务来更改 HGS 策略。
这也意味着，两个 Active Directory 环境的域和企业管理员不应为同一人，并且 HGS 使用与 Hyper-v 主机相同的 Active Directory 林。
任何可以授予自身访问权限的人都面临安全风险。

### <a name="using-just-enough-administration"></a>使用足够的管理
HGS 附带了[足够的管理](https://aka.ms/JEAdocs)（JEA）角色，可帮助你更安全地管理它。
JEA 可帮助用户将管理员任务委派给非管理员用户，这意味着管理 HGS 策略的人员实际上不需要是整个计算机或域的管理员。
JEA 的工作原理是限制用户可在 PowerShell 会话中运行的命令，并在后台使用临时本地帐户（每个用户会话都是唯一的）来运行通常需要提升的命令。

HGS 附带2个预配置的 JEA 角色：
- 允许用户管理所有 HGS 策略的**Hgs 管理员**，包括授权新主机运行受防护的 vm。
- 仅允许用户审核现有策略的**HGS 审阅者**。 它们不能对 HGS 配置进行任何更改。

若要使用 JEA，首先需要创建一个新的标准用户，并使其成为 HGS 管理员或 HGS 审阅者组的成员。
如果使用 @no__t 为 HGS 设置新林，则这些组将分别命名为 "*servicename*Administrators" 和 "*servicename*审校"，其中*servicename*是 HGS 群集的网络名称。
如果已将 HGS 加入到现有域中，则应引用在 `Initialize-HgsServer` 中指定的组名称。

**为 HGS 管理员和审阅者角色创建标准用户**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**具有审阅者角色的审核策略**

在通过网络连接到 HGS 的远程计算机上，在 PowerShell 中运行以下命令，以使用审阅者凭据输入 JEA 会话。
需要注意的是，由于审阅者帐户只是标准用户，因此不能用于常规的 Windows PowerShell 远程处理、对 HGS 的远程桌面访问等。

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

然后，你可以检查会话中允许的哪些命令与 @no__t 0，并运行任何允许的命令来审核配置。
在下面的示例中，我们将检查在 HGS 上启用的策略。

```powershell
Get-Command

Get-HgsAttestationPolicy
```

使用 JEA 会话完成后，请键入命令 `Exit-PSSession` 或其别名 `exit`。 

**使用管理员角色向 HGS 添加新策略**

若要实际更改策略，需要使用属于 "hgsAdministrators" 组的标识连接到 JEA 终结点。
在下面的示例中，我们演示了如何将新的代码完整性策略复制到 HGS 并使用 JEA 注册它。
语法可能与你使用的不同。
这是为了满足 JEA 中的某些限制，例如不能访问完整的文件系统。

```powershell
$cipolicy = Get-Item "C:\temp\cipolicy.p7b"
$session = New-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsadmin01' -ConfigurationName 'microsoft.windows.hgs'
Copy-Item -Path $cipolicy -Destination 'User:' -ToSession $session

# Now that the file is copied, we enter the interactive session to register it with HGS
Enter-PSSession -Session $session
Add-HgsAttestationCiPolicy -Name 'New CI Policy via JEA' -Path 'User:\cipolicy.p7b'

# Confirm it was added successfully
Get-HgsAttestationPolicy -PolicyType CiPolicy

# Finally, remove the PSSession since it is no longer needed
Exit-PSSession
Remove-PSSession -Session $session
```

## <a name="monitoring-hgs"></a>监视 HGS
### <a name="event-sources-and-forwarding"></a>事件源和转发
来自 HGS 的事件将显示在 Windows 事件日志中的2个源下：
- **HostGuardianService-证明**
- **HostGuardianService-KeyProtection**

若要查看这些事件，可打开事件查看器并导航到 HostGuardianService-HostGuardianService-KeyProtection。

在大型环境中，通常最好将事件转发到中央 Windows 事件收集器，使事件的分析变得更简单。
有关详细信息，请查看[Windows 事件转发文档](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx)。

### <a name="using-system-center-operations-manager"></a>使用 System Center Operations Manager
你还可以使用 System Center 2016-Operations Manager 来监视 HGS 和受保护的主机。
受保护的结构管理包具有事件监视器，用于检查可能导致数据中心停机的常见错误配置，包括未通过证明的主机和用于报告错误的 HGS 服务器。

若要开始，请[安装并配置 SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager)并[下载受保护的结构管理包](https://www.microsoft.com/download/details.aspx?id=52764)。
随附的管理包指南介绍了如何配置管理包并了解其监视器的作用域。

## <a name="backing-up-and-restoring-hgs"></a>备份和还原 HGS
### <a name="disaster-recovery-planning"></a>灾难恢复规划
在起草灾难恢复计划时，请务必考虑受保护的构造中主机保护者服务的独特要求。
如果你丢失了部分或全部 HGS 节点，你可能会遇到会阻止用户启动其受防护 Vm 的即时可用性问题。
在丢失整个 HGS 群集的情况下，你将需要手动备份 HGS 配置以还原 HGS 群集并恢复正常操作。
本部分介绍为此类方案做好准备所需的步骤。

首先，请务必了解 HGS 对于备份至关重要的内容。
HGS 保留了几条信息，这些信息可帮助 it 确定哪些主机有权运行受防护的 Vm。
这包括：
1. Active Directory 包含受信任主机的组的安全标识符（使用 Active Directory 证明时）;
2. 环境中每个主机的唯一 TPM 标识符;
3. 主机的每个唯一配置的 TPM 策略;与
4. 确定允许在主机上运行的软件的代码完整性策略。

这些证明项目需要与托管构造的管理员协调才能获得，这可能导致在发生灾难后再次难以获取此信息。

此外，HGS 还需要访问2个或更多证书，用于对启动受防护的 VM 所需的信息进行加密和签名（密钥保护程序）。
这些证书众所周知（由受防护的 Vm 的所有者用来授权构造运行其 Vm），并且必须在发生灾难后恢复无缝恢复体验。
发生灾难后，如果不使用相同的证书还原 HGS，则需要更新每个 VM，以授权新密钥对其信息进行解密。
出于安全原因，只有 VM 所有者可以更新 VM 配置以授权这些新密钥，这意味着在发生灾难后未能还原密钥，因此，每个 VM 所有者都需要采取措施使其 Vm 再次运行。

#### <a name="preparing-for-the-worst"></a>准备最差
若要为完全丢失 HGS 做好准备，需要执行两个步骤：
1. 备份 HGS 证明策略
2. 备份 HGS 密钥

[备份 HGS](#backing-up-hgs)部分提供了有关如何执行上述两个步骤的指导。

此外，建议您备份在其 Active Directory 域或 Active Directory 本身有权管理 HGS 的用户的列表。

应定期执行备份，以确保信息是最新的并安全存储，以避免篡改或被盗。

**不建议**备份或尝试还原 HGS 节点的整个系统映像。
如果你丢失了整个群集，最佳做法是设置全新的 HGS 节点并仅还原 HGS 状态，而不是整个服务器操作系统。

#### <a name="recovering-from-the-loss-of-one-node"></a>从一个节点丢失恢复
如果你在 HGS 群集中丢失了一个或多个节点（而不是每个节点），只需遵循部署指南中的指南[将节点添加到群集](guarded-fabric-configure-additional-hgs-nodes.md)。
认证策略将自动同步，作为具有随附密码的 PFX 文件提供给 HGS 的任何证书都将自动同步。
对于使用指纹（通常为不可导出和硬件支持的证书）添加到 HGS 的证书，你将需要确保每个新节点都有权访问每个证书的私钥。

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>恢复整个群集损失
如果你的整个 HGS 群集出现故障，并且你无法将其恢复为联机状态，则你将需要从备份还原 HGS。
从备份还原 HGS 需要首先按照[部署指南中的指南](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)设置新的 hgs 群集。
在设置恢复 HGS 环境时，强烈建议（但不要求）使用相同的群集名称，以协助主机中的名称解析。
使用相同的名称可以避免使用新的证明和密钥保护 Url 重新配置主机。
如果已将对象还原到 Active Directory 域后备 HGS，则建议你在初始化 HGS 服务器之前删除代表 HGS 群集、计算机、服务帐户和 JEA 组的对象。

设置第一个 HGS 节点（例如，已安装并初始化它）后，将按照[从备份还原 HGS 中](#restoring-hgs-from-a-backup)的过程进行操作，以还原认证策略和密钥保护证书的公共部分。
你将需要根据证书提供程序的指导手动还原证书的私钥（例如，在 Windows 中导入证书，或配置对支持 HSM 的证书的访问）。
设置第一个节点后，可以继续在[群集中安装其他节点](guarded-fabric-configure-additional-hgs-nodes.md)，直到达到所需的容量和复原能力。

### <a name="backing-up-hgs"></a>备份 HGS
HGS 管理员应负责定期备份 HGS。
完整备份将包含必须适当保护的敏感密钥材料。
如果不受信任的实体能够访问这些密钥，则他们可以使用该材料来设置恶意的 HGS 环境，以损害受防护的 Vm。

**备份证明策略**若要备份 HGS 证明策略，请在任何工作的 HGS 服务器节点上运行以下命令。
系统将提示您提供密码。
此密码用于加密使用 PFX 文件（而不是证书指纹）添加到 HGS 的任何证书。

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> 如果你使用的是管理员信任的证明，则必须单独备份由 HGS 使用的安全组中的成员身份来授权受保护的主机。
> HGS 将只备份安全组的 SID，而不是它们内的成员身份。
> 在发生灾难时，这些组会丢失，需要重新创建组，并再次将每个受保护的主机添加到这些组中。

**备份证书**

@No__t-0 命令将在运行命令时备份添加到 HGS 的任何基于 PFX 的证书。
如果使用指纹将证书添加到 HGS （对于不可导出和硬件支持的证书，则需要手动备份证书的私钥）。
若要确定哪些证书已注册到 HGS 并需要手动备份，请在任何工作的 HGS 服务器节点上运行以下 PowerShell 命令。

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

对于列出的每个证书，你将需要手动备份私钥。
如果你使用的是不可导出的基于软件的证书，则应与证书颁发机构联系，以确保其具有证书的备份，并/或者可以根据需要重新发出证书。
对于创建并存储在硬件安全模块中的证书，应查阅设备的文档以获取有关灾难恢复计划的指南。

你应将证书备份与证明策略备份一起存储在安全位置，以便可以同时还原这两个部分。

**要备份的其他配置**

备份的 HGS 服务器状态不包括你的 HGS 群集的名称、Active Directory 中的任何信息或用于保护与 HGS Api 的通信的任何 SSL 证书。
这些设置对于一致性很重要，但在发生灾难后，使 HGS 群集恢复联机状态并不重要。

若要捕获 HGS 服务的名称，请运行 `Get-HgsServer` 并记下证明和密钥保护 Url 中的平面名称。
例如，如果证明 URL 为 "<http://hgs.contoso.com/Attestation>"，则 "hgs" 为 HGS 服务名称。

应像管理任何其他 Active Directory 域一样管理 HGS 使用的 Active Directory 域。
在灾难发生后恢复 HGS 时，无需重新创建当前域中存在的完全相同的对象。
但是，如果您备份 Active Directory，并保留有权管理系统的 JEA 用户的列表以及由管理员信任的证明用于授权受保护主机的任何安全组的成员身份，则会使恢复更容易。

若要标识为 HGS 配置的 SSL 证书的指纹，请在 PowerShell 中运行以下命令。
然后，你可以根据证书提供商的说明备份这些 SSL 证书。

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>从备份还原 HGS
以下步骤介绍了如何从备份还原 HGS 设置。
这些步骤与以下两种情况有关：您正在尝试撤消对已运行的 HGS 实例所做的更改，并且在您上一次群集完全丢失后，当您成为新的 HGS 群集时。

#### <a name="set-up-a-replacement-hgs-cluster"></a>设置替换 HGS 群集
在还原 HGS 之前，你需要有一个已初始化的 HGS 群集，你可以将配置还原到该群集。
如果只是导入意外删除到现有（正在运行的）群集的设置，则可以跳过此步骤。
如果要从完全丢失的 HGS 恢复，则需按照[部署指南中的指南](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)安装并初始化至少一个 hgs 节点。

具体而言，你将需要：
1. [设置 hgs 域或将](guarded-fabric-choose-where-to-install-hgs.md)hgs 加入现有域
2. 使用现有密钥*或*一组临时密钥[初始化 HGS 服务器](guarded-fabric-initialize-hgs.md)。 从 HGS 备份文件导入实际密钥后，可以[删除临时密钥](#renewing-or-replacing-keys)。
3. 从备份[导入 HGS 设置](#import-settings-from-a-backup)以还原受信任的主机组、代码完整性策略、tpm 基线和 tpm 标识符

> [!TIP]
> 新的 HGS 群集不需要使用与从中导出备份文件的 HGS 实例相同的证书、服务名称或域。

#### <a name="import-settings-from-a-backup"></a>从备份导入设置

若要从备份文件将证明策略、基于 PFX 的证书和非 PFX 证书的公钥还原到 HGS 节点，请在初始化的 HGS 服务器节点上运行以下命令。
系统将提示你输入创建备份时指定的密码。

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

如果只想导入受管理的受信任证明策略或受 TPM 信任的证明策略，则可以通过将 @no__t 0 或 @no__t 标志指定为[HgsServerState](https://technet.microsoft.com/library/mt652168.aspx)来实现此目的。

确保已安装最新的 Windows Server 2016 累积更新，然后再运行 `Import-HgsServerState`。
否则，可能会导致导入错误。

> [!NOTE]
> 如果在已安装了一个或多个策略的现有 HGS 节点上还原策略，则导入命令将为每个重复策略显示错误。
> 这是预期的行为，在大多数情况下可以放心地忽略。

#### <a name="reinstall-private-keys-for-certificates"></a>重新安装证书的私钥
如果使用指纹添加了用于创建备份的 HGS 上的任何证书，则备份文件中仅包含这些证书的公钥。
这意味着，你将需要为这些证书中的每个证书手动安装和/或授予访问权限，然后才能通过 Hyper-v 主机处理请求。
完成该步骤所需的操作因证书最初颁发方式的不同而异。
对于证书颁发机构颁发的软件支持的证书，你将需要联系你的 CA 以获取私钥，并按照它们的说明在**每个**HGS 节点上安装它。
同样，如果你的证书支持硬件，你将需要咨询你的硬件安全模块供应商的文档，以在每个 HGS 节点上安装所需的驱动程序以连接到 HSM，并向每台计算机授予对私钥的访问权限。

提醒使用指纹添加到 HGS 的证书需要手动将私钥复制到每个节点。
你将需要在添加到已还原的 HGS 群集的每个附加节点上重复此步骤。

#### <a name="review-imported-attestation-policies"></a>查看导入的证明策略
从备份导入设置后，建议你仔细检查所有使用 `Get-HgsAttestationPolicy` 导入的策略，以确保只有你信任的用于运行受防护的 Vm 的主机才能成功证明。
如果找到不再符合安全状况的任何策略，则可以[禁用或删除它们](#review-attestation-policies)。

#### <a name="run-diagnostics-to-check-system-state"></a>运行诊断以检查系统状态
完成设置并还原 HGS 节点的状态之后，您应该运行 HGS 诊断工具来检查系统的状态。
为此，请在你还原了配置的 HGS 节点上运行以下命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

如果 "总体结果" 未 "通过"，则需要执行其他步骤才能完成系统的配置。
检查 subtest 中报告的有关详细信息的消息。

## <a name="patching-hgs"></a>修补 HGS
务必要使主机保护者服务节点保持最新状态，方法是在最新的累积更新推出时进行安装。如果要设置全新的 HGS 节点，强烈建议您在安装 HGS 角色之前安装任何可用的更新，或者对其进行配置。
这将确保任何新功能或更改的功能将立即生效。

修补受保护的构造时，强烈建议先升级*所有*hyper-v 主机，**然后再升级 HGS**。
这是为了确保在 Hyper-v 主机经过更新*后*对 HGS 上的证明策略进行的任何更改，以提供它们所需的信息。
如果更新将更改策略的行为，则不会自动启用这些策略，以免中断构造。
此类更新要求你遵循以下部分中的指导来激活新的或更改的证明策略。
建议你阅读 Windows Server 的发行说明和你安装的所有累积更新，以检查是否需要策略更新。

### <a name="updates-requiring-policy-activation"></a>需要策略激活的更新
如果 HGS 的更新引入或明显更改了证明策略的行为，则需要执行额外的步骤来激活已更改的策略。
仅在导出和导入 HGS 状态之后才会进行策略更改。
仅应在对环境中的所有主机和所有 HGS 节点应用累积更新之后，才能激活新的或更改的策略。
每台计算机都进行了更新后，在任何 HGS 节点上运行以下命令以触发升级过程：

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

如果引入了新策略，则默认情况下会将其禁用。
若要启用新策略，请先在 Microsoft 策略列表中找到它（以 "HGS_" 为前缀），然后使用以下命令启用它：

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>管理证明策略
HGS 维护多个证明策略，这些策略定义主机在被视为 "正常" 并允许运行受防护的 Vm 时必须满足的最低要求。
其中的某些策略由 Microsoft 定义，而其他策略则由你添加，以定义你的环境中允许的代码完整性策略、TPM 基线和主机。
需要定期维护这些策略，以确保在更新和替换主机时主机可以继续证明，并确保阻止不受信任的主机或配置成功证明。

对于管理员信任的证明，只有一个策略可确定主机是否正常：已知的受信任安全组中的成员身份。
TPM 证明更为复杂，它涉及各种策略来度量系统的代码和配置，然后确定其是否正常。

一次可以同时为一个 HGS 配置 Active Directory 和 TPM 策略，但该服务将仅检查策略是否为当前模式，当主机尝试证明时，它将针对该模式进行配置。
若要检查 HGS 服务器的模式，请运行 `Get-HgsServer`。

### <a name="default-policies"></a>默认策略
对于受 TPM 信任的证明，在 HGS 上配置了多个内置策略。
其中部分策略为 "已锁定"--表示出于安全原因，不能禁用它们。
下表说明了每个默认策略的用途。

策略名称                    | 用途
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | 要求主机启用安全启动。 这对于度量启动二进制文件和其他 UEFI 锁定的设置是必需的。
Hgs_UefiDebugDisabled          | 确保主机未启用内核调试器。 已阻止用户模式调试器和代码完整性策略。
Hgs_SecureBootSettings         | 否定策略，以确保主机至少匹配一个（管理员定义的） TPM 基线。
Hgs_CiPolicy                   | 消极策略，以确保主机使用一个管理员定义的 CI 策略。
Hgs_HypervisorEnforcedCiPolicy | 要求虚拟机监控程序强制实施代码完整性策略。 如果禁用此策略，则会受损针对内核模式代码完整性策略的攻击进行保护。
Hgs_FullBoot                   | 确保主机未从睡眠或休眠状态中恢复。 必须正确地重新启动主机或关闭主机才能传递此策略。
Hgs_VsmIdkPresent              | 需要在主机上运行基于虚拟化的安全性。 IDK 表示加密发送回主机安全内存空间的信息所需的密钥。
Hgs_PageFileEncryptionEnabled  | 要求在主机上加密页面页面。 如果检查了租户机密的未加密页面文件，禁用此策略可能会导致信息泄露。
Hgs_BitLockerEnabled           | 需要在 Hyper-v 主机上启用 BitLocker。 出于性能原因，默认情况下禁用此策略，不建议启用此策略。 此策略与受防护的 Vm 本身的加密无关。
Hgs_IommuEnabled               | 要求主机使用 IOMMU 设备来防止直接内存访问攻击。 禁用此策略并使用未启用 IOMMU 的主机可能会公开租户 VM 机密，以直接进行内存攻击。
Hgs_NoHibernation              | 需要在 Hyper-v 主机上禁用休眠。 禁用此策略可能允许主机将受防护的 VM 内存保存到未加密的休眠文件。
Hgs_NoDumps                    | 需要在 Hyper-v 主机上禁用内存转储。 如果禁用此策略，则建议你配置转储加密以防止将受防护的 VM 内存保存到未加密的故障转储文件。
Hgs_DumpEncryption             | 如果在 Hyper-v 主机上启用了内存转储，则需要使用 HGS 信任的加密密钥对其进行加密。 如果主机上未启用转储，则不会应用此策略。 如果同时禁用了此策略和*Hgs @ no__t-1NoDumps* ，则可将受防护的 VM 内存保存到未加密的转储文件。
Hgs_DumpEncryptionKey          | 否定策略：确保配置为允许内存转储的主机使用的是管理员定义的转储文件加密密钥。 禁用了*Hgs @ no__t-1DumpEncryption*时，不会应用此策略。

### <a name="authorizing-new-guarded-hosts"></a>授权新的受保护主机
若要授权新主机成为受保护的主机（例如证明成功），HGS 必须信任该主机，并（配置为使用受 TPM 信任的证明）在该主机上运行的软件。
授权新主机的步骤会有所不同，这取决于当前配置了 HGS 的证明模式。
若要查看受保护的构造的证明模式，请在任何 HGS 节点上运行 `Get-HgsServer`。

#### <a name="software-configuration"></a>软件配置
在新的 Hyper-v 主机上，确保已安装 Windows Server 2016 Datacenter edition。
Windows Server 2016 Standard 无法在受保护的构造中运行受防护的 Vm。
主机可能已安装桌面体验或服务器核心。

在具有桌面体验和服务器核心的服务器上，需要安装 Hyper-v 和主机保护者 Hyper-v 支持服务器角色：

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>管理员信任的证明
若要在使用管理员信任的证明时在 HGS 中注册新主机，必须首先将该主机添加到它所加入的域中的安全组。
通常，每个域都将有一个安全组用于受保护的主机。
如果已使用 HGS 注册该组，则需要执行的唯一操作是重新启动主机以刷新其组成员身份。

可以通过运行以下命令来检查 HGS 信任哪些安全组：

```powershell
Get-HgsAttestationHostGroup
```

若要向 HGS 注册新的安全组，请首先在主机的域中捕获该组的安全标识符（SID），并向 HGS 注册 SID。

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

部署指南中提供了有关如何在主机域和 HGS 之间设置信任关系的说明。

#### <a name="tpm-trusted-attestation"></a>受 TPM 信任的证明
如果在 TPM 模式下配置了 HGS，主机必须通过 "Hgs_" 前缀的所有锁定策略和 "enabled" 策略，以及至少一个 TPM 基线、TPM 标识符和代码完整性策略。
每次添加新主机时，都需要向 HGS 注册新的 TPM 标识符。
只要该主机运行相同的软件（并且应用了相同的代码完整性策略）和 TPM 基线作为环境中的另一台主机，就不需要添加新的 CI 策略或基线。

**为新主机添加 TPM 标识符**在新主机上运行以下命令来捕获 TPM 标识符。
请确保为主机指定一个唯一的名称，以帮助你在 HGS 上查找该主机。
如果停止主机，或想要阻止它在 HGS 中运行受防护的 Vm，将需要此信息。

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

将此文件复制到你的 HGS 服务器，并运行以下命令以向 HGS 注册该主机。

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**添加新的 TPM 基线**如果新主机正在为你的环境运行新的硬件或固件配置，则可能需要采用新的 TPM 基线。
为此，请在主机上运行以下命令。

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> 如果收到一条错误消息，指出主机未通过验证，并且不能成功证明，请不要担心。
> 这是一项先决条件检查，确保你的主机可以运行受防护的 Vm，并且可能意味着你尚未应用代码完整性策略或其他所需的设置。
> 请阅读错误消息，进行任何建议的更改，然后重试。
> 或者，你可以通过将 `-SkipValidation` 标志添加到命令来随时跳过验证。

将 TPM 基线复制到 HGS 服务器，并将其注册到以下命令。
建议使用命名约定，以帮助你了解 Hyper-v 主机的此类的硬件和固件配置。

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**添加新的代码完整性策略**如果更改了在 Hyper-v 主机上运行的代码完整性策略，则需要先向 HGS 注册新策略，然后这些主机才能成功证明。
在用作环境中受信任 Hyper-v 计算机的主映像的引用主机上，使用 `New-CIPolicy` 命令捕获新的 CI 策略。
建议使用 Hyper-v 主机 CI 策略的**FilePublisher**级别和**哈希**回退。
应该首先在审核模式下创建 CI 策略，以确保一切按预期运行。
验证系统上的示例工作负荷后，可以强制实施该策略，并将强制版本复制到 HGS。
有关代码完整性策略配置选项的完整列表，请参阅[Device Guard 文档](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)。

```powershell
# Capture a new CI policy with the FilePublisher primary level and Hash fallback and enable user mode code integrity protections
New-CIPolicy -FilePath 'C:\temp\ws2016-hardware01-ci.xml' -Level FilePublisher -Fallback Hash -UserPEs

# Apply the CI policy to the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer

# Check the event log for any untrusted binaries and update the policy if necessary
# Consult the Device Guard documentation for more details

# Change the policy to be in enforced mode
Set-RuleOption -FilePath 'C:\temp\ws2016-hardare01-ci.xml' -Option 3 -Delete

# Apply the enforced CI policy on the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer
```

创建、测试和实施策略后，将二进制文件（p7b）复制到 HGS 服务器并注册策略。

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**添加内存转储加密密钥**

当禁用了*hgs @ no__t-1NoDumps*策略并启用了*hgs @ no__t-3DumpEncryption*策略后，受保护的主机将能够具有要启用的内存转储（包括故障转储），前提是这些转储已加密。 受保护的主机只有在禁用了内存转储或使用 HGS 知道的密钥对其进行加密的情况才会通过证明。 默认情况下，不在 HGS 上配置转储加密密钥。

若要将转储加密密钥添加到 HGS，请使用 @no__t cmdlet 向 HGS 提供转储加密密钥的哈希。
如果在配置了转储加密的 Hyper-v 主机上捕获 TPM 基线，则哈希将包含在 tcglog 中，并可提供给 `Add-HgsAttestationDumpPolicy` cmdlet。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

或者，可以直接向 cmdlet 提供哈希的字符串表示形式。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

如果选择在受保护的构造中使用不同的密钥，请确保将每个唯一的转储加密密钥添加到 HGS。
使用 HGS 无法识别的密钥加密内存转储的主机将不会通过证明。

有关[在主机上配置转储加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)的详细信息，请参阅 hyper-v 文档。

#### <a name="check-if-the-system-passed-attestation"></a>检查系统是否通过了证明
向 HGS 注册必要的信息后，应检查主机是否通过证明。
在新添加的 Hyper-v 主机上，运行 `Set-HgsClientConfiguration` 并为你的 HGS 群集提供正确的 Url。
可以通过在任何 HGS 节点上运行 `Get-HgsServer` 来获取这些 Url。

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

如果生成的状态不是 "IsHostGuarded：True "需要对配置进行故障排除。
在证明失败的主机上，运行以下命令以获取有关可能有助于解决失败的证明的问题的详细报告。

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> 如果使用的是 Windows Server 2019 或 Windows 10，版本1809，并且使用的是代码完整性策略，则 `Get-HgsTrace` 可能会为**代码完整性策略活动**诊断返回失败。
> 如果这是唯一失败的诊断，则可以安全地忽略此结果。

### <a name="review-attestation-policies"></a>查看证明策略
若要查看在 HGS 上配置的策略的当前状态，请在任何 HGS 节点上运行以下命令：

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

如果发现已启用的策略不再满足安全要求（例如，已被视为不安全的旧代码完整性策略），则可以通过在以下命令中替换策略名称来禁用它：

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

同样，你可以使用 `Enable-HgsAttestationPolicy` 来重新启用策略。

如果不再需要策略，并希望从所有 HGS 节点中删除该策略，请运行 `Remove-HgsAttestationPolicy -Name 'PolicyName'` 以永久删除该策略。

## <a name="changing-attestation-modes"></a>更改证明模式
如果你使用管理员信任的证明启动了受保护的构造，则你可能希望在你的环境中有足够的与 TPM 2.0 兼容的主机之后升级到更强大的 TPM 证明模式。
当你准备好进行切换时，你可以在 HGS 中预加载所有证明项目（CI 策略、TPM 基线和 TPM 标识符），同时继续运行具有管理员信任的证明的 HGS。
为此，只需遵循[授权新的受保护主机](#authorizing-new-guarded-hosts)部分中的说明。

将所有策略添加到 HGS 后，下一步就是在主机上运行综合证明尝试，以查看它们是否会在 TPM 模式下传递证明。
这不会影响 HGS 的当前操作状态。
下面的命令必须在对环境中的所有主机具有访问权限的计算机上运行，并且必须在至少一个 HGS 节点上运行。
如果你的防火墙或其他安全策略阻止了此操作，则可以跳过此步骤。
如果可能，我们建议运行综合证明，以提供有关 "翻转" 到 TPM 模式是否会导致 Vm 停机的良好指示。 

```powershell
# Get information for each host in your environment
$hostNames = 'host01.contoso.com', 'host02.contoso.com', 'host03.contoso.com'
$credential = Get-Credential -Message 'Enter a credential with admin privileges on each host'
$targets = @()
$hostNames | ForEach-Object { $targets += New-HgsTraceTarget -Credential $credential -Role GuardedHost -HostName $_ }

$hgsCredential = Get-Credential -Message 'Enter an admin credential for HGS'
$targets += New-HgsTraceTarget -Credential $hgsCredential -Role HostGuardianService -HostName 'HGS01.bastion.local'

# Initiate the synthetic attestation attempt
Get-HgsTrace -RunDiagnostics -Target $targets -Diagnostic GuardedFabricTpmMode 
```

诊断完成后，请查看输出的信息，以确定是否有任何主机在 TPM 模式下出现失败的证明。
重新运行诊断，直到从每个主机获得 "pass"，然后继续将 HGS 更改为 TPM 模式。

**更改为 TPM 模式**只需一秒钟即可完成。
在任何 HGS 节点上运行以下命令以更新证明模式。

```powershell
Set-HgsServer -TrustTpm
```

如果遇到问题，需要切换回 Active Directory 模式，则可以通过运行 @no__t 来执行此操作。

确认所有内容均按预期工作后，应从 HGS 中删除所有受信任的 Active Directory 主机组，并删除 HGS 与 fabric 域之间的信任。
如果你保留 Active Directory 信任，则会有用户重新启用信任并将 HGS 切换为 Active Directory 模式的风险，这可能会允许不受信任的代码在受保护的主机上以未选中状态运行。

## <a name="key-management"></a>密钥管理
受保护的构造解决方案使用多个公钥/私钥对来验证解决方案中各种组件的完整性，并对租户机密进行加密。
使用至少两个证书（使用公钥和私钥）配置了主机保护者服务，这些证书用于对用于启动受防护的 Vm 的密钥进行签名和加密。
必须谨慎管理这些密钥。
如果某个攻击者获取了私钥，他们将能够 unshield 在你的构造上运行的任何 Vm，或设置一个使用较弱的证明策略的冒名顶替者 HGS 群集，绕过你放置的保护。
如果在发生灾难时丢失私钥，而在备份中找不到私钥，则需要设置一对新的密钥，并让每个 VM 进行重新注册以授权新证书。

本部分介绍了一些常规的关键管理主题，这些主题可帮助你配置密钥，使其正常运行。

### <a name="adding-new-keys"></a>添加新密钥
尽管必须用一组密钥初始化 HGS，但你可以将多个加密和签名密钥添加到 HGS。
向 HGS 添加新密钥的两个最常见原因是：
1. 为了支持 "自带密钥" 方案，租户会将私钥复制到您的硬件安全模块，并且仅授权其密钥启动其受防护的 Vm。
2. 若要替换 HGS 的现有密钥，请首先添加新密钥并保留两组密钥，直到每个 VM 配置都已更新为使用新密钥。

根据所使用的证书类型，添加新密钥的过程会有所不同。

**选项 1：添加存储在 HSM @ no__t 中的证书

建议使用硬件安全模块（HSM）中创建的证书来保护 HGS 密钥。
Hsm 确保密钥的使用与数据中心内对安全敏感设备的物理访问相关联。
每个 HSM 都是不同的，并且具有创建证书并将其注册到 HGS 的唯一过程。
下面的步骤旨在为使用 HSM 的证书提供大致指导。
请参阅 HSM 供应商文档，了解确切的步骤和功能。

1. 在群集中的每个 HGS 节点上安装 HSM 软件。 根据你是使用网络还是本地 HSM 设备，你可能需要配置 HSM 以授予计算机对其密钥存储的访问权限。
2. 在 HSM 中创建2个证书，其中包含2048位用于加密和签名的**RSA 密钥**
    1. 使用 HSM 中的**数据加密**密钥用法属性创建加密证书
    2. 使用 HSM 中的 "**数字签名**密钥用法" 属性创建签名证书
3. 按照 HSM 供应商的指南，在每个 HGS 节点的本地证书存储中安装证书。
4. 如果 HSM 使用粒度权限向特定应用程序或用户授予使用私钥的权限，则需要向 HGS 组托管服务帐户授予对证书的访问权限。 可以通过运行 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName` 来查找 HGS gMSA 帐户的名称。
5. 在以下命令中，将指纹替换为证书的指纹，以将签名和加密证书添加到 HGS：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**选项 2：添加不可导出的软件证书 @ no__t-0

如果你的公司或公共证书颁发机构颁发的软件支持的证书具有不可导出的私钥，则需要使用证书的指纹将证书添加到 HGS。
1. 根据证书颁发机构的说明在计算机上安装证书。
2. 向 HGS 组托管服务帐户授予对证书私钥的读取访问权限。 可以通过运行 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName` 来查找 HGS gMSA 帐户的名称。
3. 使用以下命令将证书注册到 HGS，并使用证书的指纹（更改*加密*为*签名*证书签名）：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> 你将需要手动安装私钥，并授予对每个 HGS 节点上的 gMSA 帐户的读取访问权限。
> HGS 无法自动复制其指纹注册的*任何*证书的私钥。

**Option 3：添加存储在 PFX 文件中的证书 @ no__t

如果你有一个软件支持的证书，其中包含可使用 PFX 文件格式存储并使用密码保护的可导出私钥，则 HGS 可以自动管理你的证书。
使用 PFX 文件添加的证书会自动复制到 HGS 群集的每个节点，并且 HGS 保护对私钥的访问。
若要使用 PFX 文件添加新证书，请在任何 HGS 节点上运行以下命令 *（将 "* 更改*加密*为签名证书"）：

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**标识和更改主证书**尽管 HGS 可以支持多个签名证书和加密证书，但它使用一个对作为其 "主要" 证书。
如果有人下载该 HGS 群集的监护人元数据，则将使用这些证书。
若要检查当前标记为主要证书的证书，请运行以下命令：

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

若要设置新的主加密或签名证书，请使用以下命令查找所需证书的指纹，并将其标记为主证书：

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>续订或替换密钥
当你创建 HGS 使用的证书时，将根据你的证书颁发机构的策略和你的请求信息为这些证书分配一个过期日期。
通常，在证书有效性非常重要的情况下（例如保护 HTTP 通信），必须在证书过期之前续订证书，以避免服务中断或令人不安错误消息。
HGS 并不使用证书。
HGS 只是使用证书来创建和存储非对称密钥对。
HGS 上的过期加密或签名证书不表示受防护的 Vm 的保护不受保护。
此外，HGS 不会执行证书吊销检查。
如果吊销了 HGS 证书或颁发机构的证书，则不会影响 HGS 证书的使用。

如果有理由相信自己的私钥已被盗，只需担心 HGS 证书。
在这种情况下，受防护的 Vm 的完整性存在风险，因为在这种情况下，HGS 加密和签名密钥对的所有权足以删除 VM 上的防护保护，或者创建具有较弱证明策略的虚设 HGS 服务器。

如果你发现自己在这种情况下，或需要遵守标准来定期刷新证书密钥，则以下步骤概述了在 HGS 服务器上更改密钥的过程。
请注意，以下指南表示一项重要的任务，这些任务将导致为 HGS 群集提供的每个 VM 服务中断。
若要最大程度地减少服务中断和确保租户 Vm 的安全性，需要对更改 HGS 密钥进行适当规划。

在 HGS 节点上，执行以下步骤以注册一对加密证书和签名证书。
若要详细了解如何添加新密钥，请参阅添加[新密钥](#adding-new-keys)部分。
1. 为 HGS 服务器创建一对新的加密证书和签名证书。 理想情况下，将在硬件安全模块中创建这些。
2. 向**HgsKeyProtectionCertificate**注册新的加密证书和签名证书

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. 如果使用了指纹，则需要在群集中的每个节点上安装私钥，并授予 HGS gMSA 对密钥的访问权限。
4. 使新证书成为 HGS 中的默认证书

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

此时，使用从 HGS 节点获取的元数据创建的屏蔽数据将使用新证书，但现有 Vm 将继续工作，因为旧证书仍在那里。
若要确保所有现有 Vm 都可以使用新密钥，需要更新每个 VM 上的密钥保护程序。
此操作要求涉及 VM 所有者（拥有 "所有者" 监护人的个人或实体）。
对于每个受防护的 VM，请执行以下步骤：
5. 关闭 VM。 在剩余步骤完成之后，无法重新打开 VM，否则你将需要再次开始该过程。
6. 将当前密钥保护程序保存到文件中： `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. 将 KP 传输到 VM 所有者
8. 让所有者从 HGS 下载更新的保护者信息，并将其导入本地系统
9. 通过运行以下命令，将当前的 KP 读入内存，向新的监护人授予对 KP 的访问权限，并将其保存到新文件中：

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. 将更新的 KP 复制回托管结构
11. 将 KP 应用于原始 VM：

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. 最后，启动 VM 并确保其成功运行。

> [!NOTE]
> 如果 VM 所有者在 VM 上设置了不正确的密钥保护程序，并且未授权构造运行 VM，则无法启动受防护的 VM。
> 若要返回到上一个已知良好的密钥保护程序，请运行 `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

所有 Vm 都已更新为授权新的监护人密钥后，你可以禁用并删除旧密钥。

13. 从 @no__t 获取旧证书的指纹

14. 通过运行以下命令来禁用每个证书：  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. 在确保 Vm 仍可在禁用证书的情况下启动后，请运行以下命令，从 HGS 删除证书：

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> VM 备份将包含旧的密钥保护程序信息，这些信息允许使用旧证书启动 VM。
> 如果你知道私钥已泄露，则应假设 VM 备份也可以泄露，并采取适当的措施。
> 销毁备份（. .vmcx）中的 VM 配置将删除密钥保护程序，但代价是需要使用 BitLocker 恢复密码才能在下一次启动 VM。

### <a name="key-replication-between-nodes"></a>节点之间的密钥复制
HGS 群集中的每个节点都必须配置为具有相同的加密、签名和（配置时） SSL 证书。
这是为了确保与群集中的任何节点联系的 Hyper-v 主机可以成功地处理其请求。

**如果通过基于 PFX 的证书初始化了 hgs 服务器**，则 hgs 会自动在群集中的每个节点之间复制这些证书的公钥和私钥。
只需在一个节点上添加密钥。

**如果用证书引用或指纹初始化了 hgs 服务器**，则 hgs 只会将证书中的*公钥*复制到每个节点。
此外，在这种情况下，HGS 无法在任何节点上自行授予对私钥的访问权限。
因此，您需要负责执行以下操作：
1. 在每个 HGS 节点上安装私钥
2. 向 HGS 组托管服务帐户（gMSA）授予对每个节点上的私钥的访问权限，这些任务增加了额外的操作负担，不过，它们对于支持 HSM 的密钥和包含不可导出的私钥的证书是必需的。

**SSL 证书**永远不会以任何形式复制。
您需要在每次选择续订或替换 SSL 证书时，用同一 SSL 证书初始化每个 HGS 服务器并更新每个服务器。
替换 SSL 证书时，建议使用[HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet 来执行此操作。

## <a name="unconfiguring-hgs"></a>取消对 HGS 的

如果需要停止或重新配置 HGS 服务器，可以使用[HgsServer](https://technet.microsoft.com/library/mt652176.aspx)或[HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet 执行此操作。

### <a name="clearing-the-hgs-configuration"></a>清除 HGS 配置

若要从 HGS 群集中删除节点，请使用[HgsServer](https://technet.microsoft.com/library/mt652176.aspx) cmdlet。
此 cmdlet 将在运行它的服务器上进行以下更改：

- 注销证明和密钥保护服务
- 删除 "JEA" 管理终结点
- 从 HGS 故障转移群集中删除本地计算机

如果该服务器是群集中的最后一个 HGS 节点，则群集及其相应的分布式网络名称资源也将被销毁。

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

清除操作完成后，可以将 HGS 服务器重新初始化为[HgsServer](https://technet.microsoft.com/library/mt652185.aspx)。
如果使用[HgsServer](https://technet.microsoft.com/library/mt652169.aspx)来设置 Active Directory 域服务域，则在清除操作后，该域将保持配置并可操作。

### <a name="uninstalling-hgs"></a>卸载 HGS

如果要从 HGS 群集中删除节点**并**将其上运行的 Active Directory 域控制器降级，请使用[HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet。
此 cmdlet 将在运行它的服务器上进行以下更改：

- 注销证明和密钥保护服务
- 删除 "JEA" 管理终结点
- 从 HGS 故障转移群集中删除本地计算机
- 将 Active Directory 域控制器降级（如果已配置）

如果该服务器是群集中的最后一个 HGS 节点，则域、故障转移群集和群集的分布式网络名称资源也将被销毁。

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

完成卸载操作并重新启动计算机后，可以使用[HgsServer](https://technet.microsoft.com/library/mt652169.aspx)重新安装 ADDC 和 HGS，或将计算机加入域，并使用[HgsServer](https://technet.microsoft.com/library/mt652185.aspx)初始化该域中的 HGS 服务器。

如果不再想要使用该计算机作为 HGS 节点，可以从 Windows 中删除该角色。

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
