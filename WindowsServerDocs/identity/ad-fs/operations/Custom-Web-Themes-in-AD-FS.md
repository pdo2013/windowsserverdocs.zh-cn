---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: "广告 FS 中自定义 Web 主题"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 300c9fda84285ddfc52a4f47ea0198deb6fd33ef
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="custom-web-themes-in-ad-fs"></a>广告 FS 中自定义 Web 主题 

>适用于：Windows Server 2016，Windows Server 2012 R2

已发货的 out\ of\ the\-瓜主题称为默认值。 你可以导出的默认主题和，以便你可以快速开始使用它。 你可以自定义的外观和行为，其中包括布局通过修改.css 文件、导入和应用此新的主题，然后你可以在其中使用自定义的外观和行为。 使用.css 文件还使更轻松地处理你的 web 设计。  
  
以下 cmdlet 创建重复默认 web 主题一个自定义 web 主题。  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
你可以修改.css 文件，并使用新的.css 文件配置的新 web 主题。 若要导出 web 主题，请使用以下 cmdlet。  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
将应用到新的主题.css 文件，请使用以下 cmdlet。  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
以下 cmdlet 创建新的样式表中的自定义 web 主题。  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
若要自定义 web 主题广告 FS 应用，使用以下 cmdlet。  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
若要添加到广告 FS JavaScript，使用以下 cmdlet。  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
