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
ms.openlocfilehash: fb186c3ba5cfb3acb9bfd0c3139b09b992fb8863
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190208"
---
# <a name="add-help-desk-link"></a>添加帮助台链接 


## <a name="to-add-a-help-desk-link"></a>若要添加帮助台链接  
若要添加显示在登录的帮助台链接\-在页上，使用以下 Windows PowerShell cmdlet 和语法。  

![添加技术支持](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> 此 cmdlet 中的 `linkText` 参数并不是必需的，除非使用另一个值而不是默认值 *Help*。 使用默认值的优点是它们已本地化到所有客户端区域设置。 自定义该页面后，将优先该自定义；因此，你应该为你想要支持的所有语言进行自定义。  


## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
