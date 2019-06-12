---
title: 步骤 7 测试连接返回到公司网络时
description: 本主题是一部分的测试实验室指南-使用 Windows Server 2016 的 Windows NLB 的群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: af22b1bbc923b9a06e4aebb910690050af1b7492
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446633"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>步骤 7 测试连接返回到公司网络时

>适用于：Windows 服务器 （半年频道），Windows Server 2016

因此，当用户返回到公司网络，它们是访问资源而无需进行任何配置更改，许多用户将远程位置和公司网络中、 之间移动。 远程访问使这可能因为当 DirectAccess 客户端返回公司网络时，它能够连接到网络位置服务器。 一旦成功建立到网络位置服务器的 HTTPS 连接，DirectAccess 客户端禁用 DirectAccess 客户端配置，并使用直接连接到公司网络。  
  
### <a name="test-connectivity-on-client1"></a>在 CLIENT1 上测试连接  
  
1. 关闭客户端 1 从 Homenet 子网或虚拟交换机中拔出客户端 1 并将其连接到公司网络子网或虚拟交换机。 在 CLIENT1 上，打开并以 corp\user1 身份登录。  
  
2. 打开提升的 Windows PowerShell 窗口，键入**ipconfig /all**，然后按 ENTER。 输出将指示客户端 1 有本地 IP 地址，并且没有任何 active 6to4、 Teredo 或 IP-HTTPS 隧道。  
  
3. 测试连接到网络共享在 APP2 上。 上**启动**屏幕上，键入<strong>\\\APP2\Files</strong>，然后按 ENTER。 你将能够在该文件夹中打开该文件。  
  


