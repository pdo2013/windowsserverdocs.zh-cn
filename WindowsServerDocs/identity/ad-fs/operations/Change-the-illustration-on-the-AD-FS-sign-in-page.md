---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: "更改广告 FS 登录页面上的图示"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac5e60aaad864248b58a3908e7aa9622165fbc14
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>更改广告 FS 登录页面上的图示

>适用于：Windows Server 2016，Windows Server 2012 R2

## <a name="change-the-illustration"></a>更改图示  


若要更改图，请在左侧，sign\ 在页面显示图形使用以下 Windows PowerShell PowerShell cmdlet 和语法。  

![更改图示](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我们建议为图所要将文件大小不大于 200 KB @ 96 DPI 的 1420 x 1080 像素尺寸。  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  
  
