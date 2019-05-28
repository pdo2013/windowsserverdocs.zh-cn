---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: 删除 Microsoft 版权
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6e15f9d1490ad62f1458cd32da6e78a6febec58d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189032"
---
# <a name="remove-the-microsoft-copyright"></a>删除 Microsoft 版权 


 
默认情况下，AD FS 页面包含 Microsoft 版权。 若要从自定义页面删除此版权，可以使用以下过程。 

![删除版权](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>删除 Microsoft 版权  
  
1. 创建基于默认主题的自定义主题。

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. 通过指定输出文件夹来导出该主题。  

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. 找到`Style.css`位于输出文件夹中的文件。 通过使用前面的示例，该路径将为 `C:\CustomWebTheme\Css\Style.css.`
  
4. 打开`Style.css`使用编辑器，如记事本的文件。  
  
5. 找到 `#copyright` 部分，然后将其更改为以下内容：  

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. 创建基于新的自定义主题`Style.css`文件。  

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. 激活新主题。  

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

现在，应不再看到在登录页的底部版权所有。

![删除版权](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
