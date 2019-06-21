---
title: 步骤 3 配置 DC1
description: 本主题是一部分的测试实验室指南-使用 OTP 身份验证和 Windows Server 2016 的 RSA SecurID 演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 338e214ac10796d3f9864aef74190f2d27f173b7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281349"
---
# <a name="step-3-configure-dc1"></a>步骤 3 配置 DC1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

DC1 将充当域控制器、 DNS 服务器和为 corp.contoso.com 域的 DHCP 服务器。 配置 DC1，如下所示：  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>验证 User1 拥有在 DC1 上定义的用户主体名称  
  
1.  在 DC1 上，打开服务器管理器，然后单击**AD DS**的左窗格中。 右键单击**DC1** ，然后选择**Active Directory 用户和计算机**。 在左窗格中展开**corp.contoso.com\Users**，然后双击 User1。  
  
2.  上**帐户**选项卡上确认**用户登录名**设置为 User1。 如果没有，然后输入**User1**中**用户登录名**字段。  
  
3.  单击 **“确定”** 。 关闭 **“Active Directory 用户和计算机”** 控制台。  
  


