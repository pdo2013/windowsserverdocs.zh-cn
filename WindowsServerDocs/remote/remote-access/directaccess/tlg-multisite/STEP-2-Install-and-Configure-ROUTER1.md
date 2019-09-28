---
title: 步骤2安装和配置 ROUTER1
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c6bff2acc15b7ff90731e0113ae0d5a429c635c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404799"
---
# <a name="step-2-install-and-configure-router1"></a>步骤2安装和配置 ROUTER1

>适用于：Windows Server（半年频道）、Windows Server 2016

在此多站点测试实验室指南中，路由器计算机在公司网络和2公司网络之间提供了 IPv4 和 IPv6 桥梁，并充当了 ip-https 和 Teredo 通信的路由器。  
  
- 在 ROUTER1 上安装操作系统 
  
- 配置 TCP/IP 属性并重命名计算机  
  
- 关闭防火墙
  
- 配置路由和转发
  
## <a name="install-the-operating-system-on-router1"></a>在 ROUTER1 上安装操作系统  
首先，安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-router1"></a>在 ROUTER1 上安装操作系统  
  
1.  开始安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 （完全安装）。  
  
2.  按照说明完成安装，指定本地管理员账户的密码（强）。 使用本地管理员账户登录。  
  
3.  将 ROUTER1 连接到具有 Internet 访问权限的网络，并运行 Windows 更新以安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的最新更新，然后从 Internet 断开连接。  
  
4.  将 ROUTER1 连接到公司网络和2公司网络子网。  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>配置 TCP/IP 属性并重命名计算机  
在路由器上配置 TCP/IP 设置，并将计算机重命名为 ROUTER1。  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>配置 TCP/IP 属性和重命名计算机  
  
1.  在服务器管理器控制台中，单击 "**本地服务器**"，然后在 "**有线以太网连接**" 旁边的 "**属性**" 区域中，单击链接。  
  
2.  在 "**网络连接**" 窗口中，右键单击连接到公司网络的网络适配器，单击 "**重命名**"，键入 "**公司**网络"，然后按 enter。  
  
3.  右键单击 "**企业**网络"，然后单击 "**属性**"。  
  
4.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
5.  单击 **“使用下面的 IP 地址”** 。 在 " **IP 地址**" 中，键入**10.0.0.254**。 在 "**子网掩码**" 中，键入**255.255.255.0**，然后单击 **"确定"** 。  
  
6.  单击“Internet 协议版本 6 (TCP/IPv6)”，然后单击“属性”。  
  
7.  单击 **"使用下列 IPv6 地址"** 。 在 " **IPv6 地址**" 中，键入 " **2001： db8：1：： fe**"。 在 "**子网前缀长度**" 中，键入**64**，然后单击 **"确定"** 。  
  
8.  在 "**公司属性**" 对话框中，单击 "**关闭**"。  
  
9. 在 "**网络连接**" 窗口中，右键单击连接到2公司网络的网络适配器，单击 "**重命名**"，键入 " **2-** 网络"，然后按 enter。  
  
10. 右键单击 " **2-企业**"，然后单击 "**属性**"。  
  
11. 单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
12. 单击 **“使用下面的 IP 地址”** 。 在 " **IP 地址**" 中，键入**10.2.0.254**。 在 "**子网掩码**" 中，键入**255.255.255.0**，然后单击 **"确定"** 。  
  
13. 单击“Internet 协议版本 6 (TCP/IPv6)”，然后单击“属性”。  
  
14. 单击 **"使用下列 IPv6 地址"** 。 在 " **IPv6 地址**" 中，键入 " **2001： db8：2：： fe**"。 在 "**子网前缀长度**" 中，键入**64**，然后单击 **"确定"** 。  
  
15. 在 " **2-公司属性**" 对话框中，单击 "**关闭**"。  
  
16. 关闭 **“网络连接”** 窗口。  
  
17. 在服务器管理器控制台中，在 "**本地服务器**" 的 "**属性**" 区域中，单击 "**计算机名**" 旁边的链接。  
  
18. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
19. 在 "**计算机名/域更改**" 对话框的 "**计算机名**" 中，键入**ROUTER1**，然后单击 **"确定"** 。  
  
20. 当系统提示你必须重新启动计算机时，请单击“确定”。  
  
21. 在“系统属性” 对话框中单击“关闭”。  
  
22. 当系统提示你重新启动计算机时，请单击“立即重新启动”。  
  
23. 重新启动计算机后，请用本地管理员帐户登录。  
  
## <a name="turn-off-the-firewall"></a>关闭防火墙  
此计算机配置为仅提供公司网络和2网络子网之间的路由;因此，必须关闭防火墙。  
  
### <a name="to-turn-off-the-firewall"></a>关闭防火墙  
  
1.  在 "**开始**" 屏幕上，键入 "**services.msc**"，然后按 enter。  
  
2.  在 "高级安全 Windows 防火墙" 中，单击 "**操作**" 窗格中的 "**属性**"。  
  
3.  在 "**高级安全 Windows 防火墙**" 对话框的 "**域配置文件**" 选项卡上的 "**防火墙状态**" 中，单击 "**关闭**"。  
  
4.  在 "**高级安全 Windows 防火墙**" 对话框中，在 "**专用配置文件**" 选项卡上的 "**防火墙状态**" 中，单击 "**关闭**"。  
  
5.  在 "**高级安全 Windows 防火墙**" 对话框的 "**公共配置文件**" 选项卡上的 "**防火墙状态**" 中，单击 "**关闭**"，然后单击 **"确定"** 。  
  
6.  关闭 "高级安全 Windows 防火墙"。  
  
## <a name="configure-routing-and-forwarding"></a>配置路由和转发  
若要在公司网络和2公司网络之间提供路由和转发服务，必须在网络接口上启用转发，并在子网之间配置静态路由。  
  
### <a name="to-configure-static-routes"></a>配置静态路由  
  
1.  在 "**开始**" 屏幕上，键入 "**cmd.exe**"，然后按 enter。  
  
2.  使用以下命令在两个网络适配器的 IPv4 和 IPv6 接口上启用转发。 输入每个命令后，按 ENTER。  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  在公司网络和2公司网络子网之间启用 ip-https 路由。  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  在公司网络和2公司网络子网之间启用 Teredo 路由。  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  关闭命令提示符窗口。
