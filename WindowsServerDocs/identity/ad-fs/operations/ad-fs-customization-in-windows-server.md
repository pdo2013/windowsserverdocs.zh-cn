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
ms.openlocfilehash: b8832e7e53e94761a489e850726bbd206b8be62b
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863434"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Windows Server 2016 中的 AD FS 自定义


为了响应来自使用 AD FS 的组织的反馈, 我们添加了其他工具来自定义由 AD FS 保护的单个应用程序的用户登录体验。  
除了指定基于应用程序的 web 内容 (如说明文本和链接) 外, 现在可以为每个应用程序指定整个 web 主题。  这包括徽标、插图、样式表或整个 onload 文件。  
  
## <a name="global-settings"></a>全局设置    
对于常规全局设置, 可以参阅自定义 Windows Server 2012 R2 中 AD FS 附带的[AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="pre-requisites"></a>先决条件  
尝试本文档中所述的过程之前, 需要满足以下先决条件。  
  
-   Windows Server 2016 TP4 或更高版本中的 AD FS  
  
## <a name="configure-ad-fs-relying-parties"></a>配置 AD FS 信赖方  
可以使用下面的 PowerShell 示例配置每个信赖方登录 web 元素和主题:  
  
### <a name="customize-messages"></a>自定义消息  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>自定义公司名称、徽标和图像  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>自定义整页  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>自定义主题和高级自定义主题  
  
对于自定义主题, 请参阅[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)和[AD FS 登录页的高级自定义。](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>为每个 RP 分配自定义 web 主题  
  
若要为每个 RP 分配自定义主题, 请使用以下过程:  
  
1. 在 AD FS 中创建一个新主题作为默认全局主题的副本  
`New-AdfsWebTheme -Name AppSpecificTheme -SourceName default`  
2. 导出自定义主题  
`Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme`  
3. 自定义主题文件 (图像、css、onload)-在你喜爱的编辑器中, 或替换文件  
4. 将自定义文件从文件系统导入到 AD FS (面向新主题)  
`Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}`  
5. 向特定 RP (或 RP) 应用新的自定义主题  
`Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme`  
  
## <a name="home-realm-discovery"></a>主领域发现  
对于 home 领域发现自定义, 请参阅[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="updated-password-page"></a>已更新密码页  
有关自定义 "更新密码" 页的信息, 请参阅[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="customizing-and-alternate-ids"></a>自定义和备用 Id  
用户可以使用 Active Directory 域服务 (AD DS) 所接受的任意形式的用户标识符登录到已启用 Active Directory 联合身份验证服务 (AD FS) 的应用程序。 其中包括用户主体名称 (upn) (johndoe@contoso.com) 或域限定 sam 帐户名称 (contoso\johndoe 或 com\johndoe)。  有关详细信息, 请参阅[配置备用登录 ID。](Configuring-Alternate-Login-ID.md)  
  
你可能还需要自定义 AD FS 登录页面, 以便向最终用户授予有关备用登录 ID 的一些提示。 可以通过添加自定义登录页说明来完成此操作。有关详细信息, 请参阅[自定义 AD FS 登录页。](https://technet.microsoft.com/library/dn280950.aspx)   
  
还可以通过在用户名字段上自定义 "使用组织帐户登录" 字符串来实现此目的。  有关详细信息, 请参阅[AD FS 登录页的高级自定义](https://technet.microsoft.com/library/dn636121.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
