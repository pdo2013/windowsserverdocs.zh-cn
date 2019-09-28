---
title: 步骤5从 Internet 和群集测试 DirectAccess 连接
description: 本主题是测试实验室指南的一部分-使用 windows Server 2016 的 Windows NLB 在群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1b5708e51b2653444fb3eb636baac6a165dfc55d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404844"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>步骤5从 Internet 和群集测试 DirectAccess 连接

>适用于：Windows Server（半年频道）、Windows Server 2016

CLIENT1 现已准备好进行 DirectAccess 测试。  
  
- 测试来自 Internet 的 DirectAccess 连接。 将 CLIENT1 连接到模拟的 Internet。 连接到模拟 Internet 时，会为客户端分配公共 IPv4 地址。 在为 DirectAccess 客户端分配公用 IPv4 地址时，它会尝试使用 IPv6 转换技术建立与远程访问服务器的连接。  
  
- 通过群集测试 DirectAccess 客户端连接。 测试群集功能。 开始测试之前，我们建议你将 EDGE1 和 EDGE2 关闭至少五分钟。 出现此问题的原因有很多，其中包括 ARP 缓存超时和与 NLB 相关的更改。 在测试实验室中验证 NLB 配置时，你将需要患者，因为在一段时间后，配置中的更改将不会立即反映到连接中。 执行以下任务时，请务必记住这一点。  
  
    > [!TIP]  
    > 建议在执行此过程之前清除 Internet Explorer 缓存，并在每次通过不同的远程访问服务器测试连接时，确保测试连接而不是从缓存中检索网页。  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>从 Internet 测试 DirectAccess 连接  
  
1. 从公司网络交换机中拔下 CLIENT1，并将其连接到 Internet 交换机。 等待30秒。  
  
2. 在提升的 Windows PowerShell 窗口中，键入**ipconfig/flushdns** ，然后按 enter。 当客户端计算机连接到公司网络时，此操作会刷新客户端 DNS 缓存中仍存在的名称解析条目。  
  
3. 在 Windows PowerShell 窗口中，键入**get-dnsclientnrptpolicy** ，然后按 enter。  
  
   该输出显示名称解析策略表 (NRPT) 的当前设置。 这些设置指示，与 corp.contoso.com 的所有连接应由远程访问 DNS 服务器解析，IPv6 地址为2001： db8：1：：2。 另外，请注意，NRPT 条目表明该值指示名称 nls.corp.contoso.com 存在免除条目；免除列表上的名称不由远程访问 DNS 服务器应答。 你可以对远程访问 DNS 服务器 IP 地址执行 ping 操作，以确认与远程访问服务器的连接;例如，你可以对2001： db8：1：：2执行 ping 操作。  
  
4. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 应会看到来自 APP1 的 IPv6 地址的答复，在本例中为2001： db8：1：：3。  
  
5. 在 Windows PowerShell 窗口中，键入**ping app2** ，然后按 enter。 你应该看到由 EDGE1 分配给 APP2 的来自 NAT64 地址的答复，在本例中为 fdc9:9f4e:eb1b:7777::a00:4。  
  
   Ping APP2 的功能非常重要，因为 success 表明你能够使用 NAT64/DNS64 建立连接，因为 APP2 是仅适用于 IPv4 的资源。  
  
6. 将 Windows PowerShell 窗口保持打开状态以进行下一过程。  
  
7. 打开 Internet Explorer，在 Internet Explorer 地址栏中，输入 **https://app1/** ，然后按 enter。 在 APP1 上，你将看到默认的 IIS 网站。  
  
8. 在 Internet Explorer 地址栏中，输入 **https://app2/** "，然后按 enter。 在 APP2 上，你将看到默认网站。  
  
9. 在 "**开始**" 屏幕上，键入<strong>\\ \ App2\Files</strong>，然后按 enter。 双击“新文本文档”文件。  
  
    这说明你能够使用 SMB 连接到仅 IPv4 服务器，以获取资源域中的资源。  
  
10. 在 "**开始**" 屏幕上，键入 "**services.msc**"，然后按 enter。  
  
11. 请注意，在 "**高级安全 Windows 防火墙**" 控制台中，只有**专用**或**公用配置文件**处于活动状态。 若要正常运行，必须启用 Windows 防火墙。 如果禁用了 Windows 防火墙，则 DirectAccess 连接将不起作用。  
  
12. 在控制台的左窗格中，展开 "**监视**" 节点，然后单击 "**连接安全规则**" 节点。 应会看到活动连接安全规则：**Directaccess 策略-ClientToCorp**、 **Directaccess 策略-ClientToDNS64NAT64PrefixExemption**、 **Directaccess 策略-ClientToInfra**和**directaccess 策略-ClientToNlaExempt**。 向右滚动中间窗格以显示**第一身份验证方法**和**第二身份验证方法**列。 请注意，第一个规则（ClientToCorp）使用 Kerberos V5 建立 intranet 隧道，第三个规则（ClientToInfra）使用 NTLMv2 建立基础结构隧道。  
  
13. 在控制台的左窗格中，展开 "**安全关联**" 节点，然后单击 "**主模式**" 节点。 请注意，基础结构使用 NTLMv2 和 intranet 隧道安全关联（使用 Kerberos V5）建立隧道安全关联。 右键单击显示 "**用户（Kerberos V5）** " 作为**第二身份验证方法**的条目，然后单击 "**属性**"。 在 "**常规**" 选项卡上，请注意**第二个 "身份验证本地 ID** " 为**CORP\User1**，表示 User1 能够使用 Kerberos 成功地向公司域进行身份验证。  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>通过群集测试 DirectAccess 客户端连接  
  
1. 在 EDGE2 上执行正常关闭。  
  
   运行这些测试时，可以使用 "网络负载平衡管理器" 查看服务器的状态。  
  
2. 在 CLIENT1 上的 "Windows PowerShell" 窗口中，键入**ipconfig/flushdns** ，然后按 enter。 这会刷新客户端 DNS 缓存中仍存在的名称解析条目。  
  
3. 在 Windows PowerShell 窗口中，ping APP1 和 APP2。 应该会收到来自这两个资源的答复。  
  
4. 在 "**开始**" 屏幕上，键入<strong>\\ \ app2\files</strong>。 你应看到 APP2 计算机上的共享文件夹。 在 APP2 上打开文件共享的功能表明，需要用户的 Kerberos 身份验证的第二个隧道工作正常。  
  
5. 打开 "Internet Explorer"，然后打开 https://app1/ 并 https://app2/ 的网站。 打开这两个网站的功能可确认第一个和第二个隧道都已启动并正常运行。 关闭 Internet Explorer。  
  
6. 启动 EDGE2 计算机。  
  
7. 在 EDGE1 上，执行正常关闭。  
  
8. 等待5分钟，然后返回到 CLIENT1。 执行步骤2-5。 这会确认 CLIENT1 在 EDGE1 变为不可用后能够以透明方式故障转移到 EDGE2。
