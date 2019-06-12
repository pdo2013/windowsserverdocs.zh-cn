---
title: 创建用于运行后初始配置任务的 PostIC.cmd 文件
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: e15cb8591fc701094dde884d0a55e08d2cf422bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433599"
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>创建用于运行后初始配置任务的 PostIC.cmd 文件

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

可以通过编写自己的代码，然后从名为 PostIC.cmd 的脚本文件调用代码来添加后初始配置自定义。 使用 PostIC.cmd 文件时，必须遵循以下指导原则：  
  
- 必须以静默方式运行自定义代码（它不能显示用户界面）。  
  
- 自定义代码无法启动重新启动服务器的操作。 初始配置将重新启动服务器，并将此作为最后一项任务。  
  
- 必须在三分钟之内运行你的自定义代码。  
  
  将 PostIC.cmd 文件定义为当代码运行成功时返回 0。 如果返回任何其他值，操作系统将查找名为 [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure)的文件，该文件包含当 PostIC.cmd 文件中的代码未成功运行时应该运行的代码。 PostIC.cmd 文件和 SetupFailure.cmd 文件必须位于 C:\Windows\Setup\Scripts。  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>定义后初始配置自定义  
  
1.  编写从 PostIC.cmd 脚本调用的代码。  
  
2.  使用记事本创建名为 PostIC.cmd 的文件，并添加对步骤 1 中创建的代码的调用。 确保代码返回成功值。  
  
3.  将 PostIC.cmd 保存在 C:\Windows\Setup\Scripts 中。  
  
4.  （可选）创建 SetupFailure.cmd 文件，该文件在 PostIC.cmd 返回 0 以外的任何值时运行代码。  
  
###  <a name="BKMK_SetupFailure"></a> SetupFailure.cmd  
 可以使用 SetupFailure.cmd 提供有关初始配置问题的通知。 SetupFailure.cmd 文件包含出现问题时要运行的代码。 SetupFailure.cmd 文件位于 C:\Windows\Setup\Scripts，当执行安装任务过程中出现问题或当 PostIC.cmd 文件返回 0 以外的值时会运行该文件。  
  
##### <a name="to-define-notifications"></a>定义通知  
  
1.  编写从 SetupFailure.cmd 脚本调用的代码。  
  
2.  使用记事本创建名为 SetupFailure.cmd 的文件，并添加对步骤 1 中创建的代码的调用。 确保代码返回成功值。  
  
3.  将 SetupFailure.cmd 保存在 C:\Windows\Setup\Scripts 目录中。  
  
## <a name="see-also"></a>请参阅  
 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)