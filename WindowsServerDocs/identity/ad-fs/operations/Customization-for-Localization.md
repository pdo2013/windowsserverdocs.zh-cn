---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: "自定义进行本地化"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac206d5aa8af970b65a014955ac66a8cf2835eb6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="customization-for-localization"></a>自定义进行本地化 

>适用于：Windows Server 2016，Windows Server 2012 R2

可能会本地化 web 内容时为英语以外的语言。 当你本地化会注意以下事项。  
  
自定义的内容后，自定义优先;因此，你应该自定义你想要支持的所有语言版本。 自定义的所有内容都采用区域设置参数。 配置本地化的内容时，它了 country\ 较少的区域设置首先配置，例如，"短"之前，如配置国家/地区和 region\ 特定区域设置"en\-我们"。  
  
下图显示一些其他代码示例。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
下图显示一些其他代码示例。  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
如果你希望自定义以外英语 Unicode 输入所需的语言的 web 内容时，我们建议你使用的 Windows PowerShell ISE。 有关其他信息，请参阅[引入 Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
