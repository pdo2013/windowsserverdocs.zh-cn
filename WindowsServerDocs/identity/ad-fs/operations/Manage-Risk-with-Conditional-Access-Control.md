---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: "管理具有条件访问控制的风险"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2ad7d1467abd6d69077b515b8c69a65f7e70f19
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-conditional-access-control"></a>管理具有条件访问控制的风险

>适用于： Windows Server 2012 R2


-   [广告 FS 中的关键的概念条件访问控制](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [管理具有条件访问控制的风险](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>主要的概念-中广告 FS 条件访问控制
广告 FS 的总体功能是处理访问标记包含一组的索赔。 有关广告金融服务接受，然后问题哪些索赔决策受索赔规则。

广告 FS 中的访问控制实现用于发出许可证或拒绝将确定用户是否的索赔颁发授权索赔规则或将允许的一组用户访问广告 FS 保护资源。 授权规则只能信赖方信任上设置。

|规则选项|规则逻辑|
|---------------|--------------|
|允许所有用户|如果接收的索赔类型等于*任何声称类型*和重视等*任何值*，然后问题声称与值等*允许*|
|允许访问到用户与此传入的声明|如果接收的索赔类型等于*指定索赔类型*和重视等*指定声明值*，然后问题声称与值等*允许*|
|拒绝访问到用户与此传入的声明|如果接收的索赔类型等于*指定索赔类型*和重视等*指定声明值*，然后问题声称与值等*拒绝*|

有关这些规则选项和逻辑的详细信息，请参阅[何时使用授权索赔规则](https://technet.microsoft.com/library/ee913560.aspx)。

在 Windows Server 2012 R2 的广告 FS 中, 访问控制增强具有多个因素，包括用户、设备、位置和身份验证数据。 这可通过索赔类型到更多适用于授权索赔规则。  换言之，在 Windows Server 2012 R2 中广告 FS，可以实施基于用户的身份或一组成员网络位置、设备上的条件访问控制 (无论是工作区已加入详细信息，请参阅[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md))，以及（无论执行多重身份验证 (MFA)）的身份验证状态。

在 Windows Server 2012 R2 的广告 FS 中的条件访问控制提供以下优势：

-   灵活和表现力每个应用程序授权策略，，从而使得可以允许或拒绝访问根据用户、设备、网络位置和身份验证的状态

-   创建颁发授权规则信赖方应用程序

-   常见的条件访问控制方案丰富的 UI 体验

-   高级条件访问控制方案丰富索赔语言和 Windows PowerShell 支持

-   自定义 (每个依赖方应用程序) 拒绝访问的消息。 有关详细信息，请参阅[自定义的广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)。 通过能够自定义这些消息，你可以说明原因用户被拒绝访问和还有助于自助的补救，即，例如，提示用户到工作区加入他们的设备。 有关详细信息，请参阅[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

下表包含在 Windows Server 2012 R2，以实现条件访问控制用于广告 FS 中提供的所有声明类型。

|声称类型|描述|
|--------------|---------------|
|电子邮件地址|用户的电子邮件地址。|
|给定的名称|给定的用户的名称。|
|名称|唯一的名称的用户，|
|UPN|主要用户名 (UPN) 的用户。|
|常见的名称|常见的用户的名称。|
|广告 FS 1 x 电子邮件地址|当与广告 FS 1.1 或广告 F 1.0 互操作的用户电子邮件地址。|
|组|一组的用户的成员。|
|广告 FS 1 x UPN|当与广告 FS 1.1 或广告 F 1.0 进行交互时，用户 UPN。|
|角色|用户可以角色。|
|姓氏|用户姓。|
|PPID|用户专用标识符。|
|名称 ID|用户 SAML 名称标识符。|
|身份验证的时间戳。|用于显示的用户已经过验证的日期和时间。|
|身份验证方法|用来用户身份验证方法。|
|拒绝仅组 SID|仅拒绝 SID 用户组。|
|拒绝仅主要 SID|仅拒绝主要 SID 用户。|
|拒绝主要的组 SID|仅拒绝主要组 SID 的用户。|
|组 SID|用户组 SID。|
|主要组 SID|主要组 SID 的用户。|
|主要 SID|主要 SID 用户。|
|Windows 的帐户名称|域帐户域名 \ 表单中的用户的名称。|
|已注册用户|用户注册使用此设备。|
|设备标识符。|设备标识符。|
|注册设备的标识符|注册设备标识符。|
|注册设备中的显示名称|注册设备的显示名称。|
|设备操作系统类型|设备操作系统类型。|
|设备操作系统版本|设备操作系统版本。|
|是管理设备|由管理服务管理设备。|
|转发客户端 IP|用户的 IP 地址。|
|客户端应用程序|客户端应用程序的类型。|
|设置用户代理客户端|设备类型客户端使用访问该应用程序。|
|客户端 IP|客户端的 IP 地址。|
|端点路径|这可用于确定无源客户端与活动绝对端点路径。|
|代理服务器|DNS 传递请求联盟服务器代理服务器的名称。|
|应用程序的标识符|依赖方标识符。|
|应用程序策略|应用程序的证书的策略。|
|关键颁发机构的标识符|登录颁发的证书证书颁发机构键标识符扩展。|
|基本约束|证书基本约束之一。|
|增强的关键使用情况|描述证书增强密钥用法之一。|
|发行商|证书颁发机构颁发 X.509 证书名称。|
|发行商名称|识别的证书颁发的名称。|
|主要的使用情况|证书的密钥用法之一。|
|不晚于|日期之后证书不再是有效的本地时间。|
|不之前|本地时间证书生效日期。|
|证书策略|证书颁发的策略。|
|公钥|证书公钥。|
|证书原始数据|证书原始数据。|
|主题替代名称|证书替换名称之一。|
|序列号|证书的序列号。|
|签名算法|用于创建的签名证书的算法。|
|主题|从证书主题。|
|主题关键的标识符|证书主题关键标识符。|
|主题名称|从证书主题识别的名。|
|V2 模板名称|在使用对发出或续订证书版本 2 证书模板的名称。 这是 Microsoft 特定值。|
|V1 模板名称|1 版本证书模板发出或续订证书时使用的名称。 这是 Microsoft 特定值。|
|指纹|指纹的证书。|
|509 版本 x|证书 X.509 格式版本。|
|内部企业网络|用于指示是否发出请求从内公司的网络。|
|密码的过期时间|用于显示密码过期时的时间。|
|密码的到期日期|用于显示的数天后到期密码。|
|更新密码 URL|用于显示更新密码服务的 web 地址。|
|身份验证方法引用|用于指示圣克里斯托弗用于验证用户的身份验证方法。|

## <a name="BKMK_2"></a>管理具有条件访问控制的风险
使用可用的设置，有多种方法在其中可以管理通过实现条件访问控制风险。

### <a name="common-scenarios"></a>常见的场景
例如，假设简单实现条件访问控制（信赖方信任）某个特定应用程序用户的组成员数据基于的方案。 换言之，你可以将设置为颁发授权规则联合身份验证的服务器上允许在您的广告属于某一组的用户域某个特定应用程序受 AD FS 访问。  详细的步骤的说明操作（使用的 UI 和 Windows PowerShell）实现此方案中有述[演练指南：管理条件访问控制风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)。 为了完成此演练中的步骤，必须将设置为实验环境，并按照中的步骤[设置适用于 Windows Server 2012 R2 中广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

### <a name="advanced-scenarios"></a>高级的方案
在 Windows Server 2012 R2 中广告 FS 中实现条件访问控制的其他示例如下：

-   允许受 AD FS 仅当 MFA 与该用户的身份验证应用程序访问

    你可以使用以下代码：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   允许受 AD FS 仅当到用户访问请求来自注册的工作区连接设备的应用程序访问

    你可以使用以下代码：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   允许受 AD FS 仅当到已与 MFA 验证其身份一个用户访问请求来自注册的工作区连接设备的应用程序访问

    你可以使用下面的代码

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   允许受 AD FS 仅当的访问权限请求已与 MFA 验证其身份一个用户来自的应用程序访问联网。

    你可以使用以下代码：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>请参阅
[演练指南：管理具有条件访问控制风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)<ph x="2">
</ph>[设置适用于 Windows Server 2012 R2 中广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



