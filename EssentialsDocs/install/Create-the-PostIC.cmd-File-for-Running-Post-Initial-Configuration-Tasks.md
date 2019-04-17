---
title: "创建正在运行的文章初始配置任务 PostIC.cmd 文件"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f5042204cd189e3101f5e0126fd98e786a49032d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>创建正在运行的文章初始配置任务 PostIC.cmd 文件

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以通过书写自己的代码，并从一个名为 PostIC.cmd 的脚本文件调用该代码添加后初始配置自定义项。 使用 PostIC.cmd 文件时，你必须遵循以下指南：  
  
-   自定义代码必须静默运行（无法显示用户界面）。  
  
-   自定义代码不能启动服务器的重新启动。 在初始配置将重新启动服务器作为最后一项任务。  
  
-   自定义代码必须运行三分钟在或更短。  
  
 定义 PostIC.cmd 文件如果成功运行该代码返回为 0。 如果返回任何其他值，名为的文件查找操作系统[SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure)，其中包含如果未成功运行 PostIC.cmd 文件中的代码，应运行的代码。 PostIC.cmd 文件和 SetupFailure.cmd 文件必须 C:\Windows\Setup\Scripts 所在的位置。  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>若要定义后初始配置自定义项  
  
1.  编写从 PostIC.cmd 脚本调用的代码。  
  
2.  使用记事本，创建一个名 PostIC.cmd 文件并将通话添加到您在第 1 步中创建的代码。 确保你的代码返回成功值。  
  
3.  在 C:\Windows\Setup\Scripts 保存 PostIC.cmd。  
  
4.  （可选）创建一个 SetupFailure.cmd 文件，其中如果 PostIC.cmd 返回任何 0 之外运行的代码。  
  
###  <a name="BKMK_SetupFailure"></a>SetupFailure.cmd  
 你可以通过使用 SetupFailure.cmd 提供通知在初始配置的问题。 SetupFailure.cmd 文件包含你想要出现问题，则运行的代码。 SetupFailure.cmd 文件位于 C:\Windows\Setup\Scripts 并且任一出现问题时，安装程序任务或 PostIC.cmd 文件返回一个值以外 0 运行。  
  
##### <a name="to-define-notifications"></a>若要定义通知  
  
1.  编写从 SetupFailure.cmd 脚本调用的代码。  
  
2.  使用记事本，创建一个名 SetupFailure.cmd 文件并将通话添加到您在第 1 步中创建的代码。 确保你的代码返回成功值。  
  
3.  在 C:\Windows\Setup\Scripts 保存 SetupFailure.cmd。  
  
## <a name="see-also"></a>请参阅  
 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)