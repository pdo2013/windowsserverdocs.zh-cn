---
title: "自定义注册的 Microsoft Online 备份 Service 任务"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cd148e0e58cd80dbff7f7884ead95dc1e46b6257
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>自定义注册的 Microsoft Online 备份 Service 任务

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

默认情况下，**注册 Microsoft Online 备份服务**任务上**设备**仪表板中的选项卡中打开 Microsoft Online Service 备份网站。 网站提供有关该服务的信息，并帮助您订阅服务，并从中下载所需的软件。  
  
 你可以自定义**注册 Microsoft Online 备份服务**以下两种方式任务：  
  
-   你可以将替换默认网站的 URL URL 表示自定义的用户体验。 可替换默认 URL，请打开注册表编辑器、创建的注册表项：**HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \OnlineBackup\LinkUrl**，并作为键值将自定义的 URL。  
  
-   你可以隐藏任务。 若要隐藏任务，打开注册表编辑器并创建的注册表项：**HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \OnlineBackup\OnlineBackupInstalled**。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)