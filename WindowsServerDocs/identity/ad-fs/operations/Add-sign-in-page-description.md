---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Add sign @ no__t-0in 页面说明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3b34a4e54aebd5b9dc3655eecd770a25f7ea97cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407742"
---
# <a name="add-sign-in-page-description"></a>Add sign @ no__t-0in 页面说明


## <a name="to-add-sign-in-page-description"></a>添加 sign @ no__t-0in 页面说明  
若要向 sign @ no__t-1in 页添加 sign @ no__t-0in 页面说明，请使用以下 Windows PowerShell cmdlet 和语法。  

![添加登录说明](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> `SignInPageDescriptionText` 参数的字符串同时支持带标记和不带标记的纯 HTML。 因此，你还可以运行以下 cmdlet 而不使用 &lt; p @ no__t 标记。  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

自定义 sign @ no__t-0in 页面后，将优先使用自定义;因此，你应该为你想要支持的所有语言进行自定义。 所有自定义的内容都使用区域设置参数。 配置本地化内容时，应首先使用国家 @ no__t-0less 区域设置进行配置，例如 "en"，然后再配置国家/地区 @ no__t-1specific 区域设置，例如 "en @ no__t-2us"。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
