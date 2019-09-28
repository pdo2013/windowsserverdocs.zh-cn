---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: 本地化自定义
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ba3441d362960e054791ca3d6872dba6c7bd9a12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357977"
---
# <a name="customization-for-localization"></a>本地化自定义 


可以将 Web 内容本地化为英语以外的语言。 当你在本地化时，请注意以下事项。  
  
自定义内容后，将优先该自定义；因此，你应该为你想要支持的所有语言进行自定义。 所有自定义的内容都使用区域设置参数。 配置本地化内容时，请先使用国家 @ no__t-0less 区域设置对其进行配置，例如 "en"，然后再配置国家和地区 @ no__t-1specific 区域设置，例如 "en @ no__t-2us"。  
  
下面显示了一些其他代码示例。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
下面显示了一些其他代码示例。  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
如果要将 web 内容自定义为英语之外的其他语言，则建议使用 Windows PowerShell ISE。 有关其他信息，请参阅 [Windows PowerShell ISE 简介](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
