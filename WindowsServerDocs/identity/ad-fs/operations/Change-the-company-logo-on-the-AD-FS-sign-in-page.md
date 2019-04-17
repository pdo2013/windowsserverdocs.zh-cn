---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: "更改广告 FS 登录页面上的公司徽标"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca54abe7fe852b22f2f4d9a717e38d219fa50694
ms.sourcegitcommit: a00fc4426dc4ad0098257f01f0124d6c733d1aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2018
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>更改广告 FS 登录页面上的公司徽标

>适用于：Windows Server 2016，Windows Server 2012 R2

#### <a name="change-company-logo"></a>更改公司徽标  
若要更改的公司 sign\ 在页面上显示的徽标时，使用以下 PowerShell Windows PowerShell cmdlet 和语法。  

![更改徽标](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我们建议为徽标后，使之 96 dpi 的文件大小不应超过 10 KB @ 260 x 350 尺寸。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> `TargetName`参数是必需的。 名为与广告 FS 发布默认主题*默认*。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
