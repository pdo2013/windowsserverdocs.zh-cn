---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: "自定义的显示名称和了身份验证方法说明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>自定义的显示名称和了身份验证方法说明 

>适用于：Windows Server 2016，Windows Server 2012 R2

若要自定义的显示名称，并且你可以使用的身份验证方法的说明`Set-AdfsAuthenticationProviderWebContent`PowerShell cmdlt。  才能使用此 cmdlt，必须首先获取你想要自定义的身份验证方法的名称。  这可以使用`Get-AdfsGlobalAuthenticationPolicy`。  我们在下面的示例看到，我们 sign\ 页面，在显示以下:"登录使用 X.509 密钥"。  我们想要简化这对于我们的用户。  
  
![自定义的显示名称](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
首先我们收到的验证方法的名称，，然后我们编辑显示的文本。  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![自定义的显示名称](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
现在可以看到我们显示消息已发生更改。  
  
![自定义的显示名称](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
