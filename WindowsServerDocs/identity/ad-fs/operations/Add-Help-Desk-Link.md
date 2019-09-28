---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: 添加帮助台链接
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1673e6ee6357a9d59e8ac5891625d453bb434088
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358463"
---
# <a name="add-help-desk-link"></a>添加帮助台链接 


## <a name="to-add-a-help-desk-link"></a>添加帮助台链接  
若要添加在 sign @ no__t-0in 页面上显示的帮助台链接，请使用以下 Windows PowerShell cmdlet 和语法。  

![添加支持人员](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> 此 cmdlet 中的 `linkText` 参数并不是必需的，除非使用另一个值而不是默认值 *Help*。 使用默认值的优点是它们已本地化到所有客户端区域设置。 自定义该页面后，将优先该自定义；因此，你应该为你想要支持的所有语言进行自定义。  


## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
