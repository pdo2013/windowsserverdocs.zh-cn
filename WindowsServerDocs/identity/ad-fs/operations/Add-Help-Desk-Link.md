---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: "添加帮助 Desk 链接"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d16cc0a75bfe636c29b44687b669e87f31b69ce
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2018
---
# <a name="add-help-desk-link"></a>添加帮助 Desk 链接 

>适用于：Windows Server 2016，Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>若要添加的帮助 Desk 链接  
若要添加 sign\ 在页面显示的帮助 desk 链接，使用以下 Windows PowerShell PowerShell cmdlet 和语法。  

![添加服务台](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> `linkText`参数此 cmdlet 在不需要，除非你使用默认值，即比另一个值*帮助*。 使用默认值的优势是它们本地化向所有客户端区域设置。 自定义项页面自定义后，将优先;因此，你应该自定义你想要支持的所有语言版本。  


## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
