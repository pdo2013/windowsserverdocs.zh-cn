---
title: "身份验证的策略和身份验证策略思"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>身份验证的策略和身份验证策略思

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员介绍了身份验证策略思和可以限制对这些思的帐户的策略。 它还说明了如何使用身份验证的策略以帐户的限制。

身份验证策略思和附带的策略提供了一种包含高权限凭据系统的仅相关到所选的用户、 计算机或服务的方式。 可以定义思，并使用 Active Directory 管理中心和 Active Directory 的 Windows PowerShell cmdlet 管理 Active Directory 域服务 (广告 DS) 中。

身份验证策略思是的容器管理员可以向其分配用户帐户、 计算机帐户和服务的帐户。 然后可以由该容器到应用了身份验证策略管理的帐户设置。 这减少了管理员联系以用于个人帐户，资源跟踪访问，并且可以帮助防止恶意用户访问其他资源的凭据被盗。

引入 Windows Server 2012 R2 的功能，可用于创建策略群组举办一组高权限的用户的身份验证。 然后，可以将此容器身份验证策略分配到其中在域中使用特权的帐户的限制。 保护用户安全组帐户时，将应用其他控件，如独占使用 Kerberos 协议。

使用这些功能，你可以通过限制对高值主机高值帐户使用量。 例如，你可以创建包含企业版、 架构和域管理员新林管理员思。 然后你可以使用身份验证策略配置思以便无法密码和智能卡基于从系统域控制器和域管理员主机以外的身份验证。

有关配置身份验证策略思和策略身份验证信息，请参阅[如何配置保护帐户](how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policy-silos"></a>有关身份验证策略思
身份验证策略思控件可通过思限制哪些帐户，并定义身份验证策略适用于的成员。 你可以创建思根据你的组织的要求。 思有的用户、 计算机和服务的方案下表中定义的 Active Directory 对象。

**身份验证策略思 active Directory 方案**

|显示名称|描述|
|--------|--------|
|身份验证策略思|此类实例定义身份验证的策略和分配的用户、 计算机和服务的相关的行为。|
|身份验证策略思|此类容器可能包含身份验证策略思对象。|
|身份验证策略思强制执行|指定是否强制身份验证策略思。<br /><br />当不强制，默认情况下策略是在审核模式。 生成事件指示潜在成功和失败，但不是会保护措施应用到系统。|
|分配的身份验证策略思下列|这是属性 msDS AssignedAuthNPolicySilo 的后台链接。|
|身份验证策略思成员|指定哪些主体赋予 AuthNPolicySilo。|
|身份验证策略思成员下列|这是属性 msDS AuthNPolicySiloMembers 的后台链接。|

身份验证策略思可以通过使用 Active Directory 管理控制台或 Windows PowerShell 配置。 有关详细信息，请参阅[如何配置保护帐户](how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policies"></a>有关身份验证策略
身份验证策略定义 Kerberos 协议票证许可票证 (TGT) 生命周期内属性和身份验证访问控制条件帐户类型。 该策略内置，并控制称为身份验证策略思广告 DS 容器。

身份验证策略控制如下：

-   帐户已设置为可非续订 TGT 寿命。

-   设备帐户需要满足使用密码或证书登录条件。

-   用户和设备需要满足进行身份验证服务运行该帐户的一部分的条件。

Active Directory 的帐户类型决定为以下一项的调用方的角色：

-   **用户**

    用户始终应该组中的受保护的用户安全，默认情况下拒绝尝试使用 NTLM 身份验证的成员。

    可以配置策略设置较短的值的用户帐户 TGT 整个使用期限内或用户帐户可以登录到的设备的限制。 可以在身份验证策略控制的用户和他们的设备需要满足进行身份验证服务的条件配置丰富表情。

    有关详细信息，请参阅[保护用户安全组](protected-users-security-group.md)。

-   **服务**

    独立托管服务帐户、 组托管服务帐户、 或来自使用帐户的服务这两种类型的自定义帐户对象。 策略可以控制条件，用于 Active Directory 身份与特定设备限制托管的服务帐户凭据设置设备的访问权限。 因为将无法传入的所有身份验证服务绝不应保护用户安全组中的成员。

-   **计算机**

    使用计算机帐户对象或从计算机帐户对象派生自定义帐户对象。 策略可以设置访问所需以允许对基于用户和设备属性帐户的身份验证的控制条件。 因为将无法传入的身份验证所有计算机绝不应保护用户安全组中的成员。 默认情况下，尝试使用 NTLM 身份验证被拒绝。 不应将 TGT 生存期配置计算机帐户。

> [!NOTE]
> 它是可以对帐户的一组设置身份验证策略，而无需将到身份验证策略思策略。 当你拥有一个帐户保护，你可以使用此策略。

**身份验证策略 active Directory 架构**

由方案下表中定义的用户、 计算机和服务的 Active Directory 对象的策略。

|键入|显示名称|描述|
|----|--------|--------|
|策略|身份验证策略|此类实例定义分配主体身份验证策略行为。|
|策略|身份验证策略|此类容器可能包含身份验证策略对象。|
|策略|身份验证实施的策略|指定是否强制身份验证的策略。<br /><br />当不强制，默认情况下策略在审核模式中，并且生成表示潜在成功并失败的事件，但保护不适用于系统。|
|策略|分配的身份验证策略下列|这是属性 msDS AssignedAuthNPolicy 的后台链接。|
|策略|分配的身份验证策略|这应该 AuthNPolicy 指定此主体到应用。|
|用户|用户身份验证策略|这应该 AuthNPolicy 指定适用于赋予此思对象的用户。|
|用户|用户身份验证策略下列|这是属性 msDS UserAuthNPolicy 的后台链接。|
|用户|ms-DS-User-Allowed-To-Authenticate-To|此特性用于确定的一套允许进行身份验证服务用户帐户运行的原则。|
|用户|ms-DS-User-Allowed-To-Authenticate-From|此特性用于确定的设备的用户帐户有权登录的组。|
|用户|用户 TGT 生命周期内|指定 Kerberos TGT 颁发给用户 （表达秒后） 的最长时间。 结果 Tgt 可以非续订。|
|计算机|计算机身份验证策略|这应该 AuthNPolicy 指定适用于赋予此思对象的计算机。|
|计算机|计算机身份验证策略下列|这是属性 msDS ComputerAuthNPolicy 的后台链接。|
|计算机|ms-DS-Computer-Allowed-To-Authenticate-To|此特性用于确定的一套允许通过的身份验证服务计算机帐户运行的原则。|
|计算机|计算机 TGT 生命周期内|指定 Kerberos TGT 颁发给 （表达秒后） 的计算机的最长时间。 不建议若要更改此设置。|
|服务|身份验证服务的策略|这应该 AuthNPolicy 指定应用于赋予此思对象的服务。|
|服务|服务身份验证策略下列|这是属性 msDS ServiceAuthNPolicy 的后台链接。|
|服务|ms-DS-Service-Allowed-To-Authenticate-To|此特性用于确定的一套允许通过的身份验证服务帐户下运行的服务的原则。|
|服务|ms-DS-Service-Allowed-To-Authenticate-From|此特性用于确定的设备的服务帐户有权登录的组。|
|服务|服务 TGT 生存期|指定 Kerberos TGT 颁发给服务 （表达秒后） 的最长时间。|

可以通过使用 Active Directory 管理控制台或 Windows PowerShell 为每个思配置身份验证的策略。 有关详细信息，请参阅[如何配置保护帐户](how-to-configure-protected-accounts.md)。

## <a name="how-it-works"></a>它的工作原理
此部分中介绍了身份验证策略思和认证策略结合的受保护的用户安全组和实现 Kerberos 协议 Windows 中的工作方式。

-   [如何与身份验证思和策略使用 Kerberos 协议](#BKMK_HowKerbUsed)

-   [如何限制用户登录的工作原理](#BKMK_HowRestrictingSignOn)

-   [限制服务票证颁发的工作原理](#BKMK_HowRestrictingServiceTicket)

**受保护的帐户**

保护用户安全组触发非配置保护和域中与运行 Windows Server 2012 R2 的主域控制器的域控制器上的设备和主计算机运行的 Windows Server 2012 R2 和 Windows 8.1。 具体取决于域功能级别的帐户，进一步受保护的用户安全组成员保护因为更改在 Windows 中支持的身份验证方法。

-   组成员的保护用户安全不能使用 NTLM、 简要身份验证或 CredSSP 默认凭据委派身份验证。 在设备上运行的 Windows 8.1，将使用这些安全支持提供商 (Ssp) 的任何一个，受保护的用户安全组成员的帐户时，将无法到某个域身份验证。

-   Kerberos 协议将不预身份验证过程中使用较弱 DES 或 RC4 加密类型。 这意味着必须配置域支持至少 AES 加密类型。

-   用户帐户不能委派与 Kerberos 受限或不受约束委派。 这意味着前者连接到其他系统可能会失败，用户是否保护用户安全组中的成员。

-   通过使用身份验证的策略和思，可以通过 Active Directory 管理中心访问是配置的默认 Kerberos Tgt 生命周期内设置的 4 个小时。 这意味着，过后四个小时，用户必须进行身份验证再次。

有关该组安全的详细信息，请参阅[保护用户如何组适用](protected-users-security-group.md#BKMK_HowItWorks)。

**思和身份验证策略**

身份验证策略思和身份验证策略利用现有 Windows 身份验证的基础结构。 使用 NTLM 协议被拒绝，并使用与更新的加密类型 Kerberos 协议。 身份验证策略补充保护用户安全组，通过提供适用于帐户，除了提供的服务和计算机的帐户的限制的可配置限制的方法。 在 Kerberos 协议身份验证服务 (AS) 或票证许可服务 (TGS) exchange 强制身份验证的策略。 有关如何 Windows 使用 Kerberos 协议，并做了哪些更改来支持身份验证策略思和策略身份验证的详细信息，请参阅：

-   [版本 Kerberos 5 身份验证协议的工作原理](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [在 Kerberos 身份验证更改](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx)（Windows Server 2008 R2 和 Windows 7）

### <a name="BKMK_HowKerbUsed"></a>如何与身份验证策略思和策略使用 Kerberos 协议
当域帐户链接到身份验证策略思，并且用户在登录时，安全帐户管理器添加了身份验证策略思包含值为思的索赔类型。 此声明的帐户将提供有针对性的思访问。

当域帐户的身份验证服务请求收到该域控制器上强制执行身份验证策略，域控制器将返回配置整个使用期限内与非续订 TGT （除非较短时间为域 TGT 整个使用期限内）。

> [!NOTE]
> 域帐户必须具有配置的 TGT 生命周期内，并且必须直接策略链接或通过思会员间接链接。

当身份验证策略审核模式中，并且在该域控制器上收到域帐户的身份验证服务请求时，域控制器检查是否身份验证允许设备，以便它可以记录警告，如果故障。 审核身份验证策略不改变过程，因此如果它们不能满足该策略要求，不会失败身份验证请求。

> [!NOTE]
> 必须直接将其链接到策略或通过思会员间接链接域帐户。

在执行身份验证策略，身份验证服务 armored 域帐户的身份验证服务请求收到该域控制器上时，域控制器检查，是否设备允许身份验证。 如果无法正常工作，域控制器返回一条错误消息，并将记录事件。

> [!NOTE]
> 必须直接将其链接到策略或通过思会员间接链接域帐户。

当身份验证策略审核模式中，并且对于域帐户域控制器收到票证许可服务请求时，域控制器检查是否基于申请票证特权特性证书 (PAC) 数据，允许身份验证它失败时，它日志一条警告消息。 PAC 包含各种类型的授权数据，包括组的用户是用户具有的权利的成员和内容策略适用于用户。 此信息用于生成用户访问标记。 如果是这使用户、 设备或服务访问身份验证强制身份验证策略，域控制器检查，如果允许身份验证，则将基于申请票证 PAC 数据。 如果无法正常工作，域控制器返回一条错误消息，并将记录事件。

> [!NOTE]
> 域帐户必须直接链接或通过思成员审核身份验证策略，这使用户、 设备或服务，身份验证链接

你可以使用单个身份验证策略为思的所有成员也可以为用户的计算机，，托管的服务帐户使用单独的策略。

可以通过使用 Active Directory 管理控制台或 Windows PowerShell 为每个思配置身份验证的策略。 有关详细信息，请参阅[如何配置保护帐户](how-to-configure-protected-accounts.md)。

### <a name="BKMK_HowRestrictingSignOn"></a>如何限制用户登录的工作原理
因为这些身份验证策略应用到帐户，它也适用于由服务使用的帐户。 如果你想要限制主机特定于服务的密码的使用量，此设置很有用。 例如，组托管的服务帐户被配置允许主机检索从 Active Directory 域服务的密码。 但是，该密码可用于从任何主机初始身份验证。 通过应用访问控制情况，可以通过限制为仅一组可以检索密码的主机密码实现更多一层保护。

作为系统、 网络服务或其他服务本地身份运行的服务的连接到网络服务，当他们使用主计算机帐户。 计算机帐户能受限。 因此即使不适用于 Windows 主计算机帐户使用该服务，不能受限。

限制用户登录特定主机要求验证身份的主机域控制器。 当与 Kerberos 程度 （即动态访问控制的一部分） 中使用 Kerberos 身份验证，从中用户身份验证的主机 TGT 提供密钥 Distribution 中心。 完成以确定是否允许主机访问检查用于此 armored TGT 的内容。

当用户登录到 Windows，或将其域凭据凭据提示中输入应用程序，默认情况下时，Windows 会向域控制器发送 unarmored 的 AS 要求。 如果用户的计算机不支持从发送该请求保护作用计算机运行的 Windows 7 或 Windows Vista 中，如请求无法正常工作。

下面介绍了过程：

-   域控制器在运行 Windows Server 2012 R2 域中的查询的用户帐户，并确定是否它通过限制需要 armored 的请求最初身份验证身份验证策略配置。

-   请求，域控制器将失败。

-   由于程度需，用户可以尝试使用一台运行 Windows 8.1 或 Windows 8，支持 Kerberos 程度尝试的登录过程已启用计算机登录。

-   Windows 检测到域支持 Kerberos 程度，并发送 armored 的 AS-必需重试登录请求。

-   域控制器执行访问检查，使用中用于 armor 请求 TGT 配置的访问控制条件和客户端操作系统身份信息。

-   如果无法访问检查，域控制器拒绝该请求。

即使在操作系统支持 Kerberos 程度，访问控制要求可以应用，而被授予访问权限，必须满足。 用户在登录到 Windows 或其域凭据输入凭据提示符应用程序。 默认情况下，Windows 将 unarmored 的 AS 必需发送到域控制器。 如果用户发送该请求从计算机中支持的程度，如 Windows 8.1 或 Windows 8 认证策略计算如下：

1.  域控制器在运行 Windows Server 2012 R2 域中的查询的用户帐户，并确定是否它通过限制需要 armored 的请求最初身份验证身份验证策略配置。

2.  使用中用于 armor 请求 TGT 配置的访问控制条件和系统的身份的信息，域控制器执行访问检查。 访问检查成功。

    > [!NOTE]
    > 如果配置传统工作组限制，这些还需要满足。

3.  域控制器回复 armored 回复 （AS 销售代表），并将继续进行身份验证。

### <a name="BKMK_HowRestrictingServiceTicket"></a>限制服务票证颁发的工作原理
当某个帐户被禁止并具有 TGT 的用户尝试连接到该服务 (例如，方法是打开的应用程序需要身份验证服务是由该服务服务主要名称 (SPN)，按以下顺序发生：

1.  在尝试连接到 SPN1 SPN 从 Windows 将 TGS 必需发送到请求服务票证 SPN1 域控制器。

2.  在运行 Windows Server 2012 R2 域控制器查找 SPN1 Active Directory 域服务帐户查找该服务，并确定该帐户，通过限制服务票证颁发身份验证策略配置。

3.  域控制器执行访问检查使用中 TGT 配置的访问控制条件和用户的身份的信息 访问检查无法正常工作。

4.  域控制器拒绝该请求。

当允许帐户，因为该帐户符合身份验证的策略设置访问控制条件，并且具有 TGT 的用户尝试连接到该服务 (如需要身份验证方法是打开应用程序到该服务 SPN 标识服务)，按照以下顺序发生：

1.  在尝试连接到 SPN1，Windows 将 TGS 必需发送到请求服务票证 SPN1 域控制器。

2.  在运行 Windows Server 2012 R2 域控制器查找 SPN1 Active Directory 域服务帐户查找该服务，并确定该帐户，通过限制服务票证颁发身份验证策略配置。

3.  域控制器执行访问检查使用中 TGT 配置的访问控制条件和用户的身份的信息 访问检查成功。

4.  域控制器回复票证许可的服务回复 （TGS 代表） 与该请求。

## <a name="BKMK_ErrorandEvents"></a>关联的错误和信息事件消息
下表介绍了与用户保护安全组相关联的事件以及引用了身份验证策略思认证策略。

在应用程序和服务日志以记录事件**Microsoft\Windows\Authentication**。

有关疑难解答步骤，使用这些事件，请参阅[疑难解答身份验证策略](how-to-configure-protected-accounts.md#BKMK_TroubleshootAuthnPolicies)和[疑难解答相关受保护的用户的事件](how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents)。

|事件 ID，并登录|描述|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures 域控制器**|原因： NTLM 登录失败出现了身份验证策略配置原因。<br /><br />一场活动已登录的域控制器，以指示 NTLM 身份验证失败，因为需要，访问控制限制也无法为 NTLM 应用这些限制。<br /><br />显示的帐户、 设备、 策略和思名称。|
|105<br /><br />**AuthenticationPolicyFailures 域控制器**|原因： Kerberos 限制失败时发生因为不允许从特定设备的身份验证。<br /><br />指示 Kerberos TGT 被拒绝，因为该设备不符合强制的访问控制限制的域控制器在记录的事件。<br /><br />显示的帐户、 设备、 策略、 思名称和 TGT 生命周期。|
|305<br /><br />**AuthenticationPolicyFailures 域控制器**|原因： 潜在 Kerberos 限制故障可能会出现因为不允许从特定设备的身份验证。<br /><br />在审核模式下信息的事件登录以确定 Kerberos TGT 因为该设备不符合访问控制限制将被拒绝域控制器。<br /><br />显示的帐户、 设备、 策略、 思名称和 TGT 生命周期。|
|106<br /><br />**AuthenticationPolicyFailures 域控制器**|原因： Kerberos 限制故障发生原因设备的用户了不允许通过身份验证的服务器。<br /><br />指示 Kerberos 服务票证被拒绝，因为用户、 设备或两者都不符合强制的访问控制限制的域控制器在记录的事件。<br /><br />显示设备、 策略和思名称。|
|306<br /><br />**AuthenticationPolicyFailures 域控制器**|原因： Kerberos 限制失败可能原因是用户或设备了不允许通过身份验证的服务器。<br /><br />在审核模式下信息的事件登录域控制器，以指示 Kerberos 服务票证将被拒绝，因为用户、 设备或两者都不符合访问控制限制。<br /><br />显示设备、 策略和思名称。|

## <a name="see-also"></a>请参阅
[如何配置受保护的帐户](how-to-configure-protected-accounts.md)

[凭据保护和管理](credentials-protection-and-management.md)

[受保护的用户安全组](protected-users-security-group.md)


