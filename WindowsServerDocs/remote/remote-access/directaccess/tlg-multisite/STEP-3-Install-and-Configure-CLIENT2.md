---
title: 步骤 3 的安装和配置客户端 2
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2c7ff4953fd4369340f55f40f93cfc01d4240b26
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281460"
---
# <a name="step-3-install-and-configure-client2"></a>步骤 3 的安装和配置客户端 2

>适用于：Windows 服务器 （半年频道），Windows Server 2016

CLIENT2 是 Windows 7&reg;用于演示的计算机向后兼容性的 Windows Server 2016 的服务器上运行的远程访问。  
  
1. 在 CLIENT2 上安装操作系统。 安装 Windows&reg; 7 企业版或 Windows&reg;对 CLIENT2 7 旗舰版。  
  
2. 若要将客户端 2 加入到 CORP 域。 将客户端 2 加入到 corp.contoso.com 域中。  
  
## <a name="to-install-the-operating-system-on-client2"></a>在 CLIENT2 上安装操作系统  
  
1.  启动 Windows 7 安装。  
  
2.  时系统会提示输入用户名称，键入**User1**。 当系统提示您输入计算机名称时，键入**CLIENT2**。  
  
3.  当系统提示输入密码的值时，键入强密码两次。  
  
4.  当系统提示输入保护设置的值时，单击**使用推荐设置**。  
  
5.  系统提示输入计算机的当前的位置，单击**工作网络**。  
  
6.  连接到具有 Internet 访问的网络的客户端 2 并运行 Windows 更新安装的 Windows 7 中，最新的更新，然后从 Internet 断开连接。  
  
7.  连接到公司网络子网的客户端 2。  
  
## <a name="user-account-control"></a>用户帐户控制  
在配置 Windows 7 操作系统时，需要单击**继续**上**用户帐户控制**(UAC) 对话框中，对于某些任务。 某些配置任务需要 UAC 审批。 系统提示后，将始终单击**继续**授权这些更改。  
  
## <a name="to-join-client2-to-the-corp-domain"></a>若要将客户端 2 加入到 CORP 域  
  
1.  单击“开始”  、右键单击“计算机”  ，然后单击“属性”  。  
  
2.  上**系统**页上，在**计算机名称、 域和工作组设置**区域中，单击**更改设置**。  
  
3.  在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
4.  上**计算机名/域更改**对话框中，单击**域**，类型**corp.contoso.com**，然后单击**确定**。  
  
5.  当系统提示输入用户名和密码的值时，键入用户名称和 User1 域帐户的密码，然后单击**确定**。  
  
6.  在出现欢迎使用 corp.contoso.com 域的对话框时，单击 **“确定”** 。  
  
7.  当看到一个对话框，框，提示您重新启动计算机，单击**确定**。  
  
8.  上**系统属性**对话框中，单击**关闭**，并看到一个对话框，提示您重新启动计算机，请单击**立即重新启动**。  
  
9. 在计算机重新启动后，请以 corp\user1 身份登录。
