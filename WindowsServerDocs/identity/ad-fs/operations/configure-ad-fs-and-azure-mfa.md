---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: "广告金融服务 2016 年和 Azure MFA 配置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be8e88ae36f344f1265fb76e66c19e0ac8aeb533
ms.sourcegitcommit: ffdfbb76c7f775e20d089d3f662d4f9a7d642f1e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2018
---
# <a name="configure-ad-fs-2016-and-azure-mfa"></a>广告金融服务 2016 年和 Azure MFA 配置

>适用于：Windows Server 2016

如果你的组织使用 Azure AD 联合，你可以使用 Azure 多重身份验证安全广告 FS 资源，在本地到并在云中。 Azure MFA 使您能够消除密码并提供更安全的方式进行身份验证。  从 Windows Server 2016 开始，你可以现在 Azure MFA 为主要身份验证配置。 
  
与不同与在 Windows Server 2012 R2 的广告 FS 广告 FS 2016 Azure MFA 适配器直接与 Azure AD 集成，不需要本地 Azure MFA 服务器上。   Azure MFA 适配器已内置于 Windows Server 2016，并且无需额外安装。

## <a name="registering-users-for-azure-mfa-with-ad-fs-2016"></a>对于与广告 FS 2016 Azure MFA 注册用户
广告 FS 不支持内联"证明向上"或注册的 Azure MFA 安全验证信息，例如电话号码或移动的应用。 这意味着通过访问之前使用 MFA Azure AD FS 应用程序对进行身份验证 https://account.activedirectory.windowsazure.com/Proofup.aspx 必须向上获取考验用户。 当尝试使用 Azure AD FS 在的 MFA 进行身份验证已不尚未考验向上在 Azure AD 的用户时，他们将获得广告 FS 错误。  作为广告 FS 管理员，你可以自定义到 proofup 页面改为指导用户此错误体验。  你可以执行此操作使用 onload.js 自定义检测广告 FS 页面中的错误消息字符串，并显示新消息指导用户访问 https://aka.ms/mfasetup，然后重新尝试身份验证。 详细指导的下方看到"自定义广告 FS 网页指导用户 MFA 验证方法注册"在此篇文章中。

>[!NOTE]
> 以前，用户需要具有 MFA 注册（访问 https://account.activedirectory.windowsazure.com/Proofup.aspx，例如通过 aka.ms/mfasetup 的快捷方式）的身份验证。  未注册 MFA 验证信息广告 FS 用户现在，可以访问 Azure AD 的快捷方式 aka.ms/mfasetup 通过 proofup 页面使用仅主要身份验证 (如 Windows 的集成身份验证或用户名和密码广告 FS 通过 web 页面)。  如果用户没有配置的验证方法、Azure AD 将执行内联注册用户会看到消息"你的管理员已要求你设置的其他安全验证该帐户"，并然后，用户可以选择"设置它现在"。
> 已具有至少一个 MFA 验证方法配置用户仍将提示您提供 MFA 访问 proofup 页面。

### <a name="recommended-deployment-topologies"></a>推荐的部署拓扑

#### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA 与主要身份验证
有几个要 Azure MFA 用作与广告 FS 主要身份验证的出色原因：
 - 若要避免密码登录到 Azure AD、Office 365 和其他广告 FS 应用
 - 保护密码基于登录通过要求如密码之前的验证码其他因素

如果你想要作为主要身份验证方法广告 FS 中使用 Azure MFA 获得以下好处，你可能还想要保留能够使用 Azure AD 条件访问，包括"真 MFA"按提示输入其他在广告 FS 因素。

你现在可以通过配置 Azure AD 域设置如何 MFA 本地（到 $True 设置"SupportsMfa"）上的执行此操作。  在此配置，广告 FS 可以会提示您按 Azure AD 条件访问方案需要它用于进行额外的身份验证或"如此 MFA"。  

如上所述，任何广告 FS 用户有尚未应通过自定义的广告 FS 错误页面提示注册（配置的 MFA 验证信息）来访问 aka.ms/mfasetup 配置验证信息，然后重新尝试广告 FS 登录。  
因为为主 Azure MFA 被认为唯一的因素之后初始配置用户将需要提供其他因素，若要管理或更新中的 Azure AD，或访问其他其验证信息需要 MFA 的资源。


#### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>作为额外的身份验证到 Office 365 azure MFA
如果想要 Office 365 或信赖的第三方广告 FS 中额外的身份验证方法为拥有 Azure MFA，最佳选项以前配置 Azure AD 如何 Azure AD 时触发复合 MFA，在本地中广告金融服务和 MFA 执行主要身份验证。 现在，你可以使用 Azure MFA 作为额外的身份验证在广告 FS 域 SupportsMfa 设置设为 $True 时。  

如上所述，任何广告 FS 用户有尚未应通过自定义的广告 FS 错误页面提示注册（配置的 MFA 验证信息）来访问 aka.ms/mfasetup 配置验证信息，然后重新尝试广告 FS 登录。  


## <a name="pre-requisites"></a>先决条件  
使用 Azure MFA 进行身份验证与广告 FS 时，将需要以下先决条件：  
  
- [使用 Azure Active Directory Azure 订阅](https://azure.microsoft.com/pricing/free-trial/)。  
- [Azure 多重身份验证](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  

>[!NOTE]   
> Azure AD 和 Azure MFA 将包括在 Azure AD 高级版和企业移动套件 (EMS)。  如果你有任何一种方法不需要单独的订阅。   
- Windows Server 2016 广告 FS 本地环境。  
- 你的本地环境[使用 Azure AD 联盟。](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Windows Azure Active Directory 的 Windows PowerShell 模块](https://go.microsoft.com/fwlink/p/?linkid=236297)。  
- 全球管理员权限的 Azure AD 你实例上使用 Azure AD PowerShell 配置。  
- 若要配置 MFA Azure AD FS 电场的日落企业管理员凭据。  
  
  
## <a name="configure-the-ad-fs-servers"></a>配置广告 FS 服务器  
为了完成 MFA Azure AD fs 配置，你需要配置每个使用所述的步骤的广告 FS 服务器。 

>[!NOTE]
>确保在执行这些步骤**所有**场中的广告 FS 服务器。 如果你已在你电场的日落有多个广告 FS 服务器，你可以执行远程使用 Azure AD Powershell 必要的配置。  
  
  
  
### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>第 1 步：使用每个广告 FS server 生成的 Azure MFA 证书`New-AdfsAzureMfaTenantCertificate`cmdlet。   
你需要执行第一个改进就是生成的 Azure MFA 使用证书。  这可以使用 PowerShell。  可以在本地计算机证书商店中，找到生成的证书并将它与主题名称中包含 Azure AD 目录 TenantID 标记。    
  
![广告金融服务和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  
  
请注意，TenantID 你目录中 Azure AD 的名称。  使用以下 PowerShell cmdlet 生成新的证书。  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  
      
![广告金融服务和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-azure-multi-factor-auth-client-spn"></a>第 2 步：添加新的凭据 Azure 多重身份验证的客户端 SPN 到   
启用广告 FS 服务器与 Azure 多重身份验证的客户端进行通信，以便你需要添加 spn Azure 多重身份验证的客户端的凭据。 使用生成的证书`New-AdfsAzureMFaTenantCertificate`cmdlet 将作为这些的凭据。 执行以下使用 PowerShell Azure 多重身份验证的客户端 SPN 中添加新的凭据。  

>[!NOTE]   
>为了完成此步骤，你需要连接到 Azure AD PowerShell 使用 Connect-MsolService 与你实例。  这些步骤假设你已连接通过 PowerShell。  有关信息，请参阅[连接 MsolService。](https://msdn.microsoft.com/library/dn194123.aspx)  
     
  
2. **设置为新的凭据的证书，对 Azure 多重身份验证的客户端**  
    
    `New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64 `

>[!IMPORTANT]
>  需要在所有你电场的日落中的广告 FS 服务器上运行该命令。  Azure ADMFA 将无法在已具有对 Azure 多重身份验证的客户端设置为新的凭据证书的服务器上。 

>[!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 是 Azure 多重身份验证的客户端 guid。  
  
## <a name="configure-the-ad-fs-farm"></a>配置广告 FS 电场的日落  
  
完成每个广告 FS 服务器上的上一节后，你将需要运行`Set-AdfsAzureMfaTenant`cmdlet。  
  
此 cmdlet 需要为广告 FS 场执行只有一次。  使用 PowerShell 完成此步骤。    
  
>[!NOTE]  
>你将需要重启一场中的每个服务器上的广告 FS 服务，这些更改生效之前。  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  
      
![广告金融服务和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
此后，你将看到 Azure MFA 可以作为主要身份验证方法为 intranet 和外部网络使用。    
  
![广告金融服务和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>续订和管理广告 FS Azure MFA 证书
以下指南指导你如何管理您的广告 FS 服务器上的 Azure MFA 证书。
默认情况下，当您使用 Azure MFA 配置广告 FS 证书生成通过 New-AdfsAzureMfaTenantCertificate PowerShell cmdlet 为期 2 年是否有效。  如何关闭以确定到期证书，以及然后续订，并且安装新的证书，请使用下面的过程。

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>评估广告 FS Azure MFA 证书到期日期
每个广告 FS 服务器，在本地计算机上我的应用商店中，将使用自我的签名的证书"OU = Microsoft 广告 FS Azure MFA"中的发行商和主题。  这是 Azure MFA 证书。  检查该证书若要确定过期日期每个广告 FS 服务器上的有效期。  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>每个广告 FS 服务器上创建新 FS Azure AD MFA 证书
如果你证书的有效期内即将到期，首先续订进程生成新的 Azure MFA 证书每个广告 FS 服务器上。 Powershell 命令窗口中，在生成一个新的证书，使用以下 cmdlet 每个广告 FS 服务器上：

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

由于此 cmdlet，将生成有效将来 2 天从 2 天 + 2 年到一个新的证书。  此 cmdlet 或新证书将不会影响广告金融服务和 Azure MFA 操作。 (请注意：2 天延迟有意且提供了执行以下步骤来组织中配置新证书，广告 FS 开始使用 Azure MFA 之前的时间。)


### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>在 Azure AD 组织中配置每个新的广告 FS Azure MFA 证书
使用 Azure AD PowerShell 模块的每个新证书（每个广告 FS 在服务器上），更新 Azure AD 租户设置按以下步骤 (请注意：你必须首次连接到使用 Connect-MsolService 运行以下命令租户）。

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $certbase64
```
    Where $certbase64 is the new certificate.  The base64 encoded certificate can be obtained by exporting the certificate (without the private key) as a DER encoded file and opening in Notepad.exe, then copy/pasting to the PSH session and assigning to the variable $certbase64

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>确认新的证书将用于 Azure MFA
一次新证书成为有效广告 FS 将它们买和开始使用 Azure MFA 为在一天到数个小时内的每个各自的证书。  后发生这种情况，在每个服务器你将看到广告 FS 管理员事件日志中提供以下信息中记录的事件：日志名称：广告 FS/管理员源：广告 FS 日期：2018 年 2 月 27 日晚上 7:33:31 事件 ID: 547 任务类别：None 级别：信息关键字：广告 FS 用户：DOMAIN\adfssvc 计算机：ADFS.domain.contoso.com 说明：Azure MFA 租户证书已续订。  

TenantId: contoso.onmicrosoft.com。旧的指纹：7CC103D60967318A11D8C51C289EF85214D9FC63。 旧的到期日期：9 月 15/2019 年晚上 9:43:17。 新的指纹：8110D7415744C9D4D5A4A6309499F7B48B5F3CCF。 新的到期日期：2/27/2020 年 2:16:07 上午。

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>广告 FS 网页指导用户注册 MFA 验证方法自定义

使用下面的示例，以自定义您不具有尚未考验向上用户的广告 FS 网页（配置 MFA 验证信息）。

### <a name="find-the-error"></a>查找该错误
首先，有几个不同的错误消息广告 FS 将返回的用户缺少验证信息以防万一。
如果你正在使用 Azure MFA 作为主要身份验证，未 proofed 用户将看到广告 FS 错误页面包含以下消息：
```
    <div id="errorArea"> 
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred 
        </div> 
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information. 
        </div>
```
当试图作为额外的身份验证的 Azure AD 时，未 proofed 用户将看到广告 FS 错误页面包含以下消息：
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

### <a name="catch-the-error-and-update-the-page-text"></a>捕获错误并更新页面文本
捕捉错误和显示用户自定义指南就为已属于广告 FS onload.js 文件终止附加 javascript web（1）中搜索标识错误字符串，（2）提供的自定义的主题 web 内容。  (指南一般而言如何自定义 onload.js 文件，请参阅[此处](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/advanced-customization-of-ad-fs-sign-in-pages)。)

