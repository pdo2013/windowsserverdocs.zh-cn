---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: "演练指南-管理敏感应用程序的其他多重身份验证的风险"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>演练指南：管理敏感应用程序的其他多重身份验证的风险

>适用于： Windows Server 2012 R2


## <a name="about-this-guide"></a>有关此指南
此演练提供上用户的组成员数据基于配置多重身份验证 (MFA) 在 Windows Server 2012 R2 Active Directory 联合身份验证服务 (广告 FS) 中的说明进行操作。

有关 MFA 和身份验证机制广告 FS 中的详细信息，请参阅[管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

此演练由以下各部分组成：

-   [步骤 1： 实验环境设置](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [第 2 步： 验证默认广告 FS 身份验证机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [第 3 步： 在您联合身份验证的服务器上配置 MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [第 4 步： 验证 MFA 机制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>步骤 1： 实验环境设置
完成此演练，以便你需要的环境中包含以下组件：

-   带有测试用户和组帐户，运行 Windows Server 2012 R2 或升级到 Windows Server 2012 R2 其架构与 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上运行 Active Directory 域的 Active Directory 域

-   Windows Server 2012 R2 上运行的联盟服务器

-   承载示例应用程序的 web 服务器

-   客户端计算机可以从这里访问示例应用程序

> [!WARNING]
> （两者中生产和测试环境） 强烈建议你不要不使用同一台计算机是您联合身份验证的服务器和您的 web 服务器。

在此环境中，联合身份验证的服务器问题，以便用户可以访问示例应用程序所需的索赔。 Web 服务器承载将信任展示索赔，用户的示例应用联盟服务器的问题。

如何设置此环境中的说明，请参阅[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>第 2 步： 验证默认广告 FS 身份验证机制
在此步骤中将验证默认广告 FS 访问控制机制 (**形式的身份验证**为联网和**Windows 身份验证**的 intranet)，其中用户会重定向到登录页面广告 FS、 提供有效的凭据，而被授予访问权限的应用程序。 你可以使用**Robert Hatley** AD 帐户并**claimapp**示例的应用程序中配置[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

1.  在客户端计算机上，打开浏览器窗口中，并导航到你的示例应用程序： **https://webserv1.contoso.com/claimapp**。

    此操作会自动将重定向到请求联合身份验证的服务器，并提示你使用的用户名和密码登录。

2.  输入凭据的**Robert Hatley**中创建的 AD 帐户[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    你将被授予访问权限的应用程序。

## <a name="BKMK_3"></a>第 3 步： 在您联合身份验证的服务器上配置 MFA
有两个部分配置 MFA 在 Windows Server 2012 R2 的广告 FS 中：

-   [选择额外的身份验证方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [设置 MFA 策略](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>选择额外的身份验证方法
为了设置 MFA，必须选择额外的身份验证方法。 此演练额外的身份验证方法为在你可以选择以下两个选项：

-   选择[证书身份验证](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7)适用于在 Windows Server 2012 R2 的广告 FS 默认情况下的方法

-   将配置并选择[Windows Azure 多重身份验证](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>证书身份验证
完成以下过程证书身份验证上选择作为额外的身份验证方法之一：

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>作为额外的身份验证方法通过广告 FS 管理控制台配置证书身份验证

1.  在您联合身份验证的服务器，在广告 FS 管理控制台中，导航到**认证策略**节点下,**多重身份验证**部分中，单击**编辑**旁边的链接**全球设置**子部分。

2.  在**编辑策略全球的身份验证**窗口中，选择**证书身份验证**作为额外的身份验证方法，然后单击**确定**。

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>作为额外的身份验证方法通过 Windows PowerShell 配置证书身份验证

1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令：

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > 若要验证是否已成功运行此命令，你可以运行`Get-AdfsGlobalAuthenticationPolicy`命令。

#### <a name="BKMK_8"></a>Windows Azure 多重身份验证
完成以下过程以便下载，将配置并选择**Windows Azure 多重身份验证**作为额外的身份验证您联合身份验证的服务器上：

1.  [创建多重身份验证提供程序通过 Windows Azure 门户](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [下载 Windows Azure 多重身份验证的服务器](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [在您联合身份验证的服务器上安装的 Windows Azure 多重身份验证的服务器](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [作为额外的身份验证方法配置 Windows Azure 多重身份验证](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>创建多重身份验证提供程序通过 Windows Azure 门户

1.  登录到 Windows Azure 门户以管理员身份登录。

2.  在左侧，选择 Active Directory。

3.  在 Active Directory 页面顶部，选择**多重身份验证的提供商**。  依次单击底部，**新建**。

4.  下**应用服务-> Active Directory**、 选择**多重身份验证的提供商**，然后选择**快速创建**。

5.  下**应用服务**、 选择**活动 Auth 提供商**，然后选择**快速创建**。

6.  填写以下字段和选择**创建**。

    1.  **名称**-多重身份验证提供商的名称。

    2.  **使用模型**-多重身份验证提供商的使用模式。

        -   **每次身份验证**-购买每身份验证费用的方式。 通常用于使用消费者面向应用程序中的 Windows Azure 多重身份验证的方案。

        -   **每个启用用户**-购买每位用户启用费用的方式。  通常用于员工面向方案，如 Office 365。

        使用模式的其他信息，请参阅[定价详细信息的 Windows Azure](http://www.windowsazure.com/pricing/details/active-directory/)。

    3.  **目录**-多重身份验证提供与之关联的 Windows Azure Active Directory 租户。 这是可选的如提供商没有保护本地应用程序时要链接到 Windows Azure Active Directory。

7.  一旦你单击创建、 多重身份验证提供程序将创建和您应该会看到一条消息，指出： 成功创建了多重身份验证的提供商。  单击**确定**。

接下来，必须下载 Windows Azure 多重身份验证的服务器。 你可以启动 Windows Azure 多重身份验证门户通过 Windows Azure 门户来执行此操作。

##### <a name="BKMK_b"></a>下载 Windows Azure 多重身份验证的服务器

1.  登录到 Windows Azure 门户以 administrator 身份，并在上面的过程中创建的多重身份验证提供单击。 然后单击**管理**按钮。

    这将启动**Windows Azure 多重身份验证**portal。

2.  在**Windows Azure 多重身份验证**门户，单击**下载**，然后单击**下载**以下载 Windows Azure 多重身份验证的服务器的副本。

Windows Azure 多重身份验证的服务器下载可执行文件，必须联合身份验证的服务器上安装它。

##### <a name="BKMK_c"></a>在您联合身份验证的服务器上安装的 Windows Azure 多重身份验证的服务器

1.  下载并双击可执行文件的 Windows Azure 多重身份验证的服务器。  这将开始安装。

2.  阅读协议，选择在许可协议屏幕上，**我同意**单击**下一步**。

3.  确保正确的目标文件夹，然后单击**下一步**。

4.  一旦将完成安装，请单击**完成**。

现在你可以进行启动联盟服务器安装的 Windows Azure 多重身份验证服务器，并对其进行配置作为额外的身份验证方法。

##### <a name="BKMK_d"></a>作为额外的身份验证方法配置 Windows Azure 多重身份验证

1.  启动**Windows Azure 多重身份验证**在你已将它安装在您联合身份验证的服务器，并在欢迎页面上，检查**跳过使用身份验证配置向导**复选框，然后单击**下一步**。

2.  若要激活多重身份验证的服务器，请返回到页面中多重身份验证管理门户多重身份验证的服务器的下载位置，然后单击**生成激活凭据**按钮。 在多重身份验证的服务器用户界面中，输入生成的凭据，然后单击**激活**。

3.  接下来，**多重身份验证的服务器**用户界面提示你运行**多服务器配置向导**。  选择**否**。

    > [!IMPORTANT]
    > 你可以跳过完成**多服务器配置向导**给定用来完成此演练只有一个联盟服务器实验环境。 但是，如果您的环境中包含几个联合身份验证的服务器，你必须安装多重身份验证的服务器和完成**多服务器配置向导**以启用你联合身份验证的服务器上运行多重服务器之间复制每个联合身份验证的服务器。

4.  在**多重身份验证的服务器**用户界面中，选择**用户**图标，单击**从 Active Directory 导入**、 选择**Robert Hatley**帐户准备 Windows Azure 多重身份验证，然后依次单击**导入**。

5.  在**用户**列表中，选择**Robert Hatley**帐户，请单击**编辑**，并在**编辑用户**窗口中，提供此帐户的移动电话号码，请确保**启用**复选框处于选中状态，，然后单击**应用**。

6.  在**用户**列表中，选择**Robert Hatley**帐户，然后单击**测试**。 在**测试用户**窗口中，提供有关的凭据**Robert Hatley**帐户。 当手机响铃，请按 # 来完成帐户验证。

7.  在**多重身份验证的服务器**用户界面中，选择**广告 FS**图标，请确保**允许用户注册**，**允许用户选择方法**(包括**电话呼叫**和**短信**)，**使用安全问题进行回退**和**启用日志记录**选中复选框、 单击**安装广告 FS 适配器**，并完成**多重身份验证广告 FS 适配器**安装向导。

    > [!NOTE]
    > **多重身份验证广告 FS 适配器**安装向导将创建一个名为安全组**PhoneFactor 管理员**你 Active Directory 中，然后为该组添加联合身份验证服务广告 FS 服务帐户。
    > 
    > 建议你验证你的域控制器上的**PhoneFactor 管理员**确实创建组，并且广告 FS 服务帐户该组的成员。
    > 
    > 如有必要，广告 FS 服务将帐户添加到**PhoneFactor 管理员**手动分组域控制器。

    在安装广告 FS 适配器上的其他详细信息，请单击多重身份验证的服务器右上角的帮助链接。

8.  在你联合身份验证服务、 您联合身份验证的服务器，在注册适配器启动 Windows PowerShell 命令窗口中，并运行以下命令： `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`。 适配器立即注册为**WindowsAzureMultiFactorAuthentication**。  你必须重新启动您的广告 FS 服务的注册，以使其生效。

9. 若要将 Windows Azure 多重身份验证配置为额外的身份验证方法、 广告 FS 管理控制台中，在导航到**认证策略**节点下,**多重身份验证**部分中，单击**编辑**旁边的链接**全球设置**子部分。 在**编辑策略全球的身份验证**窗口中，选择**多重身份验证**作为额外的身份验证方法，然后单击**确定**。

    > [!NOTE]
    > 你可以自定义的名称和的 Windows Azure 多重身份验证方法，以及任何说明配置第三方身份验证方法，通过运行出现在您的广告 FS UI**组 AdfsAuthenticationProviderWebContent** cmdlet。 有关详细信息，请参阅[https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>设置 MFA 策略
为了使 MFA，你必须设置 MFA 策略联合身份验证的服务器上。 此演练根据我们的 MFA 策略**Robert Hatley**经历 MFA，因为他属于所需的帐户**财经**设置中的组[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

你可以设置通过广告 FS 管理控制台或使用 Windows PowerShell MFA 策略。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>若要配置基于 'claimapp 通过广告 FS 管理控制台的用户的组成员数据 MFA 策略

1.  在您联合身份验证的服务器，在广告 FS 管理控制台中，导航到**认证策略**\\**每依赖方信任**节点，然后选择表示示例应用的依赖方信任 (**claimapp**)。

2.  在**操作**页面，或通过右键单击**claimapp**、 选择**编辑自定义多重身份验证**。

3.  在**编辑依赖的 claimapp 信任的方**窗口中，单击**添加**按钮旁边**用户中的组**列表。 键入**财经**中创建你的广告组的名称[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)，单击**检查名称**，并解决名称时，单击**确定**。

4.  单击**确定**中**编辑依赖的 claimapp 信任的方**窗口。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>若要配置根据 'claimapp 通过 Windows PowerShell 的用户的组成员数据 MFA 策略

1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令：

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  在同一 Windows PowerShell 命令窗口中，运行以下命令：

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > 请确保替换值广告组 SID 为 < group_SID >**财经**。

## <a name="BKMK_4"></a>第 4 步： 验证 MFA 机制
在此步骤中将验证设置上述步骤中 MFA 功能。 你可以使用下面的过程验证**Robert Hatley**广告用户都可以访问你的示例应用程序和这一次需要经历 MFA，因为他属于**财经**组。

1.  在客户端计算机上，打开浏览器窗口中，并导航到你的示例应用程序： **https://webserv1.contoso.com/claimapp**。

    此操作会自动将重定向到请求联合身份验证的服务器，并提示你使用的用户名和密码登录。

2.  输入凭据的**Robert Hatley** AD 帐户。

    此时，你将配置 MFA 策略，因为用户将提示您进行额外的身份验证。 默认邮件文本**出于安全原因，我们需要其他信息来验证你的帐户。** 但是，此短信可以完全自定义。 有关如何自定义的登录体验的详细信息，请参阅[自定义广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)。

    如果证书身份验证配置作为额外的身份验证方法，是默认的邮件文本**选择你想要使用的身份验证的证书。如果你取消该操作，请关闭你的浏览器，然后重试。**

    如果你将 Windows Azure 多重身份验证配置作为额外的身份验证方法、 是默认的邮件文本**通话将被放到你的手机，以完成你的身份验证。** 有关使用 Windows Azure 多重身份验证登录和验证的首选方法使用不同的选项的详细信息，请参阅[Windows Azure 多重身份验证概述](https://technet.microsoft.com/library/dn249479.aspx)。

## <a name="see-also"></a>请参阅
[控制与其他多重身份验证的敏感应用程序的风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



