---
title: 管理 QoS 策略
description: 本主题提供有关如何创建和管理优质的服务 (QoS) 策略，在 Windows Server 2016 中的说明进行操作。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5baa83e41c79a558f5bd32f77a234367726592c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-qos-policy"></a>管理 QoS 策略

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解有关使用 QoS 策略向导创建、编辑或删除 QoS 策略。

>[!NOTE]
>  本主题中，除了以下 QoS 策略管理文档才可用。
> 
>  - [QoS 策略事件和错误](qos-policy-errors.md)

在 Windows 操作系统、QoS 策略使用组策略进行管理组合标准基于 QoS 的功能。 组策略对象轻松 QoS 策略应用使该组合的配置。 Windows 包含一个 QoS 策略向导，从而助你实现以下任务。

-  [创建 QoS 策略](#bkmk_createpolicy)

-  [查看、编辑或删除 QoS 策略](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>创建 QoS 策略

创建 QoS 策略之前，非常重要，您将了解用于管理网络通信的两个键 QoS 控件：

- DSCP 值

-   调节率

### <a name="prioritizing-traffic-with-dscp"></a>用 DSCP 交通设置优先级

业务线应用程序上例中所述，你可以通过使用定义站网络通信的优先级**指定...**配置 QoS 策略特定 DSCP 值。 

中所述 RFC 2474，DSCP 允许从 0 到 63 要指定 IPv4 数据包 TOS 字段在和 IPv6 中的路况类字段内值。 网络路由器使用 DSCP 值进行分类网络数据包以及相应地排队它们。
  
> [!NOTE]
>  默认情况下，Windows 交通具有 DSCP 值为 0。
  
数队列和优先级它们的行为需要设计为你的组织 QoS 策略的一部分。 例如，可以选择你的组织有五个队列：延迟敏感交通、控制交通、关键业务交通、最佳的流量和批量数据传输交通。  
  
### <a name="throttling-traffic"></a>限制的流量

DSCP 值，以及阻止是用于管理网络带宽另一个主要的控件。 如上文所述，你可以使用**指定调节率**与特定调节率站交通配置 QoS 策略设置。 使用限制，QoS 策略限制指定的调节率到网络传出流量。 这两个 DSCP 参与，并限制可以一起使用，实际上管理交通。

>[!NOTE]
>默认情况下，**指定调节率**未选中复选框。

若要创建 QoS 策略，编辑的组策略对象 (GPO) 从组策略管理控制台 (GPMC) 工具中的设置。 然后，GPMC 打开组策略对象编辑器。

必须唯一 QoS 策略名称。 策略如何应用到服务器和最终用户取决于 QoS 策略组策略对象编辑器中的存储位置：

- 在计算机配置 windows Settings\QoS 策略 QoS 策略适用于无论当前登录该用户的计算机。 你通常使用服务器计算机计算机基于 QoS 策略。

- 他们有登录后，无论哪台计算机他们已登录到，在用户配置 windows Settings\QoS 策略 QoS 策略适用于用户。

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>若要使用 QoS 策略向导中创建新的 QoS 策略

-   在组策略对象编辑器，右键单击任一**QoS 策略**节点，然后依次单击**创建新的策略**。

### <a name="wizard-page-1---policy-profile"></a>第 1-策略配置文件的向导页

QoS 策略向导中的第一页，你可以指定策略名称和配置 QoS 如何控制传出网络通信。

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>配置 QoS 基于策略向导的策略个人资料页面

1. 在**策略名称**，QoS 策略为键入一个名称。 该名称必须唯一标识该策略。

2. （可选）使用**指定...**以启用 DSCP 标记，并配置之间 0 和 63 DSCP 值。

3. （可选）使用**指定调节率**启用流量限制，并配置调节率。 限制率值必须大于 1，并且你可以指定千字节 / 第二个 \(KBps\) 或第二个 \(MBps\) 每兆字节为单位。

4. 单击**下一步**。

### <a name="wizard-page-2---application-name"></a>向导页面 2-应用程序的名称

在 QoS 策略向导第二个页面，可以应用策略于到特定应用的所有应用具体由其可执行文件的名称，为路径和应用程序的名称，或为特定 URL 处理请求 HTTP 服务器应用。

- **所有应用程序**指定 QoS 策略向导中的第一页上的交通管理设置适用于所有应用。

- **仅具有此可执行文件名称的应用程序**指定 QoS 策略向导中的第一页上的交通管理设置为特定应用。 可执行文件名称和.exe 文件扩展名必须结束。

- **只有 HTTP 服务器的应用程序对请求的响应，对此 URL**指定 QoS 策略向导中的第一页上的交通管理设置应用到仅某些 HTTP 服务器应用。

（可选），你可以输入应用程序路径。 若要指定的应用程序路径，包括应用名称的路径。 路径可能包括环境变量。 例如，应用程序 Path\MyApp.exe %ProgramFiles%\My 或者 c:\program files\my application path\myapp.exe。

>[!NOTE]
>应用程序路径无法包括路径对应于符号的链接。

必须符合 URL [RFC 1738](http://tools.ietf.org/html/rfc1738)、的形式`http[s]://<hostname\>:<port\>/<url-path>`。 你可以使用通配符，`‘*’`，对于`<hostname>`和/或`<port>`，例如`http://training.\*/, https://\*.\*`，但通配符无法表示的子`<hostname>`或`<port>`。

换言之，这两`http://my\*site/`也`https://\*training\*/`有效。 

（可选）你还可以查看**包括子目录和文件**执行匹配的所有子目录和按照 URL 的文件。 例如，如果选中此选项，并且 URL `http://training`，QoS 策略会考虑请求` http://training/video`非常匹配。

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>若要配置 QoS 策略向导中的应用程序的名称页

1. 在**QoS 此策略适用于**，选择**所有应用程序**或**仅应用程序与此可执行文件名称**。

2. 如果你选择**仅应用程序与此可执行文件名称**，指定结束.exe 文件扩展名可执行文件名称。

3. 单击**下一步**。

### <a name="wizard-page-3---ip-addresses"></a>向导页面 3 平板电脑的 IP 地址

QoS 策略向导中的第三个页中，您可以指定 IP 地址 QoS 策略，其中包括以下条件：

- 所有源 IPv4 或 IPv6 地址或特定的源 IPv4 或 IPv6 地址

- 所有目标 IPv4 或 IPv6 地址或特定目标 IPv4 或 IPv6 地址

如果你选择**仅适用于以下的源 IP 地址**或**仅限以下目标 IP 地址**，你必须键入下列情况之一：

- IPv4 地址如 `192.168.1.1`

- 使用网络前缀长度符号，如 IPv4 地址前缀 `192.168.1.0/24`

- 如 IPv6 地址 `3ffe:ffff::1`

- IPv6 地址前缀如 `3ffe:ffff::/48`

如果你选择这两个**仅适用于以下的源 IP 地址**和**仅限以下目标 IP 地址**、地址或地址前缀必须任一 IPv4 IPv6 基于或的。

如果你指定 HTTP 服务器应用程序的 URL 上一页向导中，你将注意到在此页上向导 QoS 策略源 IP 地址将显示为灰色。 

因为源 IP 地址 HTTP 服务器地址，下面不可配置也是如此。 相反，你仍可以通过指定目的地 IP 地址自策略。 这使你使用的相同 HTTP 服务器应用针对不同的客户端创建不同的策略。

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>若要配置 QoS 策略向导的 IP 地址页

1. 在**QoS 此策略适用于**（源）中，选择**任何源 IP 地址**或**仅为以下 IP 源地址**。

2. 如果你选择**仅以下源 IP 地址**、指定 IPv4 或 IPv6 地址或前缀。

3. 在**QoS 此策略适用于**（目标），选择**任何目的地的地址**或**仅限以下 IP 目的地的地址。**

4. 如果你选择**仅限以下 IP 目的地的地址**，指定 IPv4 或 IPv6 地址的类型对应的前缀或前缀源地址指定。

5.  单击**下一步**。  

### <a name="wizard-page-4---protocols-and-ports"></a>向导页面 4-协议和端口

在 QoS 策略向导第四个页面上，你可以指定类型的路况和受向导中的第一页上的设置的端口。 你可以指定：  
-   TCP 通信、UDP 通信或两者都  

-   所有源端口、的各种来源端口或特定的源端口

-   所有目标端口、一系列目标端口或特定的目标端口  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>若要配置 QoS 策略向导协议和端口页面

1. 在**选择该 QoS 策略适用于协议**、选择**TCP**，**UDP**，或**TCP 和 UDP**。

2. 在**指定源端口号**、选择**从任意源代码端口**或**从此源端口号**。

3. 如果你选择**从此源端口号**，键入之间 1 和 65535 端口号。

     或者，你可以指定端口范围，将以格式"*低*:*高*，"在*低*和*高*含表示限和端口范围的上限。 *低*和*高*每必须是 1 到 65535 之间的数字。 无空格允许分号（:）字符之间的数字。

4. 在**指定目的地端口号**、选择**到任意目标端口**或**到此目标端口号**。

5. 如果你选择**到此目标端口号**在上一步中，键入之间 1 和 65535 端口号。

若要完成创建的新 QoS 策略，请单击**完成**上**协议和端口**QoS 策略向导中的页面。 完成后，新 QoS 策略列出组策略对象编辑器的详细信息窗格中。  
  
若要将 QoS 策略设置应用到用户或计算机，链接组策略对象，QoS 策略位于哪个到 Active Directory 域服务容器，例如域、某个站点，或者部门 (OU)。  
  
##  <a name="bkmk_editpolicy"></a>查看、编辑或删除 QoS 策略

查看或编辑策略的属性时显示的属性页之前对应 QoS 策略向导所述的页面。  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>若要查看 QoS 策略属性  
  
-   右键单击组策略对象编辑器的详细信息窗格中的策略名称，然后单击**属性**。  
  
     组策略对象编辑器显示属性页具有以下选项卡：  
  
    -   策略配置文件  
  
    -   应用名  
  
    -   IP 地址  
  
    -   协议和端口  
  
### <a name="to-edit-a-qos-policy"></a>若要编辑 QoS 策略  
  
-   右键单击组策略对象编辑器的详细信息窗格中的策略名称，然后单击**编辑现有策略**。  
  
     组策略对象编辑器显示**编辑现有 QoS 策略**对话框。  
  
### <a name="to-delete-a-qos-policy"></a>若要删除 QoS 策略  
  
-   右键单击组策略对象编辑器的详细信息窗格中的策略名称，然后单击**删除策略**。  
  
### <a name="qos-policy-gpmc-reporting"></a>QoS 策略 GPMC 报告 

QoS 策略了大量应用在你的组织后，它可能会有用或必要定期查看策略如何应用。 可以使用报告 GPMC 查看 QoS 特定用户或计算机策略的摘要。  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>若要运行的 QoS 策略报告组策略结果向导  
  
-   在 GPMC，右键单击**组策略结果**节点，然后依次选择的选项菜单**组策略结果向导。**  
  
组策略结果生成后，单击**设置**选项卡。在**设置**选项卡上，可以下的"计算机配置 windows Settings\QoS 策略"和"用户配置 windows Settings\QoS 策略"节点找到 QoS 策略。  
  
在**设置**选项卡上，按 QoS 策略名称其 DSCP 值、调节率、策略条件列出 QoS 策略和赢得 GPO 列出在同一行上。  
  
组策略结果视图唯一标识获胜 GPO。 当多个 Gpo 已 QoS QoS 策略同名的策略时，，GPO GPO 最高优先级的应用。 这是获胜 GPO。 冲突 QoS 策略（由策略名称标识）连接到较低的优先级 GPO 不会应用。 注意 GPO 优先级定义相应地部署在网站、域或 OU，哪些 QoS 策略。 部署，在用户或计算机级别后[QoS 策略优先规则](#BKMK_precedencerules)确定允许哪种通信，并阻止。  
  
QoS 策略 DSCP 值、调节率和策略情况也是可见在组策略对象编辑器 (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>漫游和远程用户的高级的设置  
QoS 策略目标是管理企业网络上的交通。 在移动版的情况下，打开或关闭的企业网络，可能会发送用户通信。 因为 QoS 策略无关离开的企业网络时，只能在连接到的企业为 Windows 8、Windows 7 或 Windows Vista 的网络接口上启用 QoS 策略。  
  
例如，用户可能会她笔记本电脑从连接到虚拟专用网络 (VPN) 通过她企业网络咖啡店。 VPN 物理的网络接口（如无线）不会有 QoS 策略应用。 但是，VPN 接口没有 QoS 策略应用，因为它连接到企业版。 如果用户稍后输入没有广告 DS 信任关系的另一个企业网络，将不会启用 QoS 策略。  
  
请注意，这些移动方案不适用于服务器工作负载。 例如，具有多个网络适配器的服务器可能休息的企业网络边。 选择 IT 部门可能有 QoS 策略调节交通 egresses 企业;但是，此发送此出口通信的网络适配器无法一定连接回企业网络。 出于此原因，QoS 策略始终在运行 Windows Server 2012 的计算机的所有网络接口启用。  
  
> [!NOTE]
>  支持选择性仅适用于 QoS 策略而不适用于下面将要讨论本文档中的高级 QoS 设置。  
  
### <a name="advanced-qos-settings"></a>高级的 QoS 设置

高级的 QoS 设置提供适用于 IT 管理员管理计算机网络消耗和 DSCP 标记其他控件。 而可在计算机和用户级别应用 QoS 策略，高级的 QoS 设置仅在计算机级别，应用。

#### <a name="to-configure-advanced-qos-settings"></a>若要配置 QoS 的高级的设置

1.  单击**计算机配置**，然后单击**组策略中的 Windows 设置**。
  
2.  右键单击**QoS 策略**，然后单击**高级 QoS 设置**。

     下图显示了这两个高级 QoS 设置的选项卡：**归巢的 TCP 交通**和**DSCP 参与覆盖**。
  
> [!NOTE]
>  高级的 QoS 设置是计算机级别组策略设置。
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>高级 QoS 设置：归巢的 TCP 通信

**归巢的 TCP 交通**控制接收器侧 TCP 带宽消耗而 QoS 策略影响站 TCP 和 UDP 通信。 

通过设置吞吐量更低级别上**归巢的 TCP 交通**选项卡上，将限制 TCP 公布 TCP 接收窗口的大小。 此设置的影响将吞吐量更高，并将具有高带宽或延迟（带宽延迟产品）的 TCP 连接的利用率链接。 默认情况下，计算机运行的 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 设置为最大的吞吐量级别。
  
TCP 接收从以前版本的 Windows Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 中的改动窗口。 以前版本的 Windows 有限 TCP 端接收的窗口，最多 64 千字节 (KB)，而 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 动态调整接收一侧的窗口的大小达 16 兆字节 (MB)。 控件中的入站 TCP 通信，您可以通过设置会变得 TCP 收到窗口的最大值控制的入站的吞吐量级别。 级别对应以下最大的值。 
  
|归巢的吞吐量级别|最多|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

实际窗口的大小可能等于或小于的最大，具体取决于网络条件的值。

###### <a name="to-set-the-tcp-receive-side-window"></a>若要设置 TCP 收到一侧的窗口

1. 在组策略对象编辑器中，单击**本地计算机策略**，单击**Windows 设置**，右键单击**QoS 策略**，然后单击**高级 QoS 设置**。
  
2. 在**TCP 接收吞吐量**、选择**配置 TCP 接收吞吐量**，然后选择所需的吞吐量程度。

3.  GPO 到 OU 链接。

#### <a name="advanced-qos-settings-dscp-marking-override"></a>高级 QoS 设置：DSCP 参与覆盖

标记覆盖 DSCP 限制的应用程序，若要指定的功能，或"标记"— DSCP 值其他比 QoS 策略中指定。 通过指定应用程序都可以设置 DSCP 值，应用程序可以设置非零值 DSCP 值。 

通过指定**忽略**、应用程序使用 QoS Api 将设置为零，其 DSCP 值，并且仅 QoS 策略可以设置 DSCP 值。 

默认情况下，计算机运行的 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 允许的应用程序指定 DSCP 值;应用程序和设备，不要使用 QoS Api 未重写。

##### <a name="wireless-multimedia-and-dscp-values"></a>无线多媒体和 DSCP 值

[Wi-fi 联盟](https://go.microsoft.com/fwlink/?LinkId=160769)已为无线多媒体建立认证 \(WMM\) 定义访问的四个类别 Wi\ Fi 无线网络上的排列优先顺序网络通信的 \(WMM_AC\) 传输。 访问类别包括 \（在最高为最低 priority\ 的顺序）：语音、视频、最佳努力和背景;分别缩写为 VO、六部分、BE 和黑。 WMM 规范定义哪些 DSCP 值与每个访问四个类别：
  
|DSCP 值|WMM 访问类别|
|----------|-------------------|
|48-63|(VO) 语音|
|32-47|视频六（部分）|
|24-31, 0-7|最大努力 (BE)|
|8-23|后台（黑）|

你可以创建使用这些 DSCP 值，以确保对 WMM 无线适配器 Wi\-Fi Certified™，则笔记本电脑接收优先的处理时 WMM 接入点的与 Wi\ Fi 认证关联的 QoS 策略。
  
### <a name="BKMK_precedencerules"></a>QoS 策略优先规则

类似于 GPO 的优先级，QoS 策略具有优先规则解决冲突当多个 QoS 策略适用于一组特定的交通。 对于站 TCP 或 UDP 通信，可以一次，这意味着 QoS 策略没有累积效果，如在会要求和限制率应用只有一个 QoS 策略。

一般情况下，将 wins QoS 最匹配的条件的策略。 当多个 QoS 策略应用时，规则分为三个类别：用户级别而不是计算机级;与网络五次; 的应用程序和网络中 quintuple。

通过*网络 quintuple*，我们意味着源 IP 地址、目的地的 IP 地址、源端口、目的地端口，并协议 \(TCP/UDP\)。  

 **用户级别 QoS 策略优先计算机级别 QoS 策略**

此规则大大简化了 QoS Gpo 网络管理员的管理，特别是对于用户 – 基于组策略。 例如的网络管理员，要定义 QoS 用户组策略，如果他们只需可以创建并分配给该组 GPO。 他们无需担心有关哪些计算机这些用户登录到和是否这些计算机将有冲突 QoS 策略定义的因为存在冲突，如果用户级别策略始终优先。

> [!NOTE]
>  用户级别 QoS 策略仅适用于由该用户的通信。 特定的计算机，和本身，该计算机其他用户不会遵守该用户定义的任何 QoS 策略。

 **应用程序针对性和拍摄其优先网络五次**

当多个 QoS 策略匹配特定通信时，应用的更多特定策略。 之间识别的应用程序的策略，包括发送的应用程序的文件路径策略被认为是更多具体比另一个仅识别的应用名称（无路径）的策略。 如果使用的应用程序的多个策略仍然适用，优先级规则使用 quintuple 网络查找最佳匹配。

或者，多个 QoS 策略可能适用于相同交通通过指定非重叠条件。 之间的应用程序状况网络五次，指定的应用程序的策略被认为是更多具体和应用。 

例如，policy_A 仅指定的应用名称 (app.exe)，并 policy_B 指定目的地 IP 地址 192.168.1.0 月 24 小时。 当这些 QoS 策略冲突 \（app.exe 发送交通到 192.168.4.0/24\ 的范围内的 IP 地址），policy_A 获取应用。

 **更多特定优先网络内 quintuple**

在网络五次策略冲突，最匹配的条件的策略优先。 例如，假设 policy_C 指定来源 IP 地址"任何"，请目标 IP 地址 10.0.0.1，源端口"任意，"目标端口"任何"，并协议"TCP"。 

接下来，假设 policy_D 指定来源 IP 地址"任何"，请目标 IP 地址 10.0.0.1，源端口"任何，"目标端口 80，以及协议"TCP"。 然后 policy_C 和 policy_D 匹配目标 10.0.0.1:80 的连接。 QoS 策略应用最特定匹配的条件的策略，因为 policy_D 优先在此示例中。  
  
但是，QoS 策略可能具有相同的数量的条件。 例如，几个策略可能每指定只有一（但不是相同）条网络五次。 在网络五次，之间以下顺序是从降低优先调高：

- 源 IP 地址

- 目标 IP 地址

- 源端口

- 目标端口

- 协议（TCP 或 UDP）

在某个特定的条件，如 IP 地址的更多具体 IP 地址重视具有较高优先级;例如，IP 地址 192.168.4.1 是更多具体 192.168.4.0 月 24 比。

设计 QoS 策略尽可能具体简化你的组织对的策略是实际上的理解能力。

本指南中的下一步主题，请参阅[QoS 策略事件和错误](qos-policy-errors.md)。

本指南中的第一个主题，请参阅[质量服务 (QoS) 策略](qos-policy-top.md)。
