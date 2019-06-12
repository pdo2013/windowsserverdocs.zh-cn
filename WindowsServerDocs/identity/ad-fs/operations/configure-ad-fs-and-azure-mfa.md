---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: 配置 AD FS 2016 和 Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 62b366b8fa388319a758ab853d28d1c49cb1bf06
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719717"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>将 Azure MFA 配置为使用 AD FS 身份验证提供程序

如果你的组织已与 Azure AD 联合，您可以到安全的 AD FS 资源，同时在本地和云中使用 Azure 多重身份验证。 Azure MFA，可消除密码并提供更安全的方式进行身份验证。  从 Windows Server 2016 开始，你现在可以将 Azure MFA 配置为主要身份验证或将其用作其他身份验证提供程序。 
  
与不同 Windows Server 2012 R2 中的 AD FS 与 AD FS 2016 Azure MFA 适配器直接与 Azure AD 集成并不需要本地 Azure MFA 服务器。   Azure MFA 适配器内置于 Windows Server 2016 中，并且没有无需进行其他安装。


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>针对 Azure MFA 的 AD FS 注册用户

AD FS 不支持内联&#34;证明了&#34;，或注册 Azure MFA 安全验证信息，例如电话号码或移动应用。 这意味着用户必须获取访问防向上[ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx)之前使用 Azure MFA 进行身份验证对 AD FS 应用程序。 时具有未尚未还会适应了在 Azure AD 尝试使用在 AD FS 的 Azure MFA 进行身份验证的用户，他们将得到一个 AD FS 错误。  作为 AD FS 管理员，可以自定义此错误体验，使用户引导至 proofup 页相反。  你可以使用 onload.js 自定义检测在 AD FS 页中的错误消息字符串并显示新消息，引导用户访问[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)，然后重新尝试执行身份验证。 有关详细指南请参阅"自定义 AD FS 网页来指导用户注册 MFA 的验证方法"下面这篇文章中。

>[!NOTE]
> 以前，用户需要使用 MFA 进行注册的身份验证 (访问[ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx)，例如通过快捷方式[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup))。  现在，未注册 MFA 验证信息的 AD FS 用户可以访问 Azure AD&#34;s proofup 页，通过该快捷方式[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)仅使用主身份验证 (如 Windows 集成身份验证或用户名和密码通过 AD FS web 页面)。  如果用户不没有配置任何验证方法，Azure AD 将执行内联注册用户将在其中看到消息&#34;管理员要求设置其他安全性验证此帐户&#34;，然后，用户可以和选择&#34;立即设置&#34;。
> 已配置至少一个 MFA 验证方法的用户将仍会提示您提供 MFA 访问 proofup 页时。

## <a name="recommended-deployment-topologies"></a>建议的部署拓扑

### <a name="azure-mfa-as-primary-authentication"></a>主要身份验证作为 azure MFA

有几个重要原因，若要使用 Azure MFA 与 AD FS 主身份验证方式：

 - 若要避免使用密码登录到 Azure AD 为 Office 365 和其他 AD FS 的应用
 - 若要保护密码基于登录通过要求的另一因素如密码之前的验证码

如果你想要使用 Azure MFA 作为 AD FS 中的主要身份验证方法，以获得这些益处，您可能还想要保留的功能使用 Azure AD 条件性访问，包括&#34;true MFA&#34; ，通过提示输入 AD FS 中的其他因素。

现在可以执行此操作通过配置为在本地的 MFA 的 Azure AD 域设置 (设置&#34;SupportsMfa&#34;到 $True)。  在此配置中，AD FS 可以将 Azure AD 提示提供执行其他身份验证或&#34;true MFA&#34;需要它的条件性访问方案。  

上文所述，任何 AD FS 用户未注册 （已配置的 MFA 验证信息） 应提示用户通过与自定义 AD FS 的错误页以访问[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)配置验证信息，然后重新尝试执行 AD FS 登录名。  
因为作为主 Azure MFA 被视为一个因素后初始配置的用户将需要提供另一个因素来管理或更新 Azure AD 中的其验证信息或访问其他资源的要求进行 MFA。

>[!NOTE]
> ADFS 2019 所需对 Active Directory 声明提供方信任的定位点声明类型进行修改并将修改这从 windowsaccountname UPN。 执行下面提供的 PowerShell cmdlet。 这具有对 AD FS 场的内部功能没有影响。 您可能会注意到一些用户可能会重新提示输入凭据后进行此更改。 再次登录之后, 最终用户会看到任何区别。 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>作为附加身份验证到 Office 365 的 azure MFA

以前，如果想要获取 Azure MFA 与 AD FS 中的 Office 365 或其他信赖方，最佳选择附加身份验证方法来配置 Azure AD，以执行复合的 MFA，在 AD FS 和 MFA 在本地执行主要身份验证是 tr通过 Azure AD iggered。 现在，你可以使用 Azure MFA 作为附加身份验证 AD FS 中，域 SupportsMfa 设置设置为 $True 时。  

上文所述，任何 AD FS 用户未注册 （已配置的 MFA 验证信息） 应提示用户通过与自定义 AD FS 的错误页以访问[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)配置验证信息，然后重新尝试执行 AD FS 登录名。  

## <a name="pre-requisites"></a>系统必备组件

使用 Azure MFA 进行身份验证使用 AD FS 时，以下系统必备组件是必需的：  
  
- [Azure 订阅与 Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/)。  
- [Azure 多重身份验证](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) 


> [!NOTE]
> Azure AD 和 Azure MFA 都包括在 Azure AD Premium 和企业移动性套件 (EMS)。  如果有任何一种方法不需要单独的订阅。

- Windows Server 2016 AD FS 在本地环境。  
   - 服务器需要能够通过端口 443 与以下 Url 通信。
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- 在本地环境是[与 Azure AD 联合。](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Windows Azure Active Directory 模块的 Windows PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0)。  
- 全局管理员权限在 Azure AD 的实例上对其使用 Azure AD PowerShell 进行配置。  
- 若要为 Azure MFA 配置 AD FS 场的企业管理员凭据。  
  
## <a name="configure-the-ad-fs-servers"></a>配置 AD FS 服务器

若要完成 Azure MFA for AD FS 配置，你需要配置每个 AD FS 服务器使用所述的步骤。 

>[!NOTE]
>确保在执行这些步骤**所有**AD FS 服务器场中的。 如果场中有多个 AD FS 服务器，则可以执行远程使用 Azure AD PowerShell 所需的配置。  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>第 1 步：为 Azure MFA 中生成证书，每个 AD FS 服务器使用`New-AdfsAzureMfaTenantCertificate`cmdlet

需要执行第一件事是生成适用于 Azure MFA，若要使用的证书。  这可以使用 PowerShell。  可以在本地计算机证书存储中找到生成的证书和使用者名称包含在 Azure AD 目录的 TenantID 标记。

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

请注意 TenantID，是在 Azure AD 中目录的名称。  使用以下 PowerShell cmdlet 以生成新证书。  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>步骤 2：将新凭据添加到 Azure 多重身份验证客户端的服务主体

若要启用 AD FS 服务器与 Azure 多重身份验证客户端进行通信，需要将凭据添加到 Azure 多重身份验证客户端服务主体。 使用生成的证书`New-AdfsAzureMFaTenantCertificate`cmdlet 将作为这些凭据。 执行以下操作使用 PowerShell 将新凭据添加到 Azure 多重身份验证客户端的服务主体。  

> [!NOTE]
> 若要完成此步骤中需要连接到 Azure AD PowerShell 中使用的实例`Connect-MsolService`。  这些步骤假定你已通过 PowerShell 连接。  有关信息请参阅[ `Connect-MsolService`。](https://msdn.microsoft.com/library/dn194123.aspx)  

**针对 Azure 多重身份验证客户端设置为新的凭据的证书**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> 需要在所有场中的 AD FS 服务器上都运行此命令。  Azure AD MFA 将在不具有针对 Azure 多重身份验证客户端设置为新的凭据的证书的服务器上失败。

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 是 Azure 多因素身份验证客户端的 GUID。  
  
## <a name="configure-the-ad-fs-farm"></a>配置 AD FS 场  
  
完成每个 AD FS 服务器上的一节后,，你将需要运行`Set-AdfsAzureMfaTenant`cmdlet。  
  
此 cmdlet 需要执行一次即可为 AD FS 场。  使用 PowerShell 来完成此步骤。
  
> [!NOTE]  
> 你将需要重启场中的每个服务器上的 AD FS 服务，才能使这些更改的影响。  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
在此之后，您将看到 Azure MFA 是作为 intranet 和 extranet 使用的主要身份验证方法。    
  
![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>续订和管理 AD FS Azure MFA 证书

以下指南指导您如何管理在 AD FS 服务器上的 Azure MFA 证书。
默认情况下，当使用 Azure MFA 配置 AD FS 证书通过生成`New-AdfsAzureMfaTenantCertificate`PowerShell cmdlet 的有效期为 2 年。  若要确定如何接近过期证书，以及然后续订并安装新证书，请使用以下过程。

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>评估 AD FS Azure MFA 证书到期日期

每个 AD FS 服务器，在本地计算机上我的存储，将使用自签名的证书&#34;OU = Microsoft AD FS Azure MFA&#34;中的颁发者和使用者。  这是 Azure MFA 证书。  检查以确定到期日期每个 AD FS 服务器上此证书的有效期。  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>每个 AD FS 服务器上创建新的 AD FS Azure MFA 证书

如果您的证书的有效期即将达到其最终，通过生成每个 AD FS 服务器上的新 Azure MFA 证书开始续订过程。 在 PowerShell 命令窗口中，生成使用以下 cmdlet 的每个 AD FS 服务器上的新证书：

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

由于此 cmdlet 将生成的有效日期从将来 2 天到 2 天 + 2 年的新证书。  此 cmdlet，则新证书将不会影响 AD FS 和 Azure MFA 的操作。 (注意： 2 天的延迟是有意而为，并提供执行以下步骤来使用 Azure mfa AD FS 前租户中配置新的证书的时间。)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>在 Azure AD 租户中配置每个新的 AD FS Azure MFA 证书

使用 Azure AD PowerShell 模块，为每个新证书 （在每个 AD FS 服务器上），更新 Azure AD 租户设置，如下所示 (注意： 必须先连接到使用租户`Connect-MsolService`运行以下命令)。

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

`$certbase64` 是的新证书。  可以通过为 DER 编码的文件和中 Notepad.exe，打开，然后复制/粘贴到 PowerShell 会话并为变量分配导出 （不带私钥） 的证书获取的 base64 编码证书`$certbase64`。

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>请验证新的证书将用于 Azure MFA

一旦新证书变为有效，AD FS 将选取它们，并开始使用 Azure mfa 在一天到几小时内的每个相应的证书。  一旦发生这种情况，将每个服务器上看到以下信息在 AD FS 管理员事件日志中记录一个事件：

```
Log Name:      AD FS/Admin
Source:        AD FS
Date:          2/27/2018 7:33:31 PM
Event ID:      547
Task Category: None
Level:         Information
Keywords:      AD FS
User:          DOMAIN\adfssvc
Computer:      ADFS.domain.contoso.com
Description:
The tenant certificate for Azure MFA has been renewed.  

TenantId: contoso.onmicrosoft.com.
Old thumbprint: 7CC103D60967318A11D8C51C289EF85214D9FC63.
Old expiration date: 9/15/2019 9:43:17 PM.
New thumbprint: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF.
New expiration date: 2/27/2020 2:16:07 AM.
```

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>自定义 AD FS 网页，以指导用户注册 MFA 的验证方法

使用下面的示例自定义 AD FS 网页的用户不具有尚未防向上 （配置 MFA 验证信息）。

### <a name="find-the-error"></a>找出错误

首先，有几个 AD FS 将在该用户在其中缺少验证信息的情况下返回不同的错误消息。
如果你使用 Azure MFA 作为主要身份验证，取消已通过用户将看到包含以下消息的 AD FS 错误页面：
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
如果正在尝试作为附加身份验证的 Azure AD，取消已通过用户将看到包含以下消息的 AD FS 错误页面：
```
<div id='mfaGreetingDescription' class='groupMargin'>For security reasons, we require additional information to verify your account (mahesh@jenfield.net)</div>
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            The selected authentication method is not available for &#39;username@contoso.com&#39;. Choose another authentication method or contact your system administrator for details.
        </div>
```

### <a name="catch-the-error-and-update-the-page-text"></a>捕获错误并更新页文本

若要捕获错误并向用户显示自定义指导只需将 javascript 追加到 onload.js 文件，属于 AD FS web 主题的末尾。  这允许您执行以下操作：
 - 搜索标识的错误字符串
 - 提供的自定义 web 内容。  

> [!NOTE]
> 有关如何自定义 onload.js 文件的常规指南，请参阅文章[Advanced Customization of AD FS 登录页](advanced-customization-of-ad-fs-sign-in-pages.md)。

下面是一个简单的示例，你可能想要扩展：

1. 在主 AD FS 服务器上打开 Windows PowerShell 并运行以下命令创建新的 AD FS Web 主题：
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. 接下来，创建文件夹，并导出默认 AD FS Web 主题：

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. 在文本编辑器中打开 C:\Theme\script\onload.js 文件
4. 将以下代码追加到 onload.js 文件的末尾
    
    ``` JavaScript
    //Custom Code
    //Customize MFA exception
    //Begin

    var domain_hint = "<YOUR_DOMAIN_NAME_HERE>";
    var mfaSecondFactorErr = "The selected authentication method is not available for";
    var mfaProofupMessage = "You will be automatically redirected in 5 seconds to set up your account for additional security verification. Once you have completed the setup, please return to the application you are attempting to access.<br><br>If you are not redirected automatically, please click <a href='{0}'>here</a>."
    var authArea = document.getElementById("authArea");
    if (authArea) {
        var errorMessage = document.getElementById("errorMessage");
        if (errorMessage) {
            if (errorMessage.innerHTML.indexOf(mfaSecondFactorErr) >= 0) {

                //Hide the error message
                var openingMessage = document.getElementById("openingMessage");
                if (openingMessage) {
                    openingMessage.style.display = 'none'
                }
                var errorDetailsLink = document.getElementById("errorDetailsLink");
                if (errorDetailsLink) {
                    errorDetailsLink.style.display = 'none'
                }

                //Provide a message and redirect to Azure AD MFA Registration Url
                var mfaRegisterUrl = "https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1&whr=" + domain_hint;
                errorMessage.innerHTML = "<br>" + mfaProofupMessage.replace("{0}", mfaRegisterUrl);
                window.setTimeout(function () { window.location.href = mfaRegisterUrl; }, 5000);
            }
        }
    }

    //End Customize MFA Exception
    //End Custom Code
    ```
    > [!IMPORTANT]
    > 您需要更改"< YOUR_DOMAIN_NAME_HERE >";若要使用你的域名。 例如：`var domain_hint = "contoso.com";`
    
5. 保存 onload.js 文件
6. 通过键入以下 Windows PowerShell 命令将 onload.js 文件导入到您的自定义主题：
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}
    ```
7. 最后，将自定义 AD FS Web 主题应用通过键入以下 Windows PowerShell 命令：
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>后续步骤

[管理 TLS/SSL 协议和使用的 AD FS 和 Azure MFA 的密码套件](manage-ssl-protocols-in-ad-fs.md)
