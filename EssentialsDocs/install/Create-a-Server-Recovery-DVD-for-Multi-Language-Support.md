---
title: "创建多语言支持服务器恢复 DVD"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
4author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac547f97b48e4cd0ebf87e0935cadc2c539b4d0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>创建多语言支持服务器恢复 DVD

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a>创建本地管理服务器上的服务器设置和多语言支持服务器恢复 DVD  
  
> [!NOTE]
>  你必须先创建多语言的 Windows 映像中所述[演练： 多语言的 Windows 映像创建](https://technet.microsoft.com/library/jj126995)添加到 install.wim Windows Server Essentials langauage pack 之前。  
  
 有两个阶段安装程序： Windows 预安装环境 (Windows PE) 和初始配置。 默认情况下，不会显示在初始配置语言中选择页。  
  
-   对于 OEM 远程管理安装或 OEM 预安装的情况下，你需要添加注册表关键使用以下命令以显示在初始配置的语言中选择页。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  当在此实验中，Oem 创建映像时，他们必须选择**英语**作为设置的 Windows PE 阶段语言。  
  
-   对于经销商选项工具包 (ROK) 方案的客户将收到 DVD，并且可能是，有些硬件。 客户应当能够在 Windows PE 设置期间选择的语言和语言中选择页面不会再显示在初始配置过程。  
  
 你可以选择发货包含多种语言的单个双层 DVD。  
  
 此部分中介绍了如何将语言支持添加到 Windows 设置。 主要工具自定义 Windows PE 3.0 是部署映像服务和管理 (DISM)、 命令行工具。 此解决方法可以让以下情形：  
  
1.  创建多语言安装  
  
2.  创建可分发媒体  
  
### <a name="prerequisites"></a>先决条件  
 若要添加 Windows 安装到多语言的支持，你需要以下各项：  
  

-   提供的工具和创建自定义的 WinPE 映像所需的源代码文件的技术人员计算机。 有关详细信息，请参阅[准备技术人员计算机](Prepare-the-Technician-Computer.md)。  

-   提供的工具和创建自定义的 WinPE 映像所需的源代码文件的技术人员计算机。 有关详细信息，请参阅[准备技术人员计算机](../install/Prepare-the-Technician-Computer.md)。  

  
-   Windows Server Essentials DVD。  
  
-   Windows Server Essentials 语言包 DVD。  
  
###  <a name="BKMK_Steps"></a>添加多个语言支持  
 添加 Windows 安装到多语言支持你更新 Install.wim 通过添加 Windows Server 2012 和 Windows Server Essentials 语言包到它。  
  
#### <a name="update-installwim"></a>更新 Install.wim  
 在此步骤中，你将 Windows Server 2012 和 Windows Server Essentials 语言包添加到 Install.wim。  
  
> [!NOTE]
>  验证你的 Windows Server 2012 安装语言包。 这可以确保你获取的相应品牌。 Windows Server 2012 多语言用户界面语言包是适用于[Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx)。请按照说明进行操作，述[演练：多语言在创建多语言的 Windows 映像创建](https://technet.microsoft.com/library/jj126995.aspx)之前添加 Windows Server essentials 语言包。  
>   
>  将提供语言包媒体 \Language Packs\\ < CultureName\ > 在 Windows Server Essentials 语言包。  
  
> [!NOTE]
>  并非所有语言包可能不可用之前版本的 Windows Server 2012。  
  
###### <a name="to-add-language-packs-to-installwim"></a>将语言包添加到 Install.wim  
  
1.  如下所示 （本例中使用法语） 添加到 Install.wim 的操作系统和产品语言包：  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  添加语言特定文件支持创建客户端备份还原 USB 刷写的驱动器，使用中所述的过程[版本多语言客户端恢复媒体](Build-Multi-Language-Client-Restore-Media.md)。  

2.  添加语言特定文件支持创建客户端备份还原 USB 刷写的驱动器，使用中所述的过程[版本多语言客户端恢复媒体](../install/Build-Multi-Language-Client-Restore-Media.md)。  

  
3.  重新 Lang.ini 文件松动媒体，以反映额外的语言支持使用在创建`DISM /Gen-LangINI`命令，例如：  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  通过使用保存更改回图像`DISM /unmount /commit`命令，例如：  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
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

