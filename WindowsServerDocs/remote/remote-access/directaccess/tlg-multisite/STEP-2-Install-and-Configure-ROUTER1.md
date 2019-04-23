---
title: 步骤 2 的安装和配置路由器 1
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a771b5eb8587d23bc67a7e7769264251afdb5bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838508"
---
# <a name="step-2-install-and-configure-router1"></a>步骤 2 的安装和配置路由器 1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在此多站点测试实验室指南中，路由器计算机提供公司网络和 2 Corpnet 子网之间的 IPv4 和 IPv6 桥和充当路由器的 IP-HTTPS 和 Teredo 通信。  
  
- 路由器 1 上安装操作系统 
  
- 配置 TCP/IP 属性，并将重命名计算机  
  
- 关闭防火墙
  
- 配置路由和转发
  
## <a name="install-the-operating-system-on-router1"></a>路由器 1 上安装操作系统  
首先，安装 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-router1"></a>若要在路由器 1 上安装操作系统  
  
1.  启动 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完全安装） 安装。  
  
2.  按照说明完成安装，指定本地管理员账户的密码（强）。 使用本地管理员账户登录。  
  
3.  连接到具有 Internet 访问权限的网络路由器 1 和运行 Windows 更新，以便为 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，安装最新的更新，然后从 Internet 断开连接。  
  
4.  连接到公司网络和 2 Corpnet 子网的路由器 1。  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>配置 TCP/IP 属性，并将重命名计算机  
在路由器上配置 TCP/IP 设置，并将计算机重命名为路由器 1。  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>若要配置 TCP/IP 属性和重命名计算机  
  
1.  在服务器管理器控制台中，单击**本地服务器**，然后在**属性**区域中，为下的一步**有线以太网连接**，单击的链接。  
  
2.  在中**网络连接**窗口中，右键单击连接到公司网络的网络适配器，单击**重命名**，类型**Corpnet**，然后按 ENTER。  
  
3.  右键单击**Corpnet**，然后单击**属性**。  
  
4.  单击 **“Internet 协议版本 4 (TCP/IPv4)”**，然后单击 **“属性”**。  
  
5.  单击 **“使用下面的 IP 地址”**。 在中**IP 地址**，类型**10.0.0.254**。 在中**子网掩码**，类型**255.255.255.0**，然后单击**确定**。  
  
6.  单击“Internet 协议版本 6 (TCP/IPv6)” ，然后单击“属性” 。  
  
7.  单击**使用下列 IPv6 地址**。 在中**IPv6 地址**，类型**2001:db8:1::fe**。 在中**子网前缀长度**，类型**64**，然后单击**确定**。  
  
8.  上**Corpnet 属性**对话框中，单击**关闭**。  
  
9. 在中**网络连接**窗口中，右键单击连接到 2 公司网络的网络适配器，单击**重命名**，类型**2 Corpnet**，然后按 ENTER。  
  
10. 右键单击**2 Corpnet**，然后单击**属性**。  
  
11. 单击 **“Internet 协议版本 4 (TCP/IPv4)”**，然后单击 **“属性”**。  
  
12. 单击 **“使用下面的 IP 地址”**。 在中**IP 地址**，类型**10.2.0.254**。 在中**子网掩码**，类型**255.255.255.0**，然后单击**确定**。  
  
13. 单击“Internet 协议版本 6 (TCP/IPv6)” ，然后单击“属性” 。  
  
14. 单击**使用下列 IPv6 地址**。 在中**IPv6 地址**，类型**2001:db8:2::fe**。 在中**子网前缀长度**，类型**64**，然后单击**确定**。  
  
15. 上**2 公司网络属性**对话框中，单击**关闭**。  
  
16. 关闭 **“网络连接”** 窗口。  
  
17. 在服务器管理器控制台中，在**本地服务器**，在**属性**区域中，为下的一步**计算机名称**，单击的链接。  
  
18. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”**。  
  
19. 上**计算机名/域更改**对话框中**计算机名**，类型**路由器 1**，然后单击**确定**。  
  
20. 当系统提示你必须重新启动计算机时，请单击“确定” 。  
  
21. 在“系统属性”  对话框中单击“关闭” 。  
  
22. 当系统提示你重新启动计算机时，请单击“立即重新启动” 。  
  
23. 重新启动计算机后，请使用本地管理员帐户登录。  
  
## <a name="turn-off-the-firewall"></a>关闭防火墙  
此计算机配置只是为了提供公司网络和 2 Corpnet 子网; 之间的路由因此，防火墙必须关闭状态。  
  
### <a name="to-turn-off-the-firewall"></a>若要关闭防火墙  
  
1.  上**启动**屏幕上，键入**wf.msc**，然后按 ENTER。  
  
2.  在具有高级安全性的 Windows 防火墙中**操作**窗格中，单击**属性**。  
  
3.  上**高级安全 Windows 防火墙**对话框中，在**域配置文件**选项卡上，在**防火墙状态**，单击**关闭**。  
  
4.  上**高级安全 Windows 防火墙**对话框中，在**专用配置文件**选项卡上，在**防火墙状态**，单击**关闭**。  
  
5.  上**高级安全 Windows 防火墙**对话框中，在**公共配置文件**选项卡上，在**防火墙状态**，单击**关闭**，，然后单击**确定**。  
  
6.  具有高级安全性的关闭 Windows 防火墙。  
  
## <a name="configure-routing-and-forwarding"></a>配置路由和转发  
若要提供路由和转发服务公司网络和 2 Corpnet 子网之间，必须启用网络接口上的转发并配置子网之间的静态路由。  
  
### <a name="to-configure-static-routes"></a>若要配置静态路由  
  
1.  上**启动**屏幕上，键入**cmd.exe**，然后按 ENTER。  
  
2.  启用使用以下命令的这两个网络适配器的 IPv4 和 IPv6 接口上的转发。 输入每个命令后, 按 ENTER。  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  允许公司网络和 2 Corpnet 子网之间路由的 IP-HTTPS。  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  启用 Teredo 公司网络和 2 Corpnet 子网之间路由。  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  关闭命令提示符窗口。
