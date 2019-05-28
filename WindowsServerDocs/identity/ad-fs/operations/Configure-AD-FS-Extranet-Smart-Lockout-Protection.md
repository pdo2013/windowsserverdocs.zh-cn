---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 配置 AD FS Extranet 锁定保护
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 03/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dd6dea2fb8a16bfdbe93f93fbdd1dc5ac47af4be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189903"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet 锁定和 Extranet 的智能锁定

## <a name="overview"></a>概述

Extranet 智能锁定 (ESL) 可防止你的用户遇到免受恶意活动的 extranet 帐户锁定。  

ESL 使 AD FS 登录尝试从熟悉的位置的用户和登录尝试从什么可能是攻击者之间进行区分。 AD FS 可以同时允许继续使用其帐户的有效用户锁定攻击者。 这可以防止，这样可以避免拒绝服务和对用户的密码喷射攻击的某些类。 ESL 适用于 Windows Server 2016 中的 AD FS 和内置于 Windows Server 2019 中的 AD FS。 

ESL 功能仅适用于用户名和密码身份验证请求通过 extranet 的 Web 应用程序代理或受支持的一些第三方代理。   

## <a name="additional-features-in-ad-fs-2019"></a>AD FS 2019 中的其他功能 
Extranet 中 AD FS 2019 的智能锁定将添加到 AD FS 2016 比较了以下优势： 
- 设置独立的锁定阈值为大家所熟悉且不熟悉的位置，这样很好的已知位置中的用户可以从可疑位置拥有比请求更多空间错误 
- 启用审核模式的智能锁定，同时继续强制执行以前的软锁定行为。 这样即可了解用户熟悉的位置并仍会受到可从 AD FS 2012R2 的 extranet 锁定功能。  

## <a name="how-it-works"></a>其工作原理 
### <a name="configuration-information"></a>配置信息 
启用 ESL 后，创建的新表中的项目数据库，AdfsArtifactStore.AccountActivity，并在"用户活动"主 AD FS 场中选择节点。 在 WID 配置中，此节点始终是主节点。 在 SQL 配置中，选择一个节点以进行用户活动主数据。  

若要查看用户活动为主选择的节点。 Get-AdfsFarmInformation.FarmRoles 

所有辅助节点将联系主节点上通过端口 80，若要了解密码错误计数的最新值和新的熟悉的位置值，每个新登录名，并处理该登录名后更新该节点。 

![配置](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 如果辅助节点不能联系主机，它将写入到 AD FS 管理员日志错误事件。 身份验证将继续进行处理，但 AD FS 将只能写入本地的更新的状态。 AD FS 将重试联系主每隔 10 分钟，并提供主机后，切换回主。 

### <a name="terminology"></a>术语 
- **FamiliarLocation**:在身份验证请求，ESL 会检查所有显示的 Ip。 这些 Ip 会转发 IP 的网络 ip 地址和可选的 x-转发-对于 IP 的组合。 如果请求成功，所有 Ip 都添加到帐户活动表作为"熟悉的 Ip"。 如果请求具有"熟悉 Ip"中存在的所有 Ip，请求将视为"熟悉"位置。
- **UnknownLocation**:如果传入的请求已不存在现有的"FamiliarLocation"列表中至少一个 IP，请求将视为"未知"的位置。 这是为了处理代理方案，如 Exchange Online 的旧式身份验证的 Exchange Online 地址处理成功和失败的请求。  
- **badPwdCount**:一个值，表示已提交了错误的密码的次数和身份验证不成功。 为每个用户熟悉的位置和未知位置保留单独的计数器。 
- **UnknownLockout**:每个用户如果用户锁定从未知位置访问一个布尔值。 BadPwdCountUnfamiliar 和 ExtranetLockoutThreshold 值计算此值。 
- **ExtranetLockoutThreshold**:此值确定最大错误密码尝试次数。 当达到阈值时，ADFS 将拒绝来自 extranet 的请求，直到观察窗口已通过。
- **ExtranetObservationWindow**:此值确定从未知位置的用户名和密码的请求被锁定的持续时间。在经过该窗口，将开始 ADFS 再次从未知位置执行用户名和密码身份验证。 
- **ExtranetLockoutRequirePDC**:启用时，extranet 锁定要求主域控制器 (PDC)。 禁用时，extranet 锁定将回退到另一个域控制器，以防 PDC 不可用。  
- **ExtranetLockoutMode**:控件日志仅强制实施的 vs 模式的 Extranet 的智能锁定 
    - **ADFSSmartLockoutLogOnly**:Extranet 的智能锁定已启用，但 AD FS 将仅编写管理和审核事件，但将不拒绝身份验证请求。 此模式旨在为 FamiliarLocation 填充之前启用了 ADFSSmartLockoutEnforce 最初启用。
    - **ADFSSmartLockoutEnforce**:阻止不熟悉的身份验证的请求，达到阈值时完全支持。 

支持 IPv4 和 IPv6 地址。 

### <a name="anatomy-of-a-transaction"></a>事务的剖析 
- **身份验证前检查**:在身份验证请求，ESL 会检查所有显示的 Ip。 这些 Ip 会转发 IP 的网络 ip 地址和可选的 x-转发-对于 IP 的组合。 在审核日志中，这些 Ip 中列出<IpAddress>字段以 x-ms-转发的客户端的 ip，x-转发-对于，顺序 x-ms-代理-客户端的 ip。 
 
  根据这些 Ip，ADFS 确定如果请求是从熟悉的或不熟悉的位置，然后检查各自 badPwdCount 是否小于所设定的阈值限制或上次**失败**发生尝试时间超过观察窗口时间范围。 如果这些条件之一为 true，ADFS 允许进行进一步处理此事务和凭据验证。 如果两个条件都为 false，该帐户已处于锁定状态直到观察窗口将传递。 通过观察窗口后，允许用户一尝试进行身份验证。 请注意，在 2019，ADFS 将检查根据适当的阈值限制基于是否 IP 地址的熟悉的位置匹配。
- **成功登录**:如果登录成功，从请求的 Ip 被添加到用户的熟悉的位置 IP 列表中。  
- **失败的登录**:如果登录名失败 badPwdCount 为增量递增。 如果攻击者发送更多错误密码到系统超过阈值所允许用户将进入锁定状态。 (badPwdCount > ExtranetLockoutThreshold)  

![配置](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

帐户锁定时，"UnknownLockout"值将等于 true。这意味着，用户的 badPwdCount 高于阈值即有人尝试更多密码超过了允许使用的系统。 在此状态下，有两种方法的有效用户可以登录。 
- 用户必须等待 ObservationWindow 的时间间隔或
- 若要重置锁定状态，badPwdCount 重置为零且重置 ADFSAccountLockout。 

如果没有重置发生，该帐户将允许为每个观察窗口 AD 针对单一密码尝试。 该帐户将恢复为锁定状态之后的观察窗口并尝试将重新启动。 BadPwdCount 值将仅自动重置密码成功登录后。 

### <a name="log-only-mode-versus-enforce-mode"></a>仅限日志的模式与强制模式 
仅日志模式和强制模式的过程填充 AccountActivity 表。 如果仅日志模式，则跳过和 ESL 直接转移到强制模式下没有建议的等待时间段，熟悉 Ip 的用户将不是已知的 ADFS。 在这种情况下，ESL 将类似于 ADBadPasswordCounter，可能会阻止合法用户流量，如果用户帐户是受 active 暴力破解攻击。 如果仅日志模式，则跳过，并且用户与"UnknownLockout"状态进入锁定 = TRUE，并尝试登录的 ip 不在"熟悉"的 IP 列表中，合理的密码，则它们将无法再登录。 3-7 天，以避免这种情况下建议使用仅限日志的模式。 帐户是否主动在遭到攻击，至少 24 小时的仅日志模式下，才可防止对合法用户的锁定。  

## <a name="extranet-smart-lockout-configuration"></a>Extranet 的智能锁定配置  
 
### <a name="prerequisites-for-ad-fs-2016"></a>适用于 AD FS 2016 的先决条件 
 
1. **在服务器场中的所有节点上安装更新**

   首先，确保所有 Windows Server 2016 AD FS 服务器都是最新截至 2018 年 6 月 Windows 更新，并且在 2016年场行为级别上正在运行 AD FS 2016 场。
1. **验证权限** 

   Extranet 的智能锁定要求每个 AD FS 服务器上启用 Windows 远程管理。
3. **更新项目的数据库权限** 
 
   Extranet 的智能锁定需要 AD FS 服务帐户有权在 AD FS 项目数据库中创建一个新表。 登录到 AD FS 管理员任何 AD FS 服务器，然后通过在 PowerShell 命令提示符窗口中执行以下命令授予此权限： 

   ``` powershell
   PS C:\>$cred = Get-Credential 
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred 
   ``` 
   >[!NOTE]
   >$Cred 占位符为具有 AD FS 管理员权限的帐户。 这会提供写权限来创建表。 

   上述命令可能会失败由于缺乏足够的权限，因为 AD FS 场正在使用 SQL Server，并在上面提供的凭据不具有 SQL server 上的管理员权限。 在这种情况下，你可以配置数据库权限手动 SQL Server 数据库中通过运行以下命令，在连接到 AdfsArtifactStore 数据库时。 
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

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>确保 AD FS 安全审核日志记录已启用 
此功能会利用安全审核日志，因此审核必须在 AD FS，以及所有 AD FS 服务器上的本地策略中启用。 
 
### <a name="configuration-instructions"></a>配置说明 
Extranet 的智能锁定使用 ADFS 属性**ExtranetLockoutEnabled**。 Server 2012R2 中的控件"软 Extranet 锁定"到先前使用此属性。 如果启用了 Extranet 软锁定，若要查看当前的属性配置，请运行` Get-AdfsProperties`。 

### <a name="configuration-recommendations"></a>配置建议 
在配置 Extranet 的智能锁定时，请执行设置的阈值的最佳实践：  

`ExtranetObservationWindow (new-timespan -Minutes 30)` 

`ExtranetLockoutThreshold: – 2x AD Threshold Value` 

AD 值：20，ExtranetLockoutThreshold:10 

Active Directory 锁定独立于 Extranet 智能锁定。 但是，如果启用了 Active Directory 锁定，在 AD FS ExtranetLockoutThreshold < AD 中的帐户锁定阈值 

`ExtranetLockoutRequirePDC - $false`
 
启用时，extranet 锁定要求主域控制器 (PDC)。 禁用和配置为 false，extranet 锁定将回退到另一个域控制器，以防 PDC 不可用。 
 
若要将运行此属性的设置： 

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false 
```
### <a name="enable-log-only-mode"></a>启用仅日志模式 
 
在日志唯一模式下，AD FS 填充用户熟悉的位置信息和写入安全审核事件，但不会阻止任何请求。 此模式下用于验证智能锁定正在运行，并且若要启用 AD FS，若要为用户熟悉的位置"了解"之前启用"强制实施"模式。 了解 AD FS，因为它存储每个用户的登录活动 (不论是在日志唯一模式还是强制模式下)。 设置锁定行为，以便只能通过运行下面的 commandlet 记录。  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly` 

日志仅模式旨在为临时状态，以便系统可以了解之前引入锁定强制实施与智能锁定行为的登录行为。 仅日志模式下的建议持续时间为 3 到 7 天。 如果帐户是主动受到攻击，必须至少 24 小时运行仅日志模式。 

在 AD FS 2016，如果在启用 Extranet 的智能锁定之前启用 2012R2 软 Extranet 锁定行为仅日志模式下将禁用软 Extranet 锁定行为。 AD FS 智能锁定将不锁定在仅日志模式下的用户。 但是，在本地 AD 可能锁定用户，基于 AD 的配置。 请查看 AD 锁定策略，若要了解如何在本地 AD 可以锁定用户。 

在 AD FS 2019，另一个优点是能够启用的智能锁定仅日志模式，同时继续强制执行上一软锁定行为使用以下 Powershell。 

`Set-AdfsProperties -ExtranetLockoutMode 3` 
 
对于新模式，才会生效，在重新启动所有节点上的 AD FS 服务场 

`Restart-service adfssrv` 
 
配置模式后，可以启用智能锁定使用 EnableExtranetLockout 参数 
 
`Set-AdfsProperties -EnableExtranetLockout $true` 

### <a name="enable-enforce-mode"></a>启用强制模式 
 
您可轻松与锁定阈值和观察窗口后，可以移动 ESL 为"强制"模式使用以下 PSH cmdlet: 

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce` 

对于新模式，才会生效，在重新启动所有节点上的 AD FS 服务场使用以下命令。 

`Restart-service adfssrv` 

## <a name="manage-user-account-activity"></a>管理用户帐户活动 
AD FS 提供了三个 cmdlet 来管理帐户的活动数据。 这些 cmdlet 自动连接到服务器场拥有主角色中的节点。 
>[!NOTE] 
>Just Enough Administration (JEA) 可用于委派 AD FS commandlet 重置帐户锁定。 例如，帮助台人员可以使用 ESL commandlet 委派的权限。 有关委派使用这些 cmdlet 的权限的信息，请参阅[代理 AD FS Powershell Commandlet 访问权限的非管理员用户](delegate-ad-fs-pshell-access.md)

可以通过传递重写此行为-Server 参数。 

- Get-ADFSAccountActivity 

  读取用户帐户的当前帐户活动。 该 cmdlet 始终自动连接到服务器场主使用的帐户活动 REST 终结点。 因此，所有数据应始终都保持一致。 

  例如：Get-ADFSAccountActivity user@contoso.com 

  属性: 
    - BadPwdCountFamiliar:递增身份验证成功后从已知位置。
    - BadPwdCountUnknown:当从未知位置身份验证不成功时递增
    - LastFailedAuthFamiliar:如果从熟悉的位置，身份验证不成功，LastFailedAuthUnknown 设置为不成功的身份验证的时间 
    - LastFailedAuthUnknown:如果从未知位置，身份验证不成功，LastFailedAuthUnknown 设置为不成功的身份验证的时间 
    - FamiliarLockout:布尔值，该值将为"True"，如果"BadPwdCountFamiliar"> ExtranetLockoutThreshold 
    - UnknownLockout:布尔值，该值将为"True"，如果"BadPwdCountUnknown"> ExtranetLockoutThreshold  
    - FamiliarIPs： 最大为 20 个 Ip 的熟悉的用户。 超出此限制时将删除最旧的 IP 列表中。 
-    Set-ADFSAccountActivity 
     
     添加新熟悉的位置。 熟悉的 IP 列表具有最多达 20 个条目，如果超出此限制，在列表中的最旧 IP，将删除。 

     例如：Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”

- Reset-ADFSAccountLockout 
   
  重置的每个熟悉的位置 (badPwdCountFamiliar) 的用户帐户锁定计数器或计数器熟悉的位置 (badPwdCountUnfamiliar)。 通过重置计数器，将更新的"FamiliarLockout"或"UnfamiliarLockout"值，因为重置计数器将低于阈值。  

   例如：重置 ADFSAccountLockout user@contoso.com -位置常见示例：重置 ADFSAccountLockout user@contoso.com -位置未知 

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>事件日志记录和 AD FS Extranet 锁定的用户活动信息 

### <a name="connect-health"></a>连接运行状况 
监视用户帐户活动的建议的方法是通过连接运行状况。 连接运行状况生成可下载报告风险 Ip 和错误密码尝试次数。 有风险 IP 报表中的每个项将显示有关失败的 AD FS 登录活动的次数超出指定的阈值的聚合的信息。 使用可自定义电子邮件设置将发生这种情况时，就立即可以设置电子邮件通知来提醒管理员。 有关其他信息和设置说明，请访问[Connect Health 文档](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-adfs)。 

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS Extranet 智能锁定事件。 
Extranet 的智能锁定事件能够进行编写，必须在仅日志或强制模式下启用 ESL 和启用了 ADFS 安全审核。 AD FS 将 extranet 锁定事件写入安全审核日志： 
- 当用户被锁定在扩展 （达到锁定阈值的失败登录尝试） 
- 当 AD FS 收到已处于锁定状态的用户的登录尝试 

在日志唯一模式下，可以检查锁定事件的安全审核日志。 找到任何事件，可以检查用户状态使用 Get ADFSAccountActivity cmdlet 来确定是否锁定发生从熟悉的或不熟悉的 IP 地址，并仔细检查该用户熟悉的 IP 地址的列表。 


|事件 ID|描述|
|-----|-----| 
|1203|每次错误密码尝试写入此事件。 只要 badPwdCount 达到 ExtranetLockoutThreshold 中指定的值，该帐户将被锁定在 ADFS ExtranetObservationWindow 中指定的持续时间。</br>活动 ID: %1</br>XML: %2|
|1201|锁定用户每次写入此事件。 </br>活动 ID: %1</br>XML: %2| 
|557 (ADFS 2019)| 尝试与节点 %1 上的帐户存储 rest 服务进行通信时出错。 如果这是 WID 场的主节点可能处于脱机状态。 如果这是 SQL 场 ADFS 将自动选择新用户存储主机角色的节点。| 
|562 (ADFS 2019)|出错时与帐户 communcating 将终结点存储在服务器 %1。</br>异常消息： %2| 
|563 (ADFS 2019)|计算 extranet 锁定状态时出错。 由于 %1 的值将为此用户允许设置身份验证，并继续令牌颁发。 如果这是 WID 场的主节点可能处于脱机状态。 如果这是 SQL 场 ADFS 将自动选择新用户存储主机角色的节点。</br>帐户存储服务器名称： %2</br>用户 Id: %3</br>异常消息： %4|
|512|为下列用户帐户已锁定。由于系统配置被允许登录尝试。</br>活动 ID: %1 </br>用户： %2 </br>客户端 IP: %3 </br>错误密码计数： %4  </br>持续不断地尝试密码： %5|
|515|以下用户帐户处于锁定状态，只是提供正确的密码。 此帐户可能会泄露。</br>其他数据 </br>活动 ID: %1 </br>用户： %2 </br>客户端 IP: %3 |
|516|以下用户帐户具有太多错误密码尝试因已锁定。</br>活动 ID: %1  </br>用户： %2  </br>客户端 IP: %3  </br>错误密码计数： %4  </br>持续不断地尝试密码： %5|

## <a name="esl-frequently-asked-questions"></a>ESL 方面的常见问题 
 
**将使用 Extranet 中的智能锁定的 ADFS 场强制模式下不会看到恶意用户锁定？**  

答：如果 ADFS 智能锁定设置为强制模式则永远不会将会看到合法用户的帐户已锁定的暴力破解攻击或拒绝服务。 恶意帐户锁定可能会阻止用户登录的唯一方法是，如果恶意参与者具有用户密码，或可以为该用户从已知的良好 （熟悉） IP 地址发送请求。  
 
**启用 ESL，而恶意参与者的用户密码，则会发生什么情况？**  

答：典型的暴力破解攻击方案旨在猜出密码并成功登录。  如果用户是被骗或猜出密码然后 ESL 功能不会阻止访问由于登录将满足正确的密码以及新的 IP"成功"条件。 不良参与方 IP 则会显示为一个"熟悉"。 在此方案中最佳的缓解是清除 ADFS 中的用户的活动并要求多重身份验证的用户。 我们强烈建议安装 AAD 密码保护，以确保系统不会被猜出密码。 
 
**如果我的用户从未登录已成功从 IP，然后使用错误的密码尝试几次将它们是否能够登录后最后正确键入其密码？**  

答：如果用户提交多个错误的密码 （即以合法方式错误地键入） 和以下示例尝试获取的密码正确，然后用户会立即成功登录。  这将清除错误密码计数并添加到 FamiliarIPs 列表该 IP。  但是，如果它们超过的阈值的失败的登录名从未知位置，它们将进入锁定状态，它们将需要等待过去的观察窗口并使用有效的密码登录或需要管理员干预来重置其帐户。  
 
**ESL 的工作原理在 intranet 过吗？**    答：如果客户端连接直接到 ADFS 服务器，而无需通过 Web 应用程序代理服务器将不会应用 ESL 行为。  
 
**我看到客户端 IP 字段中的 Microsoft IP 地址。没有 ESL 块 EXO 代理暴力破解攻击？**   

答：ESL 可以将有效地防止 Exchange Online 或其他传统的身份验证暴力破解攻击方案。 旧式身份验证有 00000000-0000-0000-0000-000000000000"活动 ID"。 在这些攻击中，恶意参与者充分利用 Exchange Online 的基本身份验证 （也称为旧式身份验证），以便客户端 IP 地址显示为一个 Microsoft。 云代理代表 Outlook 客户端身份验证验证中的 Exchange online 服务器。 在这些情况下，恶意的提交者的 IP 地址将在 x-ms-转发的客户端的 ip 和 IP 将 x ms-客户端 ip 值中的 Microsoft Exchange Online 服务器中。 Extranet 的智能锁定会检查网络的 Ip 转发的 Ip、 x-转发-客户端的 IP 和 x ms-客户端 ip 值。 如果请求成功，所有 Ip 都添加到熟悉的列表。 如果传入某个请求的任何提供的 Ip 不熟悉列表中则请求会标记为不熟悉。 熟悉的用户将能够成功登录时从不熟悉的位置的请求将被阻止。  


## <a name="additional-references"></a>其他参考  
[保护 Active Directory 联合身份验证服务的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

    
