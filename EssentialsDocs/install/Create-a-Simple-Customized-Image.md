---
title: "创建一个简单的自定义的映像"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e18ff5ded94127449072d28d00b98e17dbe63c3a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-simple-customized-image"></a>创建一个简单的自定义的映像

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以使用下面的过程中创建一个简单的自定义的映像：  
  
#### <a name="to-create-the-image"></a>若要创建映像  
  
1.  安装完成后服务器，在初始配置时，第一页按 Shift + F10 启动 cmd 窗口。  
  
2.  创建系统驱动器根下的 SkipIC.txt 文件。  
  
3.  重新启动的服务器。  
  
4.  开始使用 U 盘或 DVD，其中包括 unattend.xml 文件服务器。 有关创建可启动 U 盘上的信息，请参阅[创建可启动 u 盘](Create-a-Bootable-USB-Flash-Drive.md)。  
  
5.  添加到仪表板品牌徽标。 有关添加品牌的详细信息，请参阅[仪表板、 远程网站访问和启动栏添加品牌](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md)。  
  
6.  创建显示自定义的信息，如公司名称、 徽标和 EULA OOBE.xml 文件。 有关 OOBE.xml 文件的详细信息，请参阅[创建 Oobe.xml 文件包括徽标和 EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md)。  
  
7.  如果你未定义 unattend.xml 中，更改默认服务器名称。  
  
8.  默认情况下服务器名称将随机字符串。 更改为另一个 （例如，ContosoServer) 的字符串服务器名称，然后通知你客户新服务器名称。  
  
9. 中所述，以便进行部署准备图像[准备部署该映像](Preparing-the-Image-for-Deployment.md)。  
  
## <a name="see-also"></a>请参阅  
 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)