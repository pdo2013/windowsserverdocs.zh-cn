---
title: "替换为 Microsoft Online Service 经销商协议支持 O365 集成模块购买尝试端点 URL"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b690cedd2f692cc6d11af6e05dd0cd4b4ea5a1d6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>替换为 Microsoft Online Service 经销商协议支持 O365 集成模块购买尝试端点 URL

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 如果你是 Microsoft Online Service 经销商协议 (MOSRA) 合作伙伴，以确保你门户，通过处理的客户注册交易记录你将需要更换由 Windows Server Essentials Office 365 集成模块端点 Url。  
  
 集成模块使用下面的四个端点 Url:  
  
1.  Office 365 企业订阅购买端点。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \MSO\  
  
    -   键入 = 注册 SZ  
  
    -   键名称 = MOSRASTDBUY  
  
    -   值 = *xxxxx*，其中 xxxxx 是你的企业版订阅购买 URL。 例如，重视 = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Office 365 企业订阅试用端点。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \MSO\  
  
    -   键入 = 注册 SZ  
  
    -   键名称 = MOSRASTDTRY  
  
    -   值 = *xxxxx*，其中 xxxxx 是你的企业版订阅购买 URL。 例如，重视 = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Office 365 小型企业高级版订阅购买端点。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \MSO\  
  
    -   键入 = 注册 SZ  
  
    -   键名称 = MOSRALITEBUY  
  
    -   值 = *xxxxx*，其中 xxxxx 是你的企业版订阅购买 URL。 例如，重视 = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Office 365 小型企业高级版订阅试用端点。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \MSO\  
  
    -   键入 = 注册 SZ  
  
    -   键名称 = MOSRALITETRY  
  
    -   值 = *xxxxx*，其中 xxxxx 是你的企业版订阅购买 URL。 例如，重视 = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>若要添加对注册表端点 URL 键  
  
1.  在参考计算机上，单击**开始**，类型**regedit**，然后按 ENTER。  
  
2.  在左侧窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，展开**Windows Server**，然后展开**MSO**。  
  
3.  如果 MSO 不存在，请右键单击**Windows Server**，指向**新建**，单击**键**，然后键入**MSO**键的名称。  
  
4.  MSO，右键单击，然后单击**字符串值**。 键入的字符串的名称下端点字符串名称之一：  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  新的字符串右侧窗格中，右键单击，然后单击**修改**。  
  
6.  键入你在新的端点 URL**值数据**文本框中，然后单击**确定**。  
  
7.  第 4 步中列出的每个字符串名称的重复步骤 4 6。  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)[创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

