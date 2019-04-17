---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: "管理敏感应用程序的其他多重身份验证的风险"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9ee275e6fe38005394cd071e9cfe9a7999350e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>管理敏感应用程序的其他多重身份验证的风险

>适用于： Windows Server 2012 R2


-   [设置适用于 Windows Server 2012 R2 中广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [演练指南：管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [广告 fs 配置额外的身份验证方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>本指南中
本指南提供以下信息：

-   [在广告 FS 身份验证机制](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1)-适用于 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 (AD FS) 身份验证机制说明

-   [方案概述](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)-在使用 Active Directory 联合身份验证服务 (AD FS) 进行多重身份验证 (MFA) 的方案的说明基于用户组成员。

    > [!NOTE]
    > 在 Windows Server 2012 R2 的广告 FS 中可以启用 MFA 网络位置、设备身份和用户的身份或一组成员。

    详细的分步演练配置和验证此方案中的说明，请参阅[演练指南：管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

## <a name="BKMK_1"></a>主要的概念-身份验证机制广告 FS 中

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>身份验证机制广告 FS 中的权益
在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 (AD FS) 提供适用于想要访问公司资源的用户身份验证 IT 管理员提供更丰富的、更灵活套工具。 它使管理员与灵活地控制主和额外的身份验证方法、提供了丰富管理配置身份验证的体验策略（同时通过用户界面和 Windows PowerShell），并增强了访问应用和服务进行保护的广告 FS 最终用户的体验。 下面是一些保护你的应用程序和服务使用在 Windows Server 2012 R2 的广告 FS 的好处：

-   策略全球身份验证的集中管理能力，从中 IT 管理员可以选择使用哪个身份验证方法用户基于从中他们访问资源受保护的网络位置进行身份验证。 这将支持管理员可以执行以下操作：

    -   从联网规定使用更安全的身份验证方法为访问请求。

    -   启用设备无缝第二个双因素身份验证身份验证。 这将用于访问资源，因此之前访问受保护的资源提供更安全复合身份验证的已注册设备的用户的身份。

        > [!NOTE]
        > 有关设备对象、设备注册服务、加入工作区，和设备无缝第二个强双因素身份 SSO 作为的详细信息，请参阅[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

    -   设置 MFA 要求所有外部网络的访问或条件根据用户的身份、网络位置或设备，用于访问受保护的资源。

-   更大的灵活性配置认证策略：您可以使用不同的业务值配置自定义身份验证的广告 FS 保护资源的策略。 例如，你可以要求 MFA 的应用程序的高与众不同之处。

-   轻松使用：简单且直观的管理工具，例如在 GUI 基于广告 FS 管理 MMC 贴靠和 Windows PowerShell cmdlet 启用 IT 管理员可以使用相对轻松配置身份验证的策略。 使用 Windows PowerShell，可以脚本用于比例并自动执行常见的管理任务你解决方案。

-   更好地控制公司资产：由于以 administrator 身份可用于广告 FS 配置适用于特定资源的身份验证策略，你有更大控制如何公司资源通过安全。 应用程序无法重写指定的 IT 管理员的身份验证策略。 对于敏感应用程序和服务，可以在每次访问时的资源启用 MFA 要求设备身份验证、，可选择新的身份验证。

-   自定义 MFA 提供程序的支持：对于利用第三方 MFA 方法的组织，广告 FS 提供合并并使用这些身份验证方法无缝的能力。

### <a name="authentication-scope"></a>身份验证范围
在 Windows Server 2012 R2 的广告 FS 中，您可以指定身份验证策略适用于所有应用和服务进行保护的广告 FS 全局范围。  你还可以设置为特定应用程序和服务（依赖方信任）进行保护的广告 FS 认证策略。 指定身份验证策略某个特定应用程序 (每个依赖方信任) 不会覆盖策略全球身份验证。 如果全球或每依赖用户尝试此信赖的方信任验证身份时，将会触发身份验证策略要求 MFA，MFA 方信任。  策略全球身份验证是一种回退对于不具有特定的身份验证策略配置信赖方信任（应用程序和服务）。

全球身份验证策略适用于所有信赖方进行保护的广告 FS。 你可以将以下设置配置为策略全球身份验证的一部分：

-   身份验证方法为用于主要身份验证

-   设置和 MFA 的方法

-   是否已启用设备身份验证。 有关详细信息，请参阅[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

每个依赖方信任身份验证策略适用于尝试访问该信赖的方信任（应用程序或服务）专门。 你可以将以下设置配置为每个依赖方信任身份验证策略的一部分：

-   用户是否需要提供他们的凭据每次登录时

-   根据用户月组设备注册，访问请求位置数据 MFA 设置

### <a name="primary-and-additional-authentication-methods"></a>主要和额外的身份验证方法
通过在 Windows Server 2012 R2 的主要身份验证机制，除了广告 FS 管理员可以配置额外的身份验证方法。 主要的身份验证方法内置，用于验证用户的身份。 您可以配置额外的身份验证因素请求，提供了有关用户的身份的详细信息，并因此确保强的身份验证。

在 Windows Server 2012 R2 的广告 FS 中的主身份验证，你有以下选项：

-   对于发布从公司的网络之外进行访问的资源，形式的身份验证默认处于选中状态。 此外，你还可以启用证书身份验证（即，智能卡基于身份验证或适用于广告 DS 用户客户证书验证）。

-   对于 intranet 资源，默认情况下，可选择 Windows 身份验证。 此外可以还启用表单和/或的身份验证证书。

通过选择多个身份验证方法，你启用你用户可以选择哪种方法进行身份验证在你的应用或服务的登录页面。

你还可以启用设备无缝第二个强双因素身份验证。 这将用于访问资源，因此之前访问受保护的资源提供更安全复合身份验证的已注册设备的用户的身份。

> [!NOTE]
> 有关设备对象、设备注册服务、加入工作区，和设备无缝第二个强双因素身份 SSO 作为的详细信息，请参阅[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

如果指定 intranet 资源的选项（默认）的 Windows 身份验证方法，身份验证请求经历了此无缝在支持 Windows 身份验证方法。

> [!NOTE]
> Windows 身份验证上所有浏览器都不支持。 在 Windows Server 2012 R2 的广告 FS 中的身份验证机制检测到该用户的浏览器用户代理和配置设置用于确定是否该用户的代理支持 Windows 身份验证。 管理员可以向此列表添加的用户代理 (通过 Windows PowerShell`Set-AdfsProperties -WIASupportedUserAgents`命令，以便指定备用用户浏览器支持 Windows 身份验证的代理字符串。 如果客户端的用户代理不支持 Windows 身份验证，默认回退方法是形式的身份验证。

### <a name="configuring-mfa"></a>配置 MFA
有两个部分，以在 Windows Server 2012 R2 的广告 FS 配置 MFA：指定的条件下，MFA 是必需的并选择额外的身份验证方法。 有关其他身份验证方法的详细信息，请参阅[广告 FS 配置额外的验证方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)。

**MFA 设置**

以下选项都可用于 MFA 设置（条件下，若要需要 MFA）：

-   你可以要求 MFA 加入联合服务器的广告域中的特定用户和组。

-   你可以要求 MFA 注册（加入工作区）或注销（不工作区加入）的设备。

    Windows Server 2012R2 采取用户中心方法为设备对象其中代表之间的关系的现代设备user@device和公司。 设备对象新类处于在可用于提供复合身份提供给应用程序和服务的访问权限时，Windows Server 2012 R2 的广告。 广告 FS-设备注册服务 (DRS)-的新组件规定 Active Directory 中设备的身份，并将用于表示设备身份的消费者设备上设置一个证书。 你可以使用此设备到工作区的身份加入你的设备，换句话说，以将你的个人设备连接到工作区 Active Directory。 当你到工作区加入你的个人设备时，它将成为已知的设备，并且可以提供无缝受保护的资源和应用程序的第二个强双因素身份。 换言之，加入工作区设备后，该用户的身份依赖于此设备，并前访问的受保护的资源可用于无缝复合身份验证。

    在工作区连接并保持的详细信息，请参阅[连接到工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

-   如果受保护的资源的访问权限请求来自联网或 intranet，可以要求 MFA。

## <a name="BKMK_2"></a>方案概述
在此情况下，你可以启用 MFA 基于为特定应用程序用户的组成员数据。 换言之，你将可以设置为身份验证策略联合身份验证的服务器上属于某一组的用户请求访问权限的特定应用程序的 web 服务器上托管时需要 MFA。

具体而言，在此情况下，启用调用索赔基于测试应用程序身份验证策略**claimapp**、，从而使得广告用户**Robert Hatley**将需要先进行 MFA，因为他属于广告组**财经**。

步骤通过步骤说明来设置并验证此方案中提供了[演练指南：管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。 为了完成此演练中的步骤，必须将设置为实验环境，并按照中的步骤[设置适用于 Windows Server 2012 R2 中广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

启用 MFA 广告 FS 中的其他方案如下所示：

-   启用 MFA，，如果访问请求来自联网。 您可以在"设置 MFA 策略"部分中介绍的代码[演练指南：管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)与以下：

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   如果访问请求来自非工作区连接设备，请启用 MFA。  您可以在"设置 MFA 策略"部分中介绍的代码[演练指南：管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)与以下：

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   如果使用的是工作区加入，但未注册到该用户的设备访问来自用户，启用 MFA。 您可以在"设置 MFA 策略"部分中介绍的代码[演练指南：管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)与以下：

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>请参阅
[演练指南：管理敏感应用程序使用其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)<ph x="2">
</ph>[设置适用于 Windows Server 2012 R2 中广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



