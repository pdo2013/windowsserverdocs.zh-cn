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
ms.openlocfilehash: 6e0271ae3d7ac120e510a2fb81fb55c8d10b3c87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813578"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>更改 AD FS 登录页上的公司徽标

>适用于：Windows Server 2016, Windows Server 2012 R2

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
