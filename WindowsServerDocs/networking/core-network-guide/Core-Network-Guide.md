---
title: Core 网络指南
description: 本指南提供有关如何计划，并将其部署核心组件所需的完全正常网络域和新 Active Directory 新林中与 Windows Server 2016 的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90390a7b975bc358fb96715d23fe542bc261220e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide"></a>Core 网络指南

>适用于：Windows Server（半年通道），Windows Server 2016

本指南提供有关如何计划，并将其部署核心组件所需的完全正常网络域和新 Active Directory 新建森林中的说明进行操作。

> [!NOTE]
> 本指南是可供下载 TechNet 库从 Microsoft Word 格式。 有关详细信息，请参阅[Core 网络指南为 Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

本指南包含以下部分。

- [有关此指南](#BKMK_about)

- [Core 网络概述](#BKMK_overview)

- [Core 网络计划](#BKMK_planning)

- [Core 网络部署](#BKMK_deployment)

- [其他技术资源](#BKMK_resources)

- [通过 E 附录 A](#BKMK_appendix)

## <a name="BKMK_about"></a>有关此指南
本指南专为网络和系统管理员谁安装新的网络或人想要创建域基于网络替换工作组以下内容组成的网络。 本指南中提供的部署方案是预见需在将来更多服务和功能添加到你的网络时尤其有用。

建议你查看设计，并且部署指导为每台使用此部署方案以帮助你确定本指南提供服务和配置，你需要技术。

核心网络集锦网络硬件、 设备，并且需要提供一些基本服务，为你的组织的信息的技术 (IT) 的软件。

Windows Server 核心网络为您提供了很多权益，其中包括以下。

-   在计算机和其他传输控件协议/Internet 协议 (TCP/IP) 的兼容设备之间的网络连接的核心协议。 TCP/IP 是一套连接计算机和生成网络标准协议。 TCP/IP 是网络协议软件与 Microsoft 的 Windows 操作系统提供实现，并支持 tcp/ip 协议。

-   动态主机配置协议 (DHCP) 自动 IP 地址分配给计算机和其他设备配置为 DHCP 客户端。 手动配置你的网络上的所有计算机上的 IP 地址是耗时较低比动态提供计算机和其他设备，以及使用 DHCP 服务器的 IP 地址配置灵活。

-   域名系统 (DNS) 分辨率服务。 DNS 允许用户、 的计算机、 应用和服务使用完全限定域名计算机或设备在网络上查找计算机和设备的 IP 地址。

-   森林，这是一个或多个共享相同类和属性定义 （架构）、 网站和复制信息 （配置），并树林搜索功能 （全球目录） 的 Active Directory 域。

-   森林根域，这是第一个域创建新树林中。 企业管理员和方案管理员组，了树林管理组，都位于森林根域。 此外，森林根域中，使用其他域，是集合的计算机、 用户和组对象程序定义的 Active Directory 域服务 (广告 DS) 中的管理员。 这些对象共享常见的目录数据库和安全策略。 如果你的组织随着添加域，它们也可以与其他域共享安全关系。 目录服务还会存储目录数据，并允许授权的计算机、 应用程序和用户访问数据。

-   用户和计算机的帐户数据库。 目录服务提供允许您创建个人和获得授权，可连接到你的网络和访问网络资源，如应用、 数据库、 共享的文件和文件夹以及打印机的计算机用户和计算机帐户集中的用户帐户数据库。

核心网络还允许您您的组织成长和 IT 要求更改随着你的网络。 例如，使用 core 网络可以添加域、 IP 子网、 远程访问服务、 无线服务和其他功能和服务器角色由 Windows Server 2016。

### <a name="network-hardware-requirements"></a>网络硬件要求
要成功部署核心网络，你必须部署网络硬件，其中包括：

-   以太网、 Fast 以太网或千兆字节以太网电缆

-   中心、 层 2 或 3 开关、 路由器或执行转发之间计算机和设备网络通信的函数其他设备。

-   如果计算机符合他们各自的客户端和服务器操作系统的最低硬件要求。

## <a name="what-this-guide-does-not-provide"></a>本指南不提供的内容
本指南中不提供用于部署以下说明进行操作：

-   网络的硬件，例如电缆、 路由器，开关和集线器

-   其他网络资源，如打印机和文件服务器

-   Internet 连接

-   远程访问

-   无线访问

-   客户端计算机部署

> [!NOTE]
> 默认情况下，若要收到 DHCP 服务器的 IP 地址租赁配置计算机运行的 Windows 客户端操作系统。 因此，任何其他 DHCP 或 Internet 协议版本 4 (IPv4) 配置的客户端计算机是必需的。

## <a name="technology-overviews"></a>技术概述
以下部分中提供所需的技术的部署了用于创建 core 网络的简短的概述。

### <a name="active-directory-domain-services"></a>Active Directory 域服务
目录是会对象的信息存储在网络上，如用户和计算机的分层结构。 目录服务，如广告 DS 提供目录数据存储和网络的用户和管理员推出此类数据的方法。 例如，广告 DS 会存储有关用户帐户信息，包括姓名、 电子邮件地址、 密码和电话号码，并允许相同网络上的其他授权的用户，若要访问此信息。

### <a name="dns"></a>DNS
DNS 是 TCP/IP 网络，如 Internet 或组织网络名称分辨率协议。 DNS 服务器承载使客户端计算机的信息和服务，可轻松解决识别，字母数字 DNS 名为相互使用计算机的 IP 地址。

### <a name="dhcp"></a>DHCP
DHCP 是一种 IP 标准简化主机 IP 配置管理。 DHCP 标准提供适用于使用 DHCP 服务器的方法，你的网络上已启用 DHCP 的客户端管理动态分配的 IP 地址和其他相关的配置的详细信息。

DHCP 允许你使用 DHCP 服务器动态分配到计算机或其他设备，如打印机，你的本地网络上的 IP 地址。 TCP/IP 网络上的每台计算机必须具有一个唯一的 IP 地址，因为在 IP 地址与之相关子网掩码识别主计算机和子网连接到计算机。 通过使用 DHCP，你可以确保所有计算机配置为 DHCP 客户端都接收适合他们的网络位置和子网，IP 地址，并使用 DHCP 选项，如默认网关和 DNS 服务器，你可以自动提供 DHCP 客户端与他们的需求才能正常工作，你的网络上的信息。

对于基于 IP 网络的复杂性和管理的工作涉及重新配置计算机中的金额，减少了 DHCP。

### <a name="tcpip"></a>TCP/IP
在 Windows Server 2016 TCP/IP 是以下方法：

-   网络基于符合行业标准网络协议的软件。

-   可路由的企业网络协议支持基于 Windows 的本地网络 (LAN) 和宽的区域广域网环境计算机的连接。

-   核心技术和实用程序可用于连接和共享的信息对于不同系统基于 Windows 的计算机。

-   基础访问全球的 Internet 服务，例如世界范围内 Web 和文件传输协议 (FTP) 的服务器。

-   强大、 可缩放跨平台的客户端月服务器框架。

TCP/IP 提供启用基于 Windows 的计算机连接和其他 Microsoft 和非 Microsoft 系统，包括共享信息的基本 TCP/IP 实用程序：

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012R2

-   Windows 8.1

-    Windows Server 2012

-   Windows 8

-    Windows Server 2008 R2

-    Windows 7

-    Windows Server 2008

-   Windows Vista

-   Internet 主机

-   Apple Macintosh 系统

-   IBM 大型机

-   UNIX 和 Linux 系统

-   开放 VM 系统

-   准备好销售网络打印机

-   平板电脑和手机网络电话有线的以太网或 802.11 项无线技术，启用

## <a name="BKMK_overview"></a>Core 网络概述
下图显示了 Windows Server 核心网络拓扑。

![Windows Server 核心网络拓扑](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> 本指南还包括添加到你的网络拓扑可选网络策略 Server (NPS) 和 Web 服务器 (IIS) 服务器的安全网络的访问权限解决方案，如 802.1 X 有线和无线部署，你可以使用 Core 网络助手指南实现奠定的说明。 有关详细信息，请参阅[部署可选功能网络的访问权限身份验证和 Web 服务](#BKMK_optionalfeatures)。

### <a name="core-network-components"></a>核心网络组件
以下是核心网络的组件。

##### <a name="router"></a>路由器
此部署指南提供用于部署的路由器已启用 DHCP 转移的分隔的两个子网核心网络的说明进行操作。 但是，可以部署第二层开关、 3 层切换或集线器，具体取决于你的要求和资源。 如果部署切换时，切换必须是可 DHCP 转移或你必须在每个子网放置 DHCP 服务器。 部署中心时，如果你部署单个子网，并且无需 DHCP 转移或第二个范围 DHCP 服务器上。

##### <a name="static-tcpip-configurations"></a>TCP/IP 的静态配置
在此部署服务器配置具有静态 IPv4 地址。 默认情况下，若要收到 DHCP 服务器的 IP 地址租赁配置客户端计算机。

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Active Directory 域服务全球目录和 DNS 服务器 DC1
同时 Active Directory 域服务 (广告 DS) 域名系统 (DNS) 此名为 DC1，提供目录的服务器上安装和命名分辨率服务所有计算机和在网络上的设备。

##### <a name="dhcp-server-dhcp1"></a>DHCP 服务器 DHCP1
DHCP 服务器，名为 DHCP1，以提供给计算机 Internet 协议 (IP) 地址租赁堵塞范围配置。 此外可以与其他子网提供对计算机的 IP 地址租赁，如果 DHCP 转移配置到路由器上的其他范围配置 DHCP 服务器。

##### <a name="client-computers"></a>客户端计算机
作为 DHCP 客户端，从 DHCP 服务器自动获取 IP 地址和 DHCP 选项，默认配置计算机运行的 Windows 客户端操作系统。

## <a name="BKMK_planning"></a>Core 网络计划
部署核心网络之前，你必须计划以下各项。

-   [计划子网](#bkmk_NetFndtn_Pln_Subnt)

-   [计划所有服务器基本的配置](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [规划 DC1 部署](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [计划域访问](#bkmk_NetFndtn_Pln_DomAccess)

-   [规划 DHCP1 部署](#bkmk_NetFndtn_Pln_DHCP-01)

以下部分提供了其中每个项目的详细信息。

> [!NOTE]
> 有关计划部署的帮助，请参阅[附录 E-Core 网络规划准备工作表](#BKMK_E)。

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>计划子网
在传输控件协议/Internet 协议 (TCP/IP) 网络路由器用于互连硬件和软件使用不同的物理网络线段，分别称为个子网。 路由器还用于之间个子网 IP 数据包转发。 确定你的网络，其中包括数路由器，你需要在继续本指南中的说明进行操作前子网物理布局。

此外，若要配置静态 IP 地址与网络上的服务器，必须确定你想要使用子网 core 网络服务器所在的 IP 地址范围。 本指南中的专用的 IP 地址范围 10.0.0.1-10.0.0.254 和 10.0.1.1-10.0.1.254 用作的示例中，但你可以使用任何你喜欢的专用 IP 地址范围。

> [!IMPORTANT]
> 选择你想要为每个子网使用的 IP 地址范围后，请确保将你的路由器配置从同一 IP 地址范围和子网用于装有路由器的 IP 地址。 例如，如果你的路由器配置默认情况下使用 192.168.1.1 的 IP 地址，但上具有 IP 地址范围的 10.0.0.0/24 子网安装路由器，你必须重新配置路由器使用来自 10.0.0.0/24 IP 地址范围 IP 地址。

以下识别专用范围指定通过 Internet 请求评论 (RFC) 1918年的 IP 地址：

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

当你指定 RFC 1918 中使用专用的 ip 地址时，你无法直接连接到 Internet 使用专用的 IP 地址，因为 Internet 服务提供商 (ISP) 路由器自动被丢弃转到这些地址进出的请求。 若要添加 Internet 连接到你的核心网络更高版本，你必须合同 isp 获取公共的 IP 地址。

> [!IMPORTANT]
> 当使用专用 IP 地址，你必须使用某些类型的代理服务器或网络地址翻译 (NAT) 服务器转换公共的 IP 地址，可以在 Internet 上传送到本地网络上的专用的 ip 地址。 大多数路由器提供 NAT 服务，以便选择路由器 NAT 能够应非常简单。

有关详细信息，请参阅[规划 DHCP1 部署](#bkmk_NetFndtn_Pln_DHCP-01)。

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>计划所有服务器基本的配置
中每个服务器核心网络，你必须将计算机重命名指定和配置静态 IPv4 地址和其他 TCP/IP 属性计算机。

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>计划命名规则计算机和设备
为了在您的网络保持一致，它是一个好主意使用一致的服务器、 打印机和其他设备的名称。 计算机名称可用于帮助的用户和管理员轻松地找出用途和服务器、 打印机或其他设备的位置。 例如，如果你有三个 DNS 服务器、 分别旧金山、 北京，在和芝加哥中，你可能会使用命名约定*服务器函数*-*位置*-*号码*:

-   DNS DEN 01。 该名称表示丹佛科罗拉多中的 DNS 服务器。 如果其他 DNS 服务器添加中丹佛，可以增加名称中的数值，如下所示 DNS-DEN 02 和 DNS-DEN 03。

-   DNS SPAS 01。 该名称表示南帕萨迪纳，加利福尼亚中的 DNS 服务器。

-   DNS ORL 01。 该名称表示佛罗里达州中的 DNS 服务器。

本指南，服务器命名约定非常简单，并主服务器功能和数字组成。 例如，域控制器称为 DC1 并 DHCP 服务器命名 DHCP1。

建议你选择命名约定之前安装 core 网络使用本指南。

#### <a name="planning-static-ip-addresses"></a>计划静态 IP 地址
配置之前每台计算机的静态 IP 地址，你必须计划你的 ip 地址和个子网。 此外，你必须确定 DNS 服务器的 IP 地址。 如果你打算安装提供对其他个子网等的其他网络或 Internet 访问路由器，你必须知道路由器，也称为的静态 IP 地址配置默认网关、 的 IP 地址。

下表提供的静态 IP 地址配置的示例值。

|配置项目|示例值|
|-----------------------|------------------|
|IP 地址|10.0.0.2|
|子网掩码|255.255.255.0|
|默认网关 （路由器的 IP 地址）|10.0.0.1|
|首选的 DNS 服务器|10.0.0.2|

> [!NOTE]
> 如果你计划部署多个 DNS 服务器上，你还可以规划备用 DNS 服务器的 IP 地址。

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>规划 DC1 部署
以下是安装 Active Directory 域服务 (广告 DS) 之前的关键规划的步骤和 DNS DC1 上的。

#### <a name="planning-the-name-of-the-forest-root-domain"></a>计划森林根域的名称
广告 DS 设计过程中的第一步是确定多少森林需要你的组织。 森林顶级广告 DS 容器，并且包含一个或多个共享常见架构与全球目录的域。 一个组织都可以具有多个林，但对于大多数组织，单个森林设计首选的模型，最简单来管理。

当在你的组织中创建的第一个域控制器时，您将创建 （也称为森林根域） 的第一个域，第一森林。 但是，需要使用本指南此操作之前，必须确定你的组织的最佳域名。 在大多数情况下，组织名称用作域名，，并在许多情况下此域名注册。 如果你打算部署面向外部基于 Internet 的 Web 服务器来提供信息和服务为客户或合作伙伴、 选择不是使用域名和，以便你的组织拥有它，然后注册的域名。

#### <a name="planning-the-forest-functional-level"></a>计划森林功能级别
在安装广告 DS，必须选择你想要使用森林功能级别。 Windows Server 2003 Active Directory 中, 引入的域和森林功能提供启用域-或树林 Active Directory 网络环境中的功能的方式。 不同级别的域功能和森林功能我们提供，具体取决于您的环境。

森林功能启用你森林中的所有的域的功能。 有以下森林功能级别：

-    Windows Server 2008。 此森林功能级别支持运行 Windows Server 2008 和更高版本的 Windows Server 操作系统的域控制器。

-    Windows Server 2008 R2。 Windows Server 2008 R2 域控制器和运行更高版本的 Windows Server 操作系统的域控制器，支持此森林功能级别。

-    Windows Server 2012。 Windows Server 2012 域控制器和运行更高版本的 Windows Server 操作系统的域控制器，支持此森林功能级别。

-    Windows Server 2012 R2。 Windows Server 2012 R2 域控制器和运行更高版本的 Windows Server 操作系统的域控制器，支持此森林功能级别。

-    Windows Server 2016。 此森林功能级别仅支持 Windows Server 2016 域控制器和运行更高版本的 Windows Server 操作系统的域控制器。

如果要部署新林中新域，域控制器的所有运行 Windows Server 2016，建议您将配置广告 DS 与 Windows Server 2016 森林功能级别广告 DS 安装期间。

> [!IMPORTANT]
> 森林功能提升后，域控制器正在运行早期版本操作系统无法引入林。 例如，如果提升对 Windows Server 2016 的森林功能级别时，域控制器在运行 Windows Server 2012 R2 或 Windows Server 2008 无法添加到森林。

下表中提供了示例配置广告 DS 项目。

|配置项：|示例值：|
|------------------------|-------------------|
|DNS 全名|示例：<br /><br />-corp.contoso.com<br />-example.com|
|森林功能级别|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />Windows Server 2012 R2 <br />Windows Server 2016|
|Active Directory 域服务数据库文件夹位置|E:\Configuration\\<br /><br />或接受默认位置。|
|Active Directory 域服务日志文件的文件夹位置|E:\Configuration\\<br /><br />或接受默认位置。|
|Active Directory 域服务 SYSVOL 文件夹位置|E:\Configuration\\<br /><br />或接受默认位置|
|目录恢复模式的管理员密码|**J\ * p2leO4$ F**|
|Answer 文件名称 （可选）|**广告 DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>计划 DNS 区域
主要、 Active Directory 集成 DNS 服务器，在一个前进查找区域创建默认情况下安装 DNS 服务器角色过程。 向前查找区域允许计算机和设备查询另一台计算机或设备的 IP 地址根据其 DNS 名称。 除了转发搜索区域，建议你创建了 DNS 反向查找区域。 与 DNS 反向的搜索查询、 计算机或设备能够发现另一台计算机或设备使用的 IP 地址的名称。 部署了反向查找区域通常会提高 DNS 性能，并且会大大增加成功 DNS 查询的问题。

当你创建了反向查找区域时，in-addr.arpa 域中，在 DNS 标准定义和保留在 Internet DNS 空间，可以使实用可靠地执行反向查询，配置在 DNS。 若要创建反向命名空间，子域 in-addr.arpa 域中，可以形成反向排序的 IP 地址小数点符号中的数字。

In-addr.arpa 域中适用于所有根据 Internet 协议版本 4 (IPv4) 的 TCP/IP 网络解决。 新的区域向导自动假设你创建一个新反向查找区域时使用此域。

在运行新区域向导时，建议选择下列选项：

|配置项目|示例值|
|-----------------------|------------------|
|区域类型|**主要区域**，并**Active Directory 中存储区域**选择|
|活动目录区域复制范围|**到这个域中的所有 DNS 服务器**|
|第一个反向查找区域名称向导页面|**IPv4 反向查找区域**|
|第二个反向查找区域名称向导页面|网络 ID = 10.0.0。|
|动态更新|**允许仅安全的动态更新**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>计划域访问
登录到域，该计算机必须域成员计算机，并且必须在之前的登录尝试广告 DS 创建用户帐户。

> [!NOTE]
> 各个运行的 Windows 的计算机已本地用户和组的用户帐户安全帐户管理器 （三千） 的用户帐户数据库称为的数据库。 三千数据库中本地计算机上创建用户帐户时，你可以在本地计算机上记录，但你无法登录到某个域。 上域控制器，不能使用本地计算机上本地用户和组域用户帐户创建的 Active Directory 用户和计算机 Microsoft 管理控制台 (MMC)。

在首次成功登录域登录凭据后, 登录设置保持不变除非从域中删除计算机中，或者手动更改登录设置。

之前在你登录到域：

-   Active Directory 用户的计算机中创建用户帐户。 每个用户都必须具有 Active Directory 用户的计算机中 Active Directory 域服务用户帐户。 有关详细信息，请参阅[Active Directory 用户的计算机中创建用户帐户](#BKMK_createUA)。

-   确保正确的 IP 地址配置。 若要加入域的计算机，该计算机必须 IP 地址。 本指南中配置服务器的静态 IP 地址，以及客户端计算机收到 DHCP 服务器的 IP 地址租赁。 出于此原因，，必须将部署 DHCP 服务器的客户端加入域。 有关详细信息，请参阅[部署 DHCP1](#BKMK_deployDHCP01)。

-   加入域的计算机。 提供或访问网络资源的任何计算机必须加入域。 有关详细信息，请参阅[加入域和日志记录上的服务器计算机](#BKMK_joinlogserver)和[加入域和日志记录上的客户端计算机](#BKMK_joinlogclients)。

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>规划 DHCP1 部署
以下是上 DHCP1 安装 DHCP 服务器角色之前的关键规划的步骤。

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>计划 DHCP 服务器和 DHCP 转移
DHCP 消息是广播，因为它们不转发之间路由器个子网。 如果你有多个子网并且想要提供的每个子网 DHCP 服务时，你必须执行以下任一操作：

-   安装每个子网 DHCP 服务器

-   将配置路由器子网间向前 DHCP 广播的消息并配置 DHCP 服务器、 网每一个范围上的多个范围。

在大多数情况下，是网络的成本较低比部署 DHCP 服务器的每个物理段上配置路由器转发 DHCP 广播的消息。

#### <a name="planning-ip-address-ranges"></a>计划 ip 地址
每个子网必须自己独特的 IP 地址范围。 这些范围表示范围具有 DHCP 服务器上。

范围是，使用 DHCP 服务子网上计算机的 IP 地址管理分组。 管理员首先创建为每个物理子网范围，然后使用范围定义客户使用的参数。

范围具有以下属性：

-   范围内的 IP 地址在其中包括或地址，用于 DHCP 租赁服务排除在外。

-   子网掩码，确定子网前缀给定的 IP 地址。

-   它创建时分配范围名称。

-   租赁持续时间值，归入 DHCP 客户端接收动态分配的 IP 地址。

-   配置分配给 DHCP 客户端，如 DNS 服务器 IP 地址和路由器 / 默认网关 IP 地址任何 DHCP 范围选项。

-   （可选） 使用保留确保 DHCP 客户端始终接收同一 IP 地址。

部署你服务器之前, 列出你子网和你想要为每个子网使用的 IP 地址范围。

#### <a name="planning-subnet-masks"></a>计划子网掩码
通过使用子网掩码区分的网络 Id 和 IP 地址内的主机 Id。 每个子网掩码是 32 位使用的都是连续位组大量 (1) 来标识该网络 ID 和所有零 (0) 来识别主机 ID 部分的 IP 地址。

例如，通常使用具有 IP 地址 131.107.16.200 子网掩码是以下 32 位二进制数：

```
11111111 11111111 00000000 00000000
```

此子网掩码号是 16 一位跟 16 零位，指示网络 ID 和主机 ID 部分此 IP 地址的两种 16 位的长度。 通常，这子网掩码显示的小数点分隔 255.255.0.0。

下表显示子网掩码 Internet 的地址类别。

|地址类别|位子网掩码|子网掩码|
|-----------------|------------------------|---------------|
|类 A|11111111 00000000 00000000 00000000|255.0.0.0|
|类 B|11111111 11111111 00000000 00000000|255.255.0.0|
|类 C|11111111 11111111 11111111 00000000|255.255.255.0|

当在 DHCP 创建范围并范围输入的 IP 地址范围时，DHCP 提供了这些默认子网掩码值。 通常，默认子网掩码值是适用于大多数网络没有特殊的要求，其中每个 IP 网络段对应于单个物理网络。

在某些情况下，你可以使用自定义子网掩码实现 IP 子网。 可以与 IP 子网细分默认主机 ID 部分的 IP 地址，若要指定个子网，它是原始类基于网络 id。

通过自定义子网掩码长度，你可以减少位数用于实际主机 id。

若要防止寻址和路由出现问题，你应确保所有 TCP/IP 计算机在网络段上的都使用的相同子网掩码并且每台计算机或设备具有一个唯一的 IP 地址。

#### <a name="planning-exclusion-ranges"></a>计划排除范围
当您创建 DHCP 服务器上的范围内时，你指定 IP 地址范围包含所有允许 DHCP 服务器租赁 DHCP 的客户端，如计算机和其他设备的 IP 地址。 如果再转，然后手动配置某些服务器，并且静态与其他设备从同一 IP 地址范围 DHCP 服务器正在使用的 IP 地址，你会无意中创建冲突 IP 地址，你和 DHCP 服务器了具有都分配同一 IP 地址到不同的设备。

若要解决此问题，可以创建 DHCP 范围排除范围。 排除项范围是不允许使用 DHCP 服务器的范围内的 IP 地址范围内连续的 IP 地址。 如果创建排除范围，DHCP 服务器不分配在此范围内，这允许你手动分配这些地址，而无需创建冲突 IP 地址的地址。

你可以排除 IP 地址 distribution DHCP 服务器的通过创建排除范围，为每个范围。 你应该使用的所有设备上的静态 IP 地址与配置排除项。 排除的地址应该包含所有分配给其他服务器、 非 DHCP 客户端，无盘工作站或路由远程访问和 PPP 客户端的手动的 IP 地址。

建议你与额外地址以适应将来网络增长配置排除范围。 下表的 10.0.0.1-的 IP 地址范围范围内提供一个示例排除范围 10.0.0.254 和子网掩码 255.255.255.0。

|配置项目|示例值|
|-----------------------|------------------|
|排除范围启动 IP 地址|10.0.0.1|
|排除范围结束 IP 地址|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>计划 TCP/IP 静态配置
某些设备，如路由器、 DHCP 服务器和 DNS 服务器，必须配置的静态 IP 地址。 此外，你可能已其他设备，如打印机，你想要始终确保具有相同的 IP 地址。 要静态配置每个网的设备列表中，然后计划排除范围你想要使用 DHCP 服务器上以确保 DHCP 服务器不租赁静态配置设备的 IP 地址。 排除范围是从 DHCP 服务中排除的范围内的 IP 地址的有限的序列。 排除范围确保这些区域中的任何地址不向你的网络上的 DHCP 客户提供服务器。

例如，如果你拥有想要使用的静态 IP 地址配置十设备 IP 地址范围子网是通过 192.168.0.254 192.168.0.1，可以创建 192.168.0 排除范围。*x*包括十或多个 IP 地址的范围： 通过 192.168.0.15 192.168.0.1。

在此示例中，使用十排除的 IP 地址配置服务器和其他设备的静态 IP 地址，五个额外的 IP 地址左轻扫可供你可能想要在以后添加的新设备的静态配置。 使用此排除项范围 DHCP 服务器都不会通过 192.168.0.254 192.168.0.16 地址池。

下表中提供了有关广告 DS 和 DNS 的其他示例配置项目。

|配置项目|示例值|
|-----------------------|------------------|
|网络连接绑定|以太网|
|DNS 服务器设置|DC1.corp.contoso.com|
|首选的 DNS 服务器 IP 地址|10.0.0.2|
|添加范围对话框的值<br /><br />1.范围名称<br />2.起始 IP 地址<br />3.结局 IP 地址<br />4.子网掩码<br />5.默认网关 （可选）<br />6。 租赁持续时间|1.主要子网<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6。 8 天|
|IPv6 DHCP 服务器操作模式|未启用|

## <a name="BKMK_deployment"></a>Core 网络部署
若要部署核心网络的基本步骤如下：

1.  [配置所有服务器](#BKMK_configuringAll)

2.  [部署 DC1](#BKMK_deployADDNS01)

3.  [加入域的服务器计算机并登录](#BKMK_joinlogserver)

4.  [部署 DHCP1](#BKMK_deployDHCP01)

5.  [加入域的客户端计算机并登录](#BKMK_joinlogclients)

6.  [部署可选功能网络的访问权限身份验证和 Web 服务](#BKMK_optionalfeatures)

> [!NOTE]
> -   等效 Windows PowerShell 命令提供了大多数本指南中的步骤。 在运行这些 cmdlet 的 Windows PowerShell 中之前, 替换示例值适用于你的网络部署的值。 此外，你必须输入每个 cmdlet Windows PowerShell 同一行上。 本指南中, 个别 cmdlet 可能在多个行由于格式约束和文档的显示设置由你的浏览器或其他应用上会显示。
> -   本指南中的步骤不包括在其中的情况下的说明进行操作**用户帐户控制**对话框中打开请求您授予权限，才能继续。 如果此对话框打开时，你将在指南中，执行过程，并且如果对话框中已打开，以响应你的操作，请单击**继续**。

### <a name="BKMK_configuringAll"></a>配置所有服务器
其他技术，如 Active Directory 域服务或 DHCP，在安装之前很重要配置了以下各项。

-   [将计算机重命名](#BKMK_rename)

-   [配置的静态 IP 地址](#BKMK_ip)

你可以使用以下各部分每个服务器执行这些操作。

在会员**管理员**，或等效的最低要求执行这些过程。

#### <a name="BKMK_rename"></a>将计算机重命名
在此部分中，可以使用该过程更改计算机的名称。 重命名该计算机可用于操作系统已自动创建你不希望使用计算机名的情况。

> [!NOTE]
> 若要执行此过程，方法是使用 Windows PowerShell 打开 PowerShell 和上单独的行中键入以下 cmdlet，然后按 ENTER。 您还必须替换*ComputerName*你想要使用的名称。
>
> `Rename-Computer`*计算机名称*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>若要重命名计算机运行的 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012

1.  在服务器管理器中，单击**本地服务器**。 计算机**属性**详细信息窗格中显示。

2.  在**属性**中**计算机名称**，单击现有的计算机名称。 **系统属性**对话框中打开。 单击**更改**。 **计算机名月域更改**对话框中打开。

3.  在**计算机名月域更改**对话框中，在**计算机名称**，键入你的计算机的新名称。 例如，如果你想要命名计算机 DC1，键入**DC1**。

4.  单击**确定**两次，然后单击**关闭**。 如果你想要重新启动计算机立即完成名称的更改，请单击**立即重新启动**。 否则，请单击**稍后重新启动**。

> [!NOTE]
> 如何重命名运行的其他 Microsoft 操作系统的计算机上的信息，请参阅[附录 A-重命名计算机](#BKMK_A)。

#### <a name="BKMK_ip"></a>配置的静态 IP 地址
你可以使用本主题中过程配置 Internet 协议版本 4 (IPv4) 属性网络连接的静态 IP 地址的计算机运行的 Windows Server 2016。

> [!NOTE]
> 若要执行此过程，方法是使用 Windows PowerShell 打开 PowerShell 和上单独的行中键入以下 cmdlet，然后按 ENTER。 您还必须你想要使用来配置你的计算机的值更换接口名称和在此示例中的 IP 地址。
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>配置计算机运行的 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 上的静态 IP 地址

1.  在任务栏中，右键单击网络图标，，然后单击**开放的网络和共享中心**。

2.  在**网络和共享中心**，单击**更改适配器设置**。 **网络连接**文件夹将打开并显示可用网络连接。

3.  在**网络连接**，右键单击你想要配置，然后单击的连接**属性**。 网络连接**属性**对话框中打开。

4.  在网络连接**属性**对话框中，在**此连接使用下列项目**、 选择**Internet 协议版本 4 (TCP/IPv4)**，然后单击**属性**。 **Internet 协议版本 4 (TCP/IPv4) 属性**对话框中打开。

5.  在**Internet 协议版本 4 (TCP/IPv4) 属性**，在**常规**选项卡上，单击**使用以下 IP 地址**。 在**IP 地址**，键入你想要使用的 IP 地址。

6.  按 tab 可以将光标放在**子网掩码**。 自动输入子网掩码的默认值。 接受默认子网掩码，或键入你想要使用的子网掩码。

7.  在**默认网关**，键入你的默认网关的 IP 地址。

    > [!NOTE]
    > 你必须配置**默认网关**与你在你的路由器的本地网络 (LAN) 界面使用相同的 IP 地址。 例如，如果你的路由器已连接到 Internet 也移到 LAN 如宽的区域网络 (WAN)，为配置 LAN 接口同一 IP 地址，然后你会将其指定为**默认网关**。 在另一个示例中，如果你已连接到 Lan 两个，其中 LAN 一使用地址范围 10.0.0.0/24，LAN B 使用地址范围 192.168.0.0/24 路由器配置 LAN 一路由器的 IP 地址的地址从该地址的范围内，如 10.0.0.1。 此外，在该地址范围 DHCP 范围内，配置**默认网关**与 IP 地址 10.0.0.1。 对于 LAN B 中，使用该地址的范围内，如 192.168.0.1，从地址配置 LAN B 路由器界面，然后配置与 LAN B 范围 192.168.0.0/24**默认网关**192.168.0.1 的值。

8.  在**首选 DNS 服务器**，键入 IP 地址 DNS 服务器。 如果您计划使用本地计算机作为首选 DNS 服务器，请键入本地计算机的 IP 地址。

9. 在**备用 DNS 服务器**，键入备用 DNS 服务器的 IP 地址 （如果有。 如果你计划备用 DNS 服务器为使用本地计算机中，键入本地计算机的 IP 地址。

10. 单击**确定**，然后单击**关闭**。

> [!NOTE]
> 如何配置运行的其他 Microsoft 操作系统的计算机上的静态 IP 地址的信息，请参阅[附录 B-配置静态 IP 地址](#BKMK_B)。

#### <a name="BKMK_deployADDNS01"></a>部署 DC1
若要部署 DC1，其中计算机运行 Active Directory 域服务 (广告 DS) 和 DNS，你必须完成按以下顺序这些步骤：

-   执行一节中的步骤[配置所有服务器](#BKMK_configuringAll)。

-   [安装广告 DS 和 DNS 为新的林](#BKMK_installAD-DNS)

-   [Active Directory 用户的计算机中创建用户帐户](#BKMK_createUA)

-   [分配的组成员](#BKMK_assigngroup)

-   [配置 DNS 反向查找区域](#BKMK_reverse)

**管理权限**

如果你正在安装一个小的网络，并且仅网络管理员，建议你为自己创建用户帐户，然后作为企业管理员和域管理员成员添加你的用户帐户。 执行此操作将使其更容易用作所有网络资源的管理员。 建议，在你登录该帐户与仅当你需要执行的管理任务，并创建单独的用户帐户的执行非 IT 相关的任务。

如果你有一个变得更大组织具有多个管理员，请参阅广告 DS 文档以确定最佳的组成员组织员工。

**域用户帐户和本地计算机上的用户帐户之间的差异**

基于域的基础结构的优势是你不需要在每台计算机在域中创建用户帐户。 计算机是否客户端计算机或服务器也是如此。

出于此原因，你不应在域中的每台计算机上创建用户帐户。 创建 Active Directory 用户的计算机中的所有用户帐户，并使用前面的过程分配组成员。 默认情况下，所有用户帐户都是域用户组中的成员。

用户域组中的所有成员可以都登录到任何客户端计算机上后加入域。

您可以配置指定允许用户登录到计算机上的日期和时间的用户帐户。 你还可以指定每个用户可以使用哪台计算机。 若要配置这些设置，请打开 Active Directory 的用户和计算机，找到你想要配置的用户帐户，然后双击该帐户。 在用户帐户**属性**，单击**帐户**选项卡，然后单击或者**登录时间**或**登录到**。

#### <a name="BKMK_installAD-DNS"></a>安装广告 DS 和 DNS 为新的林

你可以使用下面的过程中的一个安装 Active Directory 域服务 (广告 DS) 和 DNS 和新树林中创建新的域。 

第一个过程提供有关通过 Windows PowerShell 第二个过程向你显示如何安装广告 DS 和 DNS 使用服务器管理器中执行这些操作的说明进行操作。

>[!IMPORTANT]
>完执行此过程中的步骤后，将自动重新启动计算机。

**安装广告 DS 和使用 Windows PowerShell DNS**

可以使用以下命令以安装和配置广告 DS 和 DNS。 在此示例中域名必须替换你想要使用你的域的值。

>[!NOTE]
>有关这些 Windows PowerShell 命令的详细信息，请参阅以下参考主题。
>- [安装 WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)
>- [安装 ADDSForest](https://technet.microsoft.com/itpro/powershell/windows/adds/deployment/install-addsforest)

在会员**管理员**是最低要求执行此过程。

- 以管理员身份运行 Windows PowerShell，键入以下命令，然后按 enter 键：  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

安装完成后成功，以下消息显示 Windows PowerShell 中。

    
    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...
    

- 在 Windows PowerShell 中键入以下命令，替换文本**corp.contoso.com**与你的域名，并按 ENTER:

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- 在安装和配置过程中，便可看到的 Windows PowerShell 窗口顶部，将显示以下提示。 显示后，键入的密码，然后按 ENTER。

    **SafeModeAdministratorPassword:**

- 你键入的密码，然后按 enter 键后，将显示以下确认提示。 键入相同的密码，然后按 ENTER。

    **确认 SafeModeAdministratorPassword:**

- 出现以下提示时，键入的字母**Y** ，然后按 ENTER。

    
    将配置为域控制器目标服务器，并完成此操作后重新启动。
    若要继续进行此操作吗？
    [Y] 是 [A] 不到所有 [S] 没有 [L] 暂停 [设置] 是对所有 [N]帮助 （默认为"Y"）：
    
- 如果你愿意，可以阅读的广告 DS 和 DNS 正常、 成功安装期间显示警告消息。 这些邮件正常并不是安装失败的指示。

- 成功安装后，会显示一条消息，指出你正准备已注销计算机，以便可以在重启计算机。 如果你单击**关闭**、 立即注销计算机，然后重启计算机。 如果你没有单击**关闭**，默认的一段时间后重启计算机。

- 服务器重启后，你可以验证 Active Directory 域服务和 DNS 成功的安装。 打开 Windows PowerShell、 键入以下命令，然后按 ENTER。

````
Get-WindowsFeature
````

此命令的结果将显示在 Windows PowerShell 和应类似于图所示的结果。 已安装的技术，支架左侧的技术名称中包含字符**X**的值和**安装状态**是**已安装**。

![获取 WindowsFeature 命令的结果](../media/Core-Network-Guide/server-roles-installed.jpg)

**安装广告 DS 和 DNS 服务器管理器的使用**

1.  在 DC1 上，在**服务器管理器**，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。

2.  在**开始之前**，单击**下一步**。

    > [!NOTE]
    > **开始之前**添加角色和功能向导中的页面未显示如果你之前已选择**默认情况下跳过此页**运行添加角色并功能向导。

3.  在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。

4.  在**选择目标服务器**，确保**从服务器池选择服务器**选择。 在**服务器池**，确保在本地计算机处于选中状态。 单击**下一步**。

5.  在**选择服务器角色**中**角色**，单击**Active Directory 域服务**。 在**添加所需的 Active Directory 域服务的功能**，单击**添加功能**。 单击**下一步**。

6.  在**选择功能**，单击**下一步**，并在**Active Directory 域服务**，检查信息，提供了，然后单击**下一步**。

7.  在**确认安装选择**，单击**安装**。 安装进度页面显示在安装过程中的状态。 该过程完成后，在消息详细信息，请单击**推广此域控制器服务器**。 打开 Active Directory 域服务配置向导。

8.  在**部署配置**、 选择**添加新的林**。 在**根域名**，键入你的域的完整的域名 (FQDN)。 例如，如果你 FQDN corp.contoso.com，请键入**corp.contoso.com**。单击**下一步**。

9. 在**域控制器选项**中**选择功能级别的新林和根域**，选择森林功能级别和域你想要使用的功能级别。 在**指定域控制器功能**，确保**域名系统 (DNS) 服务器**和**全球目录 (GC)**选定。 在**密码**和**确认密码**，键入你想要使用的目录服务还原模式 (DSRM) 密码。 单击**下一步**。

10. 在**DNS 选项**，单击**下一步**。

11. 在**其他选项**、 验证分配给域，NetBIOS 名称和仅在必要时对其进行更改。 单击**下一步**。

12. 在**路径**中**指定的广告 DS 数据库日志文件，SYSVOL 位置**，执行下列操作之一：

    -   接受默认值。

    -   键入你想要使用的文件夹位置**数据库文件夹**，**日志文件的文件夹**，并**SYSVOL 文件夹**。

13. 单击**下一步**。

14. 在**查看选项**，检查你的选择。

15. 如果你想要导出到一个 Windows PowerShell 脚本的设置，请单击**查看脚本**。 脚本将在记事本中打开，你可以将它保存到所需的文件夹位置。 单击**下一步**。 在**先决条件检查**，验证你的选择。 检查完成后，单击**安装**。 Windows 的提示，请单击**关闭**。 在服务器重启以完成安装的广告 DS 和 DNS。

16. 若要验证成功安装，请查看服务器管理控制台后重新启动该服务器。 广告 DS 和 DNS 应该会显示在左侧窗格中，如图所示突出显示的项目。

![广告 DS 和 DNS 服务器管理器中](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Active Directory 用户的计算机中创建用户帐户
你可以使用此过程 Active Directory 用户的计算机 Microsoft 管理控制台 (MMC) 中创建新的域用户帐户。

在会员**域管理员**，或等效的最低要求执行此过程。

> [!NOTE]
> 若要执行此过程，方法是使用 Windows PowerShell，打开 PowerShell 和键入以下 cmdlet 上一行，然后按 ENTER。 您还必须替换你想要使用的值在此示例中的用户帐户名。
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> 按 enter 键后，输入用户帐户的密码。 在帐户创建并，默认情况下，被授予会员域用户组。
>
> 使用以下 cmdlet，还可以为新的用户帐户的其他组成员。 下面的示例域管理员和企业管理员多个组中向用户 1。 确保前运行此命令你更改用户帐户名、 域名称和分组，以符合你的要求。
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>若要创建用户帐户

1.  在 DC1，在服务器管理器中，单击**工具**，然后单击**Active Directory 用户和计算机**。 Active Directory 用户和计算机 MMC 打开。 如果它未选中，请单击你的域的节点。 例如，如果你的域 corp.contoso.com，请单击**corp.contoso.com**。

2.  在详细信息窗格中，右键单击你想要添加的用户帐户的文件夹。

    **位置？**

    -   Active Directory 用户和计算机 /*域节点*/*文件夹*

3.  指向**新建**，然后单击**用户**。 **新对象-用户**对话框中打开。

4.  在**名字**，键入第一个用户名。

5.  在**首字母缩写**，键入用户的首字母;。

6.  在**姓氏**，键入最后用户名。

7.  修改**全名**添加中间名，或反向的名字和姓氏顺序。

8.  在**登录用户名**，键入用户名登录。 单击**下一步**。

9. 在**新对象-用户**中**密码**和**确认密码**，键入用户的密码，然后依次选择正确的密码选项。

10. 单击**下一步**，查看新的用户帐户设置，然后单击**完成**。

##### <a name="BKMK_assigngroup"></a>分配的组成员
你可以使用此过程添加到 Active Directory 用户和计算机 Microsoft 管理控制台 (MMC) 中的组的用户、 的电脑或组。

在会员**域管理员**，或等效的最低要求执行此过程。

###### <a name="to-assign-group-membership"></a>若要指定组成员

1.  在 DC1，在服务器管理器中，单击**工具**，然后单击**Active Directory 用户和计算机**。 Active Directory 用户和计算机 MMC 打开。 如果它未选中，请单击你的域的节点。 例如，如果你的域 corp.contoso.com，请单击**corp.contoso.com**。

2.  在详细信息窗格中，双击包含要添加成员的组的文件夹。

    位置？

    -   **Active Directory 用户和计算机**/*域节点*/*包含组文件夹*

3.  在详细信息窗格中，右键单击你想要添加到某个组中，如用户或计算机，然后单击对象**属性**。 对象**属性**对话框中打开。 单击**的成员**选项卡。

4.  在**的成员**选项卡上，单击**添加**。

5.  在**输入要选择的对象名称**，键入你想要添加对象，然后单击的组的名称**确定**。

6.  要为组成员其他用户、 组或的计算机，请重复步骤 4 和 5 此过程。

##### <a name="BKMK_reverse"></a>配置 DNS 反向查找区域
你可以使用此过程配置了反向查找区域中域名系统 (DNS)。

在会员**域管理员**是最低要求执行此过程。

> [!NOTE]
> -   中等和大型企业，建议您将配置并使用 Active Directory 用户的计算机中 DNSAdmins 组。 有关详细信息，请参阅[其他技术资源](#BKMK_resources)
> -   若要执行此过程，方法是使用 Windows PowerShell，打开 PowerShell 和键入以下 cmdlet 上一行，然后按 ENTER。 你必须还在此示例中的 DNS 反向查找区域和区域文件名称替换为你想要使用的值。 确保你反转反向区域名称的网络 ID。 例如，如果网络 ID 192.168.0，创建反向查找区域名称**0.168.192.in-addr.arpa**。
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>若要配置 DNS 反向查找区域

1.  在 DC1，在服务器管理器中，单击**工具**，然后单击**DNS**。 DNS MMC 将打开。

2.  在 DNS，如果尚未展开，双击服务器名称展开该树。 例如，如果 DNS 服务器名称 DC1，请双击**DC1**。

3.  选择**反向查找区域**，右键单击**反向查找区域**，然后单击**新建区域**。 打开新的时区向导。

4.  在**欢迎使用新的时区向导**，单击**下一步**。

5.  在**区域类型**、 选择**主要区域**。

6.  如果写域控制器 DNS 服务器，确保**Active Directory 中存储该区域**选择。 单击**下一步**。

7.  在**Active Directory 区域复制范围**、 选择**到运行此域中的域控制器上的所有 DNS 服务器**，除非你具有特定的原因，若要选择不同的选项。 单击**下一步**。

8.  在第一个**反向查找区域名称**页上，选择**IPv4 反向查找区域**。 单击**下一步**。

9. 在第二个**反向查找区域名称**页上，执行下列操作之一：

    -   在**网络 ID**，键入你的 IP 地址范围的网络 ID。 例如，如果你的 IP 地址范围 10.0.0.1 10.0.0.254 通过，请键入**10.0.0**。

    -   在**反向查找区域名称**，将自动添加你 IPv4 反向查找区域的名称。 单击**下一步**。

10. 在**动态更新**，选择你想要允许的动态更新的类型。 单击**下一步**。

11. 在**完成新的区域向导**，查看你的选择，然后单击**完成**。

#### <a name="BKMK_joinlogserver"></a>加入域的服务器计算机并登录
你已安装 Active Directory 域服务 (广告 DS)，并创建了一个或多个用户帐户有权加入域的计算机后，你可以加入家庭组 core 网络服务器域并登录到服务器到才能安装其他技术，如动态主机配置协议 (DHCP)。

在所有运行广告 DS 服务器除外部署的服务器上执行以下操作：

1.  中所述的过程的完成[配置所有服务器](#BKMK_configuringAll)。

2.  使用以下两个的过程中的说明进行操作以加入域和登录才能执行更多部署任务服务器的身份加入你服务器：

> [!NOTE]
> 若要执行此过程，方法是使用 Windows PowerShell，打开 PowerShell 和键入以下 cmdlet，然后按 ENTER。 您还必须替换域名你想要使用的名称。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 当提示你执行此操作时，键入用户名和密码有权计算机加入域帐户。 重启计算机，请键入以下命令，然后按 ENTER。
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>若要连接到域运行 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 的计算机

1.  在服务器管理器中，单击**本地服务器**。 在详细信息窗格中，单击**工作组**。 **系统属性**对话框中打开。

2.  在**系统属性**对话框中，单击**更改**。 **计算机名月域更改**对话框中打开。

3.  在**计算机名称**中**的成员**，单击**域**，然后键入你想要加入的域的名称。 例如，如果域名 corp.contoso.com，请键入**corp.contoso.com**。

4.  单击**确定**。 **Windows 安全**对话框中打开。

5.  在**计算机名月域更改**，请在**User name**，键入用户名、 在**密码**，键入的密码，然后单击**确定**。 **计算机名月域更改**对话框中，将打开欢迎你加入域。 单击**确定**。

6.  **计算机名月域更改**对话框中显示一条消息，指示你必须重新启动以应用更改计算机。 单击**确定**。

7.  在**系统属性**对话框中，在**计算机名称**选项卡上，单击**关闭**。 **Microsoft Windows**对话框中，随即将显示一条消息，再次表示你必须重新启动以应用更改计算机。 单击**立即重启**。

> [!NOTE]
> 有关如何加入域到其他 Microsoft 操作系统运行的计算机的信息，请参阅[附录 C-将计算机连接到域](#BKMK_C)。

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>若要使用运行 Windows Server 2016 的计算机在域登录

1.  注销计算机，或重启计算机。

2.  按 CTRL + ALT + 删除。 在登录屏幕出现。

3.  在左下角，单击**其他用户**。

4.  在**User name**，键入你的用户名。

5.  在**密码**，方法是键入域你的密码，然后单击箭头，或按 ENTER。

> [!NOTE]
> 有关如何使用其他 Microsoft 操作系统运行的计算机在域登录的信息，请参阅[附录 D-登录到域](#BKMK_D)。

#### <a name="BKMK_deployDHCP01"></a>部署 DHCP1
部署之前该组件的核心网络，你必须执行以下操作：

-   执行一节中的步骤[配置所有服务器](#BKMK_configuringAll)。

-   执行一节中的步骤[加入域和日志记录上的服务器计算机](#BKMK_joinlogserver)。

部署 DHCP1，是计算机运行的动态主机配置协议 (DHCP) 服务器角色，你必须完成按以下顺序这些步骤：

-   [安装动态主机配置协议 (DHCP)](#BKMK_installDHCP)

-   [创建和激活新 DHCP 范围](#BKMK_newscopeDHCP)

> [!NOTE]
> 若要使用 Windows PowerShell 执行这些过程，打开 PowerShell 和上单独的行中键入以下 cmdlet，然后按 ENTER。 你必须还替换范围名称、 IP 地址开始和结束波幅、 子网掩码和其他值在此示例中你想要使用的值。
>
> `Install-WindowsFeature DHCP -IncludeManagementTools`
>
> `Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2`
>
> `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com`

##### <a name="BKMK_installDHCP"></a>安装动态主机配置协议 (DHCP)
你可以使用此过程来安装和配置 DHCP 服务器角色使用添加角色和功能向导。

在会员**域管理员**，或等效的最低要求执行此过程。

###### <a name="to-install-dhcp"></a>若要安装 DHCP

1.  在 DHCP1，在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。

2.  在**开始之前**，单击**下一步**。

    > [!NOTE]
    > **开始之前**添加角色和功能向导中的页面未显示如果你之前已选择**默认情况下跳过此页**运行添加角色并功能向导。

3.  在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。

4.  在**选择目标服务器**，确保**从服务器池选择服务器**选择。 在**服务器池**，确保在本地计算机处于选中状态。 单击**下一步**。

5.  在**选择服务器角色**中**角色**、 选择**DHCP 服务器**。 在**添加所需的 DHCP 服务器的功能**，单击**添加功能**。 单击**下一步**。

6.  在**选择功能**，单击**下一步**，并在**DHCP 服务器**，检查信息，提供了，然后单击**下一步**。

7.  在**确认安装选择**，单击**必要时自动重新启动目标服务器**。 当提示你确认选择此选项时，请单击**是**，然后单击**安装**。 **安装进度**页面显示在安装过程中的状态。 该过程完成后，该消息"所需的配置。 在已成功安装*ComputerName*"显示时，其中*ComputerName*是你已安装在其 DHCP 服务器的计算机名称。 在消息窗口中，单击**完整 DHCP 配置**。 安装 DHCP 后配置向导将打开。 单击**下一步**。

8.  在**授权**，指定你想要使用 DHCP 服务器 Active Directory 域服务，在授权，然后单击的凭据**提交**。 完成授权后，单击**关闭**。

##### <a name="BKMK_newscopeDHCP"></a>创建和激活新 DHCP 范围
此过程用于创建新 DHCP 范围使用 DHCP Microsoft 管理控制台 (MMC)。 当你完成该过程后时，范围激活，并且你创建的排除项范围阻止 DHCP 服务器租赁用于配置为服务器静态 IP 地址和其他设备上的静态 IP 地址的需要。

在会员**DHCP 管理员**，或等效的最低要求执行此过程。

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>若要创建和激活新 DHCP 范围

1.  在 DHCP1，在服务器管理器中，单击**工具**，然后单击**DHCP**。 DHCP MMC 将打开。

2.  在**DHCP**，展开服务器的名称。 例如，如果 DHCP1.corp.contoso.com DHCP 服务器的名称，请单击的向下箭头旁边**DHCP1.corp.contoso.com**。

3.  水下世界服务器名称，请右键单击**IPv4**，然后单击**新范围**。 打开新的范围内向导。

4.  在**欢迎使用新的范围内向导**，单击**下一步**。

5.  在**范围名称**中**名称**，键入范围的名称。 例如，键入**子网 1**。

6.  在**描述**、 键入新的范围内的说明，然后单击**下一步**。

7.  在**IP 地址范围**，请执行以下操作：

    1.  在**开始 IP 地址**，键入 IP 地址的覆盖范围内的第一个 IP 地址。 例如，键入**10.0.0.1**。

    2.  在**结束 IP 地址**，键入 IP 地址的区域中的最后一个 IP 地址。 例如，键入**10.0.0.254**。 值**长度**和**子网掩码**将会自动输入，基于你输入的 IP 地址**开始 IP 地址**。

    3.  如有必要，修改中的值**长度**或**子网掩码**、 根据需要为你寻址的方案。

    4.  单击**下一步**。

8.  在**添加排除项**，请执行以下操作：

    1.  在**开始 IP 地址**，键入 IP 地址的排除项覆盖范围内的第一个 IP 地址。 例如，键入**10.0.0.1**。

    2.  在**结束 IP 地址**，键入 IP 地址的最后一个 IP 会地址中的排除项范围，例如，键入**10.0.0.15**。

9. 单击**添加**，然后单击**下一步**。

10. 在**租赁持续时间**，修改的默认值**天**，**小时**，并**分钟**，作为适用于你的网络，然后单击**下一步**。

11. 在**配置 DHCP 选项**、 选择**是，我想要立即配置这些选项**，然后单击**下一步**。

12. 在**路由器 （默认网关）**，执行下列操作之一：

    -   如果你没有在你的网络路由器，请单击**下一步**。

    -   在**IP 地址**，键入你的路由器的 IP 地址或默认网关。 例如，键入**10.0.0.1**。 单击**添加**，然后单击**下一步**。

13. 在**域名和 DNS 服务器**，请执行以下操作：

    1.  在**家长域**，键入 DNS 域的客户端解析名称使用的名称。 例如，键入**corp.contoso.com**。

    2.  在**服务器名称**，键入的客户端用于名称分辨率 DNS 计算机名称。 例如，键入**DC1**。

    3.  单击**解决**。 在添加 DNS 服务器的 IP 地址**IP 地址**。 单击**添加**、 DNS 服务器 IP 地址验证才能完成，请等待，然后单击**下一步**。

14. 在**WINS 服务器**，因为没有你的网络 WINS 服务器单击**下一步**。

15. 在**激活范围**、 选择**是，我想要立即激活此范围**。

16. 单击**下一步**，然后单击**完成**。

> [!IMPORTANT]
> 若要创建新范围的更多个子网时，重复上述步骤。 计划部署，并确保其他子网所导致的所有路由器上已启用 DHCP 邮件转发每个子网使用不同的 IP 地址范围。

### <a name="BKMK_joinlogclients"></a>加入域的客户端计算机并登录

> [!NOTE]
> 若要执行此过程，方法是使用 Windows PowerShell，打开 PowerShell 和键入以下 cmdlet，然后按 ENTER。 您还必须替换域名你想要使用的名称。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 当提示你执行此操作时，键入用户名和密码有权计算机加入域帐户。 重启计算机，请键入以下命令，然后按 ENTER。
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>若要连接到域运行 Windows 10 的计算机

1.  登录到计算机的本地管理员帐户。

2.  在**搜索 web 和 Windows**，类型**系统**。 在搜索结果中单击**系统 （控制面板）**。 **系统**对话框中打开。

3.  在**系统**，单击**高级系统设置**。 **系统属性**对话框中打开。 单击**计算机名称**选项卡。

4.  在**计算机名称**，单击**更改**。 **计算机名月域更改**对话框中打开。

5.  在**计算机名月域更改**中**的成员**，单击**域**，然后键入你想要加入的域的名称。 例如，如果域名 corp.contoso.com，请键入**corp.contoso.com**。

6.  单击**确定**。 **Windows 安全**对话框中打开。

7.  在**计算机名月域更改**，请在**User name**，键入用户名、 在**密码**，键入的密码，然后单击**确定**。 **计算机名月域更改**对话框中，将打开欢迎你加入域。 单击**确定**。

8.  **计算机名月域更改**对话框中显示一条消息，指示你必须重新启动以应用更改计算机。 单击**确定**。

9. 在**系统属性**对话框中，在**计算机名称**选项卡上，单击**关闭**。 **Microsoft Windows**对话框中，随即将显示一条消息，再次表示你必须重新启动以应用更改计算机。 单击**立即重启**。

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>若要连接到域运行 Windows 8.1 计算机

1.  登录到计算机的本地管理员帐户。

2.  右键单击**开始**，然后单击**系统**。 **系统**对话框中打开。

3.  在**系统**，单击**高级系统设置**。 **系统属性**对话框中打开。 单击**计算机名称**选项卡。

4.  在**计算机名称**，单击**更改**。 **计算机名月域更改**对话框中打开。

5.  在**计算机名月域更改**中**的成员**，单击**域**，然后键入你想要加入的域的名称。 例如，如果域名 corp.contoso.com，请键入**corp.contoso.com**。

6.  单击**确定**。 **Windows 安全**对话框中打开。

7.  在**计算机名月域更改**，请在**User name**，键入用户名、 在**密码**，键入的密码，然后单击**确定**。 **计算机名月域更改**对话框中，将打开欢迎你加入域。 单击**确定**。

8.  **计算机名月域更改**对话框中显示一条消息，指示你必须重新启动以应用更改计算机。 单击**确定**。

9. 在**系统属性**对话框中，在**计算机名称**选项卡上，单击**关闭**。 **Microsoft Windows**对话框中，随即将显示一条消息，再次表示你必须重新启动以应用更改计算机。 单击**立即重启**。

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>若要使用运行 Windows 10 的计算机在域登录

1.  注销计算机，或重启计算机。

2.  按 CTRL + ALT + 删除。 在登录屏幕出现。

3.  在左下角，单击**其他用户**。

4.  在**User name**，键入你域和用户的名称格式*域名 \*。 例如，在登录的帐户名为域 corp.contoso.com**用户 01**，类型**CORP\User 01**。

5.  在**密码**，方法是键入域你的密码，然后单击箭头，或按 ENTER。

### <a name="BKMK_optionalfeatures"></a>部署可选功能网络的访问权限身份验证和 Web 服务
如果你想要部署如无线接入点或 VPN 服务器的网络访问权限服务器安装核心网络后，建议你部署 NPS 服务器和 Web 服务器。 对于网络的访问权限部署，建议使用安全证书基于身份验证方法。 你可以使用 NPS 来管理网络的访问权限的策略和部署安全身份验证方法。 你可以使用的 Web 服务器发布你证书颁发机构） 提供的安全的身份验证的证书证书吊销列表 (CRL)。

> [!NOTE]
> 你可以通过使用 Core 网络助手指南部署服务器证书以及其他其他功能。 有关详细信息，请参阅[其他技术资源](#BKMK_resources)。

下图显示的 Windows Server 核心网络拓扑添加 NPS 和 Web 服务器。

![添加 NPS 和 Web 服务器对 Windows Server 核心网络拓扑](../media/Core-Network-Guide/cng16_overview_2.jpg)

以下各部分将 NPS 和 Web 服务器添加到你的网络提供的信息。

-   [部署 NPS1](#BKMK_deployNPS1)

-   [部署 WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>部署 NPS1
作为准备用于部署其他网络的访问权限技术，如虚拟专用网络 (VPN) 服务器、 无线接入点和 802.1 X 身份验证交换机步骤安装网络策略 Server (NPS) 服务器。

网络策略 Server (NPS) 允许你集中配置和管理网络策略使用以下功能： 远程身份验证拨用户服务 (RADIUS) 服务器和 RADIUS 代理。

NPS 是核心网络上的可选组件，但你应安装 NPS 以下任一，则：

-   你准备以展开包括远程访问服务器 RADIUS 协议，如一台运行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 和路由和远程访问服务、 终端服务网关或远程桌面网关计算机与兼容的网络。


-   你打算部署 802.1 X 身份验证有线或无线访问。

部署此角色服务之前, 你必须为 NPS 服务器配置你的计算机上执行以下步骤。

-   执行一节中的步骤[配置所有服务器](#BKMK_configuringAll)。

-   执行一节中的步骤[加入域和日志记录上的服务器计算机](#BKMK_joinlogserver)

部署 NPS1，是计算机运行的网络策略和服务的访问权限的服务器角色网络策略 Server (NPS) 角色服务，您必须完成此步骤：

-   [规划 NPS1 部署](#bkmk_NetFndtn_Pln_NPS-01)

-   [安装网络策略服务器 (NPS)](#BKMK_installNPS)

-   [默认域中注册 NPS 服务器](#BKMK_registerNPS)

> [!NOTE]
> 本指南提供了用于在服务器上独立部署 NPS 说明，或 VM 命名 NPS1。  另一个推荐的部署模型是 NPS 域控制器上的安装。 如果您愿意，而不是独立服务器上的域控制器上安装 NPS，则 DC1 上安装 NPS。

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>规划 NPS1 部署
如果你想要部署如无线接入点或 VPN 服务器的网络访问权限服务器部署核心网络后，建议你部署 NPS。

当你使用 NPS 作为远程身份验证拨用户服务 (RADIUS) 服务器时，NPS 执行身份验证和授权连接通过你的网络访问权限服务器的请求。 NPS 还允许你集中配置和管理网络策略的确定谁可以访问该网络，如何访问该网络，当他们都可以访问该网络。

以下是安装 NPS 之前的关键规划的步骤。

- 计划的用户帐户的数据库。 默认情况下，如果你加入运行的服务器 NPS 到 Active Directory 域，NPS 将执行身份验证和使用广告 DS 用户帐户数据库授权。 在某些情况下，如用作 NPS RADIUS 代理转发给其他 RADIUS 服务器，请连接请求的大型网络与您可能想要非域成员计算机上安装 NPS。

- 套餐 RADIUS 记帐。 NPS 允许你为 SQL Server 数据库或文本文件本地计算机上记录记帐数据。 如果你想要使用 SQL Server 日志记录，计划安装和运行 SQL Server 服务器的配置。

##### <a name="BKMK_installNPS"></a>安装网络策略服务器 (NPS)
你可以使用此步骤使用添加角色和功能向导安装网络策略 Server (NPS)。 NPS 是网络策略和服务的访问权限服务器角色的角色服务。

> [!NOTE]
> 默认情况下，NPS RADIUS 流量端口 1812年 1813年、 1645年，1646年所有已安装的网络适配器侦听。 如果高级安全 Windows 防火墙处于启用状态，当你安装 NPS，在安装过程中的 Internet 协议版本 6 \(IPv6\) 和 IPv4 交通自动创建防火墙例外，以便这些端口。 如果你的网络访问权限服务器配置为通过以外这些默认值端口发送 RADIUS 通信，删除 NPS 安装期间创建高级安全 Windows 防火墙在例外，并创建例外，请使用 RADIUS 交通端口。

**管理凭据**

若要完成此过程，你必须**域管理员**组。

> [!NOTE]
> 要执行此过程，方法是使用 Windows PowerShell，打开 PowerShell 和键入以下命令，然后按 ENTER。
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>若要安装 NPS

1.  在 NPS1，在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。

2.  在**开始之前**，单击**下一步**。

    > [!NOTE]
    > **开始之前**添加角色和功能向导中的页面未显示如果你之前已选择**默认情况下跳过此页**运行添加角色并功能向导。

3.  在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。

4.  在**选择目标服务器**，确保**从服务器池选择服务器**选择。 在**服务器池**，确保在本地计算机处于选中状态。 单击**下一步**。

5.  在**选择服务器角色**中**角色**、 选择**网络策略和访问服务**。 对话框中打开要求将应添加所需的网络策略和服务的访问权限的功能。 单击**添加功能**，然后单击**下一步**。

6.  在**选择功能**，单击**下一步**，并在**网络策略和访问服务**，检查信息，提供了，然后单击**下一步**。

7.  在**选择角色服务**，单击**网络策略服务器**。  在**添加所需的网络策略服务器的功能**，单击**添加功能**。 单击**下一步**。

8.  在**确认安装选择**，单击**必要时自动重新启动目标服务器**。 当提示你确认选择此选项时，请单击**是**，然后单击**安装**。 安装进度页面显示在安装过程中的状态。 该过程完成后，该消息"上的已成功安装*ComputerName*"显示时，其中*ComputerName*是你已安装在其网络策略服务器计算机的名称。 单击**关闭**。

##### <a name="BKMK_registerNPS"></a>默认域中注册 NPS 服务器
你可以使用此过程在其中服务器域加入的域中注册 NPS 服务器。

在 Active Directory 必须注册 NPS 服务器，以便并且有权授权过程阅读拨号中的用户帐户的属性。 注册 NPS 服务器添加到服务器**RAS 和 IAS 服务器**组中的 Active Directory。

**管理凭据**

若要完成此过程，你必须**域管理员**组。

> [!NOTE]
> 要使用网络 shell (Netsh) 命令 Windows PowerShell 内的执行此过程，打开 PowerShell 和键入以下命令，然后按 ENTER。
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-server-in-its-default-domain"></a>若要在其默认域中注册 NPS 服务器

1.  在 NPS1，在服务器管理器中，单击工具，然后单击**网络策略服务器**。 网络策略服务器 MMC 将打开。

2.  右键单击**NPS （本地）**，然后单击**Active Directory 中的寄存器服务器**。 **网络策略服务器**对话框中打开。

3.  在**网络策略服务器**，单击**确定**，然后单击**确定**再次。

有关网络策略服务器的详细信息，请参阅[网络策略 Server (NPS)](../technologies/nps/nps-top.md)。

#### <a name="BKMK_IIS"></a>部署 WEB1

Windows Server 2016 中的 Web 服务器 (IIS) 角色提供可靠举办的网站、 服务和应用程序的安全、 易于管理、 模块和可扩展平台。 使用 Internet 信息服务 (IIS)，你可以共享与用户 Internet、 intranet，或外部网络上的信息。 IIS 是集成 IIS、 ASP.NET、 FTP 服务、 PHP，以及 Windows 通信基础 (WCF) 统一的 web 平台。

这允许你为发布 CRL 访问由域成员计算机，除了 Web 服务器 (IIS) 服务器角色允许你设置并管理多个网站、 web 应用程序和 FTP 站点。 IIS 还提供以下优势：

-   最大限度地减少了的服务器英尺打印和自动应用程序隔离通过 web 安全。

-   轻松部署和运行的 ASP.NET、 经典 ASP 和 PHP web 相同的服务器上的应用。

-   通过提供工作进程唯一标识以及沙盒配置默认情况下，进一步降低安全风险实现应用程序隔离。

-   很容易地添加、 删除和甚至替换为内置 IIS 组件自定义模块、 适用于客户的需求。

-   加快你通过内置动态缓存和增强的压缩的网站。

部署 WEB1，是计算机上运行的 Web 服务器 (IIS) 服务器角色，必须执行以下操作：

-   执行一节中的步骤[配置所有服务器](#BKMK_configuringAll)。

-   执行一节中的步骤[加入域和日志记录上的服务器计算机](#BKMK_joinlogserver)

-   [安装 Web 服务器 (IIS) 服务器角色](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>安装 Web 服务器 (IIS) 服务器角色
若要完成此过程，你必须**管理员**组。

> [!NOTE]
> 要执行此过程，方法是使用 Windows PowerShell，打开 PowerShell 和键入以下命令，然后按 ENTER。
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  在**服务器管理器**，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。

2.  在**开始之前**，单击**下一步**。

    > [!NOTE]
    > **开始之前**添加角色和功能向导中的页面未显示如果你之前已选择**默认情况下跳过此页**运行添加角色并功能向导。

3.  在**选择安装类型**页上，单击**下一步**。

4.  在**选择目标服务器**页上，确保在本地计算机选择，然后单击**下一步**。

5.  在**选择服务器角色**页上，滚动到，请选择**Web 服务器 (IIS)**。 **添加所需的 Web 服务器 (IIS) 功能**对话框中打开。 单击**添加功能**，然后单击**下一步**。

6.  单击**下一步**直到接受你的所有默认 web 服务器设置，然后单击**安装**。

7.  检查所有安装都已成功后，然后单击**关闭**。

## <a name="BKMK_resources"></a>其他技术资源
本指南中的技术详细信息，请参阅下面的资源：

 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 技术库资源

-   [什么是 Windows Server 2016 中 Active Directory 域服务 (广告 DS) 中的新增功能](https://technet.microsoft.com/en-us/library/mt163897.aspx)

-   [Active Directory 域名服务概述](https://technet.microsoft.com/library/hh831484.aspx)在https://technet.microsoft.com/library/hh831484.aspx。

-   [域名系统 (DNS) 概述](https://technet.microsoft.com/library/hh831667.aspx)在https://technet.microsoft.com/library/hh831667.aspx。

-   [实现 DNS 管理员角色](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [动态主机配置协议 (DHCP) 概述](https://technet.microsoft.com/library/hh831825.aspx)在https://technet.microsoft.com/library/hh831825.aspx。

-   [网络策略和访问服务概述](https://technet.microsoft.com/library/hh831683.aspx)在https://technet.microsoft.com/library/hh831683.aspx。

-   [Web 服务器 (IIS) 概述](https://technet.microsoft.com/library/hh831725.aspx)在https://technet.microsoft.com/library/hh831725.aspx。

## <a name="BKMK_appendix"></a>通过 E 附录 A
以下部分包含其他配置的计算机的运行 Windows Server 2016、 Windows 10、 Windows Server 2012 和 Windows 8 以外的操作系统的信息。 此外，网络准备工作表可用于帮助你进行部署。

1.  [附录 A-重命名计算机](#BKMK_A)

2.  [附录 B-配置静态 IP 地址](#BKMK_B)

3.  [附录 C-将计算机连接到域](#BKMK_C)

4.  [附录 D-登录到域](#BKMK_D)

5.  [附录 E-规划准备工作表的核心网络](#BKMK_E)

## <a name="BKMK_A"></a>附录 A-重命名计算机
你可以使用过程在此部分中，以使具有其他计算机名称运行 Windows Server 2008 R2、 Windows 7、 Windows Server 2008 和 Windows Vista 的计算机。

-   [Windows Server 2008 R2 和 Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 和 Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 和 Windows 7
在会员**管理员**，或等效的最低要求执行这些过程。

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>若要重命名运行 Windows Server 2008 R2 和 Windows 7 的计算机

1.  单击**开始**，右键单击**计算机**，然后单击**属性**。 **系统**对话框中打开。

2.  在**计算机名称、 域和工作组设置**，单击**更改设置**。 **系统属性**对话框中打开。

    > [!NOTE]
    > 在计算机上运行 Windows 7 中，之前**系统属性**对话框打开时，**用户帐户控制**对话框中，将打开请求权限才能继续。 单击**继续**以继续。

3.  单击**更改**。 **计算机名月域更改**对话框中打开。

4.  在**计算机名称**，键入你的计算机的名称。 例如，如果你想要命名计算机 DC1，键入**DC1**。

5.  单击**确定**两次，请单击**关闭**，然后单击**立即重新启动**重启计算机。

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 和 Windows Vista
在会员**管理员**，或等效的最低要求执行这些过程。

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>若要重命名运行 Windows Server 2008 和 Windows Vista 的计算机

1.  单击**开始**，右键单击**计算机**，然后单击**属性**。 **系统**对话框中打开。

2.  在**计算机名称、 域和工作组设置**，单击**更改设置**。 **系统属性**对话框中打开。

    > [!NOTE]
    > 在计算机上运行 Windows Vista 中，之前**系统属性**对话框打开时，**用户帐户控制**对话框中，将打开请求权限才能继续。 单击**继续**以继续。

3.  单击**更改**。 **计算机名月域更改**对话框中打开。

4.  在**计算机名称**，键入你的计算机的名称。 例如，如果你想要命名计算机 DC1，键入**DC1**。

5.  单击**确定**两次，请单击**关闭**，然后单击**立即重新启动**重启计算机。

## <a name="BKMK_B"></a>附录 B-配置静态 IP 地址
本主题提供过程配置计算机运行以下操作系统上的静态 IP 地址：

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
在会员**管理员**，或等效的最低要求执行此过程。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>若要配置运行 Windows Server 2008 R2 的计算机上的静态 IP 地址

1.  单击**开始**，然后单击**控制面板**。

2.  在**控制面板**，单击**网络和 Internet**。 **网络和 Internet**打开。

    在**网络和 Internet**，单击**网络和共享中心**。 **网络和共享中心**打开。

3.  在**网络和共享中心**，单击**更改适配器设置**。 **网络连接**打开。

4.  在**网络连接**，右键单击你想要配置，然后单击网络连接**属性**。

5.  在**本地连接属性**中**此连接使用下列项目**、 选择**Internet 协议版本 4 (TCP/IPv4)**，然后单击**属性**。 **Internet 协议版本 4 (TCP/IPv4) 属性**对话框中打开。

6.  在**Internet 协议版本 4 (TCP/IPv4) 属性**，在**常规**选项卡上，单击**使用以下 IP 地址**。 在**IP 地址**，键入你想要使用的 IP 地址。

7.  按 tab 可以将光标放在**子网掩码**。 自动输入子网掩码的默认值。 接受默认子网掩码，或键入你想要使用的子网掩码。

8.  在**默认网关**，键入你的默认网关的 IP 地址。

9. 在**首选 DNS 服务器**，键入 IP 地址 DNS 服务器。 如果您计划使用本地计算机作为首选 DNS 服务器，请键入本地计算机的 IP 地址。

10. 在**备用 DNS 服务器**，键入备用 DNS 服务器的 IP 地址 （如果有。 如果你计划备用 DNS 服务器为使用本地计算机中，键入本地计算机的 IP 地址。

11. 单击**确定**，然后单击**关闭**。

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
在会员**管理员**，或等效的最低要求执行这些过程。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>若要配置运行 Windows Server 2008 计算机上的静态 IP 地址

1.  单击**开始**，然后单击**控制面板**。

2.  在**控制面板**，确认**经典视图**选中，则，然后双击**网络和共享中心**。

3.  在**网络和共享中心**中**任务**，单击**管理网络连接**。

4.  在**网络连接**，右键单击你想要配置，然后单击网络连接**属性**。

5.  在**本地连接属性**中**此连接使用下列项目**、 选择**Internet 协议版本 4 (TCP/IPv4)**，然后单击**属性**。 **Internet 协议版本 4 (TCP/IPv4) 属性**对话框中打开。

6.  在**Internet 协议版本 4 (TCP/IPv4) 属性**，在**常规**选项卡上，单击**使用以下 IP 地址**。 在**IP 地址**，键入你想要使用的 IP 地址。

7.  按 tab 可以将光标放在**子网掩码**。 自动输入子网掩码的默认值。 接受默认子网掩码，或键入你想要使用的子网掩码。

8.  在**默认网关**，键入你的默认网关的 IP 地址。

9. 在**首选 DNS 服务器**，键入 IP 地址 DNS 服务器。 如果您计划使用本地计算机作为首选 DNS 服务器，请键入本地计算机的 IP 地址。

10. 在**备用 DNS 服务器**，键入备用 DNS 服务器的 IP 地址 （如果有。 如果你计划备用 DNS 服务器为使用本地计算机中，键入本地计算机的 IP 地址。

11. 单击**确定**，然后单击**关闭**。

## <a name="BKMK_C"></a>附录 C-将计算机连接到域
你可以使用这些过程加入域到运行 Windows Server 2008 R2、 Windows 7、 Windows Server 2008 和 Windows Vista 的计算机。

-   [Windows Server 2008 R2 和 Windows 7](#BKMK_c1)

-   [Windows Server 2008 和 Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> 计算机加入到某个域中，您必须登录到计算机的本地管理员帐户或者，如果你登录到计算机中没有本地计算机管理凭据的用户帐户，必须计算机加入域的过程的本地管理员帐户提供的凭据。 此外，你必须用户帐户在你需要将计算机加入的域。 在计算机加入域的过程中，系统将提示你对于域帐户凭据 （用户名和密码）。

### <a name="BKMK_c1"></a>Windows Server 2008 R2 和 Windows 7
在会员**域用户**，或等效的最低要求执行此过程。

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>若要连接到域运行 Windows Server 2008 R2 和 Windows 7 的计算机

1.  登录到计算机的本地管理员帐户。

2.  单击**开始**，右键单击**计算机**，然后单击**属性**。 **系统**对话框中打开。

3.  在**计算机名称、 域和工作组设置**，单击**更改设置**。 **系统属性**对话框中打开。

    > [!NOTE]
    > 在计算机上运行 Windows 7 中，之前**系统属性**对话框打开时，**用户帐户控制**对话框中，将打开请求权限才能继续。 单击**继续**以继续。

4.  单击**更改**。 **计算机名月域更改**对话框中打开。

5.  在**计算机名称**，请在**的成员**、 选择**域**，然后键入你想要加入的域的名称。 例如，如果域名 corp.contoso.com，请键入**corp.contoso.com**。

6.  单击**确定**。 **Windows 安全**对话框中打开。

7.  在**计算机名月域更改**，请在**User name**，键入用户名、 在**密码**，键入的密码，然后单击**确定**。 **计算机名月域更改**对话框中，将打开欢迎你加入域。 单击**确定**。

8.  **计算机名月域更改**对话框中显示一条消息，指示你必须重新启动以应用更改计算机。 单击**确定**。

9. 在**系统属性**对话框中，在**计算机名称**选项卡上，单击**关闭**。 **Microsoft Windows**对话框中，随即将显示一条消息，再次表示你必须重新启动以应用更改计算机。 单击**立即重启**。

### <a name="BKMK_c2"></a>Windows Server 2008 和 Windows Vista
在会员**域用户**，或等效的最低要求执行此过程。

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>若要连接到域运行 Windows Server 2008 和 Windows Vista 的计算机

1.  登录到计算机的本地管理员帐户。

2.  单击**开始**，右键单击**计算机**，然后单击**属性**。 **系统**对话框中打开。

3.  在**计算机名称、 域和工作组设置**，单击**更改设置**。 **系统属性**对话框中打开。

4.  单击**更改**。 **计算机名月域更改**对话框中打开。

5.  在**计算机名称**，请在**的成员**、 选择**域**，然后键入你想要加入的域的名称。 例如，如果域名 corp.contoso.com，请键入**corp.contoso.com**。

6.  单击**确定**。 **Windows 安全**对话框中打开。

7.  在**计算机名月域更改**，请在**User name**，键入用户名、 在**密码**，键入的密码，然后单击**确定**。 **计算机名月域更改**对话框中，将打开欢迎你加入域。 单击**确定**。

8.  **计算机名月域更改**对话框中显示一条消息，指示你必须重新启动以应用更改计算机。 单击**确定**。

9. 在**系统属性**对话框中，在**计算机名称**选项卡上，单击**关闭**。 **Microsoft Windows**对话框中，随即将显示一条消息，再次表示你必须重新启动以应用更改计算机。 单击**立即重启**。

## <a name="BKMK_D"></a>附录 D-登录到域
这些步骤可用于登录到使用运行 Windows Server 2008 R2、 Windows 7、 Windows Server 2008 和 Windows Vista 的计算机的域。

-   [Windows Server 2008 R2 和 Windows 7](#BKMK_d1)

-   [Windows Server 2008 和 Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 和 Windows 7
在会员**域用户**，或等效的最低要求执行此过程。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>使用运行 Windows Server 2008 R2 和 Windows 7 的计算机在域登录

1.  注销计算机，或重启计算机。

2.  按 CTRL + ALT + 删除。 在登录屏幕出现。

3.  单击**切换用户**，然后单击**其他用户**。

4.  在**User name**，键入你域和用户的名称格式*域名 \*。 例如，在登录的帐户名为域 corp.contoso.com**用户 01**，类型**CORP\User 01**。

5.  在**密码**，方法是键入域你的密码，然后单击箭头，或按 ENTER。

### <a name="BKMK_d2"></a>Windows Server 2008 和 Windows Vista
在会员**域用户**，或等效的最低要求执行此过程。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>使用运行 Windows Server 2008 和 Windows Vista 的计算机在域登录

1.  注销计算机，或重启计算机。

2.  按 CTRL + ALT + 删除。 在登录屏幕出现。

3.  单击**切换用户**，然后单击**其他用户**。

4.  在**User name**，键入你域和用户的名称格式*域名 \*。 例如，在登录的帐户名为域 corp.contoso.com**用户 01**，类型**CORP\User 01**。

5.  在**密码**，方法是键入域你的密码，然后单击箭头，或按 ENTER。

## <a name="BKMK_E"></a>附录 E-规划准备工作表的核心网络
你可以使用此网络规划准备工作表收集安装核心网络所需的信息。 本主题提供表，其中包含为其你必须提供的信息或特定值安装或配置过程每个服务器计算机个别配置项目。 为每个配置项提供了示例值。

用于规划和跟踪，你输入用于部署的值为每个表格中提供了空间。 如果你在这些表格记录安全相关的值，你应在安全位置存储的信息。

下面的链接导致提供配置项和示例值本指南中显示的部署过程与相关联的本主题中的各部分。

1.  [安装 Active Directory 域服务和 DNS](#BKMK_FndtnPrep_InstallAD)

    -   [配置 DNS 反向查找区域](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [安装 DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [创建 DHCP 中的排除项范围](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [创建新的 DHCP 范围](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [安装网络策略服务器 （可选）](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>安装 Active Directory 域服务和 DNS
在此部分中的表列出了配置项预安装和安装的 Active Directory 域服务 (广告 DS) 和 DNS。

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>预安装的配置广告 DS 和 DNS 项目
下面的表中所述列表预安装的配置项目[配置所有服务器](#BKMK_configuringAll):

-   [配置的静态 IP 地址](#BKMK_ip)

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|IP 地址|10.0.0.2||
|子网掩码|255.255.255.0||
|默认网关|10.0.0.1||
|首选的 DNS 服务器|127.0.0.1||
|备用 DNS 服务器|10.0.0.15||

-   [将计算机重命名](#BKMK_rename)

|配置项目|示例值|值|
|----------------------|-----------------|---------|
|计算机名称|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>广告 DS 和 DNS 安装配置项目
有关 Windows Server 核心网络部署过程配置项[安装广告 DS 和 DNS 为新的林](#BKMK_installAD-DNS):

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|DNS 全名|corp.contoso.com||
|森林功能级别|Windows Server 2003||
|Active Directory 域服务数据库文件夹位置|E:\Configuration\\<br /><br />或接受默认位置。||
|Active Directory 域服务日志文件的文件夹位置|E:\Configuration\\<br /><br />或接受默认位置。||
|Active Directory 域服务 SYSVOL 文件夹位置|E:\Configuration\\<br /><br />或接受默认位置||
|目录恢复模式的管理员密码|J * p2leO4$ F||
|Answer 文件名称 （可选）|广告 DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>配置 DNS 反向查找区域

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|区域类型：|-主要区域<br />-辅助区域<br />-桩模块区域||
|区域类型<br /><br />**应用商店中 Active Directory 的区域**|选择<br />-未选择||
|Active Directory 区域复制范围|这片森林中的所有 DNS 服务器访问<br />-为所有 DNS 服务器此域中<br />访问此域中的所有域控制器<br />-到此目录分区范围内指定的所有域控制器||
|反向查找区域名称<br /><br />（IP 类型）|-IPv4 反向查找区域<br />IPv6 反向查找区域||
|反向查找区域名称<br /><br />(网络 ID)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>安装 DHCP
在此部分中的表列出了预安装和 DHCP 安装配置项目。

##### <a name="pre-installation-configuration-items-for-dhcp"></a>预安装的配置 DHCP 项目
下面的表中所述列表预安装的配置项目[配置所有服务器](#BKMK_configuringAll):

-   [配置的静态 IP 地址](#BKMK_ip)

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|IP 地址|10.0.0.3||
|子网掩码|255.255.255.0||
|默认网关|10.0.0.1||
|首选的 DNS 服务器|10.0.0.2||
|备用 DNS 服务器|10.0.0.15||

-   [将计算机重命名](#BKMK_rename)

|配置项目|示例值|值|
|----------------------|-----------------|---------|
|计算机名称|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>DHCP 安装配置项目
有关 Windows Server 核心网络部署过程配置项[安装动态主机配置协议 (DHCP)](#BKMK_installDHCP):

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|网络连接绑定|以太网||
|DNS 服务器设置|DC1||
|首选的 DNS 服务器 IP 地址|10.0.0.2||
|备用 DNS 服务器的地址|10.0.0.15||
|范围名称|Corp1||
|开始 IP 地址|10.0.0.1||
|结束 IP 地址|10.0.0.254||
|子网掩码|255.255.255.0||
|默认网关 （可选）|10.0.0.1||
|租赁持续时间|8 天||
|IPv6 DHCP 服务器操作模式|未启用||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>创建 DHCP 中的排除项范围
配置项 DHCP 中创建的范围内时创建排除范围。

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|范围名称|Corp1||
|范围描述|主要 office 子网 1||
|排除项范围开始 IP 地址|10.0.0.1||
|排除项范围结束 IP 地址|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>创建新的 DHCP 范围
有关 Windows Server 核心网络部署过程配置项[创建和激活新 DHCP 范围](#BKMK_newscopeDHCP):

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|新的范围内名称|Corp2||
|范围描述|主要 office 子网 2||
|（IP 地址范围内）<br /><br />开始 IP 地址|10.0.1.1||
|（IP 地址范围内）<br /><br />结束 IP 地址|10.0.1.254||
|长度|8||
|子网掩码|255.255.255.0||
|（排除范围内）开始 IP 地址|10.0.1.1||
|排除项范围结束 IP 地址|10.0.1.15||
|租赁持续时间<br /><br />天<br /><br />小时<br /><br />分钟|-   8<br />-   0<br />-   0||
|路由器 （默认网关）<br /><br />IP 地址|10.0.1.1||
|家长的 DNS 域|corp.contoso.com||
|DNS 服务器<br /><br />IP 地址|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>安装网络策略服务器 （可选）
在此部分中的表列出了预安装和 NPS 安装配置项目。

##### <a name="pre-installation-configuration-items"></a>预安装的配置项目
下面的三个表格列表预安装的配置项目中所述[配置所有服务器](#BKMK_configuringAll):

-   [配置的静态 IP 地址](#BKMK_ip)

|配置项目|示例值|值|
|-----------------------|------------------|----------|
|IP 地址|10.0.0.4||
|子网掩码|255.255.255.0||
|默认网关|10.0.0.1||
|首选的 DNS 服务器|10.0.0.2||
|备用 DNS 服务器|10.0.0.15||

-   [将计算机重命名](#BKMK_rename)

|配置项目|示例值|值|
|----------------------|-----------------|---------|
|计算机名称|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>网络策略服务器安装配置项目
有关 Windows Server 核心网络 NPS 部署过程配置项[安装网络策略 Server (NPS)](#BKMK_installNPS)和[默认域中注册 NPS 服务器](#BKMK_registerNPS)。

-   没有其他配置项目需要安装注册 NPS。

