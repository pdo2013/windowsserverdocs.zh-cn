---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: 在 AD FS 登录页上更改公司徽标
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b22c969e0113081e1ca8a662ae81a2ee24829835
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358305"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>在 AD FS 登录页上更改公司徽标

#### <a name="change-company-logo"></a>更改公司徽标  
若要更改在 sign @ no__t-0in 页上显示的公司徽标，请使用以下 PowerShell Windows PowerShell cmdlet 和语法。  

![更改徽标](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我们建议徽标的尺寸为 260 x 35 (96 DPI)，且文件大小不应超过 10 KB。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> `TargetName` 参数是必需的。 用 AD FS 发布的默认主题名为*default*。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
