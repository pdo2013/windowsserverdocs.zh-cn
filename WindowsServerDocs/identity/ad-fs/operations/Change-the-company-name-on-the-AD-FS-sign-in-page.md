---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: 在 AD FS 登录页上更改公司名称
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c4991b27f104cb96f55f09fa9467f2b93868b910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407738"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>在 AD FS 登录页上更改公司名称
 
若要更改在 sign @ no__t-0in 页上显示的公司名称，请使用以下 Windows PowerShell cmdlet 和语法。 默认情况下，通过使用在安装期间输入的联合身份验证服务显示名称中的值设置此值。  

![更改名称](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> 你还可以使用 Windows PowerShell 集成脚本环境 \(ISE @ no__t 来更改公司名称。 通过使用 Windows PowerShell ISE，可以在 Unicode @ no__t-0compliant 环境中显示内容。 有关其他信息，请参阅 [Windows PowerShell ISE 简介](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  
