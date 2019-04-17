---
title: "添加 Microsoft Online Service 合作伙伴协议合作伙伴的记录的信息"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 39ce43228cd7392bcc86de4a410c52676ce15047
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>添加 Microsoft Online Service 合作伙伴协议合作伙伴的记录的信息

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 如果你是 Office 365 的 Microsoft Online Service 合作伙伴协议 (MOSPA) 伙伴，以确保该进行正确补偿时订阅请求源自 Windows Server Essentials 通过 Office 365 集成的模块，需要创建一个包含您的合作伙伴的记录识别 (POR ID) 的注册表项。 以下信息是阅读，以及与服务提供商通过 Office 365 注册 Url。  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \MSO  
  
-   键入 = 字符串值  
  
-   键名称 = 合作伙伴  
  
-   值 = 的 xxxxx，其中 xxxxx 表示 POR ID  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>若要添加对注册表 POR ID 键  
  
1.  在参考计算机上，单击**开始**，类型**regedit**，然后按 ENTER。  
  
2.  在左侧窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，然后展开**Windows Server**。  
  
3.  右键单击**Windows Server**，指向**新建**，然后单击**键**。  
  
4.  键入**MSO**键的名称。  
  
5.  创建，右键单击你刚刚键，然后单击**字符串值**。  
  
6.  键入**合作伙伴**字符串，并按 ENTER 的名称。  
  
7.  右键单击新**合作伙伴**字符串的右窗格中，然后单击**修改**。  
  
8.  键入你 POR ID 采用**值数据**文本框中，然后单击**确定**。  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

