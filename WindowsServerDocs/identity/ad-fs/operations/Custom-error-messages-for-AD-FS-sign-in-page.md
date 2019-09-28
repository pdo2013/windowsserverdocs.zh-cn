---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: AD FS 登录页的自定义错误消息
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 98cd1dd6763886a9b9f63ab6eca1c52094424284
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407552"
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>AD FS 登录页的自定义错误消息  


可以对你的组织配置可以定制的自定义错误消息。 下图显示了自定义的错误页面描述和一般错误消息。 使用以下 Windows PowerShell cmdlet 自定义错误消息。  
  
![自定义错误](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>自定义错误页面描述  
若要自定义错误页面描述，请使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>自定义一般错误消息  
若要自定义一般错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>自定义授权错误消息  
若要自定义授权错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>自定义设备身份验证错误消息  
若要自定义设备身份验证错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>自定义支持通过电子邮件错误消息  
可以在 AD FS 中配置支持电子邮件地址。 如果已配置，AD FS 会自动显示一个链接，供最终用户通过电子邮件发送错误详细信息。  
  
若要自定义 "支持电子邮件" 错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>自定义信赖方授权消息  
可以在 AD FS 中配置信赖方授权错误消息。  
  
若要自定义信赖方错误消息，请使用以下 Windows PowerShell cmdlet 和语法。  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)    
