---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: 添加隐私链接
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cd559e38c38e96d1417257fe7d6ff8ccfa180c6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358418"
---
# <a name="add-privacy-link"></a>添加隐私链接 


若要添加在 sign @ no__t-0in 页面上显示的隐私链接，请使用以下 Windows PowerShell cmdlet 和语法。  

![添加隐私链接](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> 此 cmdlet 中的 `linkText` 参数并不是必需的，除非使用另一个值而不是默认值 *Privacy*。 使用默认值的优点是页面将本地化为所有客户端区域设置。 自定义 sign @ no__t-0in 页面后，将优先使用自定义;因此，你应该为你想要支持的所有语言进行自定义。 所有自定义的内容都使用区域设置参数。 配置本地化内容时，应首先使用国家 @ no__t-0less 区域设置对其进行配置，例如 "en"，然后再配置国家和地区 @ no__t-1specific 区域设置，例如 "en @ no__t-2us"。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
