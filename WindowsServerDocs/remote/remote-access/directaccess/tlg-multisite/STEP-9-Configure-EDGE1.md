---
title: 步骤 9 配置 EDGE1
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0a47a436bbd11c795caa8b402054ae0d2c3282f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281367"
---
# <a name="step-9-configure-edge1"></a>步骤 9 配置 EDGE1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

EDGE1 服务器上执行以下过程：  
  
1. EDGE1 上配置的 DNS 服务器。 它是 EDGE1 上配置 DNS 服务器从 corp2.corp.contoso.com 域所必需的。  
  
2. 配置子网之间的路由。 配置路由上 EDGE1 能够公司网络和 2 Corpnet 子网之间的通信。  
  
## <a name="IPv6"></a>EDGE1 上的 DNS 服务器配置  
  
1.  在服务器管理器控制台中，单击**本地服务器**，然后在**属性**区域中，为下的一步**Corpnet**，单击的链接。  
  
2.  在网络连接窗口中，右键单击**Corpnet**，然后单击**属性**。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
4.  在中**备用 DNS 服务器**，类型**10.2.0.1**。 然后单击 **“确定”** 。  
  
5.  单击“Internet 协议版本 6 (TCP/IPv6)”  ，然后单击“属性”  。  
  
6.  在中**备用 DNS 服务器**，类型**2001:db8:2::1** ，然后单击**确定**。  
  
7.  上**Corpnet 属性**对话框中，单击**关闭**。  
  
8.  关闭 **“网络连接”** 窗口。  
  
## <a name="ConfigRouting"></a>配置子网之间路由  
  
1.  上**启动**屏幕上，键入**cmd.exe**，右键单击**cmd**，单击**高级**，然后单击**以运行管理员**。 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。  
  
2.  在命令提示符窗口中，输入以下命令。 输入每个命令后, 按 ENTER。  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  关闭命令提示符窗口。  
  


