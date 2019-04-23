---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: 添加帮助台链接
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1654add6a81169b3d4831d6ebba320402e0734c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849858"
---
# <a name="add-help-desk-link"></a>添加帮助台链接 

>适用于：Windows Server 2016, Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>若要添加帮助台链接  
若要添加显示在登录的帮助台链接\-在页上，使用以下 Windows PowerShell cmdlet 和语法。  

![添加技术支持](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> 此 cmdlet 中的 `linkText` 参数并不是必需的，除非使用另一个值而不是默认值 *Help*。 使用默认值的优点是它们已本地化到所有客户端区域设置。 自定义该页面后，将优先该自定义；因此，你应该为你想要支持的所有语言进行自定义。  


## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
