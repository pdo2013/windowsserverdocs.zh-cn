---
title: "添加品牌仪表板、远程网站访问和启动栏"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 04/10/2014
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02088b169e44cdcf87385425e1949232ffa408a6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>添加品牌仪表板、远程网站访问和启动栏

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_Branding"></a>添加品牌仪表板、远程网站访问和启动栏  
 你可以通过在注册表中添加项目进行多个品牌添加。 注册表操作系统中的所有品牌条目位于 HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \OEM。  
  
 所有的联合品牌必须满足以下徽标要求：  
  
-   Windows Server Essentials 徽标必须具有最小的宽度**170 像素**，保持与正确纵横比，**96 DPI**。  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>若要添加通过更改注册表品牌  
  
1.  在服务器上，将鼠标移动到屏幕的右上角，然后单击**搜索**。  
  
2.  在搜索框中，键入**regedit**，然后单击**Regedit**应用程序。  
  
3.  在导航窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，展开**Windows Server**。 如果**OEM**键不存在，请完成以下步骤创建它：  
  
    1.  右键单击**Windows Server**，单击**新建**，然后单击**键**。  
  
    2.  键入**OEM**键的名称。  
  
4.  （可选）如果你要创建一个徽标条目，你可以创建其他键以区分徽标的语言版本。 例如，如果你有英文和德语版本的徽标时，你可以创建短-我们键和 de de 密钥。 由于的所有徽标文件存储在同一文件夹中，你必须为徽标图像文件的情况下提供每种语言一个唯一的名称。 例如，你将创建一个名 DashboardLogo_en.png 和 DashboardLogo_de.png 文件。  
  
5.  不论采取何右键单击**OEM**或右键单击相应的语言键，请单击**新建**，然后单击**字符串值**。  
  

6.  输入字符串的名称，然后按 ENTER。 请参阅[注册表字符串和值](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)字符串名称和数据值的表。  

6.  输入字符串的名称，然后按 ENTER。 请参阅[注册表字符串和值](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)字符串名称和数据值的表。  

  
7.  新的字符串，右键单击，然后单击**修改**。  
  
8.  输入与字符串的名称，表中的值，然后单击**确定**。  
  
9. 如果你要创建徽标图像条目或附加的链接，请将文件复制到 %programFiles%\ Windows Server \Bin\OEM。 如果不存在 OEM 目录，创建它。  
  
10. 如果进行了更改影响远程 Web 访问，客户需要服务器上的拥有后，他们必须打开远程访问 Web。 建议客户可以执行以下操作：  
  
    1.  在仪表板中，单击**设置**，然后单击**任何地方访问**选项卡。  
  
    2.  如果在任何地方访问处于打开状态，请单击**配置**，然后清除上的远程访问 Web 复选框**选择任何地方访问功能供**的任何地方访问向导设置页。  
  
    3.  单击**配置**。  
  
###  <a name="BKMK_RegStrings"></a>下表列出了注册表更改影响品牌的位置、字符串的名称，并数据值。  
  
### <a name="registry-strings-and-values"></a>注册表字符串和值  
  
|商标的位置|描述|字符串名称|数据值|  
|--------------------------|-----------------|-----------------|----------------|  
|仪表板徽标|添加到仪表板徽标的图像。 仪表板徽标必须.png 格式，并且不得大于 350 像素为 38 像素高。<br /><br /> **重要提示：**要联合仪表板与您的徽标时，你必须编辑 OPK DVD 上提供，同时按照相应空白要求添加到映像中的公司徽标的画作磁贴。 有关其他信息，请参阅示例磁贴提供了。|DashboardLogo|徽标图像文件的名称|  
|DashboardClientLogo|向仪表板客户端登录屏幕徽标的图像。|DashboardClientLogo|徽标图像文件的名称|  
|网站背景图片|更改背景图像远程访问 Web 登录页面上显示。 典型分辨率将显示，如下所示：<br /><br /> -1024 x 768 像素的分辨率精确会填满登录页面<br /><br /> -800 x 600 像素的分辨率居中页面上，并显示黑色边框<br /><br /> -将中心 1280 x 720 像素的分辨率和超过 1024 x 768 像素将不会显示|LogonBackground|背景图像文件的名称|  
|网站标题|将替换远程网站访问的站点 Windows Server Essentials 标题为你选择标题。|网站名称|新远程网站访问的站点标题|  
|网站徽标|更改默认徽标远程网站访问的站点上。 预期徽标大小 32 像素 32 像素。 如果你徽标小于或大于这，它将拉伸或降低匹配这些尺寸进行|WebsiteLogo|徽标图像文件的名称|  
|附加的网站徽标|合作伙伴徽标将显示下方远程网站访问的站点显示的 Microsoft 徽标。 徽标的预期的大小是通过 50 像素 200 高像素。 如果你徽标大于这，它将进行较小以同时保持原始纵横比适合。 如果你徽标小于该、将内 200 通过 50 像素空间居中放置和大小和纵横比都不会更改。|OEMLogo|徽标图像文件的名称|  

|网站主页并登录页面上的链接 |附加到登录页面并主页远程网站访问的站点上的链接。 包含的链接信息.xml 必须位于 %programFiles%\ Windows Server \Bin\OEM。 下面的示例显示了 xml 文件的格式：<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < 链接 Name\ = LogonLinkName ><br /> < Text\ > LogonLinkDescription < / Text\ ><br /> < Url\ > LogonLinkURL < / Url\ ><br /> < Icon\ > LinkIcon < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < 链接 Name\ = HomepageLinkName ><br /> < Text\ > HomepageLinkDescription < / Text\ ><br /> < Url\ > HomepageLinkURL < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > |LinksXML |请参阅[LinksXML 元素](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)表说明元素的列表。|  

|网站主页并登录页面上的链接 |附加到登录页面并主页远程网站访问的站点上的链接。 包含的链接信息.xml 必须位于 %programFiles%\ Windows Server \Bin\OEM。 下面的示例显示了 xml 文件的格式：<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < 链接 Name\ = LogonLinkName ><br /> < Text\ > LogonLinkDescription < / Text\ ><br /> < Url\ > LogonLinkURL < / Url\ ><br /> < Icon\ > LinkIcon < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < 链接 Name\ = HomepageLinkName ><br /> < Text\ > HomepageLinkDescription < / Text\ ><br /> < Url\ > HomepageLinkURL < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > |LinksXML |请参阅[LinksXML 元素](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)表说明元素的列表。|  

|启动栏徽标 |向启动栏徽标的图像。 启动栏徽标一定.png 格式不高于 64 像素必须在。|LaunchpadLogo |徽标图像文件的名称 |  
  
###  <a name="BKMK_Links"></a>下表列出，并描述 LinksXML 字符串名称元素。  
  
### <a name="linksxml-elements"></a>LinksXML 元素  
  
|LinksXML 元素|描述|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|登录链接名称。|  
|LogonLinkDescription|显示为登录页面链接文本。|  
|LogonLinkURL|解决了到登录页面链接的 url。|  
|LinkIcon|图标登录链接的文件的名称。 此文件应该在.xml 文件所在的文件夹位置。 链接图标的图像应为 16 x 16 像素，不过应该会.png 格式。 如果不提供 LinkIcon，则使用默认链接图标的图像。|  
|**HomepageLinks**|  
|HomepageLinkName|主页链接名称。|  
|HomepageLinkDescription|将显示为主页的链接文本。|  
|HomepageLinkURL|解决了到主页链接的 URL。|  
|HomepageLinkIcon|图标主页链接的文件的名称。 此文件应该在.xml 文件所在的文件夹位置。 HomepageLinkIcon 图像应为 16 x 16 像素，不过应该会.png 格式。 如果不提供 HomepageLinkIcon，则使用默认主页链接图标的图像。|  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

