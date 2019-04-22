---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: 添加主页链接
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823008"
---
# <a name="add-home-link"></a>添加主页链接 

>适用于：Windows Server 2016, Windows Server 2012 R2

若要添加显示在登录的主页链接\-在页上，使用以下 Windows PowerShell cmdlet 和语法。 


![添加主页链接](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> 此 cmdlet 中的 `linkText` 参数并不是必需的，除非使用另一个值而不是默认值 *Home*。 使用默认值的优点是它们已本地化到所有客户端区域设置。 登录后\-页中为自定义，优先该自定义; 因此，应该自定义你想要支持的所有语言。

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
