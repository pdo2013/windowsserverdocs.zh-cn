---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: 演练指南-使用针对敏感应用程序的附加多重身份验证管理风险
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860858"
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>操作实例指南：使用适用于敏感应用程序的附加多重身份验证管理风险

>适用于：Windows Server 2012 R2


## <a name="about-this-guide"></a>关于本指南
本演练提供了有关在 Windows Server 2012 R2 中 Active Directory 联合身份验证服务 (AD FS) 中配置多重身份验证 (MFA) 的说明基于用户的组成员身份数据。

在 AD FS 的 MFA 和身份验证机制的详细信息，请参阅[使用针对敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

本操作实例包括以下部分：

-   [步骤 1：设置实验室环境](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [步骤 2：验证默认 AD FS 身份验证机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [步骤 3：在联合服务器上配置 MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [步骤 4：验证 MFA 机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>步骤 1:设置实验室环境
若要完成本操作实例，需要一个包括以下组件的环境：

-   具有测试用户和组帐户，在 Windows Server 2012 R2 或 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上运行其架构已升级到 Windows Server 2012 R2 的 Active Directory 域上运行的 Active Directory 域

-   Windows Server 2012 R2 上运行的联合身份验证服务器

-   一台 Web 服务器，用于托管示例应用程序

-   一台客户端计算机，你可以从中访问示例应用程序

> [!WARNING]
> 强烈建议（不管是在生产环境中还是测试环境中）不要使用同一台计算机作为联合服务器和 Web 服务器。

在此环境中，联合服务器将发出所需的声明，使用户能够访问示例应用程序。 Web 服务器托管示例应用程序，该应用程序将信任提供联合服务器发出的声明的用户。

有关如何设置此环境的说明，请参阅[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>步骤 2:验证默认 AD FS 身份验证机制
在此步骤中，你将验证默认的 AD FS 访问控制机制（适用于 Extranet 的“表单身份验证” 和适用于 Intranet 的“Windows 身份验证”  ），在此过程中，用户将被重定向到 AD FS 登录页，提供有效的凭据，然后被授予应用程序的访问权限。 可以使用**Robert Hatley** AD 帐户和**claimapp**示例应用程序中配置[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

1.  在客户端计算机上打开一个浏览器窗口，并导航到示例应用程序： **https://webserv1.contoso.com/claimapp**。

    此操作会自动将请求重定向到联合服务器，并且系统会提示你使用用户名和密码登录。

2.  中的凭据类型**Robert Hatley**在中创建的 AD 帐户[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    系统将授予你对应用程序的访问权限。

## <a name="BKMK_3"></a>步骤 3:在联合服务器上配置 MFA
有两个部分： 在 Windows Server 2012 R2 中的 AD FS 中配置 MFA:

-   [选择附加身份验证方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [设置 MFA 策略](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>选择附加身份验证方法
若要设置 MFA，必须选择一个附加的身份验证方法。 在本操作实例中，若要选择附加身份验证方法，可以使用下列选项之一：

-   选择[证书身份验证](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7)是 Windows Server 2012 R2 中的 AD FS 中默认情况下提供的方法

-   配置和选择 [Windows Azure Multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>证书的身份验证
完成下列过程之一，选择证书身份验证作为附加身份验证方法：

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>通过 AD FS 管理控制台将证书身份验证配置为附加身份验证方法的步骤

1.  在联合服务器上的 AD FS 管理控制台中，导航到“身份验证策略”节点，然后在“多重身份验证”部分下，单击“全局设置”子部分旁边的“编辑”链接。

2.  在“编辑全局身份验证策略”窗口中，选择“证书身份验证”作为附加身份验证方法，然后单击“确定”。

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>通过 Windows PowerShell 将证书身份验证配置为附加身份验证方法的步骤

1.  在联合服务器上，打开 Windows PowerShell 命令窗口并运行以下命令：

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > 若要验证是否已成功运行此命令，可以运行 `Get-AdfsGlobalAuthenticationPolicy` 命令。

#### <a name="BKMK_8"></a>Windows Azure 多重身份验证
完成以下过程，以便在联合服务器上下载并配置和选择 **Windows Azure Multi-Factor Authentication** 作为附加身份验证：

1.  [创建多重身份验证提供程序通过 Windows Azure 门户](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [下载 Windows Azure 多重身份验证服务器](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [在联合身份验证服务器上安装 Windows Azure 多重身份验证服务器](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [将 Windows Azure 多重身份验证配置为附加身份验证方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>创建多重身份验证提供程序通过 Windows Azure 门户

1.  以管理员身份登录 Windows Azure 门户。

2.  在左侧选择“Active Directory”。

3.  在“Active Directory”页面顶部选择“多重身份验证提供程序”。  然后，在底部单击“新建”。

4.  下**应用服务-> Active Directory**，选择**Multi-factor Auth 提供程序**，然后选择**快速创建**。

5.  在“应用服务”下，选择“Active Authentication 提供程序”，然后选择“快速创建”。

6.  填写以下字段并选择“创建”。

    1.  **名称**-多重身份验证提供程序的名称。

    2.  **使用模型**-多重身份验证提供程序的使用模型。

        -   **每次身份验证**-按身份验证收费的购买模型。 在面向消费者的应用程序中使用 Windows Azure Multi-Factor Authentication 的方案通常使用该选项。

        -   **每个已启用用户**-根据启用的用户进行收费的购买模型。  面向员工的方案（例如 Office 365）通常使用该选项。

        有关使用模式的更多信息，请参阅 [Windows Azure 定价详细信息](http://www.windowsazure.com/pricing/details/active-directory/)。

    3.  **目录**-多重身份验证提供程序与之关联的 Windows Azure Active Directory 租户。 这是一个可选字段，因为在保护本地应用程序时，提供程序不一定要链接到 Windows Azure Active Directory。

7.  单击“创建”后，系统将创建多重身份验证提供程序，并且您应当会看到一条指示以下内容的消息：已成功创建多重身份验证提供程序。  单击“确定”。

接下来，必须下载 Windows Azure Multi-Factor Authentication 服务器。 若要执行此操作，你可以通过 Windows Azure 门户启动 Windows Azure Multi-Factor Authentication 门户。

##### <a name="BKMK_b"></a>下载 Windows Azure 多重身份验证服务器

1.  以管理员身份登录 Windows Azure 门户，再单击你在上一步骤中创建的多重身份验证提供程序。 然后单击“管理”  按钮。

    这将会启动“Windows Azure Multi-Factor Authentication”门户  。

2.  在“Windows Azure Multi-Factor Authentication”  门户中单击“下载”，然后单击 “下载”  以下载 Windows Azure Multi-Factor Authentication 服务器的副本。

下载 Windows Azure 多重身份验证服务器的可执行文件后，必须将它安装在联合服务器上。

##### <a name="BKMK_c"></a>在联合身份验证服务器上安装 Windows Azure 多重身份验证服务器

1.  下载并双击 Windows Azure Multi-Factor Authentication 服务器的可执行文件。  此时将开始安装。

2.  在“许可协议”  屏幕上，阅读协议，选择“我同意”，然后单击“下一步” 。

3.  确保目标文件夹正确并单击“下一步” 。

4.  完成安装后，单击“完成”。

现在，你可以启动安装在联合服务器上的 Windows Azure Multi-Factor Authentication 服务器，并将它配置为附加身份验证方法。

##### <a name="BKMK_d"></a>将 Windows Azure 多重身份验证配置为附加身份验证方法

1.  启动**Windows Azure 多重身份验证**从安装位置将其联合身份验证服务器，在欢迎页上，检查**跳过使用身份验证配置向导**复选框，单击**下一步**。

2.  若要激活 Multi-Factor Authentication 服务器，请在 Multi-Factor Authentication 管理门户中返回到下载 Multi-Factor Authentication 服务器时使用的页面，然后单击“生成激活凭据”按钮  。 在“Multi-Factor Authentication 服务器”用户界面中，输入生成的凭据并单击“激活” 。

3.  随后，“Multi-Factor Authentication 服务器”  用户界面将提示你运行“多服务器配置向导” 。  选择“否”。

    > [!IMPORTANT]
    > 如果实验室环境只包含一台用于完成本操作实例的联合服务器，则无需完成“多服务器配置向导”。 但是，如果你的环境包含多台联合服务器，则你必须在每台联合服务器上安装 Multi-Factor Authentication 服务器并完成“多服务器配置向导”  ，这样才能在多重身份验证服务器（在联合服务器上运行）之间启用复制。

4.  在“Multi-Factor Authentication 服务器”  用户界面中选择“用户”  图标，单击“从 Active Directory 导入” ，选择 **Robert Hatley** 帐户以便在 Windows Azure Multi-Factor Authentication 中对它进行设置，然后单击“导入” 。

5.  在“用户”  列表中选择 **Robert Hatley** 帐户，单击“编辑” ，在“编辑用户”  窗口中提供此帐户的手机号码，确保“已启用”  复选框已被选中，然后单击“应用” 。

6.  在“用户”列表中选择 **Robert Hatley** 帐户，然后单击“测试”。 在“测试用户”  窗口中，提供 **Robert Hatley** 帐户的凭据。 当手机响铃，按 # 以完成帐户验证。

7.  在“多重身份验证服务器”用户界面中选择“AD FS”图标，确保“允许用户注册”、“允许用户选择方法”（包括“电话呼叫”和“短信”）、“使用回退安全问题”和“启用日志记录”复选框已被选中，单击“安装 AD FS 适配器”，然后完成“多重身份验证 AD FS 适配器”安装向导。

    > [!NOTE]
    > “多重身份验证 AD FS 适配器”安装向导将在 Active Directory 中创建一个名为“PhoneFactor Admins”的安全组，然后将联合身份验证服务的 AD FS 服务帐户添加到此组。
    > 
    > 建议在域控制器上验证是否确实创建了“PhoneFactor Admins”组  ，以及 AD FS 服务帐户是否是此组的成员。
    > 
    > 如果需要，请将 AD FS 服务帐户手动添加到域控制器上的“PhoneFactor Admins”组。

    有关安装 AD FS 适配器的更多详细信息，请单击多重身份验证服务器右上角的“帮助”链接。

8.  若要在联合身份验证服务中注册该适配器，请在联合服务器上启动 Windows PowerShell 命令窗口并运行以下命令： `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`。 现在，该适配器已注册为“WindowsAzureMultiFactorAuthentication”。  若要使注册生效，必须重新启动 AD FS 服务。

9. 若要将 Windows Azure Multi-Factor Authentication 配置为附加身份验证方法，请在 AD FS 管理控制台中，导航到“身份验证策略”节点，然后在“多重身份验证”部分下，单击“全局设置”子部分旁边的“编辑”链接。 在“编辑全局身份验证策略”窗口中，选择“多重身份验证”作为附加身份验证方法，然后单击“确定”。

    > [!NOTE]
    > 可以通过运行 **Set-AdfsAuthenticationProviderWebContent** cmdlet 来自定义 Windows Azure Multi-Factor Authentication 方法以及配置的任何第三方身份验证方法的名称和说明，如 AD FS UI 所示。 有关详细信息，请参阅 [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>设置 MFA 策略
若要启用 MFA，必须在联合服务器上设置 MFA 策略。 对于本演练，我们的 MFA 策略，每**Robert Hatley**帐户需要运行 MFA，因为它属于**财务**中设置的组[中的 AD FS 设置实验室环境Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

可以通过 AD FS 管理控制台或使用 Windows PowerShell 来设置 MFA 策略。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>若要配置基于用户的组成员身份数据通过 AD FS 管理控制台的 claimapp 的 MFA 策略

1.  联合身份验证服务器，在 AD FS 管理控制台中，导航到**身份验证策略**\\**按信赖方信任**节点，然后选择信赖方信任表示你示例应用程序 (**claimapp**)。

2.  在“操作”页面中直接选择或者通过右键单击“claimapp”的方式选择“编辑自定义的多重身份验证”。

3.  在“编辑 claimapp 的信赖方信任”窗口中，单击“用户/组”列表旁边的“添加”按钮。 键入**财务**中创建的 AD 组的名称[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)，然后单击**检查名称**，，名称为已解决，请单击**确定**。

4.  在“编辑 claimapp 的信赖方信任”窗口中单击“确定。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>若要配置基于通过 Windows PowerShell claimapp 的用户的组成员身份数据的 MFA 策略

1.  在联合服务器上，打开 Windows PowerShell 命令窗口并运行以下命令：

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  在同一个 Windows PowerShell 命令窗口中运行以下命令：

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > 确保将 <group_SID> 替换为 AD 组 **Finance** 的 SID 值。

## <a name="BKMK_4"></a>步骤 4:验证 MFA 机制
此步骤将验证你在前一步骤中设置的 MFA 功能。 可以使用以下过程来验证 **Robert Hatley** AD 用户是否可以访问你的示例应用程序。这一次需要运行 MFA，因为它属于 **Finance** 组。

1.  在客户端计算机上打开一个浏览器窗口，并导航到示例应用程序： **https://webserv1.contoso.com/claimapp**。

    此操作会自动将请求重定向到联合服务器，并且系统会提示你使用用户名和密码登录。

2.  输入 **Robert Hatley** AD 帐户的凭据。

    此时，由于所配置的 MFA 策略的原因，系统将提示用户运行附加身份验证。 默认消息文本为“出于安全原因，我们需要其他信息来验证你的帐户。” 但是，此文本完全可自定义。 有关如何自定义登录体验的详细信息，请参阅 [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)。

    如果为附加身份验证方法配置证书身份验证，则默认消息文本是**选择你想要用于身份验证的证书。如果取消该操作，请关闭浏览器，然后重试。**

    如果已将 Windows Azure 多重身份验证配置为附加身份验证方法，则默认消息文本将是“将呼叫您的电话以完成您的身份验证。” 有关使用 Windows Azure Multi-Factor Authentication 登录及使用不同选项完成首选验证方法的详细信息，请参阅 Windows Azure Multi-Factor Authentication 概述。

## <a name="see-also"></a>请参阅
[管理敏感应用程序使用的附加多重身份验证的风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



