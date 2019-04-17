---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: "自定义的错误消息，以广告 FS 登录页面"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a015c27f784d5b1f488f984450612820d4a06b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>自定义的错误消息，以广告 FS 登录页面  

>适用于：Windows Server 2016，Windows Server 2012 R2

你可以到你的组织中配置可以量身定制的自定义的错误消息。 下图显示自定义的错误页面描述和一般性错误消息。 使用下面的 Windows PowerShell cmdlet 自定义你的错误消息。  
  
![自定义错误](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>自定义错误页面说明  
若要自定义错误页面说明，请使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>自定义的一般性错误消息  
若要自定义的一般性错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>自定义授权错误消息  
若要自定义授权错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>自定义设备身份验证错误消息  
若要自定义设备身份验证错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>自定义的支持电子邮件错误消息  
你可以在广告 FS 配置的支持电子邮件地址。 如果配置，广告 FS 自动显示为最终用户提供电子邮件错误的详细信息的链接。  
  
若要自定义的支持电子邮件的错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>自定义信赖的方授权消息  
你可以在广告 FS 配置信赖的方授权错误消息。  
  
若要自定义信赖方错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)    
