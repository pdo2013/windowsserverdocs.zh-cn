---
title: 从 NAT 设备后面的步骤 13 测试 DirectAccess 连接
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
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8a9324e7c5b72f60422b1263e76c7d5e14cccf4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880968"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>从 NAT 设备后面的步骤 13 测试 DirectAccess 连接

>适用于：Windows 服务器 （半年频道），Windows Server 2016

当 DirectAccess 客户端从 NAT 设备或 Web 代理服务器后面连接到 Internet 时，DirectAccess 客户端使用 Teredo 或 IP-HTTPS 连接到远程访问服务器。 如果 NAT 设备启用到远程访问服务器的公共 IP 地址的出站 UDP 端口 3544，则使用 Teredo。 如果 Teredo 访问不可用，则 DirectAccess 客户端通过出站 TCP 端口 443 回退到 IP-HTTPS，从而通过传统的 SSL 端口实现通过防火墙或 Web 代理服务器的访问。 如果 Web 代理要求身份验证，则 IP-HTTPS 连接将失败。 如果 Web 代理执行出站 SSL 检查，则 IP-HTTPS 连接也会失败，因为是在 Web 代理而不是在远程访问服务器上终止 HTTPS 会话。  
  
在这两个客户端计算机上执行下列步骤：  
  
1. 测试 Teredo 连接。 DirectAccess 客户端配置为使用 Teredo 时执行第一组测试。 当 NAT 设备允许对 UDP 端口 3544 进行出站访问时，这是自动设置。 首先在客户端 1 上运行测试并 CLIENT2 上运行测试。  
  
2. 测试 IP-HTTPS 连接。 DirectAccess 客户端配置为使用 IP-HTTPS 时执行第二组测试。 为了演示 IP-HTTPS 连接，在客户端计算机上禁用 Teredo。 首先在客户端 1 上运行测试并 CLIENT2 上运行测试。  
  
## <a name="prerequisites"></a>先决条件  
如果它们尚未运行，并确保它们已连接到 Internet 子网，请启动 EDGE1 和 2 EDGE1。  
  
在执行这些测试之前, 从 Internet 交换机中拔出 CLIENT1 和 CLIENT2 并将它们连接到 Homenet 交换机。 如果系统询问你希望将当前网络定义中，选择的网络类型**家庭网络**。  
  
## <a name="TeredoCLIENT1"></a>测试 Teredo 连接  
  
1.  在 CLIENT1 上，打开提升的 Windows PowerShell 窗口。  
  
2.  启用 Teredo 适配器类型**netsh 接口 teredo 设置状态 enterpriseclient**，然后按 ENTER。  
  
3.  在 Windows PowerShell 窗口中，键入**ipconfig /all**然后按 ENTER。  
  
4.  检查 ipconfig 命令的输出。  
  
    此计算机现在已从 NAT 设备后面连接到 Internet，并且分配了专用 IPv4 地址。 当 DirectAccess 客户端在 NAT 设备后面并且分配了专用 IPv4 地址时，首选的 IPv6 转换技术是 Teredo。 如果您看一下 ipconfig 命令的输出，您应看到隧道适配器 Teredo 隧道伪接口的部分，然后描述 Microsoft Teredo 隧道适配器，开头 2001:0 与正在 Teredo 一致的 IP 地址地址。 你应看到为 Teredo 隧道适配器列出的默认网关::。  
  
5.  在 Windows PowerShell 窗口中，键入**ipconfig /flushdns**然后按 ENTER。  
  
    这将刷新名称解析条目，这些条目可能从客户端计算机连接到 Internet 时开始，一直存在于客户端 DNS 缓存中。  
  
6.  在 Windows PowerShell 窗口中，键入**ping app1**然后按 ENTER。 你应看到来自 APP1 的 IPv6 地址 2001:db8:1::3 的回复。  
  
7.  在 Windows PowerShell 窗口中，键入**ping app2**然后按 ENTER。 你应看到的来自 NAT64 地址由 EDGE1 分配给 APP2，在这种情况下是 fd**c9:9f4e:eb1b**: 7777::a00:4。 请注意，粗体显示的值将取决于如何生成地址。  
  
8.  在 Windows PowerShell 窗口中，键入**ping 2 app1**然后按 ENTER。 你应看到来自 2 APP1 2001:db8:2::3 的 IPv6 地址。  
  
9. 打开 Internet Explorer，Internet Explorer 地址栏中，输入**https://2-app1/** 然后按 ENTER。 2-APP1 上，将看到默认的 IIS 网站。  
  
10. 在 Internet Explorer 地址栏中，输入**https://app2/** 然后按 ENTER。 在 APP2 上，你将看到默认网站。  
  
11. 上**启动**屏幕上，键入**\\\App2\Files**，然后按 ENTER。 双击“新文本文档”文件。 此示例演示你能够连接到 IPv4 唯一的服务器是使用 SMB 来获取 IPv4 唯一的主机上的资源。  
  
12. 重复此过程在 CLIENT2 上。  
  
## <a name="IPHTTPS_CLIENT1"></a>测试 IP-HTTPS 连接  
  
1.  在 CLIENT1 上，打开提升 Windows PowerShell 窗口并键入**netsh 接口 teredo 将禁用的状态设置**然后按 ENTER。 这将禁用客户端计算机上的 Teredo，并使客户端计算机能够将自身配置为使用 IP-HTTPS。 命令完成后，会出现“确定”响应。  
  
2.  在 Windows PowerShell 窗口中，键入**ipconfig /all**然后按 ENTER。  
  
3.  检查 ipconfig 命令的输出。 此计算机现在已从 NAT 设备后面连接到 Internet，并且分配了专用 IPv4 地址。 Teredo 将禁用，并且 DirectAccess 客户端回退到 IP-HTTPS。 当你查看 ipconfig 命令的输出时，请参阅用于隧道适配器 iphttpsinterface 2001:db8:1:1000 或与此所基于的前缀的 IP-HTTPS 地址一致 2001:db8:2:2000 开头的 IP 地址的节设置 DirectAccess 时配置。 不会看到为 IPHTTPSInterface 隧道适配器列出的默认网关。  
  
4.  在 Windows PowerShell 窗口中，键入**ipconfig /flushdns**然后按 ENTER。 这将刷新名称解析条目，这些条目在从客户端计算机连接到公司网络时开始，可能仍然存在于客户端 DNS 缓存中。  
  
5.  在 Windows PowerShell 窗口中，键入**ping app1**然后按 ENTER。 你应看到来自 APP1 的 IPv6 地址 2001:db8:1::3 的回复。  
  
6.  在 Windows PowerShell 窗口中，键入**ping app2**然后按 ENTER。 你应看到的来自 NAT64 地址由 EDGE1 分配给 APP2，在这种情况下是 fd**c9:9f4e:eb1b**: 7777::a00:4。 请注意，粗体显示的值将取决于如何生成地址。  
  
7.  在 Windows PowerShell 窗口中，键入**ping 2 app1**然后按 ENTER。 你应看到来自 2 APP1 2001:db8:2::3 的 IPv6 地址。  
  
8.  打开 Internet Explorer，Internet Explorer 地址栏中，输入**https://2-app1/** 然后按 ENTER。 2-APP1 上，将看到默认的 IIS 网站。  
  
9. 在 Internet Explorer 地址栏中，输入**https://app2/** 然后按 ENTER。 在 APP2 上，你将看到默认网站。  
  
10. 上**启动**屏幕上，键入**\\\App2\Files**，然后按 ENTER。 双击“新文本文档”文件。 此示例演示你能够连接到 IPv4 唯一的服务器是使用 SMB 来获取 IPv4 唯一的主机上的资源。  
  
11. 重复此过程在 CLIENT2 上。  
  


