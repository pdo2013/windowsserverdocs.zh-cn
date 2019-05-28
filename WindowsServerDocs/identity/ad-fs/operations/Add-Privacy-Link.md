---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: 添加隐私链接
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3617335a179ab419982ab57343999ad4fcaf522a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190160"
---
# <a name="add-privacy-link"></a>添加隐私链接 


若要添加显示在登录的隐私链接\-在页上，使用以下 Windows PowerShell cmdlet 和语法。  

![添加隐私链接](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> 此 cmdlet 中的 `linkText` 参数并不是必需的，除非使用另一个值而不是默认值 *Privacy*。 使用默认值的优点是页面将本地化为所有客户端区域设置。 登录后\-页中为自定义，优先该自定义; 因此，应该自定义你想要支持的所有语言。 所有自定义的内容都使用区域设置参数。 在配置本地化的内容时，你应配置不带有国家/地区\-更少的区域设置第一个，例如，"en"，然后再配置国家/地区和区域\-特定的区域设置，例如"en\-我们"。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
