---
title: 步骤 12 测试 DirectAccess 连接
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9c87f1823140fd6c92cf7df1f9d807545b50504e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281535"
---
# <a name="step-12-test-directaccess-connectivity"></a>步骤 12 测试 DirectAccess 连接

>适用于：Windows 服务器 （半年频道），Windows Server 2016

当它们位于 Internet 或 Homenet 网络上时，可以测试从客户端计算机的连接之前，必须确保它们具有正确的组策略设置。  
  
- 若要验证客户端具有正确的组策略  
  
- 测试通过 EDGE1 internet 的 DirectAccess 连接  
  
- 将客户端 2 移到 Win7_Clients_Site2 安全组  
  
- 测试 2 EDGE1 通过 internet 的 DirectAccess 连接  
  
## <a name="prerequisites"></a>系统必备  
这两个客户端计算机连接到公司网络的网络，并重新启动这两个客户端计算机。  
  
## <a name="policy"></a>验证客户端具有正确的组策略  
  
1.  在 CLIENT1 上，单击**启动**，类型**powershell.exe**，右键单击**powershell**，单击**高级**，然后单击**以管理员身份运行**。 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。  
  
2.  在 Windows PowerShell 窗口中，键入**ipconfig**然后按 ENTER。  
  
    请确保公司网络适配器 IPv4 地址开头 10.0.0。  
  
3.  在 Windows PowerShell 窗口中键入**Get-dnsclientnrptpolicy**然后按 ENTER。 会显示 DirectAccess 的名称解析策略表 (NRPT) 条目。  
  
    -   。 corp.contoso.com-这些设置指明应通过 DirectAccess 的 DNS 服务器，使用 IPv6 地址并使用 2001:db8:1::2 或 2001:db8:2::20 之一来解决所有到 corp.contoso.com 的连接。  
  
    -   nls.corp.contoso.com 这些设置指示名称 nls.corp.contoso.com 的例外。  
  
4.  保持 Windows PowerShell 窗口打开状态下一步的过程。  
  
5.  在 CLIENT2 上，单击**启动**，单击**所有程序**，单击**附件**，单击**Windows PowerShell**，右键单击**Windows PowerShell**，然后单击**以管理员身份运行**。 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。  
  
6.  在 Windows PowerShell 窗口中，键入**ipconfig**然后按 ENTER。  
  
    请确保公司网络适配器 IPv4 地址开头 10.0.0。  
  
7.  在 Windows PowerShell 窗口中，键入**netsh 命名空间显示策略**然后按 ENTER。  
  
    在输出中，应为两个部分：  
  
    -   。 corp.contoso.com-这些设置指明具有 IPv6 地址并使用 2001:db8:1::2 的 DirectAccess DNS 服务器应解析为 corp.contoso.com 的所有连接。  
  
    -   nls.corp.contoso.com 这些设置指示名称 nls.corp.contoso.com 的例外。  
  
8.  保持 Windows PowerShell 窗口打开状态下一步的过程。  
  
## <a name="EDGE1"></a>测试通过 EDGE1 internet 的 DirectAccess 连接  
  
1. 2-EDGE1 从 Internet 网络中拔出。  
  
2. 从公司网络交换机中拔出 CLIENT1 和 CLIENT2 并将它们连接到 Internet 交换机。 等待 30 秒。  
  
3. 在 CLIENT1 上，在 Windows PowerShell 窗口中，键入**ipconfig /all**然后按 ENTER。  
  
4. 检查 ipconfig 命令的输出。  
  
   客户端计算机现在已连接到 Internet，并具有一个公共 IPv4 地址。 当 DirectAccess 客户端的公共 IPv4 地址时，它使用 Teredo 或 IP-HTTPS IPv6 转换技术之间的 DirectAccess 客户端和远程访问服务器 IPv4 Internet 上的隧道 IPv6 消息。 请注意，Teredo 首选的转换技术。  
  
5. 在 Windows PowerShell 窗口中，键入**ipconfig /flushdns**然后按 ENTER。 这将刷新名称解析条目可能仍存在从客户端 DNS 缓存中的客户端计算机连接到公司网络时。  
  
6. 禁用 Teredo 接口，以确保客户端计算机使用的 IP-HTTPS 连接到公司网络，使用以下命令：  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. 确保已连接通过 EDGE1。 类型**netsh 接口 httpstunnel 显示接口**然后按 ENTER。  
  
   输出应包含 URL: https://edge1.contoso.com:443/IPHTTPS 。  
  
   > [!TIP]  
   > 在 CLIENT1 上，还可以运行以下 Windows PowerShell 命令：**Get NetIPHTTPSConfiguration**。 该输出显示可用的服务器 URL 连接和当前处于活动状态的配置文件。  
  
8. 在 Windows PowerShell 窗口中，键入**ping app1**然后按 ENTER。 你应看到分配给 APP1，在这种情况下，这是 2001:db8:1::3 的回复的 IPv6 地址。  
  
9. 在 Windows PowerShell 窗口中，键入**ping 2 app1**然后按 ENTER。 你应看到来自分配给 2-APP1，在这种情况下，这是 2001:db8:2::3 的 IPv6 地址。  
  
10. 在 Windows PowerShell 窗口中，键入**ping app2**然后按 ENTER。 你应看到的来自 NAT64 地址由 EDGE1 分配给 APP2，在这种情况下是 fd**c9:9f4e:eb1b**: 7777::a00:4。 请注意，粗体显示的值将取决于如何生成地址。  
  
    Ping APP2 的功能很重要，因为你能够 APP2 原样 IPv4 唯一资源建立连接使用 nat64/dns64，成功指示。  
  
11. 打开 Internet Explorer，Internet Explorer 地址栏中，输入 **https://app1/** 然后按 ENTER。 在 APP1 上，你将看到默认的 IIS 网站。  
  
12. 在 Internet Explorer 地址栏中，输入 **https://2-app1/** 然后按 ENTER。 2-APP1 上，将看到默认网站。  
  
13. 在 Internet Explorer 地址栏中，输入 **https://app2/** 然后按 ENTER。 在 APP2 上，你将看到默认网站。  
  
14. 上**启动**屏幕上，键入<strong>\\\2-App1\Files</strong>，然后按 ENTER。 双击示例文本文件。  
  
    此示例演示你能够连接到通过 EDGE1 连接时 corp2.corp.contoso.com 域中的文件服务器。  
  
15. 上**启动**屏幕上，键入<strong>\\\App2\Files</strong>，然后按 ENTER。 双击“新文本文档”文件。  
  
    此示例演示你能够连接到 IPv4 唯一的服务器使用 SMB 来获取资源域中的资源。  
  
16. 上**启动**屏幕上，键入**wf.msc**，然后按 ENTER。  
  
17. 在中**高级安全 Windows 防火墙**控制台中，注意到只有**公共配置文件**处于活动状态。 为 DirectAccess 才能正常工作，必须启用 Windows 防火墙。 如果 Windows 防火墙处于禁用状态，DirectAccess 连接不会不起作用。  
  
18. 在控制台的左窗格中，展开**监视**节点，然后单击**连接安全规则**节点。 应会看到活动连接安全规则：**DirectAccess 策略 ClientToCorp**， **DirectAccess 策略 ClientToDNS64NAT64PrefixExemption**， **DirectAccess 策略 ClientToInfra**，并**DirectAccess策略 ClientToNlaExempt**。 向下滚动至右侧，显示的中间窗格**第一身份验证方法**并**第二个身份验证方法**列。 请注意，第一条规则 (ClientToCorp) 使用 Kerberos V5 建立 intranet 隧道和第三条规则 (ClientToInfra) 使用 NTLMv2 建立基础结构隧道。  
  
19. 在控制台的左窗格中，展开**安全关联**节点，然后单击**主模式**节点。 请注意，使用 NTLMv2 的基础结构隧道安全关联和 intranet 隧道安全关联使用 Kerberos V5。 右键单击显示的条目**用户 (Kerberos V5)** 作为**第二个身份验证方法**然后单击**属性**。 上**常规**选项卡上，请注意**第二个身份验证本地 ID**是**CORP\User1**，指示 User1 已能够成功进行身份验证到 CORP 域使用Kerberos。  
  
20. 重复此过程对 CLIENT2 的步骤 3 中。  
  
## <a name="secgroup"></a>将客户端 2 移到 Win7_Clients_Site2 安全组  
  
1.  在 DC1 上，单击**启动**，类型**dsa.msc**，然后按 ENTER。  
  
2.  在 Active Directory 用户和计算机控制台中，打开**corp.contoso.com/Users** ，然后双击**Win7_Clients_Site1**。  
  
3.  上**Win7_Clients_Site1 属性**对话框中，单击**成员**选项卡上，单击**CLIENT2**，单击**删除**，单击**是**，然后单击**确定**。  
  
4.  双击**Win7_Clients_Site2**，然后在**Win7_Clients_Site2 属性**对话框中，单击**成员**选项卡。  
  
5.  单击**外**，然后在**选择用户、 联系人、 计算机或服务帐户**对话框中，单击**对象类型**，选择**计算机**，然后依次**确定**。  
  
6.  在中**输入要选择的对象名称**，类型**CLIENT2**，然后单击**确定**。  
  
7.  重新启动客户端 2 并使用 corp/User1 帐户登录。  
  
8.  在 CLIENT2 上，打开提升的 Windows PowerShell 窗口，键入**netsh 命名空间显示策略**然后按 ENTER。  
  
    在输出中，应为两个部分：  
  
    -   。 corp.contoso.com-这些设置指明具有 IPv6 地址 2001:db8:2::20 的 DirectAccess DNS 服务器应解析为 corp.contoso.com 的所有连接。  
  
    -   nls.corp.contoso.com 这些设置指示名称 nls.corp.contoso.com 的例外。  
  
## <a name="DAConnect"></a>测试 2 EDGE1 通过 internet 的 DirectAccess 连接  
  
1. 连接到 Internet 网络 2 EDGE1。  
  
2. 从 Internet 网络中拔出 EDGE1。  
  
3. 在 CLIENT1 上，打开提升的 Windows PowerShell 窗口。  
  
4. 在 Windows PowerShell 窗口中，键入**ipconfig /flushdns**然后按 ENTER。 这将刷新名称解析条目可能仍存在从客户端 DNS 缓存中的客户端计算机连接到公司网络时。  
  
5. 确保已连接通过 2 EDGE1。 类型**netsh 接口 httpstunnel 显示接口**然后按 ENTER。  
  
   输出应包含 URL: https://2-edge1.contoso.com:443/IPHTTPS 。  
  
   > [!TIP]  
   > 在 CLIENT1 上，还可以运行以下命令：**Get NetIPHTTPSConfiguration**。 该输出显示可用的服务器 URL 连接和当前处于活动状态的配置文件。  
  
   > [!NOTE]  
   > 客户端 1 自动更改通过该它连接到公司资源的服务器。 如果命令的输出显示到 EDGE1 的连接，等待大约五分钟并再试一次。  
  
6. 在 Windows PowerShell 窗口中，键入**ping app1**然后按 ENTER。 你应看到分配给 APP1，在这种情况下，这是 2001:db8:1::3 的回复的 IPv6 地址。  
  
7. 在 Windows PowerShell 窗口中，键入**ping 2 app1**然后按 ENTER。 你应看到来自分配给 2-APP1，在这种情况下，这是 2001:db8:2::3 的 IPv6 地址。  
  
8. 在 Windows PowerShell 窗口中，键入**ping app2**然后按 ENTER。 你应看到的来自 NAT64 地址由 EDGE1 分配给 APP2，在这种情况下是 fd**c9:9f4e:eb1b**: 7777::a00:4。 请注意，粗体显示的值将取决于如何生成地址。  
  
   Ping APP2 的功能很重要，因为你能够 APP2 原样 IPv4 唯一资源建立连接使用 nat64/dns64，成功指示。  
  
9. 打开 Internet Explorer，Internet Explorer 地址栏中，输入 **https://app1/** 然后按 ENTER。 在 APP1 上，你将看到默认的 IIS 网站。  
  
10. 在 Internet Explorer 地址栏中，输入 **https://2-app1/** 然后按 ENTER。 在 APP2 上，你将看到默认网站。  
  
11. 在 Internet Explorer 地址栏中，输入 **https://app2/** 然后按 ENTER。 APP3 上，将看到默认网站。  
  
12. 上**启动**屏幕上，键入<strong>\\\App1\Files</strong>，然后按 ENTER。 双击示例文本文件。  
  
    此示例演示你能够连接到 2 EDGE1 通过连接时在 corp.contoso.com 域中的文件服务器。  
  
13. 上**启动**屏幕上，键入<strong>\\\App2\Files</strong>，然后按 ENTER。 双击“新文本文档”文件。  
  
    此示例演示你能够连接到 IPv4 唯一的服务器使用 SMB 来获取资源域中的资源。  
  
14. 重复步骤 3 中的对 CLIENT2 此过程。  
  


