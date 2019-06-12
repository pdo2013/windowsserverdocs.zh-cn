---
title: 步骤 7 测试来自 Internet 的 DirectAccess 连接
description: 本主题是一部分的测试实验室指南-使用 OTP 身份验证和 Windows Server 2016 的 RSA SecurID 演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0fe096bcf697e5131eeefc2fa72e61e0096e2f94
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446916"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>步骤 7 测试来自 Internet 的 DirectAccess 连接

>适用于：Windows 服务器 （半年频道），Windows Server 2016

DirectAccess 一次性密码 (OTP) 部署进行了测试从 Homenet 的子网和现在从 Internet 进行测试。  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>若要测试从 CLIENT1 上的 Internet OTP 功能  
  
1. 确保在 CLIENT1 上，已经以身份登录**User1**。 将 CLIENT1 连接到公司网络子网。  
  
2. 上**启动**屏幕上，键入**powershell.exe**，右键单击**powershell**，单击**高级**，然后单击**运行以管理员身份**。 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。  
  
3. 在 Windows PowerShell 窗口中，键入**gpupdate /force**然后按 ENTER。  
  
4. 从 Homenet 子网中拔出客户端 1、 将其连接到 Internet，并重新启动计算机。  
  
5. 在 CLIENT1 上，打开 Internet Explorer 中，并在地址栏中，键入 **https://app1.corp.contoso.com/** 然后按 ENTER。 按 F5。  
  
   不应打开该站点。  
  
6. 上**启动**屏幕上，键入**RSA**，然后单击**RSA SecurID 标记**。  
  
7. 等待，直到 RSA SecurID 令牌更改一次性密码，然后依次**复制**。  
  
8. 单击通知区域中的“网络连接”  图标以访问 DA 媒体管理器。  
  
9. 单击**工作区连接**，然后单击**继续**。  
  
10. 按控件 + Alt + Delete，然后单击**一次性密码 (OTP)** 磁贴。  
  
11. 粘贴先前复制的八个数字令牌代码，然后单击**确定**。 等待要完成身份验证。 DirectAccess 工作区连接状态现在将**已连接**。  
  
12. 在 Internet Explorer 中，在地址栏中，键入 **https://app1.corp.contoso.com/** 然后按 ENTER。 按 F5。 在 APP1 上，你将看到默认的 IIS 网站。  
  
13. 在 Internet Explorer 地址栏中，键入 **https://app2.corp.contoso.com/** 然后按 ENTER。 按 F5。 在 APP2 上，将看到默认的 IIS 网站。  
  
14. 上**启动**屏幕上，键入<strong>\\\app1\files</strong>，然后按 ENTER。  
  
15. 在中**文件**共享的文件夹窗口中，双击**Example.txt**文件。 您将看到 Example.txt 文件的内容。  
  
16. 上**启动**屏幕上，键入<strong>\\\app2\files</strong>，然后按 ENTER。  
  
17. 在中**文件**共享的文件夹窗口中，双击**新文本文档.txt**文件。 您将看到新文本文档.txt 文件的内容。  
  


