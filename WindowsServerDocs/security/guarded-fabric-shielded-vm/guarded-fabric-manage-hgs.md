---
title: 管理主机保护者服务
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: dab27e71e42970507f321271edda90f6d161c691
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447389"
---
# <a name="managing-the-host-guardian-service"></a>管理主机保护者服务

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

主机保护者服务 (HGS) 是受保护的构造解决方案的核心。
它负责确保托管商或企业已知构造中的 HYPER-V 主机和运行受信任的软件以及用于管理用于启动受防护的 Vm 的密钥。
当租户决定信任托管其受防护的 Vm 时，它们放置他们配置和管理的主机保护者服务中的信任。
因此，它是构造的非常重要，在管理主机保护者服务，以确保安全、 可用性和可靠性的在受保护时应遵循的最佳做法。
以下各节中的指南解决最常见的操作问题面向的 HGS 管理员。

## <a name="limiting-admin-access-to-hgs"></a>限制对 HGS 管理员访问权限
由于 HGS 的安全敏感特性，而是组织的确保其管理员是组织的你的高度受信任的成员和，理想情况下，分开的构造资源管理员来说非常重要。
此外，建议您只能从使用安全通信协议，如通过 HTTPS 的 WinRM 的安全工作站管理 HGS。

### <a name="separation-of-duties"></a>职责分离
在设置 HGS，你可以只是为了 HGS，或将 HGS 加入到现有的受信任的域创建一个独立的 Active Directory 林的选项。
此决策，以及在组织中分配管理员角色为 HGS 确定信任边界。
无论谁有权访问 HGS，以管理员身份直接或间接作为管理员的其他内容 (例如 Active Directory) 可能会影响 HGS，是否可以控制在受保护的构造。
HGS 管理员选择的 HYPER-V 主机有权运行受防护的 Vm 和管理启动受防护的 Vm 所需的证书。
攻击者或恶意管理员有权访问 HGS 可以使用这种能力授权遭到入侵的主机运行受防护的 Vm，请通过删除密钥材料，和的详细信息来启动拒绝服务攻击。

若要避免此风险，它是*强*建议限制的应用 （包括 HGS 所加入的域） 的 HGS 管理员之间的重叠和 HYPER-V 环境。
通过确保没有一个管理员有权访问这两个系统，攻击者需要破坏 2 个人完成其任务关键型更改 HGS 策略 2 个不同的帐户。
这也意味着，对于两个 Active Directory 环境的域和企业管理员不应为同一个人，也不应 HGS 作为 HYPER-V 主机中使用相同的 Active Directory 林。
可以向自己授予更多资源的访问权限的任何人会带来安全风险。

### <a name="using-just-enough-administration"></a>使用 Just Enough Administration
附带了 HGS [Just Enough Administration](https://aka.ms/JEAdocs)旨在帮助你更安全地对其进行管理 (JEA) 角色。
JEA 可通过允许你委派给非管理员用户，这意味着管理 HGS 策略的人员不需要实际为整个计算机或域管理员的管理任务。
JEA 通过限制哪些用户可以在 PowerShell 会话中运行的命令并使用临时本地帐户 （特定于每个用户会话） 在后台运行通常需要在提升的命令的工作原理。

HGS 附带了预配置的 2 个 JEA 角色：
- **HGS 管理员**它们允许用户以管理所有 HGS 策略，其中包括授权新主机运行受防护的 Vm。
- **HGS 审阅者**这只允许用户审核现有策略的权限。 它们不能向 HGS 配置进行任何更改。

若要使用 JEA，首先需要创建新的标准用户并使其 HGS 管理员或 HGS 审阅者组的成员。
如果您使用了`Install-HgsServer`若要为 HGS 设置新的林，这些组将被命名为"*servicename*管理员"和"*servicename*审阅者"分别，其中*servicename*是 HGS 群集的网络名称。
如果 HGS 加入现有域，则应参考中指定的组名称`Initialize-HgsServer`。

**创建标准用户的 HGS 管理员和审阅者角色**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**与审阅者角色的审核策略**

在远程计算机上具有网络连接到 HGS，PowerShell 输入与审阅者凭据的 JEA 会话中运行以下命令。
请务必注意，因为审阅者帐户是标准用户，它不能用于常规 Windows PowerShell 远程处理，远程桌面访问 HGS，等等。

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

然后，可以选中在会话中允许的命令`Get-Command`并运行任何允许的命令要审核的配置。
在以下示例中，我们会检查 HGS 上启用了哪些策略。

```powershell
Get-Command

Get-HgsAttestationPolicy
```

键入命令`Exit-PSSession`或其别名`exit`，完成后使用 JEA 会话。 

**将新策略添加到 HGS 使用管理员角色**

若要实际更改的策略，需要连接到 JEA 终结点使用属于 hgsAdministrators 组的标识。
在以下示例中，我们演示如何将新的代码完整性策略复制到 HGS 和使用 JEA 注册它。
语法可能不同于习惯于。
这是为了支持在 JEA 中限制的一些像不有权访问完整的文件系统。

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
从 HGS 的事件将显示在 Windows 事件日志下 2 个源：
- **HostGuardianService-Attestation**
- **HostGuardianService-KeyProtection**

通过打开事件查看器并导航到 Microsoft Windows HostGuardianService 证明和 Microsoft Windows HostGuardianService KeyProtection，可以查看这些事件。

在大型环境中，通常最好是将事件转发到中心的 Windows 事件收集器的事件的分析更轻松。
有关详细信息，请查看[Windows 事件转发文档](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx)。

### <a name="using-system-center-operations-manager"></a>使用 System Center Operations Manager
您还可以使用 System Center 2016-Operations Manager 来监视 HGS 和受保护的主机。
受保护的构造管理包具有事件监视器，以检查可能会导致数据中心停机时间，包括主机未通过证明和 HGS 服务器报告的错误的常见配置错误。

若要开始，[安装和配置 SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager)并[下载的受保护的构造管理包](https://www.microsoft.com/download/details.aspx?id=52764)。
包含的管理包指南说明如何配置管理包，并了解其监视器的作用域。

## <a name="backing-up-and-restoring-hgs"></a>备份和还原 HGS
### <a name="disaster-recovery-planning"></a>灾难恢复规划
起草灾难恢复计划，时，一定要考虑在受保护的构造中的主机保护者服务的独特要求。
应会丢失某些或所有 HGS 节点，可能会遇到将阻止用户启动其受防护的 Vm 的即时可用性问题。
在中失去整个 HGS 群集方案中，需要具有的 HGS 配置手头以恢复 HGS 群集和恢复正常操作的完整备份。
本部分介绍准备此类方案所需的步骤。

首先，务必要了解必须备份的 HGS 呢。
HGS 保留几条信息，可帮助它确定哪些主机有权运行受防护的 Vm。
这包括：
1. Active Directory 安全标识符包含的组的受信任的主机 （在使用 Active Directory 证明）;
2. 在您的环境; 每台主机的唯一 TPM 标识符
3. 每个唯一配置的主机; TPM 策略和
4. 确定哪种软件可以在主机上运行的代码完整性策略。

这些认证项目需要您托管构造，若要获取，在与管理员协调可能因此很难获取此信息在发生灾难后。

此外，HGS 需要访问 2 个或多个证书用于加密和签名所需启动受防护的 VM （密钥保护程序） 的信息。
这些证书具有一些公认 （用于受防护的 Vm 的所有者授予你构造运行其虚拟机），且必须为用户提供无缝地恢复体验在发生灾难后还原。
您不要将恢复 HGS 使用相同的证书在发生灾难后，每个 VM 需要更新，以授权新密钥来解密其信息。
出于安全原因，只有 VM 所有者可以更新 VM 配置，以授权这些新的密钥、 含义还原你的密钥，灾难将导致无需采取的措施每个 VM 所有者以获取其虚拟机再次运行后失败。

#### <a name="preparing-for-the-worst"></a>为最差准备
若要准备 HGS 完全丢失，有 2 个必须采取的步骤：
1. 备份 HGS 证明策略
2. 备份 HGS 密钥

中提供了有关如何执行两个步骤指导[备份 HGS](#backing-up-hgs)部分。

此外建议，但不是要求您备份管理 HGS 其 Active Directory 域中的或 Active Directory 本身已授权用户的列表。

应定期执行备份，以确保最新的和存储安全地以避免被篡改或窃取信息。

它是**不建议这样做**备份或尝试还原整个系统的 HGS 节点映像。
以防您丢失整个群集，最佳做法是设置一个全新的 HGS 节点并还原 HGS 状态，而不是整个服务器操作系统。

#### <a name="recovering-from-the-loss-of-one-node"></a>从一个节点的丢失恢复
如果你丢失 HGS 群集中的一个或多个节点 （但不是每个节点），则可以只需[将节点添加到你的群集](guarded-fabric-configure-additional-hgs-nodes.md)遵循部署指南中的指导原则。
将会向 HGS 作为提供 PFX 文件并伴有密码的任何证书，证明策略将自动同步。
添加到 HGS 使用指纹的证书 (非可导出和硬件支持的证书，通常)，您将需要确保每个新节点有权访问每个证书的私钥。

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>整个群集断开后进行恢复
如果整个 HGS 群集出现故障时，无法将其重新联机，需要从备份中还原 HGS。
从备份中还原 HGS 涉及到第一个设置每个新的 HGS 群集[部署指南中的指南](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。
强烈建议，但不是需要，设置恢复 HGS 环境时使用相同的群集名称，以帮助从主机的名称解析。
使用相同的名称可避免无需重新配置主机与新的证明和密钥保护 Url。
如果你还原到备份 HGS 的 Active Directory 域的对象，建议删除才能初始化 HGS 服务器表示 HGS 群集、 计算机、 服务帐户和 JEA 组的对象。

设置你的第一个 HGS 节点后 （例如其已安装并已初始化），您需要遵循的过程[从备份还原 HGS](#restoring-hgs-from-a-backup)还原证明策略和密钥保护的公共部分证书。
将需要还原 certificate 提供程序的指南手动根据证书的私钥 （例如在 Windows 中，将证书导入或配置访问、 由 HSM 支持的证书）。
设置第一个节点后，可继续[安装到群集的其他节点](guarded-fabric-configure-additional-hgs-nodes.md)直到达到容量和所需的复原能力。

### <a name="backing-up-hgs"></a>备份 HGS
HGS 管理员应负责定期备份 HGS。
完整备份将包含敏感必须适当地保护的密钥材料。
不受信任的实体应获得对这些注册表项的访问权限，他们可以使用材料设置会影响用于恶意 HGS 环境受防护的 Vm。

**备份证明策略**若要备份的 HGS 证明策略，运行以下命令在任何工作 HGS 服务器节点上。
系统将提示您提供一个密码。
此密码用于加密添加到 HGS 使用 PFX 文件 （而不是证书指纹） 的任何证书。

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> 如果使用受信任的管理员证明，则必须单独备份 HGS 用于授权受保护的主机的安全组中的成员身份。
> HGS 将仅备份不在其中的成员身份的安全组的 SID。
> 以防这些组都将丢失在发生灾难期间，你将需要重新创建组，并再次添加到其每个受保护的主机。

**备份证书**

`Export-HgsServerState`命令会将任何基于 PFX 的证书添加到 HGS 时运行该命令。
如果将证书添加到 HGS 使用指纹 （一般用于非可导出和硬件支持证书），你将需要手动备份您的证书的私钥。
若要确定哪些证书向 HGS 注册和需要手动备份，在任何工作 HGS 服务器节点上运行以下 PowerShell 命令。

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

对于每个列出的证书，你将需要手动备份私钥。
如果使用基于软件的证书不可导出，应联系证书颁发机构，以确保它们具有证书的备份和/或可以重新发出它按需。
对于证书创建并存储在硬件安全模块，应该为你的设备进行灾难恢复规划指南参考的文档。

以便这两个部分可以一起还原，应将证书备份存储和证明策略备份在安全的位置。

**若要备份的其他配置**

已备份了 HGS 服务器状态将不包括 HGS 群集，从 Active Directory 的任何信息的名称或任何 SSL 证书用于保护与 HGS Api 之间的通信。
这些设置是重要的一致性，但不是严重，若要在发生灾难后获取 HGS 群集重新联机。

若要捕获的 HGS 服务的名称，请运行`Get-HgsServer`并记下证明和密钥保护 Url 中的平面名称。
例如，如果证明 URL 为"<http://hgs.contoso.com/Attestation>"，"hgs"是 HGS 服务名称。

使用 HGS 的 Active Directory 域应像任何其他 Active Directory 域进行管理。
当在发生灾难后还原 HGS，将不一定需要重新创建当前域中存在的准确对象。
但是，它会使恢复轻松如果备份 Active Directory 和保留权管理系统，以及受信任的管理员证明用于授权受保护的主机的任何安全组的成员身份的 JEA 用户的列表。

若要标识为 HGS 配置的 SSL 证书的指纹，请在 PowerShell 中运行以下命令。
然后，可以备份证书提供程序的说明根据这些 SSL 证书。

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>从备份中还原 HGS
以下步骤介绍如何从备份中还原 HGS 设置。
这些步骤都与这两种情况下尝试撤消您已在运行的 HGS 实例和要建立全新的 HGS 群集后的前一个完全丢失所做的更改。

#### <a name="set-up-a-replacement-hgs-cluster"></a>设置替换 HGS 群集
还原 HGS 之前，需要初始化的 HGS 群集可以向其还原配置。
如果只到现有群集 （运行） 中导入设置意外删除的则可以跳过此步骤。
如果要恢复的 HGS 完整数据丢失，则需要安装并初始化至少一个 HGS 节点下面[部署指南中的指南](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。

具体而言，您将需要：
1. [设置 HGS 域](guarded-fabric-choose-where-to-install-hgs.md)或加入现有域的 HGS
2. [初始化 HGS 服务器](guarded-fabric-initialize-hgs.md)使用你现有的密钥*或*临时密钥的一组。 你可以[中删除的临时密钥](#renewing-or-replacing-keys)后从 HGS 导入你的实际密钥备份文件。
3. [导入 HGS 设置](#import-settings-from-a-backup)从备份还原受信任的主机组、 代码完整性策略、 TPM 基线和 TPM 标识符

> [!TIP]
> 新的 HGS 群集不必使用相同的证书、 服务名称或域与 HGS 实例从其导出您的备份文件。

#### <a name="import-settings-from-a-backup"></a>从备份导入设置

若要还原证明策略、 基于 PFX 的证书和到 HGS 节点从备份文件的非 PFX 证书的公钥运行以下命令初始化的 HGS 服务器节点上。
系统将提示您输入在创建备份时指定的密码。

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

如果只想要导入受信任的管理员证明策略或受信任的 TPM 证明策略，就可以做到通过指定`-ImportActiveDirectoryModeState`或`-ImportTpmModeState`到标志[导入 HgsServerState](https://technet.microsoft.com/library/mt652168.aspx)。

确保最新的累积更新，Windows Server 2016 安装在运行前对`Import-HgsServerState`。
如果不这样做可能会导致导入错误。

> [!NOTE]
> 如果要还原已有一个或多个安装这些策略的现有 HGS 节点上的策略，导入命令将显示为每个重复的策略错误。
> 这是预期的行为，可以在大多数情况下安全地忽略。

#### <a name="reinstall-private-keys-for-certificates"></a>重新安装证书的私钥
如果任何上 HGS 创建备份时使用的证书添加使用指纹，仅那些证书的公钥将包含在备份文件中。
这意味着将需要手动安装和/或 HGS 可以处理来自 HYPER-V 主机的请求之前为每个这些证书授予访问权限的私钥。
完成该步骤所需的操作具体取决于最初颁发证书的方式各不相同。
提供软件支持颁发的证书由证书颁发机构，你将需要联系以获取专用密钥，然后将其安装在您的 CA**每个**HGS 节点根据其说明。
同样，如果证书不提供硬件支持，您需要请参考硬件安全模块供应商的文档，在每个 HGS 的节点连接到 HSM，并授予私有密钥对每个计算机上安装必要的驱动程序。

请注意，添加到 HGS 使用指纹的证书都需要手动复制到每个节点的私有密钥。
你将需要添加到已还原的 HGS 群集每个其他节点上重复此步骤。

#### <a name="review-imported-attestation-policies"></a>查看导入的证明策略
从备份导入你的设置后，我们建议仔细检查所有导入的策略使用`Get-HgsAttestationPolicy`以确保仅信任运行受防护的 Vm 的主机将能够成功证明。
如果发现不再与您的安全状况相匹配的任何策略，你可以[禁用或删除它们](#review-attestation-policies)。

#### <a name="run-diagnostics-to-check-system-state"></a>运行诊断来检查系统状态
完成设置以及还原 HGS 节点的状态后，应运行 HGS 诊断工具来检查系统的状态。
若要执行此操作，运行以下命令 HGS 节点上其中还原配置：

```powershell
Get-HgsTrace -RunDiagnostics
```

如果"整体结果"不"传递"，其他步骤所需完成的配置系统。
检查 subtest(s) 失败的详细信息中报告的消息。

## <a name="patching-hgs"></a>修补 HGS
请务必保留主机保护者服务节点通过安装最新的累积更新时它是最新。如果您设置了一个全新的 HGS 节点，强烈建议安装 HGS 角色在安装或对其进行配置之前所有可用更新。
这将确保任何新增或更改的功能将会立即生效。

时修补在受保护的构造，强烈建议你首先升级*所有*HYPER-V 主机**之前升级 HGS**。
这是为了确保进行任何更改上 HGS 的证明策略*后*的 HYPER-V 主机都已更新以提供其所需的信息。
如果更新要更改策略的行为，它们将不会自动启用以避免中断你的结构。
此类更新要求你按照以下部分，若要激活新的或更改证明策略中的指南。
我们建议您阅读 Windows Server 和安装，检查策略更新所需的任何累计更新的发行说明。

### <a name="updates-requiring-policy-activation"></a>更新需要策略激活
如果 HGS 的更新引入了或显著更改证明策略的行为时，需要一个额外的步骤来激活已更改的策略。
导出和导入 HGS 状态后仅实施策略更改。
已在环境中的累积更新应用于的所有主机以及 HGS 的所有节点后，仅应激活新的或更改策略。
一旦更新每台计算机，以触发升级过程的任何 HGS 节点上运行以下命令：

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

如果引入了新的策略，它将默认情况下被禁用。
若要启用新的策略，首先在 （前缀为 HGS_） 的 Microsoft 策略列表中找到它，然后启用它使用以下命令：

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>证明的管理策略
HGS 维护多个证明策略的定义的要求主机必须满足才能被视为"正常"和允许运行受防护的 Vm 的最小集。
其他一些这些策略由 Microsoft 定义的通过你在环境中定义允许代码完整性策略、 TPM 基线和主机进行添加。
这些策略的定期维护对于确保主机可以证明正确按继续更新和替换它们，并确保任何不受信任的主机或配置阻止成功证明所必需。

为受信任的管理员证明是只能有一个策略用于确定主机是否正常运行： 中的已知的受信任的安全组的成员身份。
TPM 证明更复杂一些，并且涉及各种策略来衡量的代码和配置系统之前确定是否处于正常状态。

单个 HGS 可以使用策略来配置 Active Directory 和 TPM，但该服务将仅检查当前模式时主机会尝试证明配置的策略。
若要检查的 HGS 服务器模式下，运行`Get-HgsServer`。

### <a name="default-policies"></a>默认策略
对于受信任的 TPM 证明有 HGS 上配置的多个内置策略。
这些策略的一些会"禁止"-这意味着它们不能禁用出于安全原因。
下表介绍了每个默认策略的用途。

策略名称                    | 用途
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | 需要启用安全启动的主机。 这是必要来测量的启动二进制文件和其他 UEFI 锁定设置。
Hgs_UefiDebugDisabled          | 确保主机不具有已启用内核调试程序。 使用代码完整性策略会阻止用户模式调试程序。
Hgs_SecureBootSettings         | 负的策略，以确保主机与匹配至少一个 （管理员定义） 的 TPM 基线。
Hgs_CiPolicy                   | 负的策略，以确保主机正在使用管理员定义 CI 策略之一。
Hgs_HypervisorEnforcedCiPolicy | 需要代码完整性策略的虚拟机监控程序强制执行。 禁用此策略会降低您针对内核模式代码完整性策略攻击的保护。
Hgs_FullBoot                   | 确保主机未从睡眠或休眠状态恢复。 主机必须正确地重新启动或关闭的情况下若要通过此策略。
Hgs_VsmIdkPresent              | 需要在主机上正在运行的基于虚拟化安全性。 IDK 表示发送回主机的安全的内存空间的信息进行加密所需的项。
Hgs_PageFileEncryptionEnabled  | 要求页面文件加密在主机上。 如果未加密的页面文件检查有关的租户密钥，则将禁用此策略可能导致信息公开。
Hgs_BitLockerEnabled           | 需要在 HYPER-V 主机上启用 BitLocker。 此策略已禁用默认情况下，出于性能原因，建议不要启用。 此策略在加密的受防护 Vm 本身上没有任何影响。
Hgs_IommuEnabled               | 要求主机具有 IOMMU 设备用来防止直接内存访问攻击。 启用 IOMMU 不禁用此策略和主机可以公开租户 VM 机密定向内存攻击。
Hgs_NoHibernation              | 需要休眠状态的 HYPER-V 主机上禁用。 禁用此策略可能允许主机以将受防护的 VM 内存保存到未加密的休眠文件。
Hgs_NoDumps                    | 要求禁用 HYPER-V 主机上的内存转储。 如果禁用此策略时，建议您配置转储加密以防止受防护的 VM 内存保存到未加密的故障转储文件。
Hgs_DumpEncryption             | 如果要加密的加密密钥受信任的 HGS 的 HYPER-V 主机上启用了，需要内存转储。 如果在主机上未启用转储，则此策略不适用于。 如果此策略并*Hgs\_NoDumps*则同时禁用受防护的虚拟机内存无法保存到未加密的转储文件。
Hgs_DumpEncryptionKey          | 负策略，以确保主机配置为允许内存转储使用已知的 HGS 管理员定义的转储文件加密密钥。 此策略不适用于何时*Hgs\_DumpEncryption*被禁用。

### <a name="authorizing-new-guarded-hosts"></a>新授权受保护的主机
若要授权新主机以成为受保护的主机 （例如证明成功），HGS 必须信任主机和 （如果已配置为使用受信任的 TPM 证明） 在其上运行的软件。
根据当前配置 HGS 的证明模式授权新主机的步骤有所不同。
若要检查受保护的构造的证明模式，请运行`Get-HgsServer`HGS 的任何节点上。

#### <a name="software-configuration"></a>软件配置
在新的 HYPER-V 主机上，请确保安装的 Windows Server 2016 Datacenter edition。
Windows Server 2016 Standard 无法在受保护的构造中运行受防护的 Vm。
主机名可以是已安装桌面体验或 Server Core。

在带桌面体验的服务器和服务器核心上，你需要安装 HYPER-V 和主机保护者 HYPER-V 支持服务器角色：

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>受信任的管理员证明
若要注册新的主机中 HGS 使用受信任的管理员证明时，必须首先在所加入的域中添加主机到安全组。
通常情况下，每个域都将为受保护的主机有一个安全组。
如果你已向 HGS 注册组，需要执行的唯一操作是重新启动主机后，若要刷新其组成员身份。

你可以检查哪些安全组受 HGS 通过运行以下命令：

```powershell
Get-HgsAttestationHostGroup
```

若要向 HGS 注册新的安全组，第一次捕获主机的域中的组的安全标识符 (SID)，并向 HGS 注册 SID。

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

部署指南中提供了有关如何设置主机域和 HGS 之间信任关系的说明。

#### <a name="tpm-trusted-attestation"></a>受信任的 TPM 证明
当 HGS 配置 TPM 模式下时，主机必须通过所有锁定的策略和"已启用"的策略，前缀为"Hgs_"以及至少一个 TPM 基线、 TPM 标识符和代码完整性策略。
每次添加新的主机，你将需要向 HGS 注册新的 TPM 标识符。
只要主机正在运行相同的软件 （和应用具有相同的代码完整性策略） 和作为您的环境中的另一台主机的 TPM 基线，您不需要添加新的 CI 策略或基线。

**为新的主机添加的 TPM 标识符**新主机上运行以下命令来捕获的 TPM 标识符。
请确保指定的主机，将帮助你查找上 HGS 的唯一名称。
如果取消配置主机或想要防止从 HGS 中正在运行受防护的 Vm，你将需要此信息。

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

将此文件复制到 HGS 服务器，然后运行以下命令以向 HGS 注册主机。

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**添加新的 TPM 基线**如果新主机正在运行新硬件或固件配置您的环境，你可能需要采用新的 TPM 基线。
若要执行此操作，在主机上运行以下命令。

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> 如果你收到一个错误，指示您的主机验证失败，会不成功证明这一点，不要担心。
> 这是先决条件检查以确保你的主机可以运行受防护的 Vm，并可能意味着，你具有尚未应用代码完整性策略或其他所需设置。
> 读取的错误消息，进行任何更改，建议通过它，然后重试。
> 或者，通过添加跳过这一次在验证`-SkipValidation`到命令的标志。

将 TPM 基线复制到你的 HGS 服务器，然后使用以下命令注册它。
我们建议使用可帮助你了解此类的 HYPER-V 主机的硬件和固件配置的命名约定。

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**添加新的代码完整性策略**已更改的 HYPER-V 主机上运行的代码完整性策略，将需要向 HGS 注册新的策略之前这些主机可以成功证明。
引用在主机上，它可用作你的环境中的受信任的 HYPER-V 计算机的主映像，捕获新的 CI 策略使用`New-CIPolicy`命令。
我们鼓励您使用**FilePublisher**级别和**哈希**回退的 HYPER-V 主机 CI 策略。
在审核模式下，若要确保一切按预期方式工作，应首先创建 CI 策略。
在验证系统上的示例工作负荷之后, 可以强制执行策略并将已强制实施的版本复制到 HGS。
代码完整性策略配置选项的完整列表，请查阅[Device Guard 文档](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)。

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

创建策略后，测试并强制执行，将二进制文件 (.p7b) 复制到 HGS 服务器，并注册策略。

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**添加内存转储加密密钥**

当*Hgs\_NoDumps*禁用策略和*Hgs\_DumpEncryption*启用策略，允许受保护的主机必须 （包括故障转储） 的内存转储已启用，只要这些转储进行加密。 受保护的主机将仅传递证明了禁用的内存转储或使用已知的 HGS 密钥加密它们。 默认情况下，在 HGS 配置没有转储加密密钥。

若要添加到 HGS 转储加密密钥，请使用`Add-HgsAttestationDumpPolicy`cmdlet 向 HGS 提供的转储加密密钥的哈希。
如果捕获转储加密的配置的 HYPER-V 主机上的 TPM 基线，哈希包含在 tcglog，可以提供给`Add-HgsAttestationDumpPolicy`cmdlet。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

或者，您可以直接提供给 cmdlet 的哈希值的字符串表示形式。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

请确保将每个唯一转储加密密钥添加到 HGS，如果您选择要在受保护的构造中使用不同的密钥。
正在使用不被透露给 HGS 密钥加密内存转储的主机未通过证明。

请参考有关的详细信息的 HYPER-V 文档[配置转储加密在主机上的](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。

#### <a name="check-if-the-system-passed-attestation"></a>检查系统是否通过证明
向 HGS 注册所需的信息之后，您应该检查主机是否通过证明。
在新添加的 HYPER-V 主机上运行`Set-HgsClientConfiguration`并提供 HGS 群集的正确 Url。
可以通过运行获取这些 Url `Get-HgsServer` HGS 的任何节点上。

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

如果生成的状态并不表示"IsHostGuarded:将需要排查配置问题，则 true"。
在主机上的证明失败，运行以下命令以获取详细信息可帮助您解决失败的证明的问题报告。

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> 如果您使用的 Windows Server 2019 或 Windows 10，版本 1809年并且正在使用代码完整性策略`Get-HgsTrace`可能返回的失败**代码完整性策略 Active**诊断。
> 仅故障诊断时，可以放心地忽略此结果。

### <a name="review-attestation-policies"></a>查看证明策略
若要查看上 HGS 配置的策略的当前状态，HGS 的任何节点上运行以下命令：

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

如果发现不再满足你的安全要求 （例如一个旧代码完整性策略现在被视为不安全） 的策略已启用，可以通过替换以下命令中的策略的名称来禁用它：

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

同样，可以使用`Enable-HgsAttestationPolicy`重新启用策略。

如果您不再需要策略并且想要删除从 HGS 的所有节点，运行`Remove-HgsAttestationPolicy -Name 'PolicyName'`要永久删除该策略。

## <a name="changing-attestation-modes"></a>不断变化的证明模式
如果你开始使用受信任的管理员证明您受保护的构造，则将可能想要升级到更强的 TPM 证明模式，只要你的环境中有足够 TPM 2.0 兼容的主机。
如果你已准备好切换，可以预加载所有中 HGS 的证明项目 （CI 策略、 TPM 基线和 TPM 标识符） 同时继续使用受信任的管理员证明运行 HGS。
若要执行此操作，只需按照中的说明[授权的新的受保护的主机](#authorizing-new-guarded-hosts)部分。

一旦所有策略添加到 HGS 下, 一步是针对您的主机来发现，是否它们会将证明传递 TPM 模式下运行的综合证明尝试。
这不会影响 HGS 的当前操作状态。
必须有权访问所有在环境和至少一个 HGS 节点中的主机计算机上运行以下命令。
如果你的防火墙或其他安全策略防止此情况，可以跳过此步骤。
如果可能，我们建议运行综合的证明，以便您清楚地表明是否"翻转"到 TPM 模式会导致停机时间为您的 Vm。 

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

在诊断完成后查看输出的信息来确定是否会有任何主机故障 TPM 模式下的证明。
重新运行诊断程序，直到"pass"获得每个主机，然后继续将 HGS 更改为 TPM 模式。

**更改为 TPM 模式**采用只需一秒才能完成。
若要更新的证明模式的任何 HGS 节点上运行以下命令。

```powershell
Set-HgsServer -TrustTpm
```

如果遇到问题并需要切换回 Active Directory 模式下时，就可以做到运行`Set-HgsServer -TrustActiveDirectory`。

一旦已确认一切按预期方式，应从 HGS 中删除所有受信任的 Active Directory 主机组，并删除 HGS 和 fabric 域之间的信任。
如果你离开的位置中的 Active Directory 信任，则可能会有人重新启用信任和切换 HGS 到 Active Directory 模式下，可能允许不受信任的代码在受保护的主机上运行未选中状态。

## <a name="key-management"></a>密钥管理
受保护的构造解决方案使用多个公钥/私钥对来验证解决方案中的各种组件的完整性和加密的租户密钥。
用于签名和加密用于启动受防护的 Vm 的密钥 （使用公共和私有密钥），至少两个证书配置主机保护者服务。
必须谨慎地管理这些密钥。
如果攻击者获取了私钥后，他们将能够 unshield 在结构上运行的任何 Vm 或将设置为使用较弱的证明策略以绕过实施措施的保护的冒名顶替者的 HGS 群集。
应在发生灾难期间丢失的私有密钥并不在备份中找到它们，你将需要设置新对密钥并在更新操作进行授权您的新证书的每个 VM。

本部分介绍了常规的密钥管理主题来帮助你配置你的密钥，因此它们正常运行和安全。

### <a name="adding-new-keys"></a>添加新的密钥
而必须使用一组密钥初始化 HGS，可以添加多个加密和签名密钥的 HGS。
为什么会将新的密钥添加到 HGS 的两个最常见原因如下：
1. 若要支持"自带密钥"情况下，租户将其私钥复制到你的硬件安全模块，并且仅授权以启动受防护的 Vm。
2. 若要替换现有密钥的 HGS 通过首次添加新的密钥并保留这两种密钥集之前每个 VM 配置已更新以使用新密钥。

根据您使用的证书的类型添加新密钥的过程有所不同。

**选项 1：添加存储在 HSM 中的证书**

我们建议的 HGS 密钥保护方法是使用在硬件安全模块 (HSM) 中创建的证书。
Hsm 可确保密钥的使用与到你的数据中心中的安全敏感设备的物理访问。
每个 HSM 是不同，具有一个唯一的过程来创建证书并向 HGS 注册它们。
下面的步骤用于提供有关使用 HSM 的粗略指南备份证书。
有关确切的步骤和功能，请查阅 HSM 供应商的文档。

1. 在群集中每个 HGS 节点上安装 HSM 软件。 根据所用的网络或本地 HSM 设备，可能需要配置该 HSM，以授予您对其密钥存储的计算机访问权限。
2. 在 HSM 中创建 2 个证书**2048 位 RSA 密钥**的加密和签名
    1. 创建带有的加密证书**数据加密**键在 HSM 中的使用情况属性
    2. 创建与签名证书**数字签名**键在 HSM 中的使用情况属性
3. 每个 HSM 供应商的指南的每个 HGS 节点的本地证书存储中安装的证书。
4. 如果 HSM 使用细化的权限授予特定的应用程序或用户使用的私钥的权限，需要授予你 HGS 组托管服务帐户访问权限的证书。 可以通过运行找到 HGS gMSA 帐户的名称 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. 将签名和加密证书添加到 HGS，通过将指纹与证书的以下命令中的替换为：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**选项 2：添加非可导出软件证书**

如果您有颁发你的公司的支持软件的证书或非可导出私钥的公共证书颁发机构，你将需要将证书添加到 HGS 使用其指纹。
1. 根据证书颁发机构的说明在计算机上安装证书。
2. HGS 组托管服务帐户授予读取-访问证书的私钥。 可以通过运行找到 HGS gMSA 帐户的名称 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. 证书注册 HGS 使用以下命令并替换证书的指纹 (更改*加密*到*签名*签名证书):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> 你将需要手动安装的私钥和授予给 HGS 的每个节点上的 gMSA 帐户的读取访问权限。
> HGS 不能自动复制用于私钥*任何*注册由其指纹的证书。

**选项 3:添加存储在 PFX 文件中的证书**

如果必须具有可导出的私钥可存储在 PFX 文件格式并使用密码保护的支持软件的证书，HGS 可以自动为你管理你的证书。
添加与 PFX 文件的证书会自动复制到 HGS 群集的每个节点，并 HGS 保护私有密钥的访问权限。
若要添加新证书，证书使用 PFX 文件，任何 HGS 节点上运行以下命令 (更改*加密*到*签名*签名证书):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**识别和更改主证书**虽然 HGS 可以支持多个签名和加密证书，它使用一对作为其"primary"的证书。
这些是如果有人下载该 HGS 群集的保护者元数据，将使用的证书。
若要检查哪些证书当前标记为主要证书，请运行以下命令：

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

若要设置新的主加密或签名证书，查找所需的证书的指纹，并将其标记为主要使用以下命令：

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>续订或更换密钥
当您创建使用 HGS 的证书时，证书将分配根据证书颁发机构的策略和请求信息的过期日期。
通常情况下，在证书的有效性是重要如保护 HTTP 通信的情况下，证书必须续订以避免服务中断或致命错误消息在过期之前。
HGS 不使用在此意义上的证书。
HGS 只需将证书用作方便地创建和存储的非对称密钥对。
过期的加密或 HGS 上的签名证书并不表示弱点或丢失的受防护的 Vm 的保护。
此外，不是由 HGS 执行证书吊销检查。
如果 HGS 证书或颁发机构的证书被吊销，它不会影响 HGS 的使用的证书。

您需要担心的 HGS 证书的唯一情况是如果您有理由相信其私钥已被盗。
在这种情况下，受防护的 Vm 的完整性将面临风险因为拥有 HGS 加密和签名密钥对的私钥部分是不足以删除 VM 上的防护保护或建立起了证明策略更弱的虚设 HGS 服务器。

如果发现自己处于这种情况下，或符合性标准需定期刷新证书密钥，以下步骤概述了更改 HGS 服务器上的密钥的过程。
请注意以下指南表示会导致到由 HGS 群集提供服务的每个 VM 的服务中断的一项重要工作。
需要正确规划 HGS 密钥更改以尽量减少服务中断并确保租户 Vm 的安全性。

HGS 节点上，执行以下步骤注册新的一对加密和签名证书。
请参阅章节[添加新的密钥](#adding-new-keys)有关的详细信息将新的密钥添加到 HGS 的各种方法。
1. 创建新的对的加密和签名证书的 HGS 服务器。 理想情况下，将在硬件安全模块中创建这些项目。
2. 注册新的加密和签名证书与**添加 HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. 如果使用指纹时，你将需要转到要安装的私钥，并授予 HGS gMSA 访问密钥在群集中每个节点。
4. 使新证书的默认证书中 HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

此时，屏蔽使用从 HGS 节点获取的元数据创建的数据将使用新证书，但现有 Vm 将继续起作用，因为旧证书仍有。
为了确保所有的现有 Vm 将使用新密钥，需要更新每个 VM 上的密钥保护程序。
这是需要 VM 所有者 （个人或实体的所有权的"所有者"监护人） 是所涉及的操作。
对于每个受防护的 VM，执行以下步骤：
5. 关闭 VM。 VM 不能重新打开之前的剩余步骤都完成，否则将需要重新开始该过程。
6. 将当前的密钥保护程序保存到文件： `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. 传输到 VM 所有者 KP
8. 没有所有者下载来自 HGS 的已更新的保护者信息并将其导入其本地系统上
9. 读取到内存中当前 KP、 授予对 KP，新保护者访问权限并将其保存到一个新的文件，通过运行以下命令：

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. 将更新的 KP 复制回托管构造
11. 适用于在原始 VM KP:

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. 最后，启动 VM，并确保其成功运行。

> [!NOTE]
> 如果 VM 所有者在 VM 上设置不正确的密钥保护程序，且并不授予您构造运行 VM，你将无法启动受防护的 VM。
> 若要返回到上次已知良好的密钥保护程序，请运行 `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

一旦具有更新的所有 Vm 进行授权的新的保护者密钥，可以禁用和删除旧密钥。

13. 获取从旧的证书的指纹 `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. 通过运行以下命令来禁用每个证书：  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. 确保 Vm 都仍能开始后禁用，证书删除 HGS 通过运行以下命令的证书：

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> VM 备份会包含旧允许旧的证书要用来启动该 VM 的密钥保护程序信息。
> 如果已经知道你的私匙已遭到破坏，应假定 VM 备份会受到影响，也并采取相应措施。
> 销毁备份 (.vmcx) 中的 VM 配置中将删除密钥保护程序，但代价是需要使用 BitLocker 恢复密码下一次启动 VM。

### <a name="key-replication-between-nodes"></a>节点之间的复制项
HGS 群集中的每个节点必须使用相同的加密签名，以及 （如果已配置） 配置 SSL 证书。
这是需要确保向群集中任一节点的 HYPER-V 主机可以有其请求提供服务已成功。

**如果初始化与基于 PFX 的证书的 HGS 服务器**然后 HGS 将自动在群集中的每个节点之间复制这些证书的公共和私有密钥。
只需在一个节点上添加项。

**如果初始化与证书引用的 HGS 服务器**或将仅复制指纹，则 HGS*公共*键中的每个节点的证书。
此外，HGS 不能授予本身访问权限在此方案中的任何节点上的私钥。
因此，它由你来：
1. HGS 的每个节点上安装的私钥
2. 授予对每个节点上的私钥的 HGS 组托管服务帐户 (gMSA) 访问这些任务将添加额外操作负担，但它们是由 HSM 支持的密钥和非可导出私钥的证书所必需的。

**SSL 证书**永远不会复制任何窗体中。
它由你负责初始化每个具有相同的 SSL 证书的 HGS 服务器并更新每个服务器，不论何时选择续订或替换的 SSL 证书。
当替换的 SSL 证书，建议使用完成[集 HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet。

## <a name="unconfiguring-hgs"></a>正在取消配置 HGS

如果需要解除授权或显著重新配置 HGS 服务器，您可以使用来完成[清除 HgsServer](https://technet.microsoft.com/library/mt652176.aspx)或[卸载 HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet。

### <a name="clearing-the-hgs-configuration"></a>清除 HGS 配置

若要从 HGS 群集删除节点，请使用[清除 HgsServer](https://technet.microsoft.com/library/mt652176.aspx) cmdlet。
此 cmdlet 将在其上运行的服务器上进行以下更改：

- 取消注册的证明和密钥保护服务
- 将移除"microsoft.windows.hgs"JEA 管理终结点
- 从 HGS 故障转移群集中删除本地计算机

如果服务器是群集中的最后一个 HGS 节点，群集和其相应的分布式网络名称资源将负责销毁。

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

清除操作完成后，HGS 服务器可以重新使用来初始化[Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx)。
如果您使用了[安装 HgsServer](https://technet.microsoft.com/library/mt652169.aspx)为设置了 Active Directory 域服务域，该域将保留配置和操作后清除操作。

### <a name="uninstalling-hgs"></a>卸载 HGS

如果你想要从 HGS 群集删除节点**并**将在其上运行 Active Directory 域控制器降级，请使用[卸载 HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet。
此 cmdlet 将在其上运行的服务器上进行以下更改：

- 取消注册的证明和密钥保护服务
- 将移除"microsoft.windows.hgs"JEA 管理终结点
- 从 HGS 故障转移群集中删除本地计算机
- 如果配置 Active Directory 域控制器，会将其降级

如果服务器是群集、 域、 故障转移群集中的最后一个 HGS 节点和群集的分布式网络名称资源也将被销毁。

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

卸载操作已完成并重新启动计算机后，您可以重新安装 ADDC 和使用 HGS[安装 HgsServer](https://technet.microsoft.com/library/mt652169.aspx)或将计算机加入到域并初始化与该域中的 HGS 服务器[初始化 HgsServer](https://technet.microsoft.com/library/mt652185.aspx)。

如果不再想要使用计算机作为 HGS 节点，您可以从 Windows 中删除角色。

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
