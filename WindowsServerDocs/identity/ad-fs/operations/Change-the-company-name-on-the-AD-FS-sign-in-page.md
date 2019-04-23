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
ms.openlocfilehash: 1711e7d7de871c9ae9b1b7ea7b21f6e75ae15220
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868468"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>更改 AD FS 登录页上的公司名称

>适用于：Windows Server 2016, Windows Server 2012 R2
 
若要更改显示在登录公司的名称\-在页上，使用以下 Windows PowerShell cmdlet 和语法。 默认情况下，通过使用在安装期间输入的联合身份验证服务显示名称中的值设置此值。  

![更改名称](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> 此外可以使用 Windows PowerShell 集成脚本环境\(ISE\)若要更改公司名称。 通过使用 Windows PowerShell ISE，你可以显示内容，在 Unicode\-符合环境。 有关其他信息，请参阅 [Windows PowerShell ISE 简介](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  
