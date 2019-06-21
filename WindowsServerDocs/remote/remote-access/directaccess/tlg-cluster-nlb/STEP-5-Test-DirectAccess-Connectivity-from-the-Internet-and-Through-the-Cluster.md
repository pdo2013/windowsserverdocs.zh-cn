---
title: 步骤 5 测试 DirectAccess 连接来自 Internet 和通过群集
description: 本主题是一部分的测试实验室指南-使用 Windows Server 2016 的 Windows NLB 的群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23aed915edb827fd0cd61e6778167108647269ea
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283346"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>步骤 5 测试 DirectAccess 连接来自 Internet 和通过群集

>适用于：Windows 服务器 （半年频道），Windows Server 2016

现已准备好测试 DirectAccess 客户端 1。  
  
- 测试来自 Internet 的 DirectAccess 连接。 将 CLIENT1 连接到模拟的 Internet。 连接到模拟 Internet 时，客户端分配的公共 IPv4 地址。 DirectAccess 客户端分配的公共 IPv4 地址，它尝试建立与使用 IPv6 转换技术在远程访问服务器的连接。  
  
- 测试通过群集的 DirectAccess 客户端连接。 测试群集功能。 在开始测试之前，我们建议您先关闭 EDGE1 和 EDGE2 至少五分钟。 有多种原因为此，其中包括 ARP 缓存超时和 NLB 与相关的更改。 验证 NLB 配置测试实验室中的，需要根据配置中的更改将不会立即反映在连接之前经过一段时间后请耐心等待。 务必在您执行以下任务时牢记这一点。  
  
    > [!TIP]  
    > 我们建议执行此过程之前清除 Internet Explorer 缓存，并每次测试通过不同的远程访问服务器，以确保你正在测试连接和不从缓存中检索网页的连接。  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>测试来自 Internet 的 DirectAccess 连接  
  
1. 从公司网络交换机中拔出 CLIENT1，并将其连接到 Internet 交换机。 等待 30 秒。  
  
2. 在提升的 Windows PowerShell 窗口，键入**ipconfig /flushdns**然后按 ENTER。 这将刷新名称解析条目可能仍存在从客户端 DNS 缓存中的客户端计算机连接到公司网络时。  
  
3. 在 Windows PowerShell 窗口中，键入**Get-dnsclientnrptpolicy**然后按 ENTER。  
  
   该输出显示名称解析策略表 (NRPT) 的当前设置。 这些设置表明所有连接。 应通过远程访问 DNS 服务器，使用 IPv6 地址并使用 2001:db8:1::2 解决 corp.contoso.com。 另外，请注意，NRPT 条目表明该值指示名称 nls.corp.contoso.com 存在免除条目；免除列表上的名称不由远程访问 DNS 服务器应答。 您可以 ping 远程访问 DNS 服务器 IP 地址，以确认连接到远程访问服务器;例如，您可以对 2001:db8:1::2 执行 ping。  
  
4. 在 Windows PowerShell 窗口中，键入**ping app1**然后按 ENTER。 应为 APP1，看到的回复的 IPv6 地址，它在这种情况下是 2001:db8:1::3 的回复。  
  
5. 在 Windows PowerShell 窗口中，键入**ping app2**然后按 ENTER。 你应该看到由 EDGE1 分配给 APP2 的来自 NAT64 地址的答复，在本例中为 fdc9:9f4e:eb1b:7777::a00:4。  
  
   Ping APP2 的功能很重要，因为你能够 APP2 原样 IPv4 唯一资源建立连接使用 nat64/dns64，成功指示。  
  
6. 保持 Windows PowerShell 窗口打开状态下一步的过程。  
  
7. 打开 Internet Explorer，Internet Explorer 地址栏中，输入 **https://app1/** 然后按 ENTER。 在 APP1 上，你将看到默认的 IIS 网站。  
  
8. 在 Internet Explorer 地址栏中，输入 **https://app2/** 然后按 ENTER。 在 APP2 上，你将看到默认网站。  
  
9. 上**启动**屏幕上，键入<strong>\\\App2\Files</strong>，然后按 ENTER。 双击“新文本文档”文件。  
  
    此示例演示你能够连接到 IPv4 唯一的服务器使用 SMB 来获取资源域中的资源。  
  
10. 上**启动**屏幕上，键入**wf.msc**，然后按 ENTER。  
  
11. 在中**高级安全 Windows 防火墙**控制台中，注意到只有**专用**或**公共配置文件**处于活动状态。 为 DirectAccess 才能正常工作，必须启用 Windows 防火墙。 如果 Windows 防火墙处于禁用状态，DirectAccess 连接不会不起作用。  
  
12. 在控制台的左窗格中，展开**监视**节点，然后单击**连接安全规则**节点。 应会看到活动连接安全规则：**DirectAccess 策略 ClientToCorp**， **DirectAccess 策略 ClientToDNS64NAT64PrefixExemption**， **DirectAccess 策略 ClientToInfra**，并**DirectAccess策略 ClientToNlaExempt**。 向下滚动至右侧，显示的中间窗格**第一身份验证方法**并**第二个身份验证方法**列。 请注意，第一条规则 (ClientToCorp) 使用 Kerberos V5 建立 intranet 隧道和第三条规则 (ClientToInfra) 使用 NTLMv2 建立基础结构隧道。  
  
13. 在控制台的左窗格中，展开**安全关联**节点，然后单击**主模式**节点。 请注意，使用 NTLMv2 的基础结构隧道安全关联和 intranet 隧道安全关联使用 Kerberos V5。 右键单击显示的条目**用户 (Kerberos V5)** 作为**第二个身份验证方法**然后单击**属性**。 上**常规**选项卡上，请注意**第二个身份验证本地 ID**是**CORP\User1**，指示 User1 已能够成功进行身份验证到 CORP 域使用Kerberos。  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>测试通过群集的 DirectAccess 客户端连接  
  
1. 对 EDGE2 执行正常关闭。  
  
   可以使用网络负载平衡管理器来查看服务器的状态时运行这些测试。  
  
2. 在 CLIENT1 上，在 Windows PowerShell 窗口中，键入**ipconfig /flushdns**然后按 ENTER。 这将刷新名称解析条目可能仍存在于客户端 DNS 缓存。  
  
3. 在 Windows PowerShell 窗口中，ping APP1 和 APP2。 从这两个这些资源，应该收到答复。  
  
4. 上**启动**屏幕上，键入<strong>\\\app2\files</strong>。 在 APP2 计算机上看到的共享的文件夹。 打开在 APP2 上的文件共享的能力指示需要 Kerberos 身份验证的用户，第二个隧道正常工作。  
  
5. 打开 Internet Explorer，然后打开网站 https://app1/ 和 https://app2/ 。 若要打开这两个网站的功能确认第一个和第二个隧道已启动并正常运行。 关闭 Internet Explorer。  
  
6. 启动 EDGE2 计算机。  
  
7. EDGE1 上执行正常关闭。  
  
8. 等待 5 分钟，然后返回到客户端 1。 执行步骤 2-5。 这会确认客户端 1 是能够以透明方式故障转移到 EDGE2 后 EDGE1 变得不可用。
