---
title: 步骤 6 测试 DirectAccess 连接从 Homenet 子网
description: 本主题是一部分的测试实验室指南-使用 OTP 身份验证和 Windows Server 2016 的 RSA SecurID 演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c19376b7301068639ba1315af5e9f5a9a4514b3b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283057"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>步骤 6 测试 DirectAccess 连接从 Homenet 子网

>适用于：Windows 服务器 （半年频道），Windows Server 2016

DirectAccess 一次性密码 (OTP) 部署现已完成，可以开始测试从 Homenet 子网的连接。  
  
### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>若要测试从 CLIENT1 上的 Homenet 子网的 OTP 功能  
  
1. 在 CLIENT1 上，请确保： 已经以身份登录**User1**。  
  
2. 上**启动**屏幕上，键入**powershell.exe**，右键单击**powershell**，单击**高级**，然后单击**运行以管理员身份**。 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。  
  
3. 在 Windows PowerShell 窗口中，键入**gpupdate /force**，然后按 ENTER。  
  
4. 从公司网络子网中拔出 CLIENT1，并将其连接到 Homenet 子网。  
  
5. 在 CLIENT1 上，打开 Internet Explorer 中，并在地址栏中，键入 **https://app1.corp.contoso.com/** 然后按 ENTER。 按 F5。  
  
   不应打开该站点。  
  
6. 上**启动**屏幕上，键入**RSA**，然后单击**RSA SecurID 标记**。  
  
7. 等待，直到 RSA SecurID 令牌更改一次性密码，然后依次**复制**。  
  
8. 单击通知区域中的“网络连接”  图标以访问 DA 媒体管理器。  
  
9. 单击**Contoso DirectAccess 连接**，然后单击**继续**。  
  
10. 按控件 + Alt + Delete，然后单击**一次性密码 (OTP)** 磁贴。  
  
11. 粘贴先前复制的八个数字令牌代码，然后单击**确定**。 等待要完成身份验证。 DirectAccess 工作区连接状态现在将**已连接**。  
  
12. 在 Internet Explorer 中，在地址栏中，键入 **https://app1.corp.contoso.com/** 然后按 ENTER。 按 F5。 在 APP1 上，你将看到默认的 IIS 网站。  
  
13. 在 Internet Explorer 地址栏中，键入 **https://app2.corp.contoso.com/** 然后按 ENTER。 按 F5。 在 APP2 上，将看到默认的 IIS 网站。  
  
14. 上**启动**屏幕上，键入<strong>\\\app1\files</strong>，然后按 ENTER。  
  
15. 在中**文件**共享的文件夹窗口中，双击**Example.txt**文件。 您将看到 Example.txt 文件的内容。  
  
16. 上**启动**屏幕上，键入<strong>\\\app2\files</strong>，然后按 ENTER。  
  
17. 在中**文件**共享的文件夹窗口中，双击**新文本文档.txt**文件。 您将看到新文本文档.txt 文件的内容。  
  


