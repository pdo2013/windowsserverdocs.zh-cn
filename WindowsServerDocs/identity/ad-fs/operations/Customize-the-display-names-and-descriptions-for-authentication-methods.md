---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: 自定义显示名称和说明的身份验证方法
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855188"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>自定义显示名称和说明的身份验证方法 

>适用于：Windows Server 2016, Windows Server 2012 R2

若要为身份验证方法自定义显示名称和说明，可以使用 `Set-AdfsAuthenticationProviderWebContent` PowerShell cmdlt。  若要使用此 cmdlt，你必须首先获取要自定义的身份验证方法的名称。  这可以使用 `Get-AdfsGlobalAuthenticationPolicy`来进行。  在下面的示例中我们看到，我们登录\-在页中，显示以下信息：“Sign in using an X.509 certificate”。  我们要为我们的用户简化此操作。  
  
![自定义显示名称](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
所以，首先我们获取身份验证方法的名称，然后编辑显示的文本。  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![自定义显示名称](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
现在我们可以看到显示消息已更改。  
  
![自定义显示名称](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
