---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: "添加链接隐私"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81a453b45693b8222bdfc0231885b506fdfcd2fc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="add-privacy-link"></a>添加链接隐私 

>适用于：Windows Server 2016，Windows Server 2012 R2

若要添加 sign\ 在页面显示的隐私链接，使用以下 Windows PowerShell cmdlet 和语法。  

![添加链接隐私](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> `linkText`参数此 cmdlet 在不需要，除非你使用默认值，即比另一个值*隐私*。 使用默认值的优势是页面，向所有客户端区域设置本地化。 自定义项页 sign\ 中自定义后，将优先;因此，你应该自定义你想要支持的所有语言版本。 自定义的所有内容都采用区域设置参数。 配置本地化的内容时，你应配置了 country\ 较少的区域设置首先，例如，"短"，配置国家/地区和 region\ 特定区域设置，如之前"en\-我们"。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
