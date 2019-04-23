---
title: 创建包含徽标和 EULA 的 Oobe.xml 文件
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884648"
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>创建包含徽标和 EULA 的 Oobe.xml 文件

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以通过使用 Oobe.xml 文件将你自己的最终用户许可协议 (EULA) 添加到“初始配置”中。 Oobe.xml 文件用于为呈现给最终用户的“初始配置”、“欢迎使用 Windows”和其他页面提供文本和图像。 可以添加多个 Oobe.xml 文件，以根据最终用户选择的语言和国家/地区自定义内容。 有关详细信息，请参阅 [Windows 8 的 Windows 评估和部署工具包](https://go.microsoft.com/fwlink/?LinkId=248694) 文档。  
  
 除 Microsoft EULA 之外，还显示你公司的 EULA。 在初始配置最终用户体验的过程中，首先显示的 EULA 是 Microsoft EULA，然后显示你的 EULA。 可以将你的 EULA 放置在服务器上的任何位置，你可以在 Oobe.xml 文件中指定该位置。  
  
#### <a name="to-add-your-company-eula-and-logo"></a>添加你公司的 EULA 和徽标  
  
1.  在文本编辑器（例如记事本）中打开 Oobe.xml 文件。  
  
2.  在 < logopath\>< / logopath\>标记中，输入徽标文件的绝对路径。 此文件应包含一个 240 x 100 像素的 32 位可移植网络图形 (.png) 文件。  
  
3.  在 < eulafilename\>< / eulafilename\>标记中，输入 EULA 文件的绝对路径。 EULA 文件必须为 RTF 格式 (.rtf) 文件。  
  
4.  在 < 名称\>< / 名称\>标记中，输入你的公司名称。  
  
     以下示例显示 Oobe.xml 文件中的标记：  
  
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
  
5.  保存该文件。  
  
6.  将 Oobe.xml 文件置于下列任一位置：  
  
    |Oobe.xml 位置|用于确定位置的条件|  
    |-----------------------|----------------------------------------|  
    |%windir%\system32\oobe\info\|服务器将在一个国家/地区和单语言版系统中进行发布。|  
    |%windir%\system32\oobe\info\default\\<language\>|服务器将在一个国家/地区并采用多种语言系统进行发布。|  
    |%windir%\system32\oobe\info\\< 国家/地区 > \ 和 %windir%\system32\oobe\info\\< 国家/地区 >\\< 语言\>\|服务器发布到多个国家/地区 /区域和设置需要在每个国家/地区/区域模式中，每个都有一种语言的自定义项。 其中，< 国家/地区 > 是十进制版地理位置标识符 (GeoID) 的国家/地区或区域的部署服务器，并 < 语言\>是十进制版区域设置标识符 (LCID)。|  
  
 如果你有包含白色文本的备用公司徽标，由于背景是蓝色的，所以在设置流程中的显示效果可能更佳。  你可以通过设置注册表项和值来选择性地指定此徽标。  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>通过设置 OEM 注册表项来指定公司徽标  
  
1.  在服务器上，将鼠标移动到屏幕右上角，然后单击 **“搜索”**。  
  
2.  在搜索框中，键入 **regedit**，然后单击“Regedit”应用程序。  
  
3.  在导航窗格中，导航至  **“HKEY_LOCAL_MACHINE”**，展开 **“SOFTWARE”**，展开 **“Microsoft”**，展开 **“Windows Server”**。 如果 OEM 注册表项不存在，请按照如下步骤创建该注册表项：  
  
    1.  右键单击 **“Windows Server”**，单击 **“新建”**，然后单击 **“项”**。  
  
    2.  对于项名称，键入 **OEM**。  
  
4.  （可选）如果要为徽标创建项，则可以创建不同的项以区分徽标的不同语言版本。 例如，如果你已拥有英语版和德语版的徽标，则可以为它们分别创建 en-us 项和 de-de 项。 由于所有徽标文件存储在同一文件夹中，因此提供徽标图像文件的实例时必须加上每种语言的唯一名称。 例如，可以创建名为 LogoWithWhiteText_en.png 和 LogoWithWhiteText_de.png 的文件。  
  
5.  右键单击 **“OEM”** 或右键单击相应的语言项，指向 **“新建”**，然后单击 **“字符串值”**。  
  
6.  输入字符串 LogoWithWhiteText，然后按 Enter 键。  
  
7.  右键单击新字符串，然后单击 **“修改”**。  
  
8.  输入包含徽标图像的路径，然后单击“确定”。  
  
## <a name="see-also"></a>请参阅  
 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)