---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: 添加注册\-中页面描述
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553cdcf2ac98b23d06cc43a64220cf8433117fc8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814008"
---
# <a name="add-sign-in-page-description"></a>添加注册\-中页面描述

>适用于：Windows Server 2016, Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>添加登录\-中页面描述  
若要添加的标志\-在登录页面描述\-在页上，使用以下 Windows PowerShell cmdlet 和语法。  

![在说明中添加登录](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> `SignInPageDescriptionText` 参数的字符串同时支持带标记和不带标记的纯 HTML。 因此，你还可以不使用情况下运行以下 cmdlet &lt;p&gt;标记。  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

登录后\-页中为自定义，优先该自定义; 因此，应该自定义你想要支持的所有语言。 所有自定义的内容都使用区域设置参数。 在配置本地化的内容时，它应当配置与一个国家/地区\-更少的区域设置第一个，例如，"en"，然后再配置国家/地区和区域\-特定的区域设置，例如"en\-我们"。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
