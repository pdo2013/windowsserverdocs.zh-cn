---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: 使用适用于敏感应用程序的附加多重身份验证管理风险
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 79319f54ceb14195dffd56b5a4dfe1b17f048df9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407527"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>使用适用于敏感应用程序的附加多重身份验证管理风险




-   [为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [操作实例指南：利用适用于敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [为 AD FS 配置其他身份验证方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>本指南包含的内容
本指南提供以下信息：

-   [AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1)中的身份验证机制-描述 Windows Server 2012 R2 中 Active Directory 联合身份验证服务（AD FS）提供的身份验证机制

-   [方案概述](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)-一种方案说明，其中使用 Active Directory 联合身份验证服务（AD FS）基于用户的组成员身份来启用多重身份验证（MFA）。

    > [!NOTE]
    > 在 Windows Server 2012 R2 的 AD FS 中，可以基于网络位置、设备标识和用户标识或组成员身份来启用 MFA。

    有关配置和验证此方案的详细分步操作实例说明，请参阅 [Walkthrough Guide：利用适用于敏感应用程序](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)的附加多重身份验证管理风险。

## <a name="BKMK_1"></a>关键概念-AD FS 中的身份验证机制

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>AD FS 中的身份验证机制的优势
Windows Server 2012 R2 中的 Active Directory 联合身份验证服务（AD FS）为 IT 管理员提供了更丰富、更灵活的工具集，用于对想要访问公司资源的用户进行身份验证。 它使管理员能够灵活控制主要和附加身份验证方法，为配置身份验证策略提供丰富的管理体验（通过用户界面和 Windows PowerShell），并增强访问 AD FS 保护的应用程序和服务的最终用户的体验。 下面是在 Windows Server 2012 R2 中 AD FS 保护应用程序和服务的一些优点：

-   全局身份验证策略-一种集中管理功能，IT 管理员可以从该管理功能中选择使用哪种身份验证方法根据访问受保护资源的网络位置对用户进行身份验证。 这样，管理员便可以执行以下操作：

    -   针对来自 Extranet 的访问请求强制使用更安全的身份验证方法。

    -   针对无缝第二重身份验证启用设备身份验证。 这会将用户的标识绑定到用于访问资源的已注册设备，因此，在访问受保护的资源之前，可以提供更安全的复合身份验证。

        > [!NOTE]
        > 有关设备对象、设备注册服务、Workplace Join 和设备作为无缝第二重身份验证和 SSO 的详细信息，请参阅[从任何设备加入工作区以实现 sso 和跨公司无缝第二重身份验证应用程序](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

    -   根据用户的标识、网络位置或用于访问受保护资源的设备，为所有 extranet 访问或有条件地设置 MFA 要求。

-   更灵活地配置身份验证策略：可以为具有不同业务值的 AD FS 保护资源配置自定义身份验证策略。 例如，可以要求对业务影响性大的应用程序使用 MFA。

-   易于使用：简单直观的管理工具（例如基于 GUI 的 AD FS 管理 MMC 管理单元和 Windows PowerShell cmdlet）使 IT 管理员能够相对方便地配置身份验证策略。 使用 Windows PowerShell，你可以编写解决方案的脚本以满足不同规模的用途，以及自动完成繁琐的管理任务。

-   更好地控制公司资产：由于管理员可以使用 AD FS 来配置适用于特定资源的身份验证策略，因此可以更好地控制公司资源的保护方式。 应用程序不能覆盖 IT 管理员指定的身份验证策略。 对于敏感应用程序和服务，可以启用 MFA 要求和设备身份验证，并且每次访问资源后，可以有选择性地启用全新的身份验证。

-   支持自定义 MFA 提供程序：对于利用第三方 MFA 方法的组织，AD FS 提供无缝合并和使用这些身份验证方法的功能。

### <a name="authentication-scope"></a>身份验证范围
在 Windows Server 2012 R2 的 AD FS 中，可以指定一个全局范围的身份验证策略，该策略适用于所有由 AD FS 保护的应用程序和服务。  你还可以为 AD FS 保护的特定应用程序和服务（信赖方信任）设置身份验证策略。 针对特定应用程序（按信赖方信任）指定身份验证策略不会覆盖全局身份验证策略。 如果全局或按信赖方信任身份验证策略需要 MFA，则当用户尝试验证到此信赖方信任时，将触发 MFA。  全局身份验证策略是未配置特定身份验证策略的信赖方信任（应用程序和服务）的回退。

全局身份验证策略适用于受 AD FS 保护的所有信赖方。 可将以下设置配置为全局身份验证策略的一部分：

-   用于主要身份验证的身份验证方法

-   MFA 的设置和方法

-   是否启用设备身份验证。 有关详细信息，请参阅[加入工作区以从任一设备实现 SSO 和无缝第二因素身份验证跨公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

按信赖方信任身份验证策略专门应用到访问该信赖方信任（应用程序或服务）的企图。 可将以下设置配置为按信赖方信任身份验证策略的一部分：

-   是否要求用户在每次登录时都提供其凭据

-   基于用户/组、设备注册和访问请求位置数据的 MFA 设置

### <a name="primary-and-additional-authentication-methods"></a>主要和附加身份验证方法
借助 Windows Server 2012 R2 中的 AD FS，管理员还可以配置其他身份验证方法。 主要身份验证方法是内置的，旨在验证用户的身份。 你可以配置其他身份验证因素，以请求提供有关用户标识的详细信息，从而确保更强的身份验证。

使用 Windows Server 2012 R2 中 AD FS 的主要身份验证，你可以选择以下选项：

-   对于已发布的需要从公司网络外部访问的资源，默认情况下会选择表单身份验证。 此外，你还可以启用证书身份验证（即，可以与 AD DS 配合使用的基于智能卡的身份验证或用户客户端证书身份验证）。

-   对于 Intranet 资源，默认情况下会选择 Windows 身份验证。 此外，你还可以启用表单身份验证和/或证书身份验证。

通过选择多种身份验证方法，可以让用户在应用程序或服务的登录页上选择要使用哪种方法进行身份验证。

还可以针对无缝第二重身份验证启用设备身份验证。 这会将用户的标识绑定到用于访问资源的已注册设备，因此，在访问受保护的资源之前，可以提供更安全的复合身份验证。

> [!NOTE]
> 有关设备对象、设备注册服务、Workplace Join 和设备作为无缝第二重身份验证和 SSO 的详细信息，请参阅[从任何设备加入工作区以实现 sso 和跨公司无缝第二重身份验证应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

如果为 Intranet 资源指定 Windows 身份验证方法（默认选项），身份验证请求将在支持 Windows 身份验证的浏览器上无缝运行此方法。

> [!NOTE]
> 并非所有浏览器都支持 Windows 身份验证。 Windows Server 2012 R2 中 AD FS 的身份验证机制将检测用户的浏览器用户代理，并使用可配置的设置来确定该用户代理是否支持 Windows 身份验证。 管理员可以在此用户代理列表中添加条目（通过 Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents` 命令），以便为支持 Windows 身份验证的浏览器指定备选的用户代理字符串。 如果客户端的用户代理不支持 Windows 身份验证，则默认的回退方法是表单身份验证。

### <a name="configuring-mfa"></a>配置 MFA
在 Windows Server 2012 R2 的 AD FS 中，有两部分配置 MFA：指定需要 MFA 的条件，以及选择其他身份验证方法。 有关其他身份验证方法的详细信息，请参阅[为 AD FS 配置其他身份验证方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)。

**MFA 设置**

以下选项可用于 MFA 设置（在哪些条件下需要 MFA）：

-   可以要求针对联合服务器加入到的 AD 域中的特定用户和组使用 MFA。

-   可以要求针对已注册（已加入工作区）或者未注册（未加入工作区）的设备使用 MFA。

    Windows Server 2012 R2 采用以用户为中心的方法来处理新式设备，其中设备对象表示 user@device 与公司之间的关系。 设备对象是 Windows Server 2012 R2 中 AD 中的新类，在提供对应用程序和服务的访问权限时，可用于提供复合标识。 AD FS 的新组件 - Device Registration Service (DRS) – 可以在 Active Directory 中设置设备标识，以及在使用者设备上设置将用于表示设备标识的证书。 然后，你可以使用此设备标识将设备加入工作区，换而言之，可以将个人设备连接到工作区的 Active Directory。 将个人设备加入工作区时，该设备将成为已知设备，并为受保护的资源和应用程序提供无缝第二重身份验证。 换句话说，设备加入工作区后，用户的标识将绑定到此设备，并可在访问受保护资源之前用于无缝复合标识验证。

    有关工作区加入和离开的详细信息，请参阅[从任何设备加入工作区以实现 SSO 和跨公司应用程序的无缝第二重身份验证](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

-   可以要求在对受保护资源的访问请求来自 Extranet 或 Intranet 时使用 MFA。

## <a name="BKMK_2"></a>方案概述
在此方案中，基于用户的组成员身份数据，对特定应用程序启用 MFA。 换而言之，你将在联合服务器上设置一个身份验证策略，要求属于特定组的用户请求访问 Web 服务器上托管的特定应用程序时使用 MFA。

更具体地说，在此方案中，你将会针对名为 **claimapp** 的基于声明的测试应用程序启用一个身份验证策略，方案中的 AD 用户 **Robert Hatley** 需要运行 MFA，因为他属于 AD 组 **Finance**。

@No__t-0Walkthrough Guide 中提供了设置和验证此方案的分步说明：利用适用于敏感应用程序](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)的附加多重身份验证管理风险。 若要完成本演练中的步骤，你必须设置一个实验室环境，并按照[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中的步骤进行操作。

在 AD FS 中启用 MFA 的其他方案包括：

-   如果访问请求来自 Extranet，则启用 MFA。 您可以修改 [Walkthrough Guide 的 "设置 MFA 策略" 部分中显示的代码：针对敏感应用程序的附加多重身份验证管理风险 @ no__t-0，并提供以下内容：

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   如果访问请求来自未加入工作区的设备，则启用 MFA。  您可以修改 [Walkthrough Guide 的 "设置 MFA 策略" 部分中显示的代码：针对敏感应用程序的附加多重身份验证管理风险 @ no__t-0，并提供以下内容：

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   如果访问请求来自某个用户，并且此用户的某个设备已加入工作区但尚未注册到此用户，则启用 MFA。 您可以修改 [Walkthrough Guide 的 "设置 MFA 策略" 部分中显示的代码：针对敏感应用程序的附加多重身份验证管理风险 @ no__t-0，并提供以下内容：

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>请参阅
[操作实例指南：利用适用于敏感应用程序的附加多重身份验证管理风险 @ no__t-0 @ no__t 为[Windows Server 2012 R2 中 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



