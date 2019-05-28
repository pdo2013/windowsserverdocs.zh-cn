---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: 更改 AD FS 登录页上的公司徽标
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fe5c138466ea288b5dfb8c7c284603150ab9d874
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190026"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>更改 AD FS 登录页上的公司徽标

#### <a name="change-company-logo"></a>更改公司徽标  
若要更改显示在登录公司的徽标\-在页上，使用以下 PowerShell Windows PowerShell cmdlet 和语法。  

![更改徽标](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我们建议徽标的尺寸为 260 x 35 (96 DPI)，且文件大小不应超过 10 KB。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> `TargetName` 参数是必需的。 随 AD FS 一起发布的默认主题名为*默认*。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
