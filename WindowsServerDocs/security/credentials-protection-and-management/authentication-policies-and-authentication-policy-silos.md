---
title: 身份验证策略和身份验证策略接收器
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 460837c79c0e0d2c48331ddaaffcd118fd16ebc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870608"
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>身份验证策略和身份验证策略接收器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

面向 IT 专业人士的本主题介绍身份验证策略接收器以及可以限制这些接收器的帐户的策略。 还将介绍如何使用身份验证策略限制帐户的作用域。

身份验证策略接收器和相关策略向系统提供一种用于包含较高权限凭据的方法，这些系统仅与选定用户、计算机或服务相关。 通过使用 Active Directory 管理中心和 Active Directory Windows PowerShell cmdlet，可以在 Active Directory 域服务 (AD DS) 中定义和管理接收器。

身份验证策略接收器是指管理员可向其分配用户帐户、计算机帐户和服务帐户的容器。 然后，可以通过已应用到该容器的身份验证策略来管理帐户集。 这使管理员无需频繁跟踪个人帐户对资源的访问权限，并且有助于防止恶意用户通过盗窃凭据访问其他资源。

Windows Server 2012 R2 中引入的功能，可创建身份验证策略接收器，可托管一组高权限用户。 然后可以为此容器分配身份验证策略，以限制在域中哪些位置可使用有权限的帐户。 当帐户位于受保护的用户安全组时，将应用其他控制，例如独占使用 Kerberos 协议。

使用这些功能，你可以为高价值主机限制高价值帐户的使用。 例如，你可以创建新的林管理员接收器，它包含企业管理员、架构管理员以及域管理员。 然后可以使用身份验证策略配置该接收器，使得来自域控制器和域管理员控制台之外系统的基于密码和智能卡的身份验证都会失败。

有关配置身份验证策略接收器和身份验证策略的信息，请参阅 [How to Configure Protected Accounts](how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policy-silos"></a>关于身份验证策略接收器
身份验证策略接收器将控制哪些帐户通过接收器进行限制，并定义了要应用于成员的身份验证策略。 你可以根据自己组织的要求来创建接收器。 接收器是用于用户、计算机和服务的 Active Directory 对象，如下表中的架构定义。

**身份验证策略接收器的 active Directory 架构**

|显示名称|描述|
|--------|--------|
|身份验证策略接收器|此类的实例为已分配的用户、计算机和服务定义身份验证策略及相关行为。|
|身份验证策略接收器|此类的容器可以包含身份验证策略接收器对象。|
|强制执行的身份验证策略接收器|指定是否强制执行身份验证策略接收器。<br /><br />如果不强制执行，则策略在默认情况下处于审核模式。 将生成可指示潜在成功和失败的事件，但不会将保护应用到系统。|
|已分配的身份验证策略接收器反向链接|此属性为 msDS-AssignedAuthNPolicySilo 的反向链接。|
|身份验证策略接收器成员|指定将哪些主体分配到 AuthNPolicySilo。|
|身份验证策略接收器成员反向链接|此属性为 msDS-AuthNPolicySiloMembers 的反向链接。|

通过使用 Active Directory 管理控制台或 Windows PowerShell，可以配置身份验证策略接收器。 有关详细信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policies"></a>关于身份验证策略
身份验证策略为帐户类型定义 Kerberos 协议票证授予票证 (TGT) 生存期属性和身份验证访问控制条件。 此策略在称为身份验证策略接收器的 AD DS 容器上构建，并对该容器进行控制。

身份验证策略控制以下方面：

-   帐户的 TGT 生存期（它设置为不可续订）。

-   设备帐户在使用密码或证书进行登录时需要满足的条件。

-   在对作为帐户一部分运行的服务进行身份验证时，用户和设备需要满足的条件。

Active Directory 帐户类型确定调用方的角色为以下值之一：

-   **用户**

    用户应当始终是受保护的用户安全组的成员，默认情况下，该组将拒绝使用 NTLM 尝试进行身份验证。

    可以配置策略以将用户帐户的 TGT 生存期设置为较短的值，或限制用户帐户可以登录的设备。 可在身份验证策略中配置丰富的表达式，以控制用户及其设备对服务进行身份验证时需要满足的条件。

    有关详细信息，请参阅[受保护的用户安全组](protected-users-security-group.md)。

-   **服务**

    使用独立托管的服务帐户、组托管的服务帐户，或者从这两种类型的服务帐户中派生的自定义帐户对象。 策略可以设置设备的访问控制条件，用于限制对使用 Active Directory 标识的特定设备的托管的服务帐户凭据。 服务不应是受保护的用户安全组的成员，因为这样所有传入的身份验证都将失败。

-   **计算机**

    使用计算机帐户对象或从该计算机帐户对象派生的自定义帐户对象。 根据用户和设备属性，策略可以设置对帐户进行身份验证时所需的访问控制条件。 计算机不应是受保护的用户安全组的成员，因为这样所有传入的身份验证将失败。 默认情况下，会拒绝对使用 NTLM 身份验证的尝试。 不应为计算机帐户配置 TGT 生存期。

> [!NOTE]
> 无需将策略与身份验证策略接收器相关联，即可在一组帐户上设置该策略。 当具有一个要保护的帐户时，可使用此策略。

**身份验证策略的 active Directory 架构**

用于用户、计算机和服务的 Active Directory 对象的策略由下表中的架构定义。

|在任务栏的搜索框中键入|显示名称|描述|
|----|--------|--------|
|策略|身份验证策略|此类的实例为已分配的主体定义身份验证策略行为。|
|策略|身份验证策略|此类的容器可包含身份验证策略对象。|
|策略|强制执行的身份验证策略|指定是否强制执行身份验证策略。<br /><br />若未强制执行，该策略在默认情况下将处于审核模式，并且将生成可指示潜在成功和失败的事件，但未将保护应用到系统。|
|策略|已分配的身份验证策略反向链接|此属性为 msDS-AssignedAuthNPolicy 的反向链接。|
|策略|已分配的身份验证策略|指定应将哪些 AuthNPolicy 应用到此主体。|
|“用户”|用户身份验证策略|指定应将哪些 AuthNPolicy 应用到分配给此接收器对象的用户。|
|“用户”|用户身份验证策略反向链接|此属性为 msDS-UserAuthNPolicy 的反向链接。|
|“用户”|ms-DS-User-Allowed-To-Authenticate-To|此属性用于确定允许对用户帐户下运行的服务进行身份验证的主体集。|
|“用户”|ms-DS-User-Allowed-To-Authenticate-From|此属性用于确定用户帐户有权登录的设备组。|
|“用户”|用户 TGT 生存期|指定分配给用户的 Kerberos TGT 的最长存在时间（用秒数表示）。 不可续订得出的 TGT。|
|计算机|计算机身份验证策略|指定应将哪些 AuthNPolicy 应用到分配给此接收器对象的计算机。|
|计算机|计算机身份验证策略反向链接|此属性是 msDS ComputerAuthNPolicy 的反向链接。|
|计算机|ms-DS-Computer-Allowed-To-Authenticate-To|此属性用于确定允许对计算机帐户下运行的服务进行身份验证的主体集。|
|计算机|计算机 TGT 生存期|指定分配给计算机的 Kerberos TGT 的最长存在时间（用秒数表示）。 建议不要更改此设置。|
|服务|服务身份验证策略|指定应将哪个 AuthNPolicy 应用到分配给此接收器对象的服务。|
|服务|服务身份验证策略反向链接|此属性是 msDS-ServiceAuthNPolicy 的反向链接。|
|服务|ms-DS-Service-Allowed-To-Authenticate-To|此属性用于确定允许对服务帐户下运行的服务进行身份验证的主体集。|
|服务|ms-DS-Service-Allowed-To-Authenticate-From|此属性用于确定服务帐户有权登录的设备组。|
|服务|服务 TGT 生存期|指定分配给服务的 Kerberos TGT 的最长存在时间（用秒数表示）。|

通过使用 Active Directory 管理控制台或 Windows PowerShell，可以为每个接收器配置身份验证策略。 有关详细信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

## <a name="how-it-works"></a>工作原理
本部分介绍身份验证策略接收器和身份验证策略如何与受保护的用户安全组协同工作，以及如何在 Windows 中实现 Kerberos 协议。

-   [如何使用 Kerberos 协议与身份验证接收器和策略](#BKMK_HowKerbUsed)

-   [如何限制用户登录的工作原理](#BKMK_HowRestrictingSignOn)

-   [限制服务票证颁发的工作原理](#BKMK_HowRestrictingServiceTicket)

**受保护的帐户**

受保护用户安全组将触发不可配置的保护设备和主机计算机运行 Windows Server 2012 R2 和 Windows 8.1 上以及在主域控制器运行 Windows Server 2012 R2 的域中域控制器上。 由于 Windows 所支持的身份验证方法已更改，因此将根据帐户的域功能级别，对受保护用户安全组的成员提供进一步保护。

-   受保护的用户安全组的成员无法通过使用 NTLM、摘要式身份验证或 CredSSP 默认凭据委派进行身份验证。 运行使用这些安全支持提供程序 (Ssp) 之一的 Windows 8.1 的设备上, 对域进行身份验证将失败时的帐户是受保护用户安全组的成员。

-   Kerberos 协议不会在预身份验证过程中使用较弱的 DES 或 RC4 加密类型。 这意味着，必须将域配置为至少支持 AES 加密类型。

-   用户的帐户不能被委派了约束或不受约束的 Kerberos 委派。 则意味着，若用户为受保护用户安全组的成员，之前与其他系统的连接可能失败。

-   通过使用身份验证策略和接收器，可以对默认设置为 4 小时的 Kerberos TGT 生存期进行配置，并且可通过 Active Directory 管理中心对其进行访问。 这意味着超过 4 小时后，用户必须重新进行身份验证。

有关该组安全的详细信息，请参阅 [How the Protected Users group works](protected-users-security-group.md#BKMK_HowItWorks)。

**接收器和身份验证策略**

身份验证策略接收器和身份验证策略将利用现有的 Windows 身份验证基础结构。 拒绝使用 NTLM 协议，而使用具有较新加密类型的 Kerberos 协议。 除了为服务和计算机的帐户提供限制，身份验证策略还通过提供一种将可配置限制应用到帐户的方法来补充受保护用户安全组。 在 Kerberos 协议身份验证服务 (AS) 或票证授予服务 (TGS) 交换期间，将强制执行身份验证策略。 有关 Windows 如何使用 Kerberos 协议，以及已进行哪些更改来支持身份验证策略接收器和身份验证策略的详细信息，请参阅：

-   [Kerberos 版本 5 身份验证协议的工作原理](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Kerberos 身份验证中的更改](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) （Windows Server 2008 R2 和 Windows 7）

### <a name="BKMK_HowKerbUsed"></a>如何使用 Kerberos 协议与身份验证策略接收器和策略
在域帐户可链接到身份验证策略接收器且用户进行登录时，安全帐户管理器将添加身份验证策略接收器的声明类型（将接收器作为值包含在其中）。 帐户上的此声明提供对目标接收器的访问权限。

如果强制执行身份验证策略，并且在域控制器上收到对域帐户的身份验证服务请求，则域控制器将返回已配置生存期的不可续订的 TGT（除非域 TGT 生存期较短）。

> [!NOTE]
> 域帐户必须具有已配置的 TGT 生存期，还必须可直接链接到该策略，或者可以通过接收器成员身份间接链接到该策略。

当身份验证策略处于审核模式，且在域控制器上收到对域帐户的身份验证服务请求时，域控制器将检查设备是否允许进行身份验证，以便在出现故障时记录警告。 已审核的身份验证策略不会更改此进程，因此如果身份验证请求不满足此策略的要求，这些请求不会失败。

> [!NOTE]
> 域帐户必须可直接链接到该策略，或者可以通过接收器成员身份间接链接到该策略。

如果强制执行身份验证策略而身份验证服务受到保护，并且域控制器上收到对域帐户的身份验证服务请求，域控制器将检查设备中是否允许进行身份验证。 若失败，则域控制器将返回一条错误消息并记录事件。

> [!NOTE]
> 域帐户必须可直接链接到该策略，或者可以通过接收器成员身份间接链接到该策略。

域控制器时身份验证策略处于审核模式和域帐户的域控制器收到票证授予服务请求，检查身份验证是否允许基于请求的票证特权属性证书(PAC) 数据，并出现故障时记录一条警告消息。 PAC 包含各种类型的授权数据，其中包括用户所属的组、用户所具有的权限以及适用于用户的策略类型。 此信息用于生成用户的访问令牌。 如果它是强制执行身份验证策略，它允许对用户、 设备或服务进行身份验证，如果允许身份验证的域控制器检查基于请求的票证 PAC 数据。 若失败，则域控制器将返回一条错误消息并记录事件。

> [!NOTE]
> 域帐户必须可直接链接到已审核的身份验证策略，或者可以通过接收器成员身份间接链接，该策略允许对用户、设备或服务进行身份验证。

你可以对接收器的所有成员使用一个身份验证策略，或者对用户、计算机和托管服务帐户使用单独的策略。

通过使用 Active Directory 管理控制台或 Windows PowerShell，可以为每个接收器配置身份验证策略。 有关详细信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

### <a name="BKMK_HowRestrictingSignOn"></a>如何限制用户登录的工作原理
由于这些身份验证策略可应用到帐户，因此它还将应用于服务使用的帐户。 若要对特定主机限制服务密码的使用情况，则该设置十分有用。 例如，在允许主机从 Active Directory 域服务检索密码的情况下，将配置组托管服务帐户。 但是，可从任意主机中使用该密码进行初始身份验证。 通过应用访问控制条件，可借助将密码限制到可检索密码的主机集来以实现另一层保护。

当以系统、 网络服务或其他本地服务的身份运行的服务连接到网络服务时，它们使用主机的计算机帐户。 计算机帐户不能受限制。 因此，即使服务使用不适用于 Windows 主机的计算机帐户，也无法对其进行限制。

限制用户登录到特定主机需要域控制器验证主机的标识。 当通过 Kerberos 保护（是动态访问控制的一部分）使用 Kerberos 身份验证时，密钥发行中心将与用户从中进行验证的主机的 TGT 一起提供。 这个受保护的 TGT 的内容用于完成访问检查，以确定是否允许该主机进行访问。

当用户登录 Windows 或者根据应用程序的凭据提示输入其域凭据时，默认情况下，Windows 将向域控制器发送未受保护的 AS-REQ。 如果用户从不支持保护的计算机（例如运行 Windows 7 或 Windows Vista 的计算机）发送请求，则请求失败。

以下列表介绍了过程：

-   运行 Windows Server 2012 R2 的域中的域控制器查询的用户帐户，并确定的身份验证策略可限制需要受保护的请求的初始身份验证进行配置它。

-   该域控制器将导致请求失败。

-   由于保护是必需的用户可以尝试使用运行 Windows 8.1 或 Windows 8，可支持 Kerberos 保护重试尝试登录过程的计算机登录。

-   Windows 检测到该域支持 Kerberos 保护，并发送受保护的 AS-REQ 以重试尝试登录请求。

-   域控制器通过使用用于保护该请求的 TGT 中已配置的访问控制条件和客户端操作系统的标识信息执行访问检查。

-   若访问检查失败，则域控制器将拒绝该请求。

即使在操作系统支持 Kerberos 保护时，也可以应用访问控制要求，并且必须在授予访问权限之前满足这些要求。 用户登录到 Windows，或者在应用程序的凭据提示下输入其域凭据。 默认情况下，Windows 会将未保护的 AS-REQ 发送到域控制器。 如果用户从支持保护，例如 Windows 8.1 或 Windows 8 的计算机发送请求身份验证策略评估，如下所示：

1.  运行 Windows Server 2012 R2 的域中的域控制器查询的用户帐户，并确定的身份验证策略可限制需要受保护的请求的初始身份验证进行配置它。

2.  域控制器通过使用用于保护该请求的 TGT 中已配置的访问控制条件和系统的标识信息执行访问检查。 访问检查成功。

    > [!NOTE]
    > 如果配置了传统的工作组限制，则还需要满足此类限制。

3.  域控制器使用受保护的回复 (AS-REP) 进行回复，并且身份验证将继续进行。

### <a name="BKMK_HowRestrictingServiceTicket"></a>限制服务票证颁发的工作原理
当不允许帐户并且具有 TGT 的用户尝试连接到服务 (例如，通过打开需要对服务的服务主体名称 (SPN) 标识的服务进行身份验证的应用程序，将按以下顺序发生：

1.  为了从 SPN 连接到 SPN1，Windows 将向请求到 SPN1 的服务票证的域控制器发送 TGS-REQ。

2.  运行 Windows Server 2012 R2 的域中的域控制器查找 spn1 以查找服务的 Active Directory 域服务帐户，并确定该帐户限制服务票证颁发的身份验证策略的配置。

3.  域控制器通过使用 TGT 中已配置的访问控制条件和用户的标识信息执行访问检查 访问检查失败。

4.  域控制器拒绝该请求。

因为帐户满足身份验证策略设置的访问控制条件，并且具有 TGT 的用户尝试连接到该服务帐户允许的时间 (例如通过打开应用程序需要对服务进行身份验证，它是由标识服务的 SPN），将按以下顺序发生：

1.  为了连接到 SPN1，Windows 将向请求到 SPN1 的服务票证的域控制器发送 TGS-REQ。

2.  运行 Windows Server 2012 R2 的域中的域控制器查找 spn1 以查找服务的 Active Directory 域服务帐户，并确定该帐户限制服务票证颁发的身份验证策略的配置。

3.  域控制器通过使用 TGT 中已配置的访问控制条件和用户的标识信息执行访问检查 访问检查成功。

4.  域控制器将使用票证授予服务回复 (TGS-REP) 来回复请求。

## <a name="BKMK_ErrorandEvents"></a>相关联的错误消息和信息性事件消息
下表描述了一些事件，它们与受保护用户安全组和应用到身份验证策略接收器的身份验证策略相关联。

这些事件记录在位于“Microsoft\Windows\Authentication” 的应用程序和服务日志中。

有关使用这些事件的排除故障步骤，请参阅 [Troubleshoot Authentication Policies](how-to-configure-protected-accounts.md#BKMK_TroubleshootAuthnPolicies) 和 [Troubleshoot events related to Protected Users](how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents)。

|事件 ID 和日志|描述|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController**|原因：NTLM 登录出现故障，因为已配置身份验证策略。<br /><br />在域控制器中记录了一个事件，以指示 NTLM 身份验证失败，原因在于需要访问控制限制，但无法将这些限制应用到 NTLM。<br /><br />显示帐户、设备、策略和接收器的名称。|
|105<br /><br />**AuthenticationPolicyFailures-DomainController**|原因：Kerberos 限制出现故障，原因在于未允许来自特定设备的身份验证。<br /><br />在域控制器中记录了一个事件，以指示 Kerberos TGT 受到拒绝，原因在于设备不满足强制执行的访问控制限制。<br /><br />显示帐户、设备、策略和接收器的名称以及 TGT 生存期。|
|305<br /><br />**AuthenticationPolicyFailures-DomainController**|原因：Kerberos 限制可能出现故障，原因在于未允许来自特定设备的身份验证。<br /><br />在审核模式下，信息事件将记录在域控制器中，以确定是否会因为设备不满足访问控制限制而拒绝 Kerberos TGT。<br /><br />显示帐户、设备、策略和接收器的名称以及 TGT 生存期。|
|106<br /><br />**AuthenticationPolicyFailures-DomainController**|原因：Kerberos 限制出现故障，原因在于未允许用户或设备对服务器进行身份验证。<br /><br />在域控制器中记录了一个事件，以指示 Kerberos 服务票证受到拒绝，原因在于用户和/或设备不满足强制执行的访问控制限制。<br /><br />显示设备、策略和接收器的名称。|
|306<br /><br />**AuthenticationPolicyFailures-DomainController**|原因：Kerberos 限制可能会出现故障，原因在于未允许用户或设备对服务器进行身份验证。<br /><br />在审核模式下，信息事件将记录在域控制器中，以指示 Kerberos 服务票证将被拒绝，原因在于用户和/或设备不满足访问控制限制。<br /><br />显示设备、策略和接收器的名称。|

## <a name="see-also"></a>请参阅
[如何配置受保护的帐户](how-to-configure-protected-accounts.md)

[凭据保护和管理](credentials-protection-and-management.md)

[受保护的用户安全组](protected-users-security-group.md)


