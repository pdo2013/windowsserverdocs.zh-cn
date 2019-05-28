---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: 用于本地化的自定义
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3cf209756c27c72836c7e2e1e58e84f3f5af2ca9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189183"
---
# <a name="customization-for-localization"></a>用于本地化的自定义 


可以将 Web 内容本地化为英语以外的语言。 当你在本地化时，请注意以下事项。  
  
自定义内容后，将优先该自定义；因此，你应该为你想要支持的所有语言进行自定义。 所有自定义的内容都使用区域设置参数。 在配置本地化的内容时，配置不带有国家/地区\-更少的区域设置第一个，例如，"en"，然后再配置国家/地区和区域\-特定的区域设置，例如"en\-我们"。  
  
下面显示了一些其他代码示例。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
下面显示了一些其他代码示例。  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
如果你想要自定义 web 内容为英语之外的需要 Unicode 输入的其他语言，我们建议使用 Windows PowerShell ISE。 有关其他信息，请参阅 [Windows PowerShell ISE 简介](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
