---
title: 返回公司网络时的步骤7测试连接
description: 本主题是测试实验室指南的一部分-使用 windows Server 2016 的 Windows NLB 在群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa89745d6efcae3591bba2aa5a694ee651bc9912
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404853"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>返回公司网络时的步骤7测试连接

>适用于：Windows Server（半年频道）、Windows Server 2016

许多用户都将在远程位置和公司网络之间移动，因此，当他们返回到公司网络时，无需进行任何配置更改，这一点很重要。 远程访问可以实现此功能，因为当 DirectAccess 客户端返回到公司网络时，它能够与网络位置服务器建立连接。 成功建立 HTTPS 连接到网络位置服务器后，DirectAccess 客户端将禁用 DirectAccess 客户端配置，并使用与公司网络的直接连接。  
  
### <a name="test-connectivity-on-client1"></a>在 CLIENT1 上测试连接  
  
1. 关闭 CLIENT1，然后将 CLIENT1 从 Homenet 子网或虚拟交换机中拔出，并将其连接到公司网络子网或虚拟交换机。 打开 CLIENT1，并以 Corp\user1 身份身份登录。  
  
2. 打开提升的 Windows PowerShell 窗口，键入**ipconfig/all**，然后按 enter。 输出将指示 CLIENT1 具有本地 IP 地址，并且没有活动的6to4、Teredo 或 ip-https 隧道。  
  
3. 测试与 APP2 上的网络共享的连接。 在 "**开始**" 屏幕上，键入<strong>\\ \ APP2\Files</strong>，然后按 enter。 您将能够打开该文件夹中的文件。  
  


