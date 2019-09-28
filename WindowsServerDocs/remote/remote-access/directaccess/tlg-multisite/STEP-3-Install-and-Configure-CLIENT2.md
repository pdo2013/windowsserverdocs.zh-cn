---
title: 步骤3安装和配置 CLIENT2
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b2d8cb1538de35a97ae83888d26abd7ad7bc8b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388316"
---
# <a name="step-3-install-and-configure-client2"></a>步骤3安装和配置 CLIENT2

>适用于：Windows Server（半年频道）、Windows Server 2016

CLIENT2 是一个 Windows 7 @ no__t 0 计算机，用于演示在 Windows Server 2016 服务器上运行的远程访问的向后兼容性。  
  
1. 在 CLIENT2 上安装操作系统。 在 CLIENT2 上安装 Windows @ no__t 7 Enterprise 或 Windows @ no__t-1 7 旗舰版。  
  
2. 将 CLIENT2 加入 CORP 域。 将 CLIENT2 加入到 corp.contoso.com 域。  
  
## <a name="to-install-the-operating-system-on-client2"></a>在 CLIENT2 上安装操作系统  
  
1.  开始安装 Windows 7。  
  
2.  当系统提示输入用户名时，请键入**User1**。 当系统提示你输入计算机名称时，请键入**CLIENT2**。  
  
3.  当系统提示输入密码时，请键入强密码两次。  
  
4.  如果系统提示你输入保护设置，请单击 "**使用推荐设置**"。  
  
5.  当系统提示你输入计算机的当前位置时，单击 "**工作网络**"。  
  
6.  将 CLIENT2 连接到具有 Internet 访问权限的网络，并运行 Windows 更新以安装最新的 Windows 7 更新，然后断开与 Internet 的连接。  
  
7.  将 CLIENT2 连接到公司网络子网。  
  
## <a name="user-account-control"></a>用户帐户控制  
配置 Windows 7 操作系统时，需要在 "**用户帐户控制**（UAC）" 对话框中单击某些任务的 "**继续**"。 几个配置任务需要 UAC 批准。 出现提示时，请始终单击 "**继续**" 以授权这些更改。  
  
## <a name="to-join-client2-to-the-corp-domain"></a>将 CLIENT2 加入 CORP 域  
  
1.  单击“开始”、右键单击“计算机”，然后单击“属性”。  
  
2.  在 "**系统**" 页上的 "**计算机名称、域和工作组设置**" 区域中，单击 "**更改设置**"。  
  
3.  在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
4.  在 "**计算机名/域更改**" 对话框中，单击 "**域**"，键入 " **Corp.contoso.com**"，然后单击 **"确定"** 。  
  
5.  当系统提示你输入用户名和密码时，请键入 User1 域帐户的用户名和密码，然后单击 **"确定"** 。  
  
6.  在出现欢迎使用 corp.contoso.com 域的对话框时，单击 **“确定”** 。  
  
7.  当你看到提示你重新启动计算机的对话框时，请单击 **"确定"** 。  
  
8.  在 "**系统属性**" 对话框中，单击 "**关闭**"，当你看到提示你重新启动计算机的对话框时，请单击 "**立即重新启动**"。  
  
9. 计算机重启后，以 Corp\user1 身份登录。
