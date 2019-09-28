---
title: 步骤6从 NAT 设备后面测试 DirectAccess 客户端连接
description: 本主题是测试实验室指南的一部分-使用 windows Server 2016 的 Windows NLB 在群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 472c1dc6c5531a7c8d41e40bc926bb3e25f73448
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367596"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>步骤6从 NAT 设备后面测试 DirectAccess 客户端连接

>适用于：Windows Server（半年频道）、Windows Server 2016

当 DirectAccess 客户端从 NAT 设备或 Web 代理服务器后面连接到 Internet 时，DirectAccess 客户端使用 Teredo 或 IP-HTTPS 连接到远程访问服务器。 

如果 NAT 设备对远程访问服务器的公共 IP 地址启用出站 UDP 端口3544，则使用 Teredo。 如果 Teredo 访问不可用，则 DirectAccess 客户端通过出站 TCP 端口 443 回退到 IP-HTTPS，从而通过传统的 SSL 端口实现通过防火墙或 Web 代理服务器的访问。 

如果 Web 代理要求身份验证，则 IP-HTTPS 连接将失败。 如果 Web 代理执行出站 SSL 检查，则 IP-HTTPS 连接也会失败，因为是在 Web 代理而不是在远程访问服务器上终止 HTTPS 会话。 在本部分中，你将执行与在上一部分中使用 6to4 连接进行连接时相同的测试。  
  
在这两个客户端计算机上执行下列步骤：  
  
1. 测试 Teredo 连接。 当 DirectAccess 客户端配置为使用 Teredo 时，将执行第一组测试。 当 NAT 设备允许对 UDP 端口 3544 进行出站访问时，这是自动设置。  
  
2. 测试 IP-HTTPS 连接。 当 DirectAccess 客户端配置为使用 IP-HTTPS 时，将执行第二组测试。 为了演示 IP-HTTPS 连接，在客户端计算机上禁用 Teredo。  
  
> [!TIP]  
> 建议你先清除 Internet Explorer 缓存，然后再执行这些过程，以确保测试连接而不是从缓存中检索网站页面。  
  
## <a name="prerequisites"></a>先决条件

在执行这些测试之前，将 CLIENT1 从 Internet 交换机中拔出并将其连接到 Homenet 交换机。 如果系统询问你希望将当前网络定义为哪种类型的网络，请选择“家庭网络”。  
  
如果尚未运行，请启动 EDGE1 和 EDGE2。  
  
## <a name="test-teredo-connectivity"></a>测试 Teredo 连接  
  
1. 在 CLIENT1 上，打开提升的 Windows PowerShell 窗口，键入**ipconfig/all** ，然后按 enter。  
  
2. 检查 ipconfig 命令的输出。  
  
   CLIENT1 现在已从 NAT 设备后面连接到 Internet 并且分配了专用 IPv4 地址。 当 DirectAccess 客户端在 NAT 设备后面并且分配了专用 IPv4 地址时，首选的 IPv6 转换技术是 Teredo。 如果查看 ipconfig 命令的输出，你应该会看到隧道适配器 Teredo 隧道伪接口部分，然后是说明 Microsoft Teredo 隧道适配器，其中 IP 地址以2001开头：与成为 Teredo 一致地址. 如果您看不到 Teredo 部分，使用以下命令启用 Teredo：**netsh interface Teredo set state enterpriseclient**，然后重新运行 ipconfig 命令。 你不会看到为 Teredo 隧道适配器列出的默认网关。  
  
3. 在 Windows PowerShell 窗口中，键入**ipconfig/flushdns** ，然后按 enter。  
  
   这将刷新名称解析条目，这些条目可能从客户端计算机连接到 Internet 时开始，一直存在于客户端 DNS 缓存中。  
  
4. 在 Windows PowerShell 窗口中，键入**get-dnsclientnrptpolicy** ，然后按 enter。  
  
   该输出显示名称解析策略表 (NRPT) 的当前设置。 这些设置表明所有到 corp.contoso.com 的连接应由远程访问 DNS 服务器解析，并使用 2001:db8:1::2 的 IPv6 地址。 另外，请注意，NRPT 条目表明该值指示名称 nls.corp.contoso.com 存在免除条目；免除列表上的名称不由远程访问 DNS 服务器应答。 你可以对远程访问 DNS 服务器 IP 地址执行 ping 操作，以确认连接到远程访问服务器；例如，你可以在此示例中对 2001:db8:1::2 执行 ping 操作。  
  
5. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 你应看到来自 APP1 的 IPv6 地址 2001:db8:1::3 的回复。  
  
6. 在 Windows PowerShell 窗口中，键入**ping app2** ，然后按 enter。 你应该看到由 EDGE1 分配给 APP2 的来自 NAT64 地址的答复，在本例中为 fdc9:9f4e:eb1b:7777::a00:4。  
  
7. 将 Windows PowerShell 窗口保持打开状态以进行下一过程。  
  
8. 打开 Internet Explorer，在 Internet Explorer 地址栏中，输入 **https://app1/** ，然后按 enter。 在 APP1 上，你将看到默认的 IIS 网站。  
  
9. 在 Internet Explorer 地址栏中，输入 **https://app2/** "，然后按 enter。 在 APP2 上，你将看到默认网站。  
  
10. 在 "**开始**" 屏幕上，键入<strong>\\ \ App2\Files</strong>，然后按 enter。 双击“新文本文档”文件。 此示例演示你能够连接到 IPv4 唯一的服务器是使用 SMB 来获取 IPv4 唯一的主机上的资源。  
  
## <a name="test-ip-https-connectivity"></a>测试 IP-HTTPS 的连接  
  
1. 打开提升的 Windows PowerShell 窗口，键入**netsh interface teredo set state disabled** ，然后按 enter。 这将禁用客户端计算机上的 Teredo，并使客户端计算机能够将自身配置为使用 IP-HTTPS。 命令完成后，会出现“确定”响应。  
  
2. 在 Windows PowerShell 窗口中，键入**ipconfig/all** ，然后按 enter。  
  
3. 检查 ipconfig 命令的输出。 此计算机现在已从 NAT 设备后面连接到 Internet，并且分配了专用 IPv4 地址。 Teredo 将禁用，并且 DirectAccess 客户端回退到 IP-HTTPS。 查看 ipconfig 命令的输出时，你会看到一个 "隧道适配器 iphttpsinterface" 部分，其中的 IP 地址以 "2001： db8：1： 100" 开头，这是基于在设置DirectAccess. 你不会看到为 IP-HTTPS 隧道适配器列出的默认网关。  
  
4. 在 Windows PowerShell 窗口中，键入**ipconfig/flushdns** ，然后按 enter。 这将刷新名称解析条目，这些条目在从客户端计算机连接到公司网络时开始，可能仍然存在于客户端 DNS 缓存中。  
  
5. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 你应看到来自 APP1 的 IPv6 地址 2001:db8:1::3 的回复。  
  
6. 在 Windows PowerShell 窗口中，键入**ping app2** ，然后按 enter。 你应该看到由 EDGE1 分配给 APP2 的来自 NAT64 地址的答复，在本例中为 fdc9:9f4e:eb1b:7777::a00:4。  
  
7. 打开 Internet Explorer，在 Internet Explorer 地址栏中，输入 **https://app1/** ，然后按 enter。 在 APP1 上，你将看到默认的 IIS 站点。  
  
8. 在 Internet Explorer 地址栏中，输入 **https://app2/** "，然后按 enter。 在 APP2 上，你将看到默认网站。  
  
9. 在 "**开始**" 屏幕上，键入<strong>\\ \ App2\Files</strong>，然后按 enter。 双击“新文本文档”文件。 此示例演示你能够连接到 IPv4 唯一的服务器是使用 SMB 来获取 IPv4 唯一的主机上的资源。
