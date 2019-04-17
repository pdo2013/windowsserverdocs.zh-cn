---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: "更改广告 FS 登录页面上的公司名"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8e99f6fed8922ed15bf78a98b207b6f46767763a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>更改广告 FS 登录页面上的公司名

>适用于：Windows Server 2016，Windows Server 2012 R2
 
若要更改 sign\ 在页面显示的公司名称，使用以下 Windows PowerShell PowerShell cmdlet 和语法。 此值将默认情况下使用来自你在设置过程中输入的联合身份验证服务显示名称的值。  

![更改名字](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> 你还可以使用 Windows PowerShell 脚本的集成环境 \(ISE\) 若要更改的公司名称。 通过使用 Windows PowerShell ISE，你可以在 Unicode\ 兼容的环境中显示内容。 有关其他信息，请参阅[引入 Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  
