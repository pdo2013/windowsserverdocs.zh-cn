---
title: 向仪表板、远程 Web 访问和快速启动板添加品牌
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823498"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>向仪表板、远程 Web 访问和快速启动板添加品牌

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_Branding"></a> 向仪表板、 远程 Web 访问和快速启动板添加品牌  
 可以通过在注册表中添加项来添加多个品牌。 本操作系统注册表中的所有品牌项位于 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM 下。  
  
 所有联合品牌必须满足以下徽标要求：  
  
-   Windows Server Essentials 徽标必须具有的最小宽度**170 像素**，与保持正确的纵横比， **96 DPI**。  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>通过更改注册表来添加品牌  
  
1.  在服务器上，将鼠标移动到屏幕右上角，然后单击 **“搜索”**。  
  
2.  在搜索框中，键入 **“regedit”**，然后单击 **“Regedit”** 应用程序。  
  
3.  在导航窗格中，依次展开 **“HKEY_LOCAL_MACHINE”**、**“SOFTWARE”**、**“Microsoft”** 和 **“Windows Server”**。 如果 **OEM** 项不存在，请完成下列步骤来创建该项：  
  
    1.  右键单击 **“Windows Server”**，单击 **“新建”**，然后单击 **“项”**。  
  
    2.  键入 **OEM** 作为该项的名称。  
  
4.  （可选）如果要为徽标创建项，则可以创建不同的项以区分徽标的不同语言版本。 例如，如果你已拥有英语版和德语版的徽标，则可以为它们分别创建 en-us 项和 de-de 项。 由于所有徽标文件存储在同一文件夹中，因此提供徽标图像文件的实例时必须加上每种语言的唯一名称。 例如，可以创建名为 DashboardLogo_en.png 和 DashboardLogo_de.png 的文件。  
  
5.  右键单击 **“OEM”** 或右键单击相应的语言项，指向 **“新建”**，然后单击 **“字符串值”**。  
  

6.  输入字符串的名称，然后按 Enter 键。 有关字符串名称和数据值，请参考[注册表字符串和值](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)。  

6.  输入字符串的名称，然后按 Enter 键。 有关字符串名称和数据值，请参考[注册表字符串和值](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)。  

  
7.  右键单击新字符串，然后单击 **“修改”**。  
  
8.  输入表中与此字符串名称相关联的值，然后单击 **“确定”**。  
  
9. 如果要为徽标图像或附加的链接创建项，请将此文件复制到 %programFiles%\Windows Server\Bin\OEM 中。 如果 OEM 目录不存在，请创建该目录。  
  
10. 如果所做更改会影响远程 Web 访问，则客户拥有该服务器后，必须打开远程 Web 访问。 建议客户执行下列操作：  
  
    1.  在仪表板上，单击 **“设置”**，然后单击 **“随处访问”** 选项卡。  
  
    2.  如果已打开“随处访问”，单击 **“配置”**，然后清除“设置随处访问向导”中 **“选择要启用的随处访问功能”** 页面上的“远程 Web 访问”复选框。  
  
    3.  单击 **“配置”**。  
  
###  <a name="BKMK_RegStrings"></a> 下表列出了的位置的注册表更改将影响品牌、 字符串名称和数据值。  
  
### <a name="registry-strings-and-values"></a>注册表字符串和值  
  
|品牌位置|描述|字符串名称|数据值|  
|--------------------------|-----------------|-----------------|----------------|  
|仪表板徽标|向仪表板添加徽标图像。 仪表板徽标必须采用 .png 格式，且不能大于 350 像素宽 x 38 像素高。<br /><br /> **重要说明：** 若要使用你的徽标为仪表板添加联合品牌，你必须在遵循相应的空白要求的同时对 OPK DVD 上提供的平铺图进行编辑，并将你公司的徽标附加到图像中。 有关其他信息，请参阅提供的示例平铺图。|DashboardLogo|徽标图像文件的名称|  
|DashboardClientLogo|将徽标图像添加到仪表板客户端登录屏幕。|DashboardClientLogo|徽标图像文件的名称|  
|网站背景图片|更改显示在远程 Web 访问登录页面上的背景图像。 典型的分辨率如下所示：<br /><br /> -1024 x 768 像素的分辨率，图像完全填充登录页面<br /><br /> -800 x 600 像素的分辨率将位于页面中央，显示黑色边框<br /><br /> -1280 x 720 像素的分辨率将居中显示，不会出现超过 1024 x 768 像素|LogonBackground|背景图像文件的名称|  
|网站标题|将替换为你选择的标题从 Windows Server Essentials 远程 Web 访问网站的标题。|WebsiteName|新的远程 Web 访问网站标题|  
|网站徽标|更改远程 Web 访问网站上的默认徽标。 徽标的预期大小是 32 x 32 像素。 如果你的徽标大于或小于此尺寸，则将根据这些尺寸拉伸或缩小徽标|WebsiteLogo|徽标图像文件的名称|  
|附加的网站徽标|你的合作伙伴徽标将恰好显示在远程 Web 访问网站中显示的 Microsoft 徽标下。 徽标的预期大小是 200 像素高 x 50 像素宽。 如果你的徽标大于此尺寸，则将在保持原始纵横比的情况下根据该尺寸缩小徽标。 如果你的徽标小于此尺寸，它将在 200 x 50 像素的空间中居中放置，且大小和纵横比保持不变。|OEMLogo|徽标图像文件的名称|  

|在网站主页和登录页面上的链接 |将链接附加到的登录页面和远程 Web 访问站点的主页。 包含链接信息的 .xml 文件必须位于 %programFiles%\Windows Server\Bin\OEM 中。 以下示例显示了 .xml 文件的格式：<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> < 链接名称\=LogonLinkName ><br /> <Text\>LogonLinkDescription</Text\><br /> <Url\>LogonLinkURL</Url\><br /> < 图标\>LinkIcon < / 图标\><br /> < / 链接\><br /> < / LogonLinks\><br /> <HomepageLinks\><br /> < 链接名称\=HomepageLinkName ><br /> <Text\>HomepageLinkDescription</Text\><br /> <Url\>HomepageLinkURL</Url\><br /> < / 链接\><br /> </HomepageLinks\><br /> < / OemLinks\>|LinksXML |请参阅[LinksXML 元素](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)表获取一列的元素和说明。 |  

|在网站主页和登录页面上的链接 |将链接附加到的登录页面和远程 Web 访问站点的主页。 包含链接信息的 .xml 文件必须位于 %programFiles%\Windows Server\Bin\OEM 中。 以下示例显示了 .xml 文件的格式：<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> < 链接名称\=LogonLinkName ><br /> <Text\>LogonLinkDescription</Text\><br /> <Url\>LogonLinkURL</Url\><br /> < 图标\>LinkIcon < / 图标\><br /> < / 链接\><br /> < / LogonLinks\><br /> <HomepageLinks\><br /> < 链接名称\=HomepageLinkName ><br /> <Text\>HomepageLinkDescription</Text\><br /> <Url\>HomepageLinkURL</Url\><br /> < / 链接\><br /> </HomepageLinks\><br /> < / OemLinks\>|LinksXML |请参阅[LinksXML 元素](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)表获取一列的元素和说明。 |  

|快速启动板徽标 |向快速启动板添加徽标图像。 快速启动板徽标必须采用.png 格式并且必须不能高于 64 像素。 |LaunchpadLogo |徽标图像文件的名称 |  
  
###  <a name="BKMK_Links"></a> 下表列出并描述了 LinksXML 字符串名称元素。  
  
### <a name="linksxml-elements"></a>LinksXML 元素  
  
|LinksXML 元素|描述|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|登录链接的名称。|  
|LogonLinkDescription|显示为登录页面链接的文本。|  
|LogonLinkURL|解析为登录页面链接的 URL。|  
|LinkIcon|登录链接的图标文件的名称。 此文件应与 .xml 文件位于同一文件夹位置。 链接图标图像应为 16 x 16 像素，并应采用 .png 格式。 如果不提供 LinkIcon，则会使用默认的链接图标图像。|  
|**HomepageLinks**|  
|HomepageLinkName|主页链接的名称。|  
|HomepageLinkDescription|显示为主页链接的文本。|  
|HomepageLinkURL|解析为主页链接的 URL。|  
|HomepageLinkIcon|主页链接的图标文件的名称。 此文件应与 .xml 文件位于同一文件夹位置。 HomepageLinkIcon 图像应为 16 x 16 像素，并应采用 .png 格式。 如果不提供 HomepageLinkIcon，则会使用默认的主页链接图标图像。|  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [部署准备的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

