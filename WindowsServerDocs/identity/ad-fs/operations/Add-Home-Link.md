---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: 添加主页链接
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5f1fad340629304fdf960139be05b8dbc2690e0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407772"
---
# <a name="add-home-link"></a>添加主页链接 

若要添加在 sign @ no__t-0in 页面上显示的主页链接，请使用以下 Windows PowerShell cmdlet 和语法。 


![添加主页链接](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> 此 cmdlet 中的 `linkText` 参数并不是必需的，除非使用另一个值而不是默认值 *Home*。 使用默认值的优点是它们已本地化到所有客户端区域设置。 自定义 sign @ no__t-0in 页面后，将优先使用自定义;因此，你应该为你想要支持的所有语言进行自定义。

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
