---
title: 步骤13从 NAT 设备后面测试 DirectAccess 连接
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41701592c0d9b143c84ad3fbad3fd77491eff5a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404715"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>步骤13从 NAT 设备后面测试 DirectAccess 连接

>适用于：Windows Server（半年频道）、Windows Server 2016

当 DirectAccess 客户端从 NAT 设备或 Web 代理服务器后面连接到 Internet 时，DirectAccess 客户端使用 Teredo 或 IP-HTTPS 连接到远程访问服务器。 如果 NAT 设备对远程访问服务器的公共 IP 地址启用出站 UDP 端口3544，则使用 Teredo。 如果 Teredo 访问不可用，则 DirectAccess 客户端通过出站 TCP 端口 443 回退到 IP-HTTPS，从而通过传统的 SSL 端口实现通过防火墙或 Web 代理服务器的访问。 如果 Web 代理要求身份验证，则 IP-HTTPS 连接将失败。 如果 Web 代理执行出站 SSL 检查，则 IP-HTTPS 连接也会失败，因为是在 Web 代理而不是在远程访问服务器上终止 HTTPS 会话。  
  
在这两个客户端计算机上执行下列步骤：  
  
1. 测试 Teredo 连接。 当 DirectAccess 客户端配置为使用 Teredo 时，将执行第一组测试。 当 NAT 设备允许对 UDP 端口 3544 进行出站访问时，这是自动设置。 首先在 CLIENT1 上运行测试，然后在 CLIENT2 上运行测试。  
  
2. 测试 IP-HTTPS 连接。 当 DirectAccess 客户端配置为使用 IP-HTTPS 时，将执行第二组测试。 为了演示 IP-HTTPS 连接，在客户端计算机上禁用 Teredo。 首先在 CLIENT1 上运行测试，然后在 CLIENT2 上运行测试。  
  
## <a name="prerequisites"></a>先决条件  
启动 EDGE1 和 EDGE1 （如果尚未运行），并确保它们已连接到 Internet 子网。  
  
在执行这些测试之前，将 CLIENT1 和 CLIENT2 从 Internet 交换机中拔出并将其连接到 Homenet 开关。 如果系统询问你要定义当前网络的网络类型，请选择 "**家庭网络**"。  
  
## <a name="TeredoCLIENT1"></a>测试 Teredo 连接  
  
1. 在 CLIENT1 上，打开提升的 Windows PowerShell 窗口。  
  
2. 启用 Teredo 适配器，键入**netsh interface Teredo set state enterpriseclient**，然后按 enter。  
  
3. 在 Windows PowerShell 窗口中，键入**ipconfig/all** ，然后按 enter。  
  
4. 检查 ipconfig 命令的输出。  
  
   此计算机现在已从 NAT 设备后面连接到 Internet，并且分配了专用 IPv4 地址。 当 DirectAccess 客户端在 NAT 设备后面并且分配了专用 IPv4 地址时，首选的 IPv6 转换技术是 Teredo。 如果查看 ipconfig 命令的输出，你应该会看到隧道适配器 Teredo 隧道伪接口的部分，然后是说明 Microsoft Teredo 隧道适配器，其中 IP 地址以2001:0 开头并且是 Teredo地址. 应会看到为 Teredo 隧道适配器列出的默认网关为 "：："。  
  
5. 在 Windows PowerShell 窗口中，键入**ipconfig/flushdns** ，然后按 enter。  
  
   这将刷新名称解析条目，这些条目可能从客户端计算机连接到 Internet 时开始，一直存在于客户端 DNS 缓存中。  
  
6. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 你应看到来自 APP1 的 IPv6 地址 2001:db8:1::3 的回复。  
  
7. 在 Windows PowerShell 窗口中，键入**ping app2** ，然后按 enter。 应会看到来自 EDGE1 分配给 APP2 的 NAT64 地址的答复，在本例中为 fd**c9：9f4e： eb1b**：7777：： a00：4。 请注意，由于地址的生成方式，粗体值会有所不同。  
  
8. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 你应看到来自 APP1，2001： db8：2：：3的 IPv6 地址的答复。  
  
9. 打开 Internet Explorer，在 Internet Explorer 地址栏中，输入 **https://2-app1/** ，然后按 enter。 你将在 APP1 上看到默认的 IIS 网站。  
  
10. 在 Internet Explorer 地址栏中，输入 **https://app2/** "，然后按 enter。 在 APP2 上，你将看到默认网站。  
  
11. 在 "**开始**" 屏幕上，键入<strong>\\ \ App2\Files</strong>，然后按 enter。 双击“新文本文档”文件。 此示例演示你能够连接到 IPv4 唯一的服务器是使用 SMB 来获取 IPv4 唯一的主机上的资源。  
  
12. 在 CLIENT2 上重复此过程。  
  
## <a name="IPHTTPS_CLIENT1"></a>测试 IP-HTTPS 连接  
  
1. 在 CLIENT1 上，打开提升的 Windows PowerShell 窗口，键入**netsh interface teredo set state disabled** ，然后按 enter。 这将禁用客户端计算机上的 Teredo，并使客户端计算机能够将自身配置为使用 IP-HTTPS。 命令完成后，会出现“确定”响应。  
  
2. 在 Windows PowerShell 窗口中，键入**ipconfig/all** ，然后按 enter。  
  
3. 检查 ipconfig 命令的输出。 此计算机现在已从 NAT 设备后面连接到 Internet，并且分配了专用 IPv4 地址。 Teredo 将禁用，并且 DirectAccess 客户端回退到 IP-HTTPS。 查看 ipconfig 命令的输出时，会显示一个用于隧道适配器 iphttpsinterface 的部分，该地址的 IP 地址以2001： db8：1：1000或2001： db8：2：2000与这是基于在设置 DirectAccess 时配置。 你将看不到为 IPHTTPSInterface 隧道适配器列出的默认网关。  
  
4. 在 Windows PowerShell 窗口中，键入**ipconfig/flushdns** ，然后按 enter。 这将刷新名称解析条目，这些条目在从客户端计算机连接到公司网络时开始，可能仍然存在于客户端 DNS 缓存中。  
  
5. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 你应看到来自 APP1 的 IPv6 地址 2001:db8:1::3 的回复。  
  
6. 在 Windows PowerShell 窗口中，键入**ping app2** ，然后按 enter。 应会看到来自 EDGE1 分配给 APP2 的 NAT64 地址的答复，在本例中为 fd**c9：9f4e： eb1b**：7777：： a00：4。 请注意，由于地址的生成方式，粗体值会有所不同。  
  
7. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 你应看到来自 APP1，2001： db8：2：：3的 IPv6 地址的答复。  
  
8. 打开 Internet Explorer，在 Internet Explorer 地址栏中，输入 **https://2-app1/** ，然后按 enter。 你将在 APP1 上看到默认的 IIS 网站。  
  
9. 在 Internet Explorer 地址栏中，输入 **https://app2/** "，然后按 enter。 在 APP2 上，你将看到默认网站。  
  
10. 在 "**开始**" 屏幕上，键入<strong>\\ \ App2\Files</strong>，然后按 enter。 双击“新文本文档”文件。 此示例演示你能够连接到 IPv4 唯一的服务器是使用 SMB 来获取 IPv4 唯一的主机上的资源。  
  
11. 在 CLIENT2 上重复此过程。  
  


