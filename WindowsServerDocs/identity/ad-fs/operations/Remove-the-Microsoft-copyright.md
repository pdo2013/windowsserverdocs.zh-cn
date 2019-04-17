---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: "删除 Microsoft 版权"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c2e6f9445e53a5b5867a763d58ad4a6ca3600cbe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="remove-the-microsoft-copyright"></a>删除 Microsoft 版权 

>适用于：Windows Server 2016，Windows Server 2012 R2
 
默认情况下，广告 FS 页面包含 Microsoft 版权。 若要删除此版权你自定义的网页，你可以使用下面的过程。 

![删除版权](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>若要删除的 Microsoft 版权  
  
1.  创建一个基于默认的自定义主题。  
  

    `New-AdfsWebTheme –Name custom –SourceName default ` 
 
  
2.  通过指定的输出文件夹导出主题。  

    `Export-AdfsWebTheme -Name custom -DirectoryPath C:\customWebTheme ` 

  
3.  找到位于输出文件夹 Style.css 文件。 通过使用上面的示例，会 C:\\CustomWebTheme\\Css\\Style.css 路径。  
  
4.  打开用编辑器，如记事本 Style.css 文件。  
  
5.  找到`#copyright`部分，然后再将其更改为以下：  
  `#copyright {color:#696969; display:none;} ` 
 
6.  创建一个基于新 Style.css 文件的自定义主题。  
  
    `Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}  `

7.  激活的新主题。  
  

    `Set-AdfsWebConfig -ActiveThemeName custom ` 


现在，你应该不会再看到登录页面底部的版权。

![删除版权](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
