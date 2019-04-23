---
title: 管理 QoS 策略
description: 本主题将说明了如何创建和管理 Windows Server 2016 中的服务质量 (QoS) 策略。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94e5a1832a6c1e160b9cc338d50636026a5eb751
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851668"
---
# <a name="manage-qos-policy"></a>管理 QoS 策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何使用 QoS 策略向导来创建、 编辑或删除 QoS 策略。

>[!NOTE]
>  除了本主题提供了以下的 QoS 策略管理文档。
> 
>  - [QoS 策略事件和错误](qos-policy-errors.md)

在 Windows 操作系统中，QoS 策略将基于标准的 QoS 功能与组策略的可管理性相结合。 此组合配置 QoS 策略的简单应用程序向组策略对象。 Windows 包括一个可帮助你执行以下任务的 QoS 策略向导。

-  [创建 QoS 策略](#bkmk_createpolicy)

-  [查看、 编辑或删除 QoS 策略](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>创建 QoS 策略

创建 QoS 策略之前，请务必了解用于管理网络流量的两个密钥 QoS 控件：

- DSCP 值

-   速率限制

### <a name="prioritizing-traffic-with-dscp"></a>使用 DSCP 的流量优先级

如以前的业务线应用程序示例中所述，您可以通过使用定义出站网络流量的优先级**指定 DSCP 值**具有特定 DSCP 值配置 QoS 策略。 

如 RFC 2474 中所述，DSCP 允许在值从 0 到 63 在 IPv4 数据包 TOS 字段和 IPv6 中流量类字段内指定。 网络路由器使用 DSCP 值分类网络数据包并相应地对它们进行排队。
  
> [!NOTE]
>  默认情况下，Windows 流量具有 DSCP 值为 0。
  
队列的数量及其优先级行为需要设计为你组织的 QoS 策略的构成部分。 例如，可以选择你的组织具有五个队列： 延迟敏感流量、 控制流量、 业务关键流量、 最大努力流量和大容量数据传输流量。  
  
### <a name="throttling-traffic"></a>流量节流

DSCP 值，以及限制是另一个控件用于管理网络带宽。 如前文所述，你可以使用**指定中止值等级**设置以使用出站流量的特定节流率配置 QoS 策略。 通过使用限制，QoS 策略限制到指定的限制速率的传出网络流量。 DSCP 标记和节流同时使用能够有效地管理流量。

>[!NOTE]
>默认情况下，**“指定节流率”** 复选框为未选中状态。

若要创建 QoS 策略，请编辑的组策略对象 (GPO) 从组策略管理控制台 (GPMC) 工具中的设置。 然后，GPMC 打开组策略对象编辑器。

QoS 策略的名称必须是唯一的。 如何将策略应用于服务器和最终用户取决于 QoS 策略存储在组策略对象编辑器中的位置：

- 在计算机配置 Settings\QoS 策略的 QoS 策略应用到计算机，而不考虑当前登录的用户。 对于服务器计算机，一般使用基于计算机的 QoS 策略。

- 它们有记录，哪台计算机无论他们登录到后，在用户配置 Settings\QoS 策略的 QoS 策略适用于用户。

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>若要使用 QoS 策略向导创建新的 QoS 策略

-   在组策略对象编辑器中，右键单击任一**QoS 策略**节点，，然后单击**创建新的策略**。

### <a name="wizard-page-1---policy-profile"></a>向导第 1 页-策略配置文件

QoS 策略向导的第一页，您可以指定策略名称和配置 QoS 控制传出网络流量的方式。

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>若要配置基于 QoS 的策略向导的策略概述页面

1. 在 **“策略名称”** 中，键入 QoS 策略的名称。 名称必须唯一标识该策略。

2. （可选） 使用**指定 DSCP 值**来启用 DSCP 标记，然后配置 0 和 63 之间的 DSCP 值。

3. 或者，使用 **“指定节流率”** 启用流量节流功能并配置节流率。 中止等级的值必须是大于 1，并可以指定以每秒千字节为单位\(KBps\)或每秒兆字节\(MBps\)。

4. 单击“下一步” 。

### <a name="wizard-page-2---application-name"></a>向导页 2-应用程序名称

在 QoS 策略向导的第二页中，可以将策略应用到所有应用程序，特定应用程序标识由其可执行文件的名称，为路径和应用程序名称，或为特定的 URL 处理请求的 HTTP 服务器应用程序。

- **所有应用程序**指定 QoS 策略向导的第一页上的流量管理设置应用到所有应用程序。

- **只有具有此可执行文件名称的应用程序**指定 QoS 策略向导的第一页上的流量管理设置的特定应用程序。 可执行文件的名称必须以 .exe 文件名扩展名为结尾。

- **只有 HTTP 服务器的应用程序对请求的响应此 url**指定 QoS 策略向导的第一页上的流量管理设置应用到某些 HTTP 服务器仅限于应用程序。

或者，你可以输入应用程序路径。 若要指定一个应用程序路径，则包含带有该应用程序名称的路径。 该路径可以包含环境变量。 例如，%ProgramFiles%\My Application Path\MyApp.exe 或 c:\program files\my application path\myapp.exe。

>[!NOTE]
>应用程序路径不能包含解析为符号链接的路径。

URL 必须符合[RFC 1738](https://tools.ietf.org/html/rfc1738)，形式的`http[s]://<hostname\>:<port\>/<url-path>`。 可以使用通配符`‘*’`，对于`<hostname>`和/或`<port>`，例如`https://training.\*/, https://\*.\*`，通配符不能表示的子字符串，但`<hostname>`或`<port>`。

换而言之，既不`https://my\*site/`也不`https://\*training\*/`有效。 

（可选） 可以检查**包括子目录和文件**来执行所有子目录和文件遵循一个 URL 进行都匹配。 例如，如果选中此选项，并且 URL 是`https://training`，QoS 策略将考虑请求` https://training/video`有效匹配项。

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>若要配置 QoS 策略向导的应用程序名称页

1. 在中**此 QoS 策略适用于**，选择**的所有应用程序**或**仅具有此可执行文件名称的应用程序**。

2. 如果选择了 **“仅带有该可执行名称的应用程序”**，则需指定一个以 .exe 文件名扩展名结尾的可执行名称。

3. 单击“下一步” 。

### <a name="wizard-page-3---ip-addresses"></a>向导第 3 页-IP 地址

在 QoS 策略向导的第三页可以指定 IP 地址条件对于 QoS 策略，其中包括：

- 全部的源 IPv4 或 IPv6 地址或特定的源 IPv4 或 IPv6 地址

- 所有目标 IPv4 或 IPv6 地址或特定的目标 IPv4 或 IPv6 地址

如果你选择 **“仅用于以下源 IP 地址”** 或 **“仅用于以下目标 IP 地址”**，则必须键入以下地址：

- IPv4 地址如 `192.168.1.1`

- 使用网络前缀长度表示法，例如 IPv4 地址前缀 `192.168.1.0/24`

- IPv6 地址如 `3ffe:ffff::1`

- IPv6 地址前缀如 `3ffe:ffff::/48`

如果同时选择**仅用于以下源 IP 地址**并**仅用于以下目标 IP 地址**，地址或地址前缀必须是 IPv4 或 IPv6 的基于。

如果在前一个向导页中指定的 HTTP 服务器应用程序的 URL，您会注意到此向导页上的 QoS 策略的源 IP 地址将灰显。 

因为源 IP 地址是 HTTP 服务器地址和此处不可配置，也是如此。 但是，您仍然可以自定义策略通过指定的目标 IP 地址。 这使您可通过使用相同的 HTTP 服务器应用程序以不同的客户端创建不同的策略。

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>若要配置 QoS 策略向导的 IP 地址页

1. 在中**此 QoS 策略适用于**（源），选择**任何源 IP 地址**或**仅用于以下 IP 源地址**。

2. 如果所选**仅以下 IP 源地址**，或指定的 IPv4 或 IPv6 地址前缀。

3. 在中**此 QoS 策略适用于**（目标），选择**任意目标地址**或**仅限下列 IP 目标地址。**

4. 如果所选**仅限下列 IP 目标地址**，指定 IPv4 或 IPv6 地址的类型相对应的前缀或指定的源地址前缀。

5.  单击“下一步” 。  

### <a name="wizard-page-4---protocols-and-ports"></a>向导第 4 页-协议和端口

在第四个 QoS 策略向导的页上，可以指定类型的流量和由该向导的第一页上的设置控制的端口。 可以指定：  
-   TCP 流量、UDP 流量或两者兼而有之  

-   全部源端口、一系列源端口或某个特定源端口

-   所有目标端口、 目标端口范围或特定的目标端口  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>若要配置 QoS 策略向导的协议和端口页

1. 在 **“选择适用本 QoS 策略的协议”** 界面，选择 **TCP**、**UDP** 或 **TCP 和 UDP**。

2. 在 **“指定源端口号”** 界面，选择 **“从任一源端口”** 或 **“从该源端口号”**。

3. 如果所选**来自此源端口号**，键入一个介于 1 和 65535 之间的端口号。

     或者，您可以指定端口范围，格式为"*低*:*高*，"位置*低*并*高*表示下限和该端口的上限范围，含。 *低*并*高*每个必须是介于 1 和 65535 之间的数字。 在冒号 (:) 字符及数字之间不允许有空格。

4. 在 **“指定目标端口号”** 界面，选择 **“至任一目标端口”** 或 **“至该目标端口号”**。

5. 如果在之前的步骤中选择了 **“至该目标端口号”**，则需要键入一个端口号，范围为 1 到 65535。

若要完成创建新的 QoS 策略，请单击**完成**上**协议和端口**QoS 策略向导的页。 完成后，新的 QoS 策略在组策略对象编辑器的详细信息窗格中列出。  
  
若要应用到用户或计算机的 QoS 策略设置，QoS 策略在 Active Directory 域服务容器，如域、 站点或组织单位 (OU) 所在的 GPO 的链接。  
  
##  <a name="bkmk_editpolicy"></a>查看、 编辑或删除 QoS 策略

QoS 策略向导所述的页面与以前对应时查看或编辑策略的属性，将显示的属性页。  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>若要查看 QoS 策略的属性  
  
-   右键单击详细信息窗格中的组策略对象编辑器中中的策略名称，然后单击**属性**。  
  
     组策略对象编辑器显示带有以下选项卡的属性页：  
  
    -   策略概述  
  
    -   应用程序名  
  
    -   IP 地址  
  
    -   协议和端口  
  
### <a name="to-edit-a-qos-policy"></a>若要编辑 QoS 策略  
  
-   右键单击详细信息窗格中的组策略对象编辑器中中的策略名称，然后单击**编辑现有策略**。  
  
     组策略对象编辑器显示**编辑一个现有的 QoS 策略**对话框。  
  
### <a name="to-delete-a-qos-policy"></a>若要删除 QoS 策略  
  
-   右键单击详细信息窗格中的组策略对象编辑器中中的策略名称，然后单击**删除策略**。  
  
### <a name="qos-policy-gpmc-reporting"></a>QoS 策略 GPMC 报告 

在组织内应用的 QoS 策略数之后，可能会有用或必需定期查看如何应用策略。 可以使用 GPMC 报告来查看特定用户或计算机的 QoS 策略的摘要。  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>若要运行的 QoS 策略报告组策略结果向导  
  
-   在 GPMC 中，右键单击**组策略结果**节点，并选择菜单选项**组策略结果向导。**  
  
组策略结果生成后，单击**设置**选项卡。上**设置**选项卡中，QoS 策略可以找到的"计算机配置 \windows Settings\QoS 策略"和"用户配置 \windows Settings\QoS 策略"节点下。  
  
上**设置**选项卡上，通过使用其 DSCP 值、 中止值等级、 策略条件中，QoS 策略名称列出了 QoS 策略和在同一行中入选 GPO 列出...  
  
组策略结果视图唯一地标识入选 GPO。 当多个 Gpo 有同名 QoS 策略的 QoS 策略时，应用具有最高的 GPO 优先级的 GPO。 这是入选 GPO。 冲突的 QoS 策略 （由策略名称标识） 附加到未应用 GPO 较低优先级。 请注意 GPO 优先级定义的 QoS 策略部署在站点、 域或 OU 中，根据需要。 部署后，在用户或计算机级别上， [QoS 策略优先顺序规则](#BKMK_precedencerules)确定允许和阻止的流量。  
  
QoS 策略的 DSCP 值、 中止值等级和策略条件都是可见的组策略对象编辑器 (GPOE) 中  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>漫游和远程用户的的高级的设置  
使用 QoS 策略时，目标是管理企业网络上的流量。 在移动方案中，打开或关闭企业网络，可能会发送用户流量。 因为 QoS 策略不相关离开企业的网络时，仅在 Windows 8、 Windows 7 或 Windows Vista 连接到企业的网络接口上启用 QoS 策略。  
  
例如，用户可能她便携式计算机连接到她企业网络通过虚拟专用网络 (VPN) 从咖啡店。 为 VPN，物理网络接口 （如无线） 不会应用的 QoS 策略。 但是，VPN 接口将拥有应用，因为它将连接到企业的 QoS 策略。 如果用户以后输入没有 AD DS 信任关系的另一个企业网络，将不启用 QoS 策略。  
  
请注意，这些移动方案不适用于服务器工作负荷。 例如，具有多个网络适配器的服务器可能位于以下位置上的企业网络边缘。 IT 部门可以选择建立 egresses 企业; 的 QoS 策略限制流量但是，此网络适配器发送此传出流量不会不一定是连接回企业网络中。 出于此原因，所有网络接口的运行 Windows Server 2012 的计算机上始终启用 QoS 策略。  
  
> [!NOTE]
>  选择性支持仅适用于 QoS 策略而不适用于此文档中的下一步所述的高级 QoS 设置。  
  
### <a name="advanced-qos-settings"></a>高级的 QoS 设置

高级的 QoS 设置提供了其他控制，IT 管理员可以管理计算机网络消耗和 DSCP 标记。 而可以在计算机和用户级别应用 QoS 策略，高级的 QoS 设置将应用只能在计算机级别。

#### <a name="to-configure-advanced-qos-settings"></a>若要配置高级 QoS 设置

1.  单击**计算机配置**，然后单击**组策略中的 Windows 设置**。
  
2.  右键单击**QoS 策略**，然后单击**高级 QoS 设置**。

     下图显示了这两个高级 QoS 设置选项卡：**入站 TCP 流量**并**DSCP 标记重写**。
  
> [!NOTE]
>  高级的 QoS 设置都是计算机级别的组策略设置。
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>高级 QoS 设置： 入站 TCP 流量

**入站 TCP 流量**控制 TCP 带宽消耗在接收方端，而 QoS 策略会影响出站 TCP 和 UDP 流量。 

通过级别上设置较低的吞吐量**入站 TCP 流量**选项卡上，TCP 将限制的大小及其播发 TCP 接收窗口。 此设置的效果将更高的吞吐率并链接利用率更高的带宽或延迟 （带宽延迟积） 使用 TCP 连接。 默认情况下，运行 Windows Server 2012、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的计算机设置为最大吞吐量级别。
  
TCP 接收窗口已从以前版本的 Windows 中 Windows Server 2012、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista。 以前版本的 Windows 限制到最多为 64 千字节 (KB) TCP 接收方窗口，而 Windows Server 2012、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 动态调整接收方窗口的大小最多为 16 兆字节 (MB). 在入站 TCP 流量控件中，可以通过设置 TCP 接收窗口可增至的最大值来控制入站的吞吐量级别。 级别对应于以下的最大值。 
  
|入站的吞吐量级别|最多|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

实际的窗口大小可能是等于或小于最大值，具体取决于网络条件的值。

###### <a name="to-set-the-tcp-receive-side-window"></a>若要设置 TCP 接收方窗口

1. 在组策略对象编辑器中，单击**本地计算机策略**，单击**Windows 设置**，右键单击**QoS 策略**，然后单击**高级 QoS设置**。
  
2. 在中**TCP 接收吞吐量**，选择**配置 TCP 接收吞吐量**，，然后选择所需的吞吐量级别。

3.  将 GPO 链接到 OU。

#### <a name="advanced-qos-settings-dscp-marking-override"></a>高级的 QoS 设置：DSCP 标记重写

DSCP 标记重写将限制应用程序指定的功能，或"标记"— QoS 策略中指定的以外的 DSCP 值。 通过指定允许应用程序设置 DSCP 值，应用程序可以设置 DSCP 值为非零。 

通过指定**忽略**，使用 QoS Api 的应用程序会将设置为零，其 DSCP 值并仅 QoS 策略可以设置 DSCP 值。 

默认情况下，运行 Windows Server 2016、 Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的计算机允许应用程序指定 DSCP 值;应用程序和不使用 QoS Api 的设备不会被重写。

##### <a name="wireless-multimedia-and-dscp-values"></a>无线多媒体和 DSCP 值

[Wi-fi 联盟](https://go.microsoft.com/fwlink/?LinkId=160769)已建立证书为无线多媒体\(WMM\) ，它定义四个访问类别\(WMM_AC\)优先网络通信传输 Wi\-Fi 无线网络。 访问类别包括\(最高到最低优先级顺序\)： 语音、 视频、 最佳工作量和背景; 分别缩写为 VO、 VI、 BE 和 BK。 WMM 规范定义的 DSCP 值与每个四个访问类别相对应：
  
|DSCP 值|WMM 访问类别|
|----------|-------------------|
|48-63|语音 (VO)|
|32-47|视频 (VI)|
|24-31, 0-7|最大努力 (BE)|
|8-23|背景 (BK)|

可以创建使用这些 DSCP 值以确保该可移植的 QoS 策略适用的计算机\-Fi Certified™ WMM 的无线适配器接收优先处理时与 Wi\-Fi 认证 WMM 访问点。
  
### <a name="BKMK_precedencerules"></a>QoS 策略的优先顺序规则

类似于 GPO 的优先级，QoS 策略具有优先顺序规则来解决冲突，当多个 QoS 策略应用到一组特定的流量。 对于出站 TCP 或 UDP 流量，只能有一个 QoS 策略可以应用一次，这意味着，QoS 策略没有累积的效果，例如，将在其中求和限制速率。

一般情况下，最匹配条件的 QoS 策略定为目标。 当多个 QoS 策略适用时，规则划分为三个类别： 用户级别而不是计算机级别;应用程序、 网络五元组;和在网络之间 quintuple。

通过*网络 quintuple*，源 IP 地址、 目标 IP 地址、 源端口、 目标端口和协议是指\(TCP/UDP\)。  

 **用户级 QoS 策略优先于计算机级别 QoS 策略**

此规则极大地方便了特别是对于用户基于组策略的 QoS 的 Gpo，网络管理员管理。 例如，如果网络管理员想要定义用户组的 QoS 策略，它们就可以创建和分发到该组的 GPO。 它们无需担心哪些计算机关于这些用户登录到和是否这些计算机将具有冲突的 QoS 策略定义，因为如果存在冲突，用户级别策略始终优先。

> [!NOTE]
>  用户级 QoS 策略才适用于该用户生成的流量。 为该用户定义的任何 QoS 策略将不需要的特定计算机和计算机本身，其他用户。

 **应用程序特定性和拍摄优先于网络五元组**

当多个 QoS 策略匹配的特定流量时，应用更加具体的策略。 在确定应用程序的策略，包括发送应用程序的文件路径的策略被视为比仅标识应用程序名称 （无路径） 的另一个策略更具体。 如果多个策略与应用程序仍适用，优先顺序规则将使用 quintuple 网络以找到最佳匹配项。

或者，多个 QoS 策略可能适用于相同的流量通过指定非重叠的条件。 应用程序的条件，网络五元组，指定的应用程序的策略被视为更具体，并应用。 

例如，policy_A 只需指定应用程序名称 (app.exe) 和 policy_B 指定目标 IP 地址 192.168.1.0/24。 这些 QoS 策略发生冲突时\(app.exe 将流量发送到的 192.168.4.0/24 范围之内的 IP 地址\)，policy_A 获取应用。

 **更具体将在网络中的优先 quintuple**

对于网络五元组内的策略冲突，具有最匹配的条件的策略优先。 例如，假定 policy_C 指定源 IP 地址"任何"、 目标 IP 地址 10.0.0.1、 源端口"任何"、 目标端口"任何"、 和协议"TCP"。 

接下来，假定 policy_D 指定源 IP 地址"任何"、 目标 IP 地址 10.0.0.1，源端口 80，"任何"、 目标端口和协议"TCP"。 然后 policy_C 和 policy_D 都与匹配连接到目标 10.0.0.1: 80。 QoS 策略应用的最具体的匹配条件的策略，因为 policy_D 优先在此示例中。  
  
但是，QoS 策略可能会有相同数目的条件。 例如，多个策略可能每个指定只有一个 （但不是相同） 的网络五元组。 网络五元组，在以下顺序为从高到较低优先级：

- 源 IP 地址

- 目标 IP 地址

- 源端口

- 目标端口

- 协议（TCP 或 UDP）

在一个特定的条件，例如 IP 地址中更具体的 IP 地址被视为具有高优先级;例如，一个 IP 地址 192.168.4.1 是比 192.168.4.0/24 更具体。

设计相应的 QoS 策略尽可能明确地来简化你的组织能够了解哪些策略是否有效。

本指南的下一个主题，请参阅[QoS 策略事件和错误](qos-policy-errors.md)。

本指南中的第一个主题，请参阅[服务质量 (QoS) 策略](qos-policy-top.md)。
