---
title: "安装或删除语言包"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a41b491bbe4b4a8ee7f9743dc85e5bdaffb08496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-or-remove-language-packs"></a>安装或删除语言包

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

> [!NOTE]
>  你必须先创建多语言的 Windows 映像中所述[语言包和部署](https://technet.microsoft.com/library/hh824829)之前添加了 Windows Server Essentials 语言包。  
  
 语言包仅有可用于创建多语言图像。 在此部分中的信息是特定于安装或删除 Windows Server Essentials 上的语言包。  
  
> [!NOTE]
>  如果你打算从不支持东亚语言，如 ja-jp 的客户端计算机上运行初始配置（集成电路），并且英文不包括在服务器上的多语言图像，表示网页将显示正方形。 为默认值为英语集成电路网页，为你创建多语言图像必须包括英语。  
  
## <a name="adding-language-packs-to-an-image"></a>添加语言包的图像  
 语言包都适用于 OEM 自定义 DVD。 将语言包添加到映像中之前将语言包复制到计算机的技术人员建议。  
  
 你必须使用以下命令以安装语言包：  
  
 **Dism.exe//online /Add-Package /PackagePath: C:\\ < cab 文件 directory\ 完整路径 > \lp.cab**  
  
 例如，以下命令显示如何添加德语语言包：  
  
 **Dism.exe//online /Add-Package /PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  您还必须应用来完全本地化操作系统的 Windows Server essentials 语言包。  
  
## <a name="removing-language-packs-from-an-image"></a>从映像中删除语言包  
 你可以使用以下命令以删除你不再希望包括图像中的语言包：  
  
 **Dism.exe//online /Remove-Package /PackagePath: C:\\ < cab 文件 directory\ 完整路径 > \lp.cab**  
  
 例如，以下命令显示如何删除德语语言包：  
  
 **Dism.exe//online /Remove-Package /PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

