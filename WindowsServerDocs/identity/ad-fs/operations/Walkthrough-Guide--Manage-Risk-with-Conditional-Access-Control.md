---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: 演练指南-使用条件性访问控制管理风险
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826648"
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>操作实例指南：使用条件访问控制管理风险

>适用于：Windows Server 2012 R2


## <a name="about-this-guide"></a>关于本指南
本演练提供说明来管理风险的其中一个可用的因素 （用户数据） 通过 Windows Server 2012 R2 中 Active Directory 联合身份验证服务 (AD FS) 中的条件性访问控制机制。 有关 Windows Server 2012 R2 中的 AD FS 中的条件性访问控制和授权机制的详细信息，请参阅[使用条件性访问控制管理风险](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)。

本操作实例包括以下部分：

-   [步骤 1：设置实验室环境](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [步骤 2：验证默认 AD FS 访问控制机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [步骤 3：配置基于用户数据的条件性访问控制策略](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [步骤 4：验证条件性访问控制机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>步骤 1:设置实验室环境
若要完成本操作实例，需要一个包括以下组件的环境：

-   具有测试用户和组帐户，其架构已升级到 Windows Server 2012 R2 或 Windows Server 2012 R2 上运行的 Active Directory 域中运行 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上的 Active Directory 域

-   Windows Server 2012 R2 上运行的联合身份验证服务器

-   一台 Web 服务器，用于托管示例应用程序

-   一台客户端计算机，你可以从中访问示例应用程序

> [!WARNING]
> 强烈建议（不管是在生产环境还是测试环境中）不要使用同一台计算机作为联合服务器和 Web 服务器。

在此环境中，联合服务器将发出所需的声明，使用户能够访问示例应用程序。 Web 服务器托管示例应用程序，该应用程序将信任提供联合服务器发出的声明的用户。

有关如何设置此环境的说明，请参阅[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>步骤 2:验证默认 AD FS 访问控制机制
在此步骤中，你将验证默认的 AD FS 访问控制机制，在此过程中，用户将被重定向到 AD FS 登录页，提供有效的凭据，然后被授予应用程序的访问权限。 可以使用**Robert Hatley** AD 帐户和**claimapp**示例应用程序中配置[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>验证默认 AD FS 访问控制机制的步骤

1.  在客户端计算机上打开一个浏览器窗口，并导航到示例应用程序： **https://webserv1.contoso.com/claimapp**。

    此操作会自动将请求重定向到联合服务器，并且系统会提示你使用用户名和密码登录。

2.  中的凭据类型**Robert Hatley**在中创建的 AD 帐户[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    系统将授予你对应用程序的访问权限。

## <a name="BKMK_3"></a>步骤 3:基于用户数据配置条件访问控制策略
在此步骤中，你将基于用户组成员身份数据设置一个访问控制策略。 换而言之，你将在联合服务器上，为代表示例应用程序 ( **claimapp** ) 的信赖方信任配置“颁发授权规则” 。 根据此规则的逻辑**Robert Hatley** AD 用户发出访问此应用程序，因为它属于所需的声明**财务**组。 已添加**Robert Hatley**帐户添加到**财务**组中[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

可以通过 AD FS 管理控制台或 Windows PowerShell 完成此任务。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>通过 AD FS 管理控制台基于用户数据配置条件访问控制策略的步骤

1.  在 AD FS 管理控制台中，依次导航到“信任关系” 和“信赖方信任” 。

2.  选择代表示例应用程序 (**claimapp**) 的信赖方信任，然后在“操作”  窗格中选择“编辑声明规则” ，或者通过右键单击此信赖方信任的方式进行选择。

3.  在“编辑 claimapp 的声明规则”  窗口中，选择“颁发授权规则”  选项卡，然后单击“添加规则” 。

4.  在“添加颁发授权声明规则向导” 中的“选择规则模板” 页面上，选择“根据传入声明允许或拒绝用户”  声明规则模板，然后单击“下一步” 。

5.  在“配置规则”页面上执行下述所有操作，然后单击“完成”：

    1.  输入声明规则的名称，例如 **TestRule**。

    2.  选择“组 SID”  作为“传入声明类型” 。

    3.  单击“浏览” ，键入 **Finance** 作为 AD 测试组的名称，然后解析该名称以填入“传入声明值”  字段。

    4.  选择“拒绝访问具有此传入声明的用户”选项  。

6.  在“编辑 claimapp 的声明规则”  窗口中，请确保删除你在创建此信赖方信任时按默认创建的“允许访问所有用户”  规则。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>通过 Windows PowerShell 基于用户数据配置条件访问控制策略的步骤

1.  在联合服务器上，打开 Windows PowerShell 命令窗口并运行以下命令：


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  在同一个 Windows PowerShell 命令窗口中运行以下命令：


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> 确保将 <group_SID> 替换为 AD **Finance** 组的 SID 值。

## <a name="BKMK_4"></a>步骤 4:验证条件访问控制机制
在此步骤中，你将验证前一步骤中设置的条件访问控制策略。 可以使用以下过程来验证 **Robert Hatley** AD 用户是否因属于 **Finance** 组而可以访问你的示例应用程序，以及不属于 **Finance** 组的 AD 用户是否无法访问示例应用程序。

1.  在客户端计算机上打开浏览器窗口，并导航到示例应用程序： **https://webserv1.contoso.com/claimapp**

    此操作会自动将请求重定向到联合服务器，并且系统会提示你使用用户名和密码登录。

2.  中的凭据类型**Robert Hatley**在中创建的 AD 帐户[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    系统将授予你对应用程序的访问权限。

3.  键入不属于 **Finance** 组的另一 AD 用户的凭据。 (有关如何在 AD 中创建用户帐户的详细信息，请参阅[ https://technet.microsoft.com/library/cc7833232.aspx ](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)。

    此时，由于上一步中设置的访问控制策略，拒绝访问消息显示为不属于此 AD 用户**财务**组。 默认消息文本是**你无权访问此站点。单击此处注销并重新登录或联系管理员获取权限。** 但是，此文本完全可自定义。 有关如何自定义登录体验的详细信息，请参阅 [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)。

## <a name="see-also"></a>请参阅
[使用条件性访问控制管理风险](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



