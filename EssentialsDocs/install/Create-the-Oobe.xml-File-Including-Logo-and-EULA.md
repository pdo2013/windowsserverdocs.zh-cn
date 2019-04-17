---
title: "创建包括徽标和 EULA Oobe.xml 文件"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f99a2051e114b3c890f1cdac23aebf58689980
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>创建包括徽标和 EULA Oobe.xml 文件

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

通过使用 Oobe.xml 文件，可以将你自己的最终用户许可协议 (EULA) 添加到初始配置。 Oobe.xml 是用于初始配置、欢迎使用 Windows 和其他页面提供给最终用户提供文字和图像的文件。 你可以添加多个 Oobe.xml 文件以自定义内容基于语言和国家 / 地区的最终用户选项。 有关详细信息，请参阅[Windows 评估和部署工具包 Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694)文档。  
  
 除了 Microsoft EULA 显示有关你的公司 EULA。 Microsoft EULA 将会显示在初始配置最终用户体验的第一个 EULA，然后将显示你 EULA 将。 在服务器上任意位置放你最终用户许可协议，并指定 Oobe.xml 文件中的位置。  
  
#### <a name="to-add-your-company-eula-and-logo"></a>添加你的公司 EULA 和徽标  
  
1.  在文本编辑器，如记事本中打开 Oobe.xml 文件。  
  
2.  < Logopath\ >< 内 / logopath\ > 标签中，输入你徽标文件绝对的路径。 此文件应该包含 240x100 像素的是 32 位可移植网络图形 (.png) 文件。  
  
3.  < Eulafilename\ >< 内 / eulafilename\ > 标签，请输入绝对 EULA 文件路径。 必须为 EULA 文件格式 (.rtf) 文件。  
  
4.  < Name\ >< 内 / name\ > 标签中，输入你的公司名称。  
  
     下面的示例显示了标记 Oobe.xml 文件中：  
  
    ```  
  
    <FirstExperience>  
       <oobe>  
          <oem>  
             <name>Fabrikam</name>  
             <logopath>c:\fabrikam\fabrikam.png</logopath>  
             <eulafilename>c:\fabrikam\fabrikam_eula.rtf</eulafilename>  
          </oem>  
       </oobe>  
    </FirstExperience>  
  
    ```  
  
5.  保存文件。  
  
6.  在以下位置位置 Oobe.xml 文件：  
  
    |Oobe.xml 位置|确定位置的状况|  
    |-----------------------|----------------------------------------|  
    |%windir%\system32\oobe\info\|在一个国家/地区和语言的单个系统交付服务器。|  
    |%windir%\system32\oobe\info\default\\ < language\ >|在一个国家/地区和多语言系统交付服务器。|  
    |%windir%\system32\oobe\info\\ < 国家/地区 > \ 和 %windir%\system32\oobe\info\\ < 国家/地区 > \\ < language\ > \|服务器发货到多个国家/地区，并设置要求基于每个国家/地区，每个人都带一种语言的自定义设置。 < 国家/地区 > 所在的国家或地区，其中部署服务器时，且 < language\ > 小数点版本的区域设置标识符 (LCID) 的地理位置标识符 (GeoID) 小数点版本。|  
  
 如果你有备用公司徽标白条短信时，它可以显示设置流由于蓝色的背景中更好。  （可选）可以通过设置的注册表项和值此徽标。  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>若要指定公司徽标通过设置 OEM 注册表项  
  
1.  在服务器上，将鼠标移动到屏幕的右上角，然后单击**搜索**。  
  
2.  在搜索框中，键入**regedit**，然后单击 Regedit 应用程序。  
  
3.  在导航窗格中，导航到**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，展开**Windows Server**。 如果不存在 OEM 键，创建键，如下所示：  
  
    1.  右键单击**Windows Server**，单击**新建**，然后单击**键**。  
  
    2.  键名称，请键入**OEM**。  
  
4.  （可选）如果你要创建一个徽标条目，你可以创建其他键以区分徽标的语言版本。 例如，如果你有英文和德语版本的徽标时，你可以创建短-我们键和 de de 密钥。 由于的所有徽标文件存储在同一文件夹中，你必须为徽标图像文件的情况下提供每种语言一个唯一的名称。 例如，可以创建一个名为 LogoWithWhiteText_en.png 和 LogoWithWhiteText_de.png 文件。  
  
5.  不论采取何右键单击**OEM**或右键单击相应的语言键，请单击**新建**，然后单击**字符串值**。  
  
6.  输入 LogoWithWhiteText 作为字符串进行搜索，，然后按 ENTER。  
  
7.  新的字符串，右键单击，然后单击**修改**。  
  
8.  输入包含徽标的图像的路径，然后单击确定。  
  
## <a name="see-also"></a>请参阅  
 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)