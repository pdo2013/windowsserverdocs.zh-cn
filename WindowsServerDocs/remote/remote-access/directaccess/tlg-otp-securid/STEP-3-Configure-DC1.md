---
title: 步骤3配置 DC1
description: 本主题是测试实验室指南的一部分-演示带有 OTP 身份验证的 DirectAccess 和用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7655208fb537e78839f2b459c8df0e24c0573aa7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367554"
---
# <a name="step-3-configure-dc1"></a>步骤3配置 DC1

>适用于：Windows Server（半年频道）、Windows Server 2016

DC1 充当 corp.contoso.com 域的域控制器、DNS 服务器和 DHCP 服务器。 按如下所示配置 DC1：  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>验证 User1 是否具有在 DC1 上定义的用户主体名称  
  
1.  在 DC1 上，打开服务器管理器，然后在左窗格中单击 " **AD DS** "。 右键单击 " **DC1** "，然后选择 " **Active Directory 用户和计算机**"。 在左窗格中展开 " **com\Users**"，然后双击 "User1"。  
  
2.  在 "**帐户**" 选项卡上，验证是否已将 "**用户登录名**" 设置为 User1。 如果不是，则在 "**用户登录名**" 字段中输入**User1** 。  
  
3.  单击 **“确定”** 。 关闭 **“Active Directory 用户和计算机”** 控制台。  
  


