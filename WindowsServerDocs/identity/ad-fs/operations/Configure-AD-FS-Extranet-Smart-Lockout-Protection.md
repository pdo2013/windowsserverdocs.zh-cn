---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 配置 AD FS Extranet 锁定保护
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9ef595cc98a95caca0f2043b011868e0573a5b19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407700"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet 锁定和 Extranet 智能锁定

## <a name="overview"></a>概述

Extranet 智能锁定（ESL）可防止用户遇到恶意活动的 extranet 帐户锁定。  

ESL 使 AD FS 可以区分用户在熟悉的位置进行的登录尝试，以及从攻击者可能的登录尝试。 AD FS 可以锁定攻击者，同时允许有效用户继续使用其帐户。 这可以防止和防止用户遭受拒绝服务攻击和某些类密码喷涂攻击。 ESL 适用于 Windows Server 2016 中的 AD FS，并内置于 Windows Server 2019 的 AD FS 中。

ESL 仅适用于通过 extranet 使用 Web 应用程序代理或第三方代理的用户名和密码身份验证请求。 任何第三方代理都必须支持用于替代 Web 应用程序代理的 ADFSPIP 协议，如[F5 大 IP 访问策略管理器](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191)。 请参阅第三方代理文档，确定代理是否支持 ADFSPIP 协议。   

## <a name="additional-features-in-ad-fs-2019"></a>AD FS 2019 中的其他功能
与 AD FS 2016 相比，AD FS 2019 中的 Extranet 智能锁定增加了以下优点：
- 为熟悉和不熟悉的位置设置独立的锁定阈值，以使已知良好位置的用户在出现错误时具有更多空间，而不是来自可疑位置的请求
- 为智能锁定启用审核模式，同时继续强制执行以前的软锁定行为。 这样，你就可以了解用户熟悉的位置，并且仍受 AD FS 2012R2 提供的 extranet 锁定功能的保护。  

## <a name="how-it-works"></a>工作原理
### <a name="configuration-information"></a>配置信息
如果启用了 ESL，则会在项目数据库 AdfsArtifactStore AccountActivity 中创建一个新表，并在 AD FS 场中选择一个节点作为 "用户活动" 主节点。 在 WID 配置中，此节点始终是主节点。 在 SQL 配置中，选择一个节点作为用户活动主机。  

查看选定作为用户活动主机的节点。 AdfsFarmInformation. FarmRoles

所有辅助节点将通过端口80与每个全新登录上的主节点联系，以了解错误密码计数和新的常见位置值的最新值，并在处理登录名后更新该节点。

![配置](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 如果辅助节点无法联系主节点，它会将错误事件写入 AD FS 管理日志中。 将继续处理身份验证，但 AD FS 将仅在本地写入更新的状态。 AD FS 将每10分钟重试一次，并将在主节点可用后切换回主节点。

### <a name="terminology"></a>术语
- **FamiliarLocation**：在身份验证请求期间，ESL 将检查所有提供的 Ip。 这些 ip 将是网络 IP、转发 IP 和可选的 x 转发的 IP 的组合。 如果请求成功，则所有 Ip 都作为 "熟悉的 Ip" 添加到 "帐户" 活动表。 如果请求包含 "熟悉的 Ip" 中的所有 Ip，请求将被视为 "熟悉的" 位置。
- **Unknownlocation.xsd**：如果传入的请求中至少有一个 IP 未出现在现有 "FamiliarLocation" 列表中，则该请求将被视为 "未知" 位置。 这是为了处理代理方案，例如 Exchange online 旧身份验证，其中 Exchange Online 地址处理成功和失败的请求。  
- **badPwdCount**：一个值，表示提交错误密码和身份验证失败的次数。 对于每个用户，单独的计数器保存在熟悉的位置和未知位置。
- **UnknownLockout**：如果用户被锁定，无法从未知位置访问，则为每个用户的布尔值。 此值是基于 badPwdCountUnfamiliar 和 ExtranetLockoutThreshold 值计算得出的。
- **ExtranetLockoutThreshold**：此值决定错误密码尝试的最大次数。 达到阈值时，ADFS 将拒绝来自 extranet 的请求，直到达到 "观察" 窗口。
- **ExtranetObservationWindow**：此值确定未知位置的用户名和密码请求被锁定的持续时间。当窗口通过后，ADFS 将开始再次从未知位置执行用户名和密码身份验证。
- **ExtranetLockoutRequirePDC**：启用后，extranet 锁定需要主域控制器（PDC）。 禁用后，如果 PDC 不可用，extranet 锁定将回退到另一个域控制器。  
- **ExtranetLockoutMode**：控制仅记录与强制执行的 Extranet 智能锁定模式
    - **ADFSSmartLockoutLogOnly**：已启用 Extranet 智能锁定，但 AD FS 仅写入管理和审核事件，但不会拒绝身份验证请求。 此模式最初旨在允许在启用 "ADFSSmartLockoutEnforce" 前填充 FamiliarLocation。
    - **ADFSSmartLockoutEnforce**：达到阈值时，完全支持阻止不熟悉的身份验证请求。

支持 IPv4 和 IPv6 地址。

### <a name="anatomy-of-a-transaction"></a>事务解析
- **预身份验证检查**：在身份验证请求期间，ESL 将检查所有提供的 Ip。 这些 ip 将是网络 IP、转发 IP 和可选的 x 转发的 IP 的组合。 在审核日志中，这些 ip <IpAddress>在字段中以 x-e 转发的-客户端 ip （x-e）---e-

  根据这些 Ip，ADFS 确定请求是来自熟悉还是不熟悉的位置，然后检查各自的 badPwdCount 是否小于设置阈值限制，或者上次**失败**尝试是否比观察时段时间长期限. 如果满足这些条件之一，ADFS 将允许此事务进行进一步的处理和凭据验证。 如果这两个条件都为 false，则在观察窗口通过之前，该帐户已处于锁定状态。 观察窗口通过后，允许用户尝试一次身份验证。 请注意，在2019中，ADFS 会根据 IP 地址是否与熟悉的位置匹配来检查适当的阈值限制。
- **成功登录**：如果登录成功，则会将请求中的 Ip 添加到用户的熟悉位置 IP 列表。  
- **登录失败**：如果登录失败，badPwdCount 会增加。 如果攻击者向系统发送的密码超过阈值允许的数量，用户将进入锁定状态。 （badPwdCount > ExtranetLockoutThreshold）  

![配置](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

当帐户被锁定时，"UnknownLockout" 值将等于 true。这意味着用户的 badPwdCount 超过了阈值，即有人尝试使用的密码比系统允许的多。 在此状态下，有效的用户可以通过2种方式登录。
- 用户必须等待 ObservationWindow 时间或
- 若要重置锁定状态，请将 badPwdCount 重置为0，并返回 "ADFSAccountLockout"。

如果未发生重置，则允许该帐户对每个观察窗口的 AD 进行单个密码尝试。 该帐户将在该尝试后返回到锁定状态，并将重新启动 "观察" 窗口。 BadPwdCount 值将仅在密码登录成功后自动重置。

### <a name="log-only-mode-versus-enforce-mode"></a>仅限日志模式与 "强制" 模式
在 "仅限日志" 模式和 "强制" 模式下，将填充 AccountActivity 表。 如果跳过 "只记录" 模式，并且在没有建议的等待期的情况下将 ESL 直接移到 "强制" 模式，则 ADFS 将不知道熟悉的用户 Ip。 在这种情况下，ESL 的行为类似于 "ADBadPasswordCounter"，如果用户帐户受到活动暴力攻击，可能会阻止合法用户流量。 如果 "仅限日志" 模式被绕过并且用户进入 "已锁定" 状态为 "UnknownLockout" = TRUE，并且尝试使用不在 "熟悉" 的 ip 列表中的 IP 来登录，则这些用户将无法登录。 建议仅限日志模式3-7 天，以避免这种情况。 如果帐户积极受到攻击，则至少需要24小时的 "只需记录" 模式，以防合法用户的锁定。  

## <a name="extranet-smart-lockout-configuration"></a>Extranet 智能锁定配置  

### <a name="prerequisites-for-ad-fs-2016"></a>AD FS 2016 的先决条件

1. **在场中的所有节点上安装更新**

   首先，请确保所有 Windows Server 2016 AD FS 服务器都是最新的（截至年 6 2018 月的 Windows 更新），并且 AD FS 2016 场在2016场行为级别运行。
1. **验证权限**

   Extranet 智能锁定要求在每个 AD FS 服务器上启用 Windows 远程管理。
3. **更新项目数据库权限**

   Extranet 智能锁定要求 AD FS 的服务帐户具有在 AD FS 项目数据库中创建新表的权限。 以 AD FS 管理员身份登录到任何 AD FS 服务器，然后在 PowerShell 命令提示符窗口中执行以下命令，授予此权限：

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >$Cred 占位符是具有 AD FS 管理员权限的帐户。 这应提供用于创建表的写入权限。

   上面的命令可能会因为缺少足够的权限而失败，因为您的 AD FS 场正在使用 SQL Server，而上面提供的凭据对您的 SQL Server 没有管理员权限。 在这种情况下，你可以通过在连接到 AdfsArtifactStore 数据库时运行以下命令，在 SQL Server 数据库中手动配置数据库权限。
    ```  
    # when prompted with “Are you sure you want to perform this action?”, enter Y.

    [CmdletBinding(SupportsShouldProcess=$true,ConfirmImpact = 'High')]
    Param()

    $fileLocation = "$env:windir\ADFS\Microsoft.IdentityServer.Servicehost.exe.config"

    if (-not [System.IO.File]::Exists($fileLocation))
    {
    write-error "Unable to open ADFS configuration file."
    return
    }

    $doc = new-object Xml
    $doc.Load($fileLocation)
    $connString = $doc.configuration.'microsoft.identityServer.service'.policystore.connectionString
    $connString = $connString -replace "Initial Catalog=AdfsConfigurationV[0-9]*", "Initial Catalog=AdfsArtifactStore"

    if ($PSCmdlet.ShouldProcess($connString, "Executing SQL command sp_addrolemember 'db_owner', 'db_genevaservice' "))
    {
    $cli = new-object System.Data.SqlClient.SqlConnection
    $cli.ConnectionString = $connString
    $cli.Open()

    try
    {     

    $cmd = new-object System.Data.SqlClient.SqlCommand
    $cmd.CommandText = "sp_addrolemember 'db_owner', 'db_genevaservice'"
    $cmd.Connection = $cli
    $rowsAffected = $cmd.ExecuteNonQuery()  
    if ( -1 -eq $rowsAffected )
    {
    write-host "Success"
    }
    }
    finally
    {
    $cli.CLose()
    }
    }
    ```

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>确保启用 AD FS 安全审核日志记录
此功能利用了安全审核日志，因此必须在 AD FS 和所有 AD FS 服务器上的本地策略中启用审核。

### <a name="configuration-instructions"></a>配置说明
Extranet 智能锁定使用 ADFS 属性**ExtranetLockoutEnabled**。 此属性以前用于控制服务器2012R2 中的 "Extranet 软锁定"。 如果启用了 Extranet 软锁定，若要查看当前属性配置，请` Get-AdfsProperties`运行。

### <a name="configuration-recommendations"></a>配置建议
配置 Extranet 智能锁定时，请遵循设置阈值的最佳方案：  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: – 2x AD Threshold Value`

AD 值：20，ExtranetLockoutThreshold：10

Active Directory 锁定独立于 Extranet 智能锁定工作。 但是，如果启用 Active Directory 锁定，则 AD FS AD 中 < 帐户锁定阈值的 ExtranetLockoutThreshold

`ExtranetLockoutRequirePDC - $false`

启用后，extranet 锁定需要主域控制器（PDC）。 如果禁用并配置为 "false"，则在 PDC 不可用的情况下，extranet 锁定将回退到另一个域控制器。

若要设置此属性，请运行：

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>启用仅日志模式

在仅日志模式下，AD FS 会填充用户熟悉的位置信息并写入安全审核事件，但不会阻止任何请求。 此模式用于验证智能锁定是否正在运行，以及是否允许 AD FS 在启用 "强制" 模式之前为用户 "了解" 熟悉的位置。 AD FS 学习时，它存储每个用户的登录活动（无论是在仅限日志模式还是强制模式下）。
通过运行以下 commandlet，将锁定行为设置为仅记录。  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

"仅日志" 模式旨在作为临时状态，以便在使用智能锁定行为引入锁定强制之前，系统可以学习登录行为。 仅限日志模式的建议持续时间为3-7 天。 如果帐户积极受到攻击，则仅限日志模式运行至少24小时。

在 AD FS 2016 上，如果在启用 Extranet 智能锁定之前启用了 2012R2 "Extranet 软锁定" 行为，则仅日志模式将禁用 "Extranet 软锁定" 行为。 AD FS 智能锁定将不会在仅限日志模式下锁定用户。 但是，本地 AD 可能会基于 AD 配置锁定用户。 请查看 AD 锁定策略，了解本地 AD 如何锁定用户。

在 AD FS 2019 上，另一个优点是能够为智能锁定启用仅日志模式，同时继续使用以下 Powershell 强制执行以前的软锁定行为。

`Set-AdfsProperties -ExtranetLockoutMode 3`

为了使新模式生效，请在场中的所有节点上重新启动 AD FS 服务

`Restart-service adfssrv`

配置模式后，可以使用 EnableExtranetLockout 参数启用智能锁定

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>启用强制模式

熟悉 "锁定阈值" 和 "观察" 窗口后，可以使用以下 PSH cmdlet 将 ESL 移动到 "强制" 模式：

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

为了使新模式生效，请使用以下命令在场中的所有节点上重新启动 AD FS 服务。

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>管理用户帐户活动
AD FS 提供了三个用于管理帐户活动数据的 cmdlet。 这些 cmdlet 会自动连接到场中持有主角色的节点。
>[!NOTE]
>可以使用足够的管理（JEA）委托 AD FS commandlet 来重置帐户锁定。 例如，技术支持人员可以委派使用 ESL commandlet 的权限。 有关委派使用这些 cmdlet 的权限的信息，请参阅[委派 AD FS Powershell Commandlet 对非管理员用户的访问](delegate-ad-fs-pshell-access.md)权限

可以通过传递-Server 参数来重写此行为。

- ADFSAccountActivity-UserPrincipalName

  读取用户帐户的当前帐户活动。 Cmdlet 始终使用帐户活动 REST 终结点自动连接到场主机。 因此，所有数据应始终一致。

`Get-ADFSAccountActivity user@contoso.com`

  属性:
    - BadPwdCountFamiliar:从已知位置成功进行身份验证时递增。
    - BadPwdCountUnknown:当身份验证从未知位置失败时递增
    - LastFailedAuthFamiliar:如果身份验证不成功，则将 LastFailedAuthUnknown 设置为不成功的身份验证的时间
    - LastFailedAuthUnknown:如果未知位置的身份验证失败，则将 LastFailedAuthUnknown 设置为不成功的身份验证的时间
    - FamiliarLockout:如果 "BadPwdCountFamiliar" > ExtranetLockoutThreshold，则为 "True" 的布尔值。
    - UnknownLockout:如果 "BadPwdCountUnknown" > ExtranetLockoutThreshold，则为 "True" 的布尔值。  
    - FamiliarIPs：最多可为用户所熟悉的20个 Ip。 超过此限制时，将删除列表中最早的 IP。
-    ADFSAccountActivity

     添加新的熟悉位置。 熟悉的 IP 列表最多包含20个条目，如果超过此限制，则会删除列表中最早的 IP。

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- 重置-ADFSAccountLockout

  为用户帐户重置每个熟悉的位置（badPwdCountFamiliar）或不熟悉的位置计数器（badPwdCountUnfamiliar）的锁定计数器。 重置计数器后，将更新 "FamiliarLockout" 或 "UnfamiliarLockout" 值，因为重置计数器将小于阈值。  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>事件日志记录 & AD FS Extranet 锁定的用户活动信息

### <a name="connect-health"></a>连接运行状况
监视用户帐户活动的建议方法是使用连接运行状况。 连接运行状况生成有关风险 Ip 和错误密码尝试的可下载报告。 "有风险的 IP" 报表中的每个项都显示有关超出指定阈值的失败 AD FS 登录活动的聚合信息。 通过可自定义的电子邮件设置，可以将电子邮件通知设置为警报管理员。 有关其他信息和设置说明，请访问[连接运行状况文档](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)。

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS Extranet 智能锁定事件。
要写入 Extranet 智能锁定事件，必须在 "仅限日志" 或 "强制" 模式下启用 ESL，并启用 ADFS 安全审核。
AD FS 会将 extranet 锁定事件写入安全审核日志：
- 锁定用户时（达到不成功登录尝试的锁定阈值）
- 当 AD FS 收到已处于锁定状态的用户的登录尝试时

在 "仅日志" 模式下，你可以查看安全审核日志中的锁定事件。 对于找到的任何事件，你可以使用 ADFSAccountActivity cmdlet 检查用户状态，以确定是否从熟悉或不熟悉的 IP 地址进行了锁定，并再次检查该用户的熟悉 IP 地址的列表。


|事件 ID|描述|
|-----|-----|
|1203|此事件是针对每个错误密码尝试而编写的。 一旦 badPwdCount 达到 ExtranetLockoutThreshold 中指定的值，该帐户将在 ADFS 中被锁定，以在 ExtranetObservationWindow 中指定的持续时间。</br>活动 ID：% 1</br>XML：% 2|
|1201|每次锁定用户时都会写入此事件。 </br>活动 ID：% 1</br>XML：% 2|
|557（ADFS 2019）| 尝试与节点% 1 上的帐户存储 rest 服务进行通信时出错。 如果这是一个 WID 场，则主节点可能处于脱机状态。 如果这是 SQL 场，ADFS 会自动选择一个新节点来托管用户存储主机角色。|
|562（ADFS 2019）|Communcating 服务器% 1 上的帐户存储终结点时出错。</br>异常消息：% 2|
|563（ADFS 2019）|计算 extranet 锁定状态时出错。 由于% 1 的值将允许此用户进行身份验证，因此令牌颁发将继续。 如果这是一个 WID 场，则主节点可能处于脱机状态。 如果这是 SQL 场，ADFS 会自动选择一个新节点来托管用户存储主机角色。</br>帐户存储服务器名称：% 2</br>用户 Id：% 3</br>异常消息：% 4|
|512|已锁定以下用户的帐户。由于系统配置，允许登录尝试。</br>活动 ID：% 1 </br>用户：% 2 </br>客户端 IP：% 3 </br>错误的密码计数：% 4  </br>上次错误密码尝试：% 5|
|515|以下用户帐户处于锁定状态，并且刚刚提供了正确的密码。 此帐户可能已泄露。</br>其他数据 </br>活动 ID：% 1 </br>用户：% 2 </br>客户端 IP：% 3 |
|516|由于无效的密码尝试次数过多，以下用户帐户已被锁定。</br>活动 ID：% 1  </br>用户：% 2  </br>客户端 IP：% 3  </br>错误的密码计数：% 4  </br>上次错误密码尝试：% 5|

## <a name="esl-frequently-asked-questions"></a>ESL 常见问题

**在强制模式下使用 Extranet 智能锁定的 ADFS 场是否会发现恶意用户锁定？** 

答：如果 ADFS 智能锁定设置为 "强制" 模式，则永远不会看到合法用户的帐户被暴力破解或拒绝服务锁定。 恶意帐户锁定可阻止用户登录的唯一方法是：如果错误的执行组件具有用户密码，或者可以为该用户发送已知良好（熟悉）的 IP 地址的请求。 

**启用了 ESL 并且错误的执行组件具有用户密码会发生什么情况？** 

答：强力攻击方案的典型目的是猜测密码并成功登录。  如果用户是钓鱼的，或者如果密码被猜到，ESL 功能将不会阻止访问，因为登录将满足正确密码和新 IP 的 "成功" 条件。 糟糕的执行组件 IP 将显示为 "熟悉的"。 在这种情况下，最好的缓解措施是清除 ADFS 中的用户活动，并要求用户进行多重身份验证。 强烈建议安装 AAD 密码保护，以确保猜出的密码不会进入系统。

**如果我的用户从未从某一 IP 成功登录，然后多次尝试使用错误的密码，他们最后键入正确的密码，他们将能够登录？** 

答：如果用户提交多个错误密码（例如合法的错误键入），并且在以下尝试获得密码更正后，用户将立即成功登录。  这会清除错误的密码计数，并将该 IP 添加到 FamiliarIPs 列表中。  但是，如果它们超过未知位置的失败登录阈值，它们将进入 "正在锁定" 状态，并将需要等待 "观察" 窗口，并使用有效密码登录，或者需要管理员干预来重置其帐户。  
 
**ESL 是否也适用于 intranet？**

答：如果客户端直接连接到 ADFS 服务器而不是通过 Web 应用程序代理服务器连接，则不会应用 ESL 行为。  

**我在 "客户端 IP" 字段中看到 Microsoft IP 地址。ESL 是否会阻止 EXO 除外代理暴力攻击？**  

答：ESL 可以非常有效地阻止 Exchange Online 或其他旧身份验证暴力破解攻击方案。 旧身份验证的 "活动 ID" 为00000000-0000-0000-0000-000000000000。 在这些攻击中，糟糕的执行组件利用 Exchange Online 基本身份验证（也称为传统身份验证），因此客户端 IP 地址显示为 Microsoft。 云中的 Exchange online 服务器代表 Outlook 客户端验证身份验证。 在这些情况下，恶意提交者的 IP 地址将位于 "x-毫秒-转发的客户端 ip" 中，Microsoft Exchange Online server IP 将为 "x-ms-客户端-ip" 值。
Extranet 智能锁定检查网络 Ip、转发的 Ip、x 转发的客户端 IP 和 x-客户端 ip 值。 如果请求成功，则所有 Ip 都将添加到熟悉的列表。 如果请求传入，并且任何提供的 Ip 不在熟悉的列表中，则该请求将被标记为不熟悉。 熟悉的用户将无法成功登录，同时会阻止来自不熟悉位置的请求。  

\* * 问：在启用 ESL 之前，是否可以估算 ADFSArtifactStore 的大小？

答：在启用 ESL 的情况下，AD FS 在 ADFSArtifactStore 数据库中跟踪用户的帐户活动和已知位置。 此数据库相对于所跟踪的用户数和已知位置进行缩放。 当计划启用 ESL 时，可以估算 ADFSArtifactStore 数据库的大小，以每100000用户最多1GB 的速率增长。 如果 AD FS 场使用 Windows 内部数据库（WID），则数据库文件的默认位置为 C:\Windows\WID\Data\。 若要防止填充此驱动器，请确保在启用 ESL 前至少有5GB 的可用存储。 除了磁盘存储以外，还应计划在启用 ESL 后增加的总进程内存，使其达到500000或更低的用户人口的额外内存。


## <a name="additional-references"></a>其他参考  
[保护 Active Directory 联合身份验证服务的最佳实践](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-adfsproperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
