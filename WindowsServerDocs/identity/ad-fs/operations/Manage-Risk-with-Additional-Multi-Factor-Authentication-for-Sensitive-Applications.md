---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: 使用适用于敏感应用程序的附加多重身份验证管理风险
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a37c0b0a1de06b65e0cb867ec4d160bbb0373a15
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189070"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>使用适用于敏感应用程序的附加多重身份验证管理风险




-   [为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [操作实例指南：使用针对敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [为 AD FS 配置其他身份验证方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>本指南包含的内容
本指南提供以下信息：

-   [AD FS 中的身份验证机制](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1)-Windows Server 2012 R2 中 Active Directory 联合身份验证服务 (AD FS) 中提供的身份验证机制的说明

-   [方案概述](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)-其中使用 Active Directory 联合身份验证服务 (AD FS) 启用多重身份验证 (MFA) 的方案的说明基于用户的组成员身份。

    > [!NOTE]
    > 在 Windows Server 2012 R2 中的 AD FS 可以启用 MFA 基于网络位置、 设备标识和用户标识或组成员身份。

    有关配置和验证此方案的详细分步操作实例说明，请参阅[操作实例指南：使用针对敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

## <a name="BKMK_1"></a>关键概念-AD FS 中的身份验证机制

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>AD FS 中的身份验证机制的优势
Windows Server 2012 R2 中 active Directory 联合身份验证服务 (AD FS) 提供更丰富、 更灵活的工具集与 IT 管理员想要访问公司资源的用户进行身份验证。 它使管理员灵活控制主要和附加身份验证方法，提供丰富的管理体验配置的身份验证策略 （均通过用户界面和 Windows PowerShell），并增强访问应用程序和服务的受 AD FS 的最终用户体验。 以下是一些保护你的应用程序和 Windows Server 2012 R2 中使用 AD FS 服务的优势：

-   全局身份验证策略的中央管理功能，IT 管理员可以从中选择的身份验证方法使用基于从中访问受保护的资源的网络位置的用户进行身份验证。 这样，管理员便可以执行以下操作：

    -   针对来自 Extranet 的访问请求强制使用更安全的身份验证方法。

    -   针对无缝第二重身份验证启用设备身份验证。 这会绑定到用于访问资源，从而提供更安全的复合身份验证之前访问受保护的资源的已注册设备的用户的标识。

        > [!NOTE]
        > 有关设备对象、 Device Registration Service、 工作区加入，和设备作为无缝第二重身份验证和 SSO 的详细信息，请参阅[加入工作区以从任一设备实现 SSO 和无缝第二个身份验证跨公司应用程序](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

    -   设置 MFA 要求针对所有 extranet 访问，或有条件地基于用户的标识、 网络位置或设备用于访问受保护的资源。

-   更灵活地配置身份验证策略： 可以使用不同的业务值配置为 AD FS 保护资源的自定义身份验证策略。 例如，可以要求对业务影响性大的应用程序使用 MFA。

-   易于使用： 简单直观的管理工具，例如基于 GUI 的 AD FS 管理 MMC 管理单元和 Windows PowerShell cmdlet 使 IT 管理员能够相对方便地配置身份验证策略。 使用 Windows PowerShell，你可以编写解决方案的脚本以满足不同规模的用途，以及自动完成繁琐的管理任务。

-   更好地控制公司资产： 由于以管理员身份使用 AD FS 来配置应用到特定资源的身份验证策略，可以更好地控制如何公司资源的保护。 应用程序不能覆盖 IT 管理员指定的身份验证策略。 对于敏感应用程序和服务，可以启用 MFA 要求和设备身份验证，并且每次访问资源后，可以有选择性地启用全新的身份验证。

-   支持自定义 MFA 提供程序： 对于利用第三方 MFA 方法的组织，AD FS 提供的功能，以合并并无缝地使用这些身份验证方法。

### <a name="authentication-scope"></a>身份验证范围
在 Windows Server 2012 R2 中的 AD FS 可以指定适用于所有应用程序和服务的受 AD FS 保护的全局范围内的身份验证策略。  此外可以设置特定的应用程序和服务 （信赖方信任） 的受 AD FS 的身份验证策略。 针对特定应用程序（按信赖方信任）指定身份验证策略不会覆盖全局身份验证策略。 如果全局或按信赖方信任身份验证策略需要 MFA，则当用户尝试验证到此信赖方信任时，将触发 MFA。  全局身份验证策略是未配置特定身份验证策略的信赖方信任（应用程序和服务）的回退。

全局身份验证策略适用于所有受 AD FS 的信赖方。 可将以下设置配置为全局身份验证策略的一部分：

-   用于主要身份验证的身份验证方法

-   MFA 的设置和方法

-   是否启用设备身份验证。 有关详细信息，请参阅[加入工作区以从任一设备实现 SSO 和无缝第二因素身份验证跨公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

按信赖方信任身份验证策略专门应用到访问该信赖方信任（应用程序或服务）的企图。 可将以下设置配置为按信赖方信任身份验证策略的一部分：

-   是否要求用户在每次登录时都提供其凭据

-   基于用户/组、设备注册和访问请求位置数据的 MFA 设置

### <a name="primary-and-additional-authentication-methods"></a>主要和附加身份验证方法
Windows Server 2012 R2，除了主要身份验证机制中使用 AD FS 管理员可以配置其他身份验证方法。 主要身份验证方法内置于该项服务，它用于验证用户的标识。 可以配置其他身份验证因素，以请求提供了有关用户的标识的详细信息，并因此确保更强的身份验证。

在 Windows Server 2012 R2 中的 AD FS 中的主要身份验证，您可以使用以下选项：

-   对于已发布的需要从公司网络外部访问的资源，默认情况下会选择表单身份验证。 此外，你还可以启用证书身份验证（即，可以与 AD DS 配合使用的基于智能卡的身份验证或用户客户端证书身份验证）。

-   对于 Intranet 资源，默认情况下会选择 Windows 身份验证。 此外，你还可以启用表单身份验证和/或证书身份验证。

通过选择多种身份验证方法，可以让用户在应用程序或服务的登录页上选择要使用哪种方法进行身份验证。

还可以针对无缝第二重身份验证启用设备身份验证。 这会绑定到用于访问资源，从而提供更安全的复合身份验证之前访问受保护的资源的已注册设备的用户的标识。

> [!NOTE]
> 有关设备对象、 Device Registration Service、 工作区加入，和设备作为无缝第二重身份验证和 SSO 的详细信息，请参阅[加入工作区以从任一设备实现 SSO 和无缝第二个身份验证跨公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

如果为 Intranet 资源指定 Windows 身份验证方法（默认选项），身份验证请求将在支持 Windows 身份验证的浏览器上无缝运行此方法。

> [!NOTE]
> 并非所有浏览器都支持 Windows 身份验证。 在 Windows Server 2012 R2 中的 AD FS 中的身份验证机制检测到用户的浏览器用户代理，并使用可配置的设置来确定该用户代理是否支持 Windows 身份验证。 管理员可以在此用户代理列表中添加条目（通过 Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents` 命令），以便为支持 Windows 身份验证的浏览器指定备选的用户代理字符串。 如果客户端的用户代理不支持 Windows 身份验证，则默认回退方法是窗体身份验证。

### <a name="configuring-mfa"></a>配置 MFA
有两个部分，若要在 Windows Server 2012 R2 中的 AD FS 中配置 MFA： 指定在哪些条件下 MFA 必需的并选择附加身份验证方法。 有关其他身份验证方法的详细信息，请参阅[适用于 AD FS 配置其他身份验证方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)。

**MFA 设置**

以下选项可用于 MFA 设置（在哪些条件下需要 MFA）：

-   可以要求针对联合服务器加入到的 AD 域中的特定用户和组使用 MFA。

-   可以要求针对已注册（已加入工作区）或者未注册（未加入工作区）的设备使用 MFA。

    Windows Server 2012 R2 到其中的设备对象表示之间的关系的现代设备采用以用户为中心的方法user@device和公司。 设备对象是在可用于提供应用程序和服务的访问权限时提供组合标识的 Windows Server 2012 R2 中 AD 中的新类。 AD FS 的新组件 - Device Registration Service (DRS) – 可以在 Active Directory 中设置设备标识，以及在使用者设备上设置将用于表示设备标识的证书。 然后，你可以使用此设备标识将设备加入工作区，换而言之，可以将个人设备连接到工作区的 Active Directory。 将个人设备加入工作区时，该设备将成为已知设备，并为受保护的资源和应用程序提供无缝第二重身份验证。 换而言之，设备已加入工作区后，用户的标识绑定到此设备，并访问受保护的资源之前可用于无缝组合标识验证。

    工作区加入和离开的详细信息，请参阅[从实现 SSO 和无缝第二因素身份验证跨公司应用程序的任一设备加入工作区](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

-   可以要求在对受保护资源的访问请求来自 Extranet 或 Intranet 时使用 MFA。

## <a name="BKMK_2"></a>方案概述
在此方案中，启用基于用户的组成员身份数据的特定应用程序的 MFA。 换而言之，你将在联合服务器上设置一个身份验证策略，要求属于特定组的用户请求访问 Web 服务器上托管的特定应用程序时使用 MFA。

更具体地说，在此方案中，你将会针对名为 **claimapp** 的基于声明的测试应用程序启用一个身份验证策略，方案中的 AD 用户 **Robert Hatley** 需要运行 MFA，因为他属于 AD 组 **Finance**。

中提供了分步步骤说明设置和验证此方案[操作实例指南：使用针对敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。 若要完成此演练中的步骤，你必须设置实验室环境，并执行中的步骤[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

启用 MFA AD FS 中的其他方案包括：

-   如果访问请求来自 Extranet，则启用 MFA。 您可以修改的"设置 MFA 策略"部分中提供的代码[操作实例指南：管理敏感应用程序使用的附加多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)以下：

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   如果访问请求来自未加入工作区的设备，则启用 MFA。  您可以修改的"设置 MFA 策略"部分中提供的代码[操作实例指南：管理敏感应用程序使用的附加多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)以下：

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   如果访问请求来自某个用户，并且此用户的某个设备已加入工作区但尚未注册到此用户，则启用 MFA。 您可以修改的"设置 MFA 策略"部分中提供的代码[操作实例指南：管理敏感应用程序使用的附加多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)以下：

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>请参阅
[操作实例指南：管理敏感应用程序使用的附加多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



