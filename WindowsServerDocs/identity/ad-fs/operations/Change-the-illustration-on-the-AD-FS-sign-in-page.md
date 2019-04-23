---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: 更改 AD FS 登录页上的插图
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f1cba9862766092c2beadb894cbac092d146887
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858238"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>更改 AD FS 登录页上的插图

>适用于：Windows Server 2016, Windows Server 2012 R2

## <a name="change-the-illustration"></a>更改图片  


若要更改图中，在左侧显示在登录图\-在页上，使用以下 Windows PowerShell cmdlet 和语法。  

![更改图](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我们建议图片的尺寸为 1420 x 1080 像素 (96 DPI)，且文件大小不应超过 200 KB。  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  
  
