---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Windows Server 2016 中的 AD FS 自定义
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b5aa22ad99529d99e2d7381a434916e8e749f185
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188762"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Windows Server 2016 中的 AD FS 自定义


在响应中使用 AD FS 的组织的反馈，我们添加了其他工具自定义用户的登录体验为受 AD FS 的单个应用程序。  
除了指定每个应用程序 web 内容，例如说明文本和链接，现在还可以指定每个应用程序的整个 web 主题。  这包括徽标、 插图、 样式表或整个 onload.js 文件。  
  
## <a name="global-settings"></a>全局设置    
有关常规的全局设置您可以访问[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)，随 Windows Server 2012 R2 中的 AD FS。  
  
## <a name="pre-requisites"></a>先决条件  
尝试此文档中所述的过程之前，需要进行以下系统必备组件。  
  
-   在 Windows Server 2016 TP4 或更高版本的 AD FS  
  
## <a name="configure-ad-fs-relying-parties"></a>配置 AD FS 信赖方  
每个信赖方登录 web 元素和主题可以配置使用以下 PowerShell 示例：  
  
### <a name="customize-messages"></a>自定义消息  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>自定义公司名称、 徽标和图像  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>自定义整个页面  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>自定义主题和高级自定义主题  
  
有关自定义主题，请参阅[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)和[Advanced Customization of AD FS 登录页。](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>分配每个 RP 的自定义 web 主题  
  
若要分配每个 RP 自定义主题，请使用以下过程：  
  
1. 默认情况下，AD FS 中的全局主题的副本作为创建新的主题  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code> 2.  导出自定义主题 <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>  
3. 在你喜爱的编辑器中自定义主题文件 （图像、 css、 onload.js）-或替换第 4 个文件。 从文件系统的自定义的文件导入 AD FS （面向新主题） <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>  
5. 将新的自定义主题应用到特定 RP （或 RP 的） <code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>主领域发现  
有关主领域发现自定义请参见[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="updated-password-page"></a>更新的密码页面  
有关自定义更新密码页面信息，请参阅[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="customizing-and-alternate-ids"></a>自定义和备用 Id  
用户可以登录到 Active Directory 联合身份验证服务 (AD FS)-已启用应用程序使用任何形式的接受的 Active Directory 域服务 (AD DS) 的用户标识符。 其中包括用户主体名称 (Upn) (johndoe@contoso.com) 或域限定 sam 帐户名 （contoso\johndoe 或 contoso.com\johndoe）。  有关详细信息，在此，请参阅[配置备用登录 id。](Configuring-Alternate-Login-ID.md)  
  
另外要自定义 AD FS 登录页后，可以为最终用户提供一些提示有关备用登录 id。 可以通过添加详细信息，请参阅自定义登录页说明操作即可[自定义 AD FS 登录页。](https://technet.microsoft.com/library/dn280950.aspx)   
  
您还可以执行此操作通过自定义用户名字段的上面"使用组织帐户登录"的字符串。  为此，请参阅上的信息[Advanced Customization of AD FS 登录页](https://technet.microsoft.com/library/dn636121.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
