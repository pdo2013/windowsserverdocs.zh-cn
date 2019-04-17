---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: "添加家庭链接"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="add-home-link"></a>添加家庭链接 

>适用于：Windows Server 2016，Windows Server 2012 R2

若要添加的家庭的链接，将显示在 sign\ 页，使用以下 Windows PowerShell cmdlet 和语法。 


![添加家庭链接](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> `linkText`参数此 cmdlet 在不需要，除非你使用默认值，即比另一个值*家庭*。 使用默认值的优势是它们本地化向所有客户端区域设置。 自定义项页 sign\ 中自定义后，将优先;因此，你应该自定义你想要支持的所有语言版本。

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
