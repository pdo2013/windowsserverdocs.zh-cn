---
title: 步骤12测试 DirectAccess 连接
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd0f8ba10536a28479269abafadaaacaffd3d0a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388380"
---
# <a name="step-12-test-directaccess-connectivity"></a>步骤12测试 DirectAccess 连接

>适用于：Windows Server（半年频道）、Windows Server 2016

当客户端计算机位于 Internet 或 Homenet 网络中时，你必须确保其具有正确的组策略设置，然后才能测试从客户端计算机的连接。  
  
- 验证客户端是否具有正确的组策略  
  
- 通过 EDGE1 测试从 Internet 进行 DirectAccess 连接  
  
- 将 CLIENT2 移到 Win7_Clients_Site2 安全组  
  
- 通过 EDGE1 测试从 Internet 进行 DirectAccess 连接  
  
## <a name="prerequisites"></a>先决条件  
将两台客户端计算机连接到公司网络，然后重新启动两台客户端计算机。  
  
## <a name="policy"></a>验证客户端是否具有正确的组策略  
  
1.  在 CLIENT1 上，单击 "**开始**"，键入 " **powershell**"，右键单击 " **Powershell**"，单击 "**高级**"，然后单击 "以**管理员身份运行**"。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  在 Windows PowerShell 窗口中，键入**ipconfig** ，然后按 enter。  
  
    请确保企业网络适配器 IPv4 地址以10.0.0 开头。  
  
3.  在 Windows PowerShell 窗口中，键入**get-dnsclientnrptpolicy** ，然后按 enter。 会显示 DirectAccess 的名称解析策略表 (NRPT) 条目。  
  
    -   . corp.contoso.com-这些设置指示与 corp.contoso.com 的所有连接都应由 DirectAccess DNS 服务器之一解析，IPv6 地址为2001： db8：1：：2或2001： db8：2：：20。  
  
    -   nls.corp.contoso.com-这些设置指示名称 nls.corp.contoso.com 有例外。  
  
4.  将 Windows PowerShell 窗口保持打开状态以进行下一过程。  
  
5.  在 CLIENT2 上，依次单击 "**开始**"、"**所有程序**"、"**附件**"、"windows **PowerShell**"，右键单击 " **windows Powershell**"，然后单击 "以**管理员身份运行**"。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
6.  在 Windows PowerShell 窗口中，键入**ipconfig** ，然后按 enter。  
  
    请确保企业网络适配器 IPv4 地址以10.0.0 开头。  
  
7.  在 Windows PowerShell 窗口中，键入**netsh namespace show policy** ，然后按 enter。  
  
    输出中应有两个部分：  
  
    -   . corp.contoso.com-这些设置指示，DirectAccess DNS 服务器应该解析与 corp.contoso.com 的所有连接，IPv6 地址为2001： db8：1：：2。  
  
    -   nls.corp.contoso.com-这些设置指示名称 nls.corp.contoso.com 有例外。  
  
8.  将 Windows PowerShell 窗口保持打开状态以进行下一过程。  
  
## <a name="EDGE1"></a>通过 EDGE1 测试从 Internet 进行 DirectAccess 连接  
  
1. 从 Internet 网络拔下 2-EDGE1。  
  
2. 将 CLIENT1 和 CLIENT2 从公司网络交换机中拔出，并将其连接到 Internet 交换机。 等待30秒。  
  
3. 在 CLIENT1 上的 "Windows PowerShell" 窗口中，键入**ipconfig/all** ，然后按 enter。  
  
4. 检查 ipconfig 命令的输出。  
  
   客户端计算机现在已连接到 Internet，并且具有公用 IPv4 地址。 如果 DirectAccess 客户端具有公用 IPv4 地址，则它将使用 Teredo 或 ip-https IPv6 转换技术通过 IPv4 Internet 在 DirectAccess 客户端与远程访问服务器之间建立 IPv6 消息的隧道。 请注意，Teredo 是首选转换技术。  
  
5. 在 Windows PowerShell 窗口中，键入**ipconfig/flushdns** ，然后按 enter。 当客户端计算机连接到公司网络时，此操作会刷新客户端 DNS 缓存中仍存在的名称解析条目。  
  
6. 禁用 Teredo 接口，以确保客户端计算机使用 IP-HTTPS 通过以下命令连接到公司网络：  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. 请确保已通过 EDGE1 连接。 键入**netsh interface httpstunnel show** interface，然后按 enter。  
  
   输出应包含 URL： https://edge1.contoso.com:443/IPHTTPS 。  
  
   > [!TIP]  
   > 在 CLIENT1 上，也可以运行以下 Windows PowerShell 命令：**NetIPHTTPSConfiguration**。 输出显示可用的服务器 URL 连接和当前活动的配置文件。  
  
8. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 应会看到分配给 APP1 的 IPv6 地址的答复，在本例中为2001： db8：1：：3。  
  
9. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 应会看到分配给 2-APP1 的 IPv6 地址的答复，在本例中为2001： db8：2：：3。  
  
10. 在 Windows PowerShell 窗口中，键入**ping app2** ，然后按 enter。 应会看到来自 EDGE1 分配给 APP2 的 NAT64 地址的答复，在本例中为 fd**c9：9f4e： eb1b**：7777：： a00：4。 请注意，由于地址的生成方式，粗体值会有所不同。  
  
    Ping APP2 的功能非常重要，因为 success 表明你能够使用 NAT64/DNS64 建立连接，因为 APP2 是仅适用于 IPv4 的资源。  
  
11. 打开 Internet Explorer，在 Internet Explorer 地址栏中，输入 **https://app1/** ，然后按 enter。 在 APP1 上，你将看到默认的 IIS 网站。  
  
12. 在 Internet Explorer 地址栏中，输入 **https://2-app1/** "，然后按 enter。 你将在 APP1 上看到默认网站。  
  
13. 在 Internet Explorer 地址栏中，输入 **https://app2/** "，然后按 enter。 在 APP2 上，你将看到默认网站。  
  
14. 在 "**开始**" 屏幕上，键入<strong>\\ \ 2-App1\Files</strong>，然后按 enter。 双击示例文本文件。  
  
    这表明在通过 EDGE1 连接时，你可以连接到 corp2.corp.contoso.com 域中的文件服务器。  
  
15. 在 "**开始**" 屏幕上，键入<strong>\\ \ App2\Files</strong>，然后按 enter。 双击“新文本文档”文件。  
  
    这说明你能够使用 SMB 连接到仅 IPv4 服务器，以获取资源域中的资源。  
  
16. 在 "**开始**" 屏幕上，键入 "**services.msc**"，然后按 enter。  
  
17. 请注意，在 "**高级安全 Windows 防火墙**" 控制台中，只有**公共配置文件**处于活动状态。 若要正常运行，必须启用 Windows 防火墙。 如果禁用了 Windows 防火墙，则 DirectAccess 连接将不起作用。  
  
18. 在控制台的左窗格中，展开 "**监视**" 节点，然后单击 "**连接安全规则**" 节点。 应会看到活动连接安全规则：**Directaccess 策略-ClientToCorp**、 **Directaccess 策略-ClientToDNS64NAT64PrefixExemption**、 **Directaccess 策略-ClientToInfra**和**directaccess 策略-ClientToNlaExempt**。 向右滚动中间窗格以显示**第一身份验证方法**和**第二身份验证方法**列。 请注意，第一个规则（ClientToCorp）使用 Kerberos V5 建立 intranet 隧道，第三个规则（ClientToInfra）使用 NTLMv2 建立基础结构隧道。  
  
19. 在控制台的左窗格中，展开 "**安全关联**" 节点，然后单击 "**主模式**" 节点。 请注意，基础结构使用 NTLMv2 和 intranet 隧道安全关联（使用 Kerberos V5）建立隧道安全关联。 右键单击显示 "**用户（Kerberos V5）** " 作为**第二身份验证方法**的条目，然后单击 "**属性**"。 在 "**常规**" 选项卡上，请注意**第二个 "身份验证本地 ID** " 为**CORP\User1**，表示 User1 能够使用 Kerberos 成功地向公司域进行身份验证。  
  
20. 在 CLIENT2 上的步骤3中重复此过程。  
  
## <a name="secgroup"></a>将 CLIENT2 移到 Win7_Clients_Site2 安全组  
  
1.  在 DC1 上，单击 "**开始**"，键入**dsa.msc**，然后按 enter。  
  
2.  在 Active Directory 用户和计算机 "控制台中，打开" **corp.contoso.com/Users** "，然后双击" **Win7_Clients_Site1**"。  
  
3.  在 " **Win7_Clients_Site1 属性**" 对话框中，单击 "**成员**" 选项卡，单击 " **CLIENT2**"，单击 "**删除**"，单击 **"是**"，然后单击 **"确定"** 。  
  
4.  双击 " **Win7_Clients_Site2**"，然后在 " **Win7_Clients_Site2 属性**" 对话框中，单击 "**成员**" 选项卡。  
  
5.  单击 "**添加**"，然后在 "**选择用户、联系人、计算机或服务帐户**" 对话框中，单击 "**对象类型**"，选择 "**计算机**"，然后单击 **"确定"** 。  
  
6.  在 "**输入要选择的对象名称**" 中，键入**CLIENT2**，然后单击 **"确定"** 。  
  
7.  重新启动 CLIENT2，并使用 corp/User1 帐户登录。  
  
8.  在 CLIENT2 上，打开提升的 Windows PowerShell 窗口，键入**netsh namespace show policy** ，然后按 enter。  
  
    输出中应有两个部分：  
  
    -   . corp.contoso.com-这些设置指示，DirectAccess DNS 服务器应该解析与 corp.contoso.com 的所有连接，IPv6 地址为2001： db8：2：：20。  
  
    -   nls.corp.contoso.com-这些设置指示名称 nls.corp.contoso.com 有例外。  
  
## <a name="DAConnect"></a>通过 EDGE1 测试从 Internet 进行 DirectAccess 连接  
  
1. 将 EDGE1 连接到 Internet 网络。  
  
2. 从 Internet 网络拔下 EDGE1。  
  
3. 在 CLIENT1 上，打开提升的 Windows PowerShell 窗口。  
  
4. 在 Windows PowerShell 窗口中，键入**ipconfig/flushdns** ，然后按 enter。 当客户端计算机连接到公司网络时，此操作会刷新客户端 DNS 缓存中仍存在的名称解析条目。  
  
5. 请确保已通过 EDGE1 进行连接。 键入**netsh interface httpstunnel show** interface，然后按 enter。  
  
   输出应包含 URL： https://2-edge1.contoso.com:443/IPHTTPS 。  
  
   > [!TIP]  
   > 在 CLIENT1 上，还可以运行以下命令：**NetIPHTTPSConfiguration**。 输出显示可用的服务器 URL 连接和当前活动的配置文件。  
  
   > [!NOTE]  
   > CLIENT1 会自动将其连接到公司资源的服务器更改。 如果该命令的输出显示与 EDGE1 的连接，请等待大约5分钟，然后重试。  
  
6. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 应会看到分配给 APP1 的 IPv6 地址的答复，在本例中为2001： db8：1：：3。  
  
7. 在 Windows PowerShell 窗口中，键入**ping app1** ，然后按 enter。 应会看到分配给 2-APP1 的 IPv6 地址的答复，在本例中为2001： db8：2：：3。  
  
8. 在 Windows PowerShell 窗口中，键入**ping app2** ，然后按 enter。 应会看到来自 EDGE1 分配给 APP2 的 NAT64 地址的答复，在本例中为 fd**c9：9f4e： eb1b**：7777：： a00：4。 请注意，由于地址的生成方式，粗体值会有所不同。  
  
   Ping APP2 的功能非常重要，因为 success 表明你能够使用 NAT64/DNS64 建立连接，因为 APP2 是仅适用于 IPv4 的资源。  
  
9. 打开 Internet Explorer，在 Internet Explorer 地址栏中，输入 **https://app1/** ，然后按 enter。 在 APP1 上，你将看到默认的 IIS 网站。  
  
10. 在 Internet Explorer 地址栏中，输入 **https://2-app1/** "，然后按 enter。 在 APP2 上，你将看到默认网站。  
  
11. 在 Internet Explorer 地址栏中，输入 **https://app2/** "，然后按 enter。 你将在 APP3 上看到默认网站。  
  
12. 在 "**开始**" 屏幕上，键入<strong>\\ \ App1\Files</strong>，然后按 enter。 双击示例文本文件。  
  
    这说明，当通过 EDGE1 连接到 corp.contoso.com 域中的文件服务器时，可以连接到该服务器。  
  
13. 在 "**开始**" 屏幕上，键入<strong>\\ \ App2\Files</strong>，然后按 enter。 双击“新文本文档”文件。  
  
    这说明你能够使用 SMB 连接到仅 IPv4 服务器，以获取资源域中的资源。  
  
14. 在步骤3中的 CLIENT2 上重复此过程。  
  


