---
title: "使用多语言客户端恢复媒体"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ad934d297c3092050bd6adbb6bb0f50d1ec6f36
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="build-multi-language-client-restore-media"></a>使用多语言客户端恢复媒体

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

> [!NOTE]
>  你必须先创建多语言的 Windows 映像中所述[演练： 多语言的 Windows 映像创建](https://technet.microsoft.com/library/jj126995)添加到 install.wim Windows Server Essentials langauage pack 之前。  
  
 在生成多语言服务器安装 DVD 时，将为服务器 install.wim 安装语言包。 将语言包的一部分安装还原向导本地化的资源。  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>若要使用多语言客户端恢复媒体  
  
1.  装载 install.wim c:\mount，在我们 c:\mount\Program Files\Windows Server\bin\ClientRestore 文件夹作为调用的客户端恢复媒体根: [RestoreMediaRoot] 下方：  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  装载客户端还原 WIM 文件在 [RestoreMediaRoot]\Sources\Boot.wim （相同步骤需要执行的 boot_x86.wim 太）。 命令行是：  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  将 WinPE Setup.cab 包添加到还原媒体中，通过运行：  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  使用记事本编辑 c:\mountRestore\windows\system32\winpeshl.ini、 填充与以下内容：  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  将语言包添加到还原媒体。 可以通过运行以下命令添加语言包：  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     需要添加以下语言包：  
  
    1.  WinPE 语言包 (lp.cab)  
  
    2.  WinPE 安装语言包 (例如，是 us.cab Setup_en WinPE WinPE Setup_ [语言].cab)  
  
    3.  对于亚洲字体，包括中文 cn、 中文细、 中文香港特别行政区、 ko kr、 ja-jp 需要安装额外的字体包 (winpe-fontsupport-[语言].cab，例如，winpe fontsupport 中文 cn.cab)  
  
6.  通过运行生成新的 Lang.ini 文件：  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  提交，并通过运行卸载映像:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  重复第 7 步为第 2 步 [RestoreMediaroot]\Sources\Boot_x86.wim。  
  
9. 提交，并通过运行卸载映像:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

