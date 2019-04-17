---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: "在 Windows Server 2016 的广告 FS 自定义"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: a2699e93a0a19cb138c1fde0c9af36774a12f865
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2017
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>在 Windows Server 2016 的广告 FS 自定义

>适用于：Windows Server 2016

响应从使用广告 FS 组织的反馈时，我们添加了额外的工具来自定义用户登录由广告 FS 受保护的各应用程序的体验。  
除了指定等描述文本和链接的每个应用程序的 web 内容时，现在你可以指定整个 web 主题每个应用程序。  这包括徽标、的插图、样式表或整个 onload.js 文件。  
  
## <a name="global-settings"></a>全球设置    
常规全球设置为你可以参考[自定义的广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)所附带广告 FS 在 Windows Server 2012 R2。  
  
## <a name="pre-requisites"></a>先决条件  
在本文中所述的步骤前必须以下先决条件。  
  
-   广告 FS 在 Windows Server 2016 TP4 或更高版本  
  
## <a name="configure-ad-fs-relying-parties"></a>配置广告 FS 依赖方  
每个信赖登录网站方元素和主题可以配置使用下面的示例 PowerShell:  
  
### <a name="customize-messages"></a>自定义邮件  
  
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
  
### <a name="customize-entire-page"></a>自定义整个页面  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>自定义主题和高级自定义主题  
  
有关自定义主题，请参阅[自定义的广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)和[高级的自定义的广告 FS 登录页面。](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>指定 RP 每自定义 web 主题  
  
若要指定 RP 每自定义主题，请使用以下步骤：  
  
1. 作为默认情况下，在广告 FS 全球主题的副本创建一个新的主题  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code>  
2.  导出的自定义主题<code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>3.自主题文件（图像、css、onload.js）-在你最喜爱的编辑器或更换文件 4。 导入自定义的文件文件系统（定位新的主题）广告 FS 到<code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>5.适用于特定 RP（或 RP 的）的新的自定义主题
<code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>家庭领域发现  
自定义有关主页领域发现[自定义的广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="updated-password-page"></a>更新的密码页面  
自定义的更新密码页面上的信息，请参阅[自定义广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="customizing-and-alternate-ids"></a>自定义和备用 Id  
用户可以登录到 Active Directory 联合身份验证服务 (广告 FS)-启用应用程序使用任何形式的用户标识符接受的 Active Directory 域服务 (广告 DS)。 其中包括 (Upn) 的用户主要名称 (johndoe@contoso.com) 或域限定三千帐户名称（contoso\johndoe 或 contoso.com\johndoe）。  有关详细信息此查看[配置替代登录 id。](Configuring-Alternate-Login-ID.md)  
  
此外希望自定义广告 FS 登录页面，以向最终用户提供一些提示有关替代登录 id。 可以通过添加详细信息，请参阅的自定义登录页面说明进行操作[自定义广告 FS 登录页面。](https://technet.microsoft.com/library/dn280950.aspx)   
  
你还可以通过上面用户名字段中的"使用组织帐户登录"的字符串自定义执行此操作。  有关此查看[高级的自定义的广告 FS 登录页面](https://technet.microsoft.com/library/dn636121.aspx)。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
