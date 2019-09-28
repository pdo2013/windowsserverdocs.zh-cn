---
title: 步骤7从 Internet 测试 DirectAccess 连接
description: 本主题是测试实验室指南的一部分-演示带有 OTP 身份验证的 DirectAccess 和用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 738e0f10762c0d292e344ba25fa34cdb0d17b766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367544"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>步骤7从 Internet 测试 DirectAccess 连接

>适用于：Windows Server（半年频道）、Windows Server 2016

DirectAccess 一次性密码（OTP）部署已从 Homenet 子网测试，现在可以从 Internet 进行测试。  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>在 CLIENT1 上通过 Internet 测试 OTP 功能  
  
1. 在 CLIENT1 上，确保以**User1**身份登录。 将 CLIENT1 连接到公司网络子网。  
  
2. 在 "**开始**" 屏幕上，键入 "**powershell**"，右键单击 " **Powershell**"，单击 "**高级**"，然后单击 "以**管理员身份运行**"。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
3. 在 Windows PowerShell 窗口中，键入**gpupdate/force** ，然后按 enter。  
  
4. 从 Homenet 子网中拔下 CLIENT1，将其连接到 Internet，然后重新启动计算机。  
  
5. 在 CLIENT1 上，打开 Internet Explorer，并在地址栏中键入 **https://app1.corp.contoso.com/** ，然后按 enter。 按 F5。  
  
   站点不应打开。  
  
6. 在 "**开始**" 屏幕上，键入 "**rsa**"，然后单击 " **rsa SecurID 令牌**"。  
  
7. 等待 RSA SecurID 令牌更改一次性密码，然后单击 "**复制**"。  
  
8. 单击通知区域中的“网络连接” 图标以访问 DA 媒体管理器。  
  
9. 单击 "**工作区连接**"，然后单击 "**继续**"。  
  
10. 按 Ctrl + Alt + Delete，然后单击 "**一次性密码（OTP）** " 磁贴。  
  
11. 粘贴先前复制的8个数字令牌符号，并单击 **"确定"** 。 等待身份验证完成。 此时将**连接**DirectAccess 工作区连接状态。  
  
12. 在 Internet Explorer 的地址栏中，键入 " **https://app1.corp.contoso.com/** "，然后按 enter。 按 F5。 在 APP1 上，你将看到默认的 IIS 网站。  
  
13. 在 Internet Explorer 地址栏中，键入 " **https://app2.corp.contoso.com/** "，然后按 enter。 按 F5。 你将在 APP2 上看到默认的 IIS 网站。  
  
14. 在 "**开始**" 屏幕上，键入<strong>\\ \ app1\files</strong>，然后按 enter。  
  
15. 在 "**文件**" 共享文件夹窗口中，双击 " **Example .txt** " 文件。 你将看到示例 .txt 文件的内容。  
  
16. 在 "**开始**" 屏幕上，键入<strong>\\ \ app2\files</strong>，然后按 enter。  
  
17. 在 "**文件**" 共享文件夹窗口中，双击新的 "**文本文档 .txt** " 文件。 你将看到新的文本文档 .txt 文件的内容。  
  


