---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 配置 AD FS Extranet 锁定保护
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 02/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 904b563da2f1404d873c7352db9eadb7bfe252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869758"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet 锁定和 Extranet 的智能锁定

# <a name="overview"></a>概述

>适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

在 Windows Server 2012 R2 上的 AD FS，我们引入了名为的安全功能[Extranet 软锁定](configure-ad-fs-extranet-soft-lockout-protection.md)。  使用此功能，AD FS 将停止为一段时间内来自 extranet 的用户进行身份验证。  这会阻止你的用户帐户被锁定在 Active Directory 中。 除了保护用户免受 AD 帐户锁定，AD FS extranet 锁定还可防止暴力密码猜测攻击。

在 2018 年 6 月，Windows Server 2016 上的 AD FS 引入**Extranet 智能锁定 (ESL)**。  ESL 使 AD FS 看起来像是来自有效用户的登录尝试和从什么可能是攻击者的登录名之间进行区分。 因此，AD FS 便可锁定攻击者同时允许继续使用其帐户的有效用户。 这可以防止对用户的拒绝的服务，可防止有针对性的攻击，例如"密码-喷涂"攻击。  
ESL 适用于 Windows Server 2016 中的 AD FS 和内置于 Windows Server 2019 中的 AD FS。

> [!NOTE]
> 此功能仅适用于**extranet 方案**哪些身份验证请求中通过 Web 应用程序代理而仅适用于**用户名和密码身份验证**。

## <a name="advantages-of-extranet-smart-lockout-in-ad-fs-2016"></a>Extranet AD FS 2016 中的智能锁定的优点
Extranet AD FS 2012 R2 中的软锁定提供以下主要优点：
- 保护中的用户帐户**暴力破解攻击**中的攻击者尝试猜测用户的密码，通过持续发送身份验证请求和从**密码喷射攻击**其中攻击者尝试使用许多不同的帐户使用常见密码
- 保护中的用户帐户**Active Directory 帐户锁定**从使用错误密码的恶意的身份验证请求。 在这种情况下，虽然用户帐户将被锁定为 extranet 访问，但用户仍然可以从公司网络登录到 AD。 这称为**软锁定**。

Extranet 的智能锁定通过添加以下基于 extranet 软锁定的优点：
- 保护用户免遭遇到**extranet 帐户锁定**恶意身份验证请求。  智能锁定将阻止潜在的恶意请求从不熟悉的位置，同时允许从熟悉的位置从 extranet 登录的真实用户 （从该用户已成功登录前的位置）。
- 具有日志的唯一模式，以便系统可以了解良好和具有潜在恶意的登录活动，但不能禁用任何帐户

## <a name="additional-advantages-of-extranet-smart-lockout-in-ad-fs-2019"></a>Extranet 中 AD FS 2019 的智能锁定的其他优点
Extranet 中 AD FS 2019 的智能锁定将添加到 AD FS 2016 比较了以下优势：
- 设置独立的锁定阈值为大家所熟悉且不熟悉的位置，这样很好的已知位置中的用户可以从可疑位置拥有比请求更多空间错误
- 启用审核模式的智能锁定，同时继续强制执行以前的软锁定行为

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2016"></a>Extranet AD FS 2016 中的智能锁定的必备组件
需要使用 AD FS 2019 ESL 以下系统必备组件。

### <a name="install-updates-on-all-nodes-in-the-farm"></a>在服务器场中的所有节点上安装更新
首先，确保所有 Windows Server 2016 AD FS 服务器都是最新截至 2018 年 6 月 Windows 更新，并且在 2016年场行为级别上正在运行 AD FS 2016 场。

### <a name="update-artifact-database-permissions"></a>更新项目的数据库权限
Extranet 的智能锁定要求 AD FS 服务帐户在 ADFS 项目数据库中具有对新表的权限。  通过 PowerShell 命令窗口中执行以下命令授予此权限：
``` powershell
PS C:\>$cred = Get-Credential
PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
```
其中`$cred`是具有 AD FS 管理员权限的帐户 （AD FS 管理员不需要权限以使更改的数据库。）

>[!NOTE]
>使用 WID 数据库的多个服务器场，在上面的 cmdlet 需要，每个 AD FS 服务器上启用 Windows 远程管理

如果还没有 AD FS 管理员权限，你可以配置数据库权限手动 SQL 或 WID 中通过运行以下命令连接到 AdfsArtifactStore 数据库时。
```
sp_addrolemember 'db_owner', 'db_genevaservice'
```
### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>确保 AD FS 安全审核日志记录已启用
此功能会利用安全审核日志，因此审核必须在 AD FS，以及所有 AD FS 服务器上的本地策略中启用。

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2019"></a>在 AD FS 2019 Extranet 智能锁定的必备组件
需要使用 AD FS 2016 ESL 以下系统必备组件。

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>确保 AD FS 安全审核日志记录已启用
此功能会利用安全审核日志，因此审核必须在 AD FS，以及所有 AD FS 服务器上的本地策略中启用。

## <a name="lockout-settings"></a>锁定设置
Extranet 的智能锁定包含的一组由新的和现有 AD FS 属性控制的新功能。

### <a name="extranet-lockout-enabled"></a>已启用的 extranet 锁定
Extranet 的智能锁定使用相同的 AD FS 属性之前已使用只是为了控制"soft"extranet 锁定。  该属性称为 ExtranetLockoutEnabled，可以通过 Get-adfsproperties 进行查看。

### <a name="extranet-smart-lockout-mode"></a>Extranet 的智能锁定模式
添加了一个名为 ExtranetLockoutMode 的新 AD FS 属性，以控制智能 vs"soft"的锁定行为。  它可以通过 Set-adfsproperties 设置，并包含 3 个值：

    - **ADPasswordCounter** – 这是不区分 ADFS"软 extranet 锁定"模式基于位置的旧。  这是默认值。

    - **ADFSSmartLockoutLogOnly** – 这是 Extranet 的智能锁定，但 AD FS 而不是拒绝身份验证请求，将仅写入管理和审核事件。

    - **ADFSSmartLockoutEnforce** -这是 Extranet 的智能锁定对阻止不熟悉的请求，达到阈值时完全支持。

在 AD FS 2019 中，因此该软锁定会继续强制执行时您要准备用于智能锁定可以组合 ADPasswordCounter 和 ADFSSmartLockoutLogOnly 的值。

### <a name="lockout-threshold-and-observation-window"></a>锁定阈值和观察窗口
AD FS 2019 中的智能锁定使用相同两个 AD FS 属性作为之前从未使用过的软锁定：ExtranetObservationWindow 和 ExtranetLockoutThreshold。

- **ExtranetLockoutThreshold&lt;整数&gt;** 这定义了最大错误密码尝试次数。 达到阈值后，在 ADFSSmartLockoutEnforce 模式下 AD FS 将拒绝来自 extranet 的请求之前已通过观察窗口。  在 ADFSSmartLockoutLogOnly 模式下，AD FS 将写入日志项仅。  
- **ExtranetObservationWindow&lt;时间跨度&gt;** 这可确定长用户名和密码锁定从不熟悉的位置的请求。AD FS 将开始执行用户名和密码身份验证再次传递窗口时。

> [!NOTE]
> AD FS extranet 锁定函数独立于 AD 锁定策略。 我们建议您将设置**ExtranetLockoutThreshold**为低于 AD 帐户锁定阈值的值的参数值。 如果不这样做将导致无法保护帐户免受被锁定在 Active Directory 中的 AD FS。 

在 AD FS 2019 中，我们引入了新的锁定阈值特定到已知良好的位置：ExtranetLockoutThresholdFamiliarLocation。
- **ExtranetLockoutThresholdFamiliarLocation&lt;整数&gt;** 这将定义的最大错误密码尝试从熟悉的位置数。 在 AD FS 2019 中，原始参数 ExtranetLockoutThreshold 适用于不熟悉的位置 （IP 地址不已知保持完好） 中。

### <a name="primary-domain-controller-requirement"></a>主域控制器要求
AD FS 2016 提供了一个参数，它允许 PDC 不可用时回退到另一个域控制器。

- **ExtranetLockoutRequirePDC&lt;布尔&gt;** extranet 锁定启用时，需要主域控制器 (PDC)。 禁用时，extranet 锁定将回退到另一个域控制器，以防 PDC 不可用。

   下面的示例显示了 cmdlet 来启用锁定并禁用 PDC 要求：

    ```powershell
    Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
    ```

## <a name="configuring-ad-fs-with-smart-lockout-in-log-only-mode"></a>使用在日志仅模式下的智能锁定配置 AD FS

### <a name="ad-fs-2016"></a>AD FS 2016
我们建议您首先设置锁定行为，以便记录只能通过运行以下 cmdlet:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly
 ```

在此模式下，AD FS 填充用户熟悉的位置信息和写入安全审核事件，但不会阻止任何请求。  此模式下用于验证智能锁定正在运行，并且若要启用 AD FS，若要为用户熟悉的位置"了解"之前启用"强制实施"模式。
了解 AD FS，因为它存储每个用户的登录活动 (不论是在日志唯一模式还是强制模式下)。 

>[!NOTE]
>配置`ExtranetLockoutMode`到`AdfsSmartlockoutLogOnly`将导致旧的 AD FS"extranet 软锁定"行为不再有效，即使`EnableExtranetLockout`属性设置为 True。  这意味着，超过了从熟悉的或不熟悉的 IP 地址的锁定阈值的用户将不会被锁定的 AD FS 智能锁定。 但是，在本地 AD 可能会锁定客户可以根据存在的配置。   请参阅[帐户锁定策略](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/account-lockout-policy)若要了解如何在本地 AD 可以锁定用户。"  这旨在为临时状态，以便系统可以了解之前再叙锁定强制使用新的智能锁定行为的登录行为。

对于新模式，才会生效，在重新启动所有节点上的 AD FS 服务场
  
  ``` powershell
PS C:\>Restart-service adfssrv
  ```
配置模式后，可以启用智能锁定使用`EnableExtranetLockout`参数


``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $true
```

请注意，您可以使用同一 cmdlet 禁用锁定

例如：禁用锁定

``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $false
```
### <a name="ad-fs-2019"></a>AD FS 2019
如果您目前不使用 AD FS Extranet 软锁定，我们建议您按照与 AD FS 2016 上述相同的指南。
如果使用软锁定，但是，我们建议设置 AD FS 2019 锁定行为，以便记录的智能锁定，但保留实施软锁定，请使用以下 powershell:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode 3
 ```

一旦执行此 cmdlet，可以使用 Get-adfsproperties 来查询 ExtranetLockoutMode AD FS 属性的值。  你将看到，其值更新 ADPasswordCounter 和 ADFSSmartLockoutLogOnly 的按位组合。

## <a name="observing-audit-events"></a>观察审核事件
AD FS 将 extranet 锁定事件写入安全审核日志：
-   当用户被锁定在扩展 （达到锁定阈值的失败登录尝试）
-   当 AD FS 收到已处于锁定状态的用户的登录尝试

在日志唯一模式下，可以检查锁定事件的安全审核日志。  找到任何事件，可以检查用户状态使用 Get ADFSAccountActivity cmdlet 来确定是否锁定发生从熟悉的或不熟悉的 IP 地址，并仔细检查该用户熟悉的 IP 地址的列表。

示例事件：
```
Log Name:      Security
Source:        AD FS Auditing
Date:          5/21/2018 12:55:59 AM
Event ID:      1210
Task Category: (3)
Level:         Information
Keywords:      Classic,Audit Failure
User:          CONTOSO\adfssvc
Computer:      ADFS2016FS1.corp.contoso.com
Description:
An extranet lockout event has occurred. See XML for failure details. 

Activity ID: fa7a8052-0694-48f0-84e2-b51cde40ac3d 

Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>
Event Xml:
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="AD FS Auditing" />
    <EventID Qualifiers="0">1210</EventID>
    <Level>0</Level>
    <Task>3</Task>
    <Keywords>0x8090000000000000</Keywords>
    <TimeCreated SystemTime="2018-05-21T00:55:59.921880300Z" />
    <EventRecordID>35521235</EventRecordID>
    <Channel>Security</Channel>
    <Computer>ADFS2016FS1.contoso.com</Computer>
    <Security UserID="S-1-5-21-1156273042-1594504307-2076964089-1104" />
  </System>
  <EventData>
    <Data>fa7a8052-0694-48f0-84e2-b51cde40ac3d</Data>
    <Data><?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase></Data>
  </EventData>
</Event>
```

## <a name="observing-user-activity"></a>观察用户活动
AD FS 提供 powershell cmdlet 来查看和管理用户帐户活动数据。  若要读取的用户帐户的当前帐户活动。  使用以下 cmdlet

``` powershell
PS C:\>Get-ADFSAccountActivity user@contoso.com
```

示例输出
```
Identifier             : CONTOSO\user
BadPwdCountFamiliar    : 0
BadPwdCountUnknown     : 0
LastFailedAuthFamiliar : 1/1/0001 12:00:00 AM
LastFailedAuthUnknown  : 1/1/0001 12:00:00 AM
FamiliarLockout        : False
UnknownLockout         : False
FamiliarIps            : {}
```

当前的活动输出包含以下数据：

**标识符**： 这是用户名

**BadPwdCountFamiliar**： 这是从 IP 地址上的"FamiliarIps"列表已的尝试的时间不正确的密码的登录尝试的当前计数

**BadPwdCountUnknown**： 这是从 IP 地址已不在的"FamiliarIps"列表上的尝试的时间不正确的密码的登录尝试的当前计数

**LastFailedAuthFamiliar**： 这是来自"FamiliarIps"列表，在尝试的时间的某个 IP 地址的最后一个不正确的密码登录尝试的时间

**LastFailedAuthUnknown**： 这是来自不是"FamiliarIps"列表尝试的时间在某个 IP 地址的最后一个不正确的密码登录尝试的时间

**FamiliarLockout**： 这指示如果用户当前处于锁定状态的正确的密码尝试从"FamiliarIps"列表上的 IP 地址 

**UnknownLockout**： 这表示是否用户当前处于锁定状态的正确的密码尝试从 IP 地址不在的"FamiliarIps"FamiliarIps 列表： 这是用户熟悉的 IP 地址的当前列表

## <a name="adjust-threshold-and-window"></a>调整阈值和窗口
在您具有运行后在日志仅模式下足够长的适用于 AD FS，若要了解登录位置的时间，您可能希望调整阈值或观察窗口中的默认设置。  这是使用`Set-AdfsProperties`如下面的示例中所示：

使用设置观察窗口`ExtranetObservationWindow`:

例如： 

``` powershell
PS C:\>Set-AdfsProperties -ExtranetObservationWindow ( new-timespan -minutes 30 )
```

其中的值是一个时间跨度

### <a name="setting-threshold-value-in-ad-fs-2016"></a>在 AD FS 2016 中设置阈值
在 AD FS 2016 中，使用 ExtranetLockoutThreshold 设置阈值：

例如：

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

### <a name="setting-threshold-values-in-ad-fs-2019"></a>在 AD FS 2019 中设置的阈值
在 AD FS 2019 中,，有已知良好和不熟悉的位置不同的阈值

若要设置的阈值不熟悉的位置，请使用用于 AD FS 2016 以上相同的属性：

例如：

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

若要设置已知良好的位置的阈值，请使用新属性 ExtranetLockoutThresholdFamiliarLocation，如下面的示例中所示：

例如：

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThresholdFamiliarLocation 10
```


## <a name="enable-enforce-mode"></a>启用强制模式
一旦您具有已在日志唯一模式下运行的 AD FS 若要了解登录位置和要观察的任何锁定活动，有足够的时间和熟练使用锁定阈值和观察窗口后，可以移动智能锁定"强制"模式下使用下面的 PSH cmdlet:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce
```

对于新模式，才会生效，在重新启动所有节点上的 AD FS 服务场

``` powershell
PS C:\>Restart-service adfssrv
```


## <a name="manage-user-account-activity"></a>管理用户帐户活动
AD FS 提供 3 个 cmdlet 来管理用户帐户活动数据。  这些 cmdlet 会自动连接到服务器场等待复制过程中的节点 (尽管可以通过传递重写此行为-Server 参数)。

> [!NOTE] 
> 有关委派使用这些 cmdlet 的权限的信息，请参阅[代理 AD FS Powershell Commandlet 访问权限的非管理员用户](delegate-ad-fs-pshell-access.md)

这些 cmdlet 包括：

`Get-ADFSAccountActivity`

读取用户帐户的当前帐户活动。  该 cmdlet 始终自动连接到服务器场主使用帐户活动 REST 终结点，从而使所有数据应始终都保持一致

``` powershell
Get-ADFSAccountActivity user@contoso.com
```
`
Set-ADFSAccountActivity
`

更新用户帐户的帐户活动。  这可用来添加新熟悉的位置或擦除的任何帐户的状态

``` powershell
Set-ADFSAccountActivity user@upnsuffix.com -FamiliarLocation “1.2.3.4”
```
`Reset-ADFSAccountLockout`

将重置的用户帐户锁定计数器

``` powershell
Reset-ADFSAccountLockout user@upnsuffix.com -Familiar
```

## <a name="troubleshooting-esl"></a>故障排除 ESL
以下可帮助您进行疑难解答的 extranet 的智能锁定功能。

### <a name="updating-database-permissions-for-esl"></a>正在更新 ESL 的数据库权限
如果从返回了任何错误`Update-AdfsArtifactDatabasePermission`cmdlet，请确认以下

1.  场节点的列表是正确的。  如果节点是在 AD FS 服务器场列表中，但不能再 active 修补程序验证会失败。  这可以通过运行问题 `remove-adfsnode <node name >`
2.  验证场中的所有节点上部署修补程序
3.  请验证传递给 cmdlet 的凭据有权修改 ad fs 项目数据库架构的所有者。  

### <a name="logging--auditing"></a>日志记录/审核
AD FS 时的身份验证请求被拒绝，因为当超过锁定阈值，将编写`ExtranetLockoutEvent`到安全审核流。  

示例事件：

Extranet 锁定事件发生。 失败详细信息，请参阅 XML。 

**活动 ID:172332e1-1301-4e56-0e00-0080000000db**

```
Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>TQDFTD\Administrator</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Intranet</NetworkLocation>
      <IpAddress>4.4.4.4</IpAddress>
      <ForwardedIpAddress />
      <ProxyIpAddress>1.2.3.4</ProxyIpAddress>
      <NetworkIpAddress>1.2.3.4</NetworkIpAddress>
      <ProxyServer>N/A</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>02/07/2018 21:47:44</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>

```

## <a name="banned-ip-addresses"></a>被阻止的 IP 地址
AD FS 2018 年 6 月更新除 extranet 的智能锁定功能，使您能够在 AD FS 中，全局配置一组 IP 地址，以便针对来自这些 IP 地址，或该请求中有这些 IP 地址**x-转发-对于**或**x-ms-转发的客户端的 ip**标头，将阻止由 AD FS。

##### <a name="adding-banned-ips"></a>添加禁止的 Ip
若要将被阻止的 Ip 添加到全局列表，请使用以下 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

允许的格式

1.  IPv4
2.  IPv6
3.  使用 IPv4 或 v6 的 CIDR 格式
4.  使用 IPv4 或 v6 的 IP 范围 (即 1.2.3.4-1.2.3.6)

#### <a name="removing-banned-ips"></a>删除禁止的 Ip
若要从全局列表中删除被阻止的 Ip，请使用以下 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>读取禁止的 Ip
若要读取当前集的被阻止的 IP 地址，使用以下 Powershell cmdlet:

``` powershell
PS C:\ >Get-AdfsProperties 
```

输出示例：

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>其他参考  
[保护 Active Directory 联合身份验证服务的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

    
