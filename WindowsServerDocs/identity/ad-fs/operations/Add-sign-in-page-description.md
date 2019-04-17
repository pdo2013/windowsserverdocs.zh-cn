---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: "添加页面 sign\\ 中的说明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 128a4ffd8d4b9dfcfe5f0e8e2df8a0e1505cbb24
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="add-sign-in-page-description"></a>添加页面 sign\ 中的说明

>适用于：Windows Server 2016，Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>若要添加页 sign\ 中的说明  
若要添加与 sign\ 页面 sign\ 在页面描述，使用以下 Windows PowerShell PowerShell cmdlet 和语法。  

![在描述添加日志](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> 对于字符串`SignInPageDescriptionText`参数支持两个纯净 HTML 标记戴眼镜和不。 因此，你也可以不使用情况下运行以下 cmdlet &lt;p&gt;标记。  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

自定义项页 sign\ 中自定义后，将优先;因此，你应该自定义你想要支持的所有语言版本。 自定义的所有内容都采用区域设置参数。 时配置本地化的内容，它应将配置了 country\ 较少的区域设置首先，例如"短"，如配置国家/地区和 region\ 特定区域设置之前"en\-我们"。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
