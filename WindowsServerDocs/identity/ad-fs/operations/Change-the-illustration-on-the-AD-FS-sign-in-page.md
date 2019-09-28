---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: 在 AD FS 登录页上更改图例
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3da7726ca625c32728fb0ae64d291ae599b6cd8d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358288"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>在 AD FS 登录页上更改图例

## <a name="change-the-illustration"></a>更改图例  


若要更改图例，请在 "no__t-0in" 页上显示的左侧图形，使用以下 Windows PowerShell cmdlet 和语法。  

![更改图示](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我们建议图片的尺寸为 1420 x 1080 像素 (96 DPI)，且文件大小不应超过 200 KB。  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  
  
