---
title: 管理 QoS 策略
description: 本主题提供有关如何创建和管理 Windows Server 2016 中的服务质量（QoS）策略的说明。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3cff51b3cf76d3224832bf99ff966bf473d6ff6c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871846"
---
# <a name="manage-qos-policy"></a>管理 QoS 策略

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来了解如何使用 QoS 策略向导来创建、编辑或删除 QoS 策略。

>[!NOTE]
>  除了本主题之外，还提供了以下 QoS 策略管理文档。
> 
>  - [QoS 策略事件和错误](qos-policy-errors.md)

在 Windows 操作系统中，QoS 策略结合了基于标准的 QoS 功能和组策略的可管理性。 通过此组合的配置，可以轻松地将 QoS 策略应用于组策略的对象。 Windows 包括一个 QoS 策略向导，可帮助您执行以下任务。

-  [创建 QoS 策略](#bkmk_createpolicy)

-  [查看、编辑或删除 QoS 策略](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>创建 QoS 策略

创建 QoS 策略之前，请务必了解用于管理网络流量的两个密钥 QoS 控制措施：

- DSCP 值

-   中止速率

### <a name="prioritizing-traffic-with-dscp"></a>通过 DSCP 确定流量优先级

如前面的业务线应用程序示例中所述，可以通过使用 "**指定 Dscp 值**" 配置具有特定 DSCP 值的 QoS 策略来定义出站网络流量的优先级。 

如 RFC 2474 中所述，DSCP 允许在 IPv4 数据包的 TOS 字段中以及 IPv6 的流量类字段中指定0到63之间的值。 网络路由器使用 DSCP 值对网络数据包进行分类并对它们进行相应的排队。
  
> [!NOTE]
>  默认情况下，Windows 流量的 DSCP 值为0。
  
队列的数量及其优先级行为需要设计为你组织的 QoS 策略的构成部分。 例如，你的组织可以选择拥有五个队列：受延迟影响的流量、控制流量、业务关键流量、尽力流量以及大容量数据传输流量。  
  
### <a name="throttling-traffic"></a>流量节流

除了 DSCP 值，限制还是管理网络带宽的另一个主要控制。 如前文所述，你可以使用 "**指定节流率**" 设置为出站流量配置具有特定限制率的 QoS 策略。 通过使用阻止功能，QoS 策略将传出的网络流量限制为指定的限制速率。 DSCP 标记和节流同时使用能够有效地管理流量。

>[!NOTE]
>默认情况下， **“指定节流率”** 复选框为未选中状态。

若要创建 QoS 策略，请从组策略管理控制台（GPMC）工具中编辑组策略对象（GPO）的设置。 然后，GPMC 打开组策略对象编辑器。

QoS 策略的名称必须是唯一的。 如何将策略应用到服务器和最终用户取决于 QoS 策略在组策略对象编辑器中的存储位置：

- 计算机配置 \Windows Settings\QoS 策略中的 QoS 策略适用于计算机，而与当前登录的用户无关。 对于服务器计算机，一般使用基于计算机的 QoS 策略。

- 用户配置单元 Settings\QoS 策略中的 QoS 策略在用户登录后适用于用户，而不管他们登录到哪台计算机。

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>使用 QoS 策略向导创建新的 QoS 策略

-   在组策略对象编辑器中，右键单击任一 " **QoS 策略**" 节点，然后单击 "**创建新策略**"。

### <a name="wizard-page-1---policy-profile"></a>向导页 1-策略配置文件

在 QoS 策略向导的第一页上，可以指定策略名称并配置 QoS 控制传出网络流量的方式。

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>若要配置基于 QoS 的策略向导的策略概述页面

1. 在 **“策略名称”** 中，键入 QoS 策略的名称。 该名称必须唯一标识策略。

2. 或者，使用 "**指定 Dscp 值**" 启用 dscp 标记，然后配置一个介于0到63之间的 dscp 值。

3. 或者，使用 **“指定节流率”** 启用流量节流功能并配置节流率。 中止速率值必须大于1，并且\(可以指定每秒\)千字节/秒或兆字节/秒\(MBps\)的单位。

4. 单击“下一步”。

### <a name="wizard-page-2---application-name"></a>向导第2页-应用程序名称

在 QoS 策略向导的第二页中，你可以将策略应用于所有应用程序、应用程序的可执行名称标识的特定应用程序、路径和应用程序名称，或者应用于处理特定 URL 请求的 HTTP 服务器应用程序。

- "**所有应用程序**" 指定 QoS 策略向导第一页上的流量管理设置应用于所有应用程序。

- **只有具有此可执行名称的应用程序**指定 QoS 策略向导第一页上的流量管理设置用于特定的应用程序。 可执行文件的名称必须以 .exe 文件名扩展名为结尾。

- **只有响应此 URL 的请求的 HTTP 服务器应用程序**指定 QoS 策略向导第一页上的流量管理设置仅应用于某些 HTTP 服务器应用程序。

或者，你可以输入应用程序路径。 若要指定一个应用程序路径，则包含带有该应用程序名称的路径。 路径可以包含环境变量。 例如，%ProgramFiles%\My Application Path\MyApp.exe 或 c:\program files\my application path\myapp.exe。

>[!NOTE]
>应用程序路径不能包含解析为符号链接的路径。

URL 必须符合[RFC 1738](https://tools.ietf.org/html/rfc1738)，格式`http[s]://<hostname\>:<port\>/<url-path>`为。 可以`‘*'`为`<port>` `<hostname>` 和/或`<port>`使用通配符，例如，通配符不能表示或的子字符串。`https://training.\*/, https://\*.\*` `<hostname>`

换句话说， `https://my\*site/`和`https://\*training\*/`均无效。 

还可以选中 "**包括子目录和文件**" 以对 URL 后面的所有子目录和文件执行匹配。 例如，如果选中此选项并且 URL 为`https://training`，则 QoS 策略会将` https://training/video`请求视为良好匹配。

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>配置 QoS 策略向导的 "应用程序名称" 页

1. 在 "**此 QoS 策略适用**于" 中，选择 "**所有应用程序**" 或 "**仅限具有此可执行名称的应用程序**

2. 如果选择了 **“仅带有该可执行名称的应用程序”** ，则需指定一个以 .exe 文件名扩展名结尾的可执行名称。

3. 单击“下一步”。

### <a name="wizard-page-3---ip-addresses"></a>向导第3页-IP 地址

在 QoS 策略向导的第三页中，可以为 QoS 策略指定 IP 地址条件，其中包括：

- 全部的源 IPv4 或 IPv6 地址或特定的源 IPv4 或 IPv6 地址

- 所有目标 IPv4 或 IPv6 地址或特定的目标 IPv4 或 IPv6 地址

如果你选择 **“仅用于以下源 IP 地址”** 或 **“仅用于以下目标 IP 地址”** ，则必须键入以下地址：

- IPv4 地址，例如`192.168.1.1`

- 使用网络前缀长度表示法的 IPv4 地址前缀，如`192.168.1.0/24`

- IPv6 地址，如`3ffe:ffff::1`

- IPv6 地址前缀，如`3ffe:ffff::/48`

如果同时**为以下源 ip 地址**和**目标 ip 地址**选择两者，则地址或地址前缀必须是基于 IPv4 或 IPv6 的。

如果在前一个向导页中指定 HTTP 服务器应用程序的 URL，则会注意到此向导页上的 QoS 策略的源 IP 地址显示为灰色。 

这是正确的，因为源 IP 地址是 HTTP 服务器地址，不能在此处进行配置。 另一方面，你仍可以通过指定目标 IP 地址来自定义该策略。 这使你可以使用相同的 HTTP 服务器应用程序为不同的客户端创建不同的策略。

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>配置 QoS 策略向导的 "IP 地址" 页

1. 在 "**此 QoS 策略适用**于" （源）中，选择 "**任意源 ip 地址** **" 或 "仅用于以下 IP 源地址"** 。

2. 如果只选择**了以下 IP 源地址**，请指定 IPv4 或 IPv6 地址或前缀。

3. 在 "**此 QoS 策略适用**于" （目标）中，选择 "**任何目标地址** **" 或 "仅用于以下 IP 目标地址"。**

4. 如果你只选择了**以下 IP 目标地址**，请指定与为源地址指定的地址或前缀的类型相对应的 IPv4 或 IPv6 地址或前缀。

5.  单击“下一步”。  

### <a name="wizard-page-4---protocols-and-ports"></a>向导页 4-协议和端口

在 QoS 策略向导的第四页上，您可以指定在向导的第一页上由设置控制的流量类型和端口。 可以指定：  
-   TCP 流量、UDP 流量或两者兼而有之  

-   全部源端口、一系列源端口或某个特定源端口

-   所有目标端口、一系列目标端口或特定的目标端口  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>配置 QoS 策略向导的 "协议和端口" 页

1. 在 **“选择适用本 QoS 策略的协议”** 界面，选择 **TCP**、**UDP** 或 **TCP 和 UDP**。

2. 在 **“指定源端口号”** 界面，选择 **“从任一源端口”** 或 **“从该源端口号”** 。

3. 如果选择了 "**从此源端口号**"，请键入介于1到65535之间的端口号。

     （可选）可以指定端口范围，格式为 "*low*：*High*"，其中 "*低*" 和 "*高*" 表示端口范围的下限和上限（含）。 *Low*和*High*都必须是介于1和65535之间的数字。 在冒号 (:) 字符及数字之间不允许有空格。

4. 在 **“指定目标端口号”** 界面，选择 **“至任一目标端口”** 或 **“至该目标端口号”** 。

5. 如果在之前的步骤中选择了 **“至该目标端口号”** ，则需要键入一个端口号，范围为 1 到 65535。

若要完成新的 QoS 策略的创建，请单击 QoS 策略向导的 "**协议和端口**" 页上的 "**完成**"。 完成后，新的 QoS 策略会在 "组策略对象编辑器的详细信息窗格中列出。  
  
若要将 QoS 策略设置应用到用户或计算机，请将 QoS 策略所在的 GPO 链接到 Active Directory 域服务容器，如域、站点或组织单位（OU）。  
  
##  <a name="bkmk_editpolicy"></a>查看、编辑或删除 QoS 策略

前面所述的 QoS 策略向导页面对应于查看或编辑策略属性时所显示的 "属性" 页。  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>若要查看 QoS 策略的属性  
  
-   在组策略对象编辑器的详细信息窗格中右键单击策略名称，然后单击 "**属性**"。  
  
     组策略对象编辑器显示具有以下选项卡的 "属性" 页：  
  
    -   策略概述  
  
    -   应用程序名  
  
    -   IP 地址  
  
    -   协议和端口  
  
### <a name="to-edit-a-qos-policy"></a>若要编辑 QoS 策略  
  
-   在组策略对象编辑器的详细信息窗格中右键单击策略名称，然后单击 "**编辑现有策略**"。  
  
     组策略对象编辑器显示 "**编辑现有的 QoS 策略**" 对话框。  
  
### <a name="to-delete-a-qos-policy"></a>若要删除 QoS 策略  
  
-   右键单击 "组策略对象编辑器的详细信息窗格中的策略名称，然后单击"**删除策略**"。  
  
### <a name="qos-policy-gpmc-reporting"></a>QoS 策略 GPMC 报告 

在整个组织中应用了大量 QoS 策略后，定期查看策略的应用方式可能会很有用或必不可少。 可以使用 GPMC 报表查看特定用户或计算机的 QoS 策略。  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>为 QoS 策略报表运行组策略结果向导  
  
-   在 GPMC 中，右键单击 "**组策略结果**" 节点，然后选择 "**组策略结果向导**" 的菜单选项。  
  
生成组策略结果后，单击 "**设置**" 选项卡。在 "**设置**" 选项卡上，可在 "计算机配置 \Windows Settings\QoS 策略" 和 "用户配置 \Windows Settings\QoS 策略" 节点下找到 QoS 策略。  
  
在 "**设置**" 选项卡上，qos 策略由其 DSCP 值、中止速率、策略条件和相同行中列出的入选 GPO 列出。  
  
组策略结果视图用于唯一地标识入选的 GPO。 如果多个 Gpo 具有具有相同 QoS 策略名称的 QoS 策略，则会应用具有最高 GPO 优先级的 GPO。 这是一个入选的 GPO。 不会应用附加到低优先级 GPO 的冲突的 QoS 策略（由策略名称标识）。 请注意，GPO 优先级定义在站点、域或 OU 中部署的 QoS 策略。 部署后，在用户或计算机级别， [QoS 策略优先规则](#BKMK_precedencerules)确定允许和阻止的流量。  
  
QoS 策略的 DSCP 值、中止速率和策略条件在组策略对象编辑器中也可见（GPOE）  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>漫游和远程用户的高级设置  
使用 QoS 策略，目标是管理企业网络上的流量。 在移动方案中，用户可能会在企业网络中发送流量。 由于 QoS 策略与企业网络不相关，因此仅在连接到适用于 Windows 8、Windows 7 或 Windows Vista 的企业的网络接口上启用 QoS 策略。  
  
例如，用户可以通过虚拟专用网络（VPN）从咖啡商店将她的便携式计算机连接到她的企业网络。 对于 VPN，物理网络接口（如无线）将不应用 QoS 策略。 但是，VPN 接口将应用 QoS 策略，因为它连接到企业。 如果用户稍后输入的其他企业网络没有 AD DS 信任关系，则将不会启用 QoS 策略。  
  
请注意，这些移动方案不适用于服务器工作负荷。 例如，具有多个网络适配器的服务器可能位于企业网络的边缘。 IT 部门可能会选择让 QoS 策略限制 egresses 企业的流量;但是，此网络适配器发送此传出流量不一定要重新连接到企业网络。 出于此原因，在运行 Windows Server 2012 的计算机的所有网络接口上始终启用了 QoS 策略。  
  
> [!NOTE]
>  选择性启用仅适用于 QoS 策略，不适用于本文档中的下一步讨论的高级 QoS 设置。  
  
### <a name="advanced-qos-settings"></a>高级 QoS 设置

高级 QoS 设置为 IT 管理员提供了用于管理计算机网络使用情况和 DSCP 标记的其他控件。 高级 QoS 设置仅适用于计算机级别，而 QoS 策略可同时应用于计算机级别和用户级别。

#### <a name="to-configure-advanced-qos-settings"></a>配置高级 QoS 设置

1.  单击 "**计算机配置**"，然后单击 "**组策略中的" Windows 设置**"。
  
2.  右键单击 " **Qos 策略**"，然后单击 "**高级 QoS 设置**"。

     下图显示了两个高级 QoS 设置选项卡：**入站 TCP 流量**和**DSCP 标记重写**。
  
> [!NOTE]
>  高级 QoS 设置是计算机级组策略设置。
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>高级 QoS 设置：入站 TCP 流量

**入站 Tcp 流量**控制接收方的 tcp 带宽消耗，而 QoS 策略会影响出站 TCP 和 UDP 流量。 

通过在**入站 Tcp 流量**选项卡上设置较低的吞吐量级别，TCP 会限制其播发的 tcp 接收窗口的大小。 此设置的影响将增加吞吐量速率，并为 TCP 连接提供更高的带宽或延迟（带宽延迟产品）的链接利用率。 默认情况下，运行 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的计算机将设置为最大吞吐量级别。
  
以前版本的 Windows 中的 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 中的 TCP 接收窗口已发生更改。 以前版本的 Windows 将 TCP 接收端窗口限制为最大为 64 kb，而 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 动态地将接收方窗口的大小限制为最大16兆字节（MB）). 在入站 TCP 流量控制中，可以通过设置 TCP 接收窗口可以增长到的最大值来控制入站吞吐量级别。 级别对应于以下最大值。 
  
|入站吞吐量级别|最多|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

实际的窗口大小可能是等于或小于最大值的值，具体取决于网络条件。

###### <a name="to-set-the-tcp-receive-side-window"></a>设置 TCP 接收端窗口

1. 在组策略对象编辑器中，单击 "**本地计算机策略**"，单击 " **Windows 设置**"，右键单击 " **qos 策略**"，然后单击 "**高级 QoS 设置**"。
  
2. 在 " **Tcp 接收吞吐量**" 中，选择 "**配置 tcp 接收吞吐量**"，然后选择所需的吞吐量级别。

3.  将 GPO 链接到 OU。

#### <a name="advanced-qos-settings-dscp-marking-override"></a>高级 QoS 设置：DSCP 标记替代

DSCP 标记替代可限制应用程序指定的功能（或 "标记"） DSCP 值（在 QoS 策略中指定的值除外）。 通过指定允许应用程序设置 DSCP 值，应用程序可以设置非零的 DSCP 值。 

通过指定**Ignore**，使用 qos api 的应用程序的 DSCP 值将设置为零，且只有 QoS 策略才能设置 DSCP 值。 

默认情况下，运行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的计算机允许应用程序指定 DSCP 值;不会重写不使用 QoS Api 的应用程序和设备。

##### <a name="wireless-multimedia-and-dscp-values"></a>无线多媒体和 DSCP 值

[Wi-fi 联盟](https://go.microsoft.com/fwlink/?LinkId=160769)已\(为无线多媒体 WMM\)建立了认证，定义了四个访问类别\(WMM_AC\) ，用于确定在 wi-fi\-上传输的网络流量的优先级Wlan 无线网络。 访问类别包括\(从最高到最低的优先级\)：语音、视频、最佳操作和背景; 分别缩写为 VO、VI、as 和 BK。 WMM 规范定义哪些 DSCP 值与四个访问类别中的每一个相对应：
  
|DSCP 值|WMM 访问类别|
|----------|-------------------|
|48-63|语音（VO）|
|32-47|视频（VI）|
|24-31、0-7|最大努力（是）|
|8-23|背景（BK）|

你可以创建使用这些 DSCP 值的 QoS 策略，以确保具有适用于 wmm\-无线适配器的 wi-fi 认证™的便携式计算机在与适用于 wmm\-接入点的 wi-fi 认证关联时接收优先级处理。
  
### <a name="BKMK_precedencerules"></a>QoS 策略优先规则

与 GPO 的优先级类似，当多个 QoS 策略应用于一组特定的流量时，QoS 策略具有优先规则来解决冲突。 对于出站 TCP 或 UDP 流量，一次只能应用一个 QoS 策略，这意味着 QoS 策略不会有累积效果，如在何处汇总限制速率。

通常，具有最匹配条件的 QoS 策略会入选。 应用多个 QoS 策略时，规则分为三个类别：用户级别与计算机级别;应用程序与网络五元组;并在网络五元组。

*网络五元组*是指源 ip 地址、目标 ip 地址、源端口、目标端口和协议\(TCP/UDP。\)  

 **用户级 QoS 策略优先于计算机级 QoS 策略**

此规则极大地简化了网络管理员对 QoS Gpo 的管理，特别是对于基于用户组的策略。 例如，如果网络管理员想要为用户组定义 QoS 策略，则他们只可以创建 GPO 并将其分发到该组。 他们无需担心这些用户登录到哪些计算机以及这些计算机是否将定义了冲突的 QoS 策略，因为如果存在冲突，则用户级策略始终优先。

> [!NOTE]
>  用户级 QoS 策略仅适用于该用户生成的流量。 特定计算机和计算机本身的其他用户将不受为该用户定义的任何 QoS 策略的制约。

 **应用程序的特征和优先于网络五元组**

当多个 QoS 策略与特定的流量匹配时，将应用更具体的策略。 在标识应用程序的策略中，包含发送应用程序的文件路径的策略被视为比仅标识应用程序名称的另一个策略（无路径）更为具体。 如果有多个应用程序的策略仍然适用，则优先规则使用网络五元组来查找最佳匹配项。

或者，通过指定非重叠条件，可以将多个 QoS 策略应用于相同的流量。 在应用程序和网络五元组的条件之间，指定应用程序的策略会被视为更为具体，并应用。 

例如，policy_A 仅指定应用程序名称（app.config），而 policy_B 指定目标 IP 地址 192.168.1.0/24。 如果这些 QoS 策略冲突\(，则 app.config 会将流量发送到 192.168.4.0/24\)范围内的 IP 地址，并应用 policy_A。

 **更多的更高优先级在网络五元组**

对于网络五元组中的策略冲突，具有最匹配条件的策略优先。 例如，假设 policy_C 指定源 IP 地址 "any"、目标 IP 地址10.0.0.1、源端口 "any"、目标端口 "any" 和协议 "TCP"。 

接下来，假设 policy_D 指定源 IP 地址 "any"、目标 IP 地址10.0.0.1、源端口 "any"、目标端口80和协议 "TCP"。 然后，policy_C 和 policy_D 都与目标10.0.0.1：80的连接匹配。 由于 QoS 策略应用具有最特定匹配条件的策略，因此在此示例中，policy_D 优先。  
  
但是，QoS 策略可能具有相同的条件数。 例如，多个策略可能每个仅指定网络五元组的一个（但不是相同）部分。 在网络五元组中，按以下顺序从高到低的顺序：

- 源 IP 地址

- 目标 IP 地址

- 源端口

- 目标端口

- 协议（TCP 或 UDP）

在特定条件下（例如 IP 地址），使用更高的优先级处理更具体的 IP 地址;例如，IP 地址192.168.4.1 比 192.168.4.0/24 更明确。

尽可能地设计你的 QoS 策略，以简化你的组织了解哪些策略是有效的。

有关本指南的下一个主题，请参阅[QoS 策略事件和错误](qos-policy-errors.md)。

有关本指南的第一个主题，请参阅[服务质量（QoS）策略](qos-policy-top.md)。
