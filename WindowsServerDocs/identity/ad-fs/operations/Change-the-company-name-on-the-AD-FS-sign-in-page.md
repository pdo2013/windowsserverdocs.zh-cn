---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: 更改 AD FS 登录页上的公司名称
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2b5c7228094305759344d5094cffa7f24a0da7a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190021"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>更改 AD FS 登录页上的公司名称
 
若要更改显示在登录公司的名称\-在页上，使用以下 Windows PowerShell cmdlet 和语法。 默认情况下，通过使用在安装期间输入的联合身份验证服务显示名称中的值设置此值。  

![更改名称](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> 此外可以使用 Windows PowerShell 集成脚本环境\(ISE\)若要更改公司名称。 通过使用 Windows PowerShell ISE，你可以显示内容，在 Unicode\-符合环境。 有关其他信息，请参阅 [Windows PowerShell ISE 简介](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  
