---
title: 为多语言支持创建服务器恢复 DVD
description: 描述如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
author: daveba
ms.author: daveba
ms.openlocfilehash: 59d8d41e5836ba88b405a058c8340f454b081c06
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980252"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>为多语言支持创建服务器恢复 DVD

>适用于：Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a>为本地管理的服务器上的多语言支持创建服务器安装和服务器恢复 DVD  
  
> [!NOTE]
>  必须首先创建多语言 Windows 映像, 如[本演练中所述:在将 windows Server](https://technet.microsoft.com/library/jj126995) Essentials 述 pack 添加到 install 之前, 需要创建多语言 windows 映像。  
  
 安装有两个阶段：Windows 预安装环境 (Windows PE) 和初始配置。 默认情况下，不会显示初始配置中的语言选择页面。  
  
- 对于 OEM 远程管理安装或 OEM 预安装方案，需要使用以下命令添加注册表项以显示初始配置中的语言选择页面。  
  
  ```  
  %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
  ```  
  
  > [!IMPORTANT]
  >  当 OEM 在实验室中创建映像时，他们必须在安装过程的 Windows PE 阶段选择 **“英语”** 作为所选语言。  
  
- 对于经销商选项工具包 (ROK)，客户会收到一张 DVD，还有可能收到若干硬件。 客户应能够在 Windows PE 安装过程中选择语言，并且在初始配置中不再显示语言选择页面。  
  
  你可以选择发运一张包含多种语言的双层 DVD。  
  
  本节介绍如何将语言支持添加到 Windows 安装程序。 自定义 Windows PE 3.0 的主要工具是部署映像服务和管理 (DISM)，它是一个命令行工具。 该解决方案适用于以下情景：  
  
1.  创建多语言安装  
  
2.  创建可分配的介质  
  
### <a name="prerequisites"></a>先决条件  
 若要将多语言支持添加到 Windows 安装程序，需满足以下条件：  
  

-   提供创建自定义 WinPE 映像所需的所有工具和源文件的技术人员计算机。 有关详细信息，请参阅 [Prepare the Technician Computer](Prepare-the-Technician-Computer.md)。  

-   提供创建自定义 WinPE 映像所需的所有工具和源文件的技术人员计算机。 有关详细信息，请参阅 [Prepare the Technician Computer](../install/Prepare-the-Technician-Computer.md)。  

  
-   Windows Server Essentials DVD。  
  
-   Windows Server Essentials 语言包 DVD。  
  
###  <a name="BKMK_Steps"></a>添加多语言支持  
 若要将多语言支持添加到 Windows 安装程序您通过将 Windows Server 2012 和 Windows Server Essentials 语言包添加到该安装程序来更新 Install。  
  
#### <a name="update-installwim"></a>更新 Install.wim  
 在此步骤中, 你将 Windows Server 2012 和 Windows Server Essentials 语言包添加到 Install。  
  
> [!NOTE]
>  验证是否为 Windows Server 2012 安装了语言包。 这可确保你获取相应的品牌。 [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx)上提供了 Windows Server 2012 多语言用户界面语言包。 请按照[本演练中所述的说明进行操作:创建多语言 windows 映像](https://technet.microsoft.com/library/jj126995.aspx)创建多语言 windows 映像, 然后在将 Windows Server Essentials 语言包添加到 install 之前创建多语言 windows 映像。  
>   
>  Windows Server Essentials 语言包在 \Language pack\\< CultureName\>的语言包媒体中提供。  
  
> [!NOTE]
>  在版本的 Windows Server 2012 之前, 并非所有语言包都可用。  
  
###### <a name="to-add-language-packs-to-installwim"></a>向 Install.wim 添加语言包  
  
1.  按以下方式向 Install.wim 添加操作系统和产品语言包（此处以法语为例）：  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  使用[生成多语言客户端还原介质](Build-Multi-Language-Client-Restore-Media.md)中所述的过程, 添加特定于语言的文件以支持创建客户端备份还原 u 盘。  

2.  使用[生成多语言客户端还原介质](../install/Build-Multi-Language-Client-Restore-Media.md)中所述的过程, 添加特定于语言的文件以支持创建客户端备份还原 u 盘。  

  
3.  在松散介质中使用 `DISM /Gen-LangINI` 命令重新创建 Lang.ini 文件以反映其他语言支持，例如：  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  使用 `DISM /unmount /commit` 命令将更改保存回映像中，例如：  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
    ```  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义](Additional-Customizations.md)   
 [准备要部署的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义](../install/Additional-Customizations.md)   
 [准备要部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

