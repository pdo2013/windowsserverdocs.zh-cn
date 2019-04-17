---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: "演练指南-管理具有条件访问控制的风险"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>演练指南：管理具有条件访问控制的风险

>适用于： Windows Server 2012 R2


## <a name="about-this-guide"></a>有关此指南
此演练提供用于管理通过条件访问控制机制在 Windows Server 2012 R2 Active Directory 联合身份验证服务 (广告 FS) 中提供的规格 （用户数据） 之一风险的说明进行操作。 有关在 Windows Server 2012 R2 的广告 FS 中的条件访问控制和授权机制的详细信息，请参阅[管理条件访问控制风险](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)。

此演练由以下各部分组成：

-   [步骤 1： 实验环境设置](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [第 2 步： 验证默认广告 FS 访问控制机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [第 3 步： 将根据用户数据的条件访问控制策略配置](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [第 4 步： 验证条件访问控制机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>步骤 1： 实验环境设置
完成此演练，以便你需要的环境中包含以下组件：

-   测试用户和组客户，升级到 Windows Server 2012 R2 或 Windows Server 2012 R2 上运行 Active Directory 域其架构与运行 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上使用 Active Directory 域

-   Windows Server 2012 R2 上运行的联盟服务器

-   承载示例应用程序的 web 服务器

-   客户端计算机可以从这里访问示例应用程序

> [!WARNING]
> （两者中生产版本或测试环境） 强烈建议你不要不使用同一台计算机是您联合身份验证的服务器和您的 web 服务器。

在此环境中，联合身份验证的服务器问题，以便用户可以访问示例应用程序所需的索赔。 Web 服务器承载将信任展示索赔，用户的示例应用联盟服务器的问题。

如何设置此环境中的说明，请参阅[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>第 2 步： 验证默认广告 FS 访问控制机制
在这一步中，您将验证默认广告 FS 访问控制机制了用户重定向到广告 FS 登录页面，提供有效的凭据，以及被授予访问权限的应用程序。 你可以使用**Robert Hatley** AD 帐户并**claimapp**示例的应用程序中配置[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>若要验证默认广告 FS 访问控制机制

1.  在客户端计算机上，打开浏览器窗口中，并导航到你的示例应用程序： **https://webserv1.contoso.com/claimapp**。

    此操作会自动将重定向到请求联合身份验证的服务器，并提示你使用的用户名和密码登录。

2.  输入凭据的**Robert Hatley**中创建的 AD 帐户[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    你将被授予访问权限的应用程序。

## <a name="BKMK_3"></a>第 3 步： 将根据用户数据的条件访问控制策略配置
在此步骤中将设置基于用户组成员数据控制策略访问。 换言之，您将配置**颁发授权规则**信赖的方信任表示示例应用程序的联盟服务器上**claimapp**。 通过此规则逻辑**Robert Hatley**广告用户将会发布访问该应用，因为他属于所需的索赔**财经**组。 你已添加**Robert Hatley**到帐户**财经**组在[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

你可以完成此任务中使用的任一广告 FS 管理控制台或通过 Windows PowerShell。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>配置条件访问基于通过广告 FS 管理控制台用户数据的控件策略

1.  广告 FS 管理控制台中，导航到**信任关系**，然后**信赖方信任**。

2.  选择信赖的方信任表示示例应用程序 (**claimapp**)，然后或者处于**操作**窗格中，或通过右键单击该信赖的方信任，选择**编辑索赔规则**。

3.  在**编辑 claimapp 索赔规则**窗口中，选择**颁发授权规则**选项卡，然后单击**添加规则**。

4.  在**添加颁发授权声称规则向导**，在**选择规则模板页面**、 选择**允许拒绝用户根据或传入声称**声称规则模板，然后单击**下一步**。

5.  在**配置规则**页上，执行以下所有，然后单击**完成**:

    1.  输入索赔规则名称，例如**TestRule**。

    2.  选择**组 SID**作为**传入声称类型**。

    3.  单击**浏览**，键入**财经**的您的广告的名称测试组中，并解决它用于**传入声称值**字段。

    4.  选择**拒绝此传入声明与用户访问**选项。

6.  在**编辑 claimapp 索赔规则**窗口中，请确保删除**允许所有用户访问**已创建默认情况下，当你创建该信赖的方信任规则。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>配置条件访问根据通过 Windows PowerShell 用户数据的控件策略

1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令：


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  在同一 Windows PowerShell 命令窗口中，运行以下命令：


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> 请务必与 SID 为您的广告的价值替换 < group_SID >**财经**组。

## <a name="BKMK_4"></a>第 4 步： 验证条件访问控制机制
在此步骤中将验证设置上一步中的条件访问控制策略。 你可以使用下面的过程验证**Robert Hatley**广告用户可以访问你示例应用，因为他属于**财经**组并不属于广告用户**财经**组无法访问示例应用程序。

1.  在客户端计算机上，打开浏览器窗口中，并导航到你的示例应用程序： **https://webserv1.contoso.com/claimapp**

    此操作会自动将重定向到请求联合身份验证的服务器，并提示你使用的用户名和密码登录。

2.  输入凭据的**Robert Hatley**中创建的 AD 帐户[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    你将被授予访问权限的应用程序。

3.  输入凭据的不属于的其他广告用户**财经**组。 (如何广告中创建用户帐户的详细信息，请参阅[https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)。

    此时，你上一步中设置访问控制策略，因为拒绝访问消息显示不属于此广告用户**财经**组。 默认邮件文本**不有权访问此网站。单击此处注销并重新登录或权限与管理员联系。** 但是，此短信可以完全自定义。 有关如何自定义的登录体验的详细信息，请参阅[自定义广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)。

## <a name="see-also"></a>请参阅
[管理具有条件访问控制风险](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



