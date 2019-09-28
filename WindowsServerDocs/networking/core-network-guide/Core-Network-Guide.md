---
title: 核心网络组件
description: 本指南提供了有关如何在 Windows Server 2016 的新林中规划和部署完全正常运行的网络和新的 Active Directory 域所需的核心组件的说明
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cae789974c3f2b4f83c9120558d77dbe27f4190a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356410"
---
# <a name="core-network-components"></a>核心网络组件

>适用于：Windows Server（半年频道）、Windows Server 2016

本指南提供了有关如何在新林中规划和部署完全正常运行的网络和新的 Active Directory 域所需的核心组件的说明。

> [!NOTE]
> 本指南可从 TechNet 库下载 Microsoft Word 格式。 有关详细信息，请参阅[Windows Server 2016 的核心网络指南](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

本指南包含下列各节。

- [关于本指南](#BKMK_about)

- [核心网络概述](#BKMK_overview)

- [核心网络规划](#BKMK_planning)

- [核心网络部署](#BKMK_deployment)

- [其他技术资源](#BKMK_resources)

- [附录 A 到 E](#BKMK_appendix)

## <a name="BKMK_about"></a>关于本指南
本指南供以下人员使用：要安装新网络或希望创建基于域的网络以替换由工作组组成的网络的网络管理员和系统管理员。 如果你预计以后需要向网络中添加更多服务和功能，则本指南中提供的部署方案会非常有用。

建议你阅读与本部署方案中所使用的每种技术相对应的设计和部署指南，以帮助你确定本指南中是否提供了你所需的服务和配置。

核心网络是一个为满足组织的信息技术 (IT) 需要提供基础服务的网络硬件、设备和软件的集合。

Windows Server 核心网络提供了包括以下各项在内的多个优点。

-   用于计算机和其他与传输控制协议/Internet 协议 (TCP/IP) 兼容设备之间进行网络连接的核心协议。 TCP/IP 是一套用于连接计算机和构建网络的标准协议。 TCP/IP 是随 Microsoft Windows 操作系统一起提供的网络协议软件，可实现和支持 TCP/IP 协议套件。

-   动态主机配置协议 (DHCP) 对计算机和其他配置为 DHCP 客户端的设备进行自动 IP 地址分配。 与使用 DHCP 服务器动态地为计算机和其他设备提供 IP 地址配置相比，在网络上的所有计算机上手动配置 IP 地址非常耗时且不太灵活。

-   域名系统 (DNS) 名称解析服务。 DNS 允许用户、计算机、应用程序和服务使用计算机或设备的完全限定的域名在网络上查找计算机和设备的 IP 地址。

-   林，是共享相同类和属性定义（架构）、站点和复制信息（配置）以及全林性搜索功能（全局编录）的一个或多个 Active Directory 域。

-   目录林根级域，它是在新林中创建的第一个域。 Enterprise Admins 组和 Schema Admins 组是位于目录林根级域中的全林性管理组。 此外，目录林根级域也与其他域一样，是在 Active Directory 域服务 (AD DS) 中由管理员定义的计算机、用户和组对象的集合。 这些对象共享公用目录数据库和安全策略。 如果你随着组织的发展添加了域，它们还可以共享与其他域之间的安全关系。 目录服务还存储目录数据，并允许已授权的计算机、应用程序和用户访问数据。

-   用户和计算机帐户数据库。 目录服务提供了集中的用户帐户数据库，它允许你为被授权连接到网络并访问网络资源（例如应用程序、数据库、共享的文件和文件夹以及打印机）的人员和计算机创建用户和计算机帐户。

核心网络还允许随着组织的发展和 IT 需要的变化缩放网络。 例如，通过核心网络，可以添加域、IP 子网、远程访问服务、无线服务以及 Windows Server 2016 提供的其他功能和服务器角色。

### <a name="network-hardware-requirements"></a>网络硬件要求
若要成功部署核心网络，必须部署网络硬件，包括以下各项：

-   以太网、快速以太网或千兆以太网电缆

-   集线器、第 2 层交换机或第 3 层交换机、路由器或在计算机和设备之间起中继网络通信作用的其他设备。

-   符合其各自的客户端和服务器操作系统最低硬件要求的计算机。

## <a name="what-this-guide-does-not-provide"></a>本指南未提供的内容
本指南未提供部署以下各项的说明：

-   网络硬件，例如电缆、路由器、交换机和集线器

-   其他网络资源，例如打印机和文件服务器

-   Internet 连接。

-   远程访问

-   无线访问

-   客户端计算机部署

> [!NOTE]
> 默认情况下，运行 Windows 客户端操作系统的计算机配置为从 DHCP 服务器接收 IP 地址租约。 因此，不需要对客户端计算机进行其他的 DHCP 或 Internet 协议版本 4 (IPv4) 配置。

## <a name="technology-overviews"></a>技术概述
以下部分简要概述了创建核心网络所必需部署的技术。

### <a name="active-directory-domain-services"></a>Active Directory 域服务
目录是在网络上存储有关对象（如用户和计算机）信息的分层结构。 目录服务（如 AD DS）提供了存储目录数据以及使此数据可供网络用户和管理员使用的方法。 例如，AD DS 存储有关用户帐户的信息，包括名称、电子邮件地址、密码和电话号码，并允许同一网络上的其他授权用户访问此信息。

### <a name="dns"></a>DNS
DNS 是一个 TCP/IP 网络（例如 Internet 或组织网络）的名称解析协议。 DNS 服务器承载的信息可以使客户端计算机和服务将容易识别的字母数字 DNS 名称解析为计算机相互之间进行通信时所用的 IP 地址。

### <a name="dhcp"></a>DHCP
DHCP 是用于简化主机 IP 配置管理的 IP 标准。 DHCP 标准将 DHCP 服务器用于管理网络上启用 DHCP 客户端的 IP 地址动态分配以及其他相关的配置详细信息。

DHCP 允许使用 DHCP 服务器动态地将 IP 地址分配到本地网络上的计算机或其他设备，如打印机。 TCP/IP 网络上的每台计算机都必须有一个独特的 IP 地址，因为 IP 地址和其相关的子网掩码同时标识主计算机和该计算机连接至的子网。 通过使用 DHCP，你可以确保所有配置为 DHCP 客户端的计算机都能收到适合于他们网络位置和子网的 IP 地址，并且通过使用 DHCP 选项，如默认网关和 DNS 服务器，你可以自动向 DHCP 客户端提供它们在网络上正常运作所需的信息。

对于基于 TCP/IP 的网络，DHCP 降低了重新配置计算机时的复杂性和所涉及的管理工作量。

### <a name="tcpip"></a>TCP/IP
Windows Server 2016 中的 TCP/IP 如下：

-   基于行业标准网络协议的网络软件。

-   支持基于 Windows 的计算机连接到局域网 (LAN) 和广域网 (WAN) 环境的可路由企业网络协议。

-   用于连接基于 Windows 的计算机与不同的系统以便共享信息的核心技术和实用程序。

-   获得对全局 Internet 服务访问权限的基础，例如万维网和文件传输协议 (FTP) 服务器。

-   强大、可扩展的跨平台客户端/服务器框架。

TCP/IP 提供了基本的 TCP/IP 实用程序，它使基于 Windows 的计算机可以与其他 Microsoft 和非 Microsoft 系统连接并共享信息，包括：

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

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

-   开放 VMS 系统

-   准备好网络的打印机

-   启用了有线以太网或无线802.11 技术的平板电脑和手机电话

## <a name="BKMK_overview"></a>核心网络概述
下图显示了 Windows Server 核心网络拓扑。

![Windows Server 核心网络拓扑](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> 本指南也包括了向你的网络拓扑添加可选“网络策略服务器 (NPS)”和“Web 服务器 (IIS)”的说明，为安全网络访问解决方案（如可使用“核心网络助理”指南实现的 802.1X 有线和无线部署）提供基础。 有关详细信息，请参阅[为网络访问身份验证和 Web 服务部署可选功能](#BKMK_optionalfeatures)。

### <a name="core-network-components"></a>核心网络组件
下面是核心网络的相关组件。

##### <a name="router"></a>路由器
此部署指南说明了如何使用路由器（已启用 DHCP 转发）分隔的两个子网部署核心网络。 但是，根据你的要求和资源，可以部署第 2 层交换机、第 3 层交换机或集线器。 如果部署交换机，则该交换机必须能够进行 DHCP 转发，或者必须在每个子网上放置 DHCP 服务器。 如果部署集线器，则部署的是单个子网且不需要 DHCP 转发或 DHCP 服务器上的其他作用域。

##### <a name="static-tcpip-configurations"></a>静态 TCP/IP 配置
此部署中的服务器配置了静态 IPv4 地址。 默认情况下，客户端计算机配置为从 DHCP 服务器接收 IP 地址租约。

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Active Directory 域服务全局编录和 DNS 服务器 DC1
此服务器上安装了 Active Directory 域服务（AD DS）和域名系统（DNS），该服务器名为 DC1，为网络上的所有计算机和设备提供目录和名称解析服务。

##### <a name="dhcp-server-dhcp1"></a>DHCP 服务器 DHCP1
用向本地子网上的计算机提供 Internet 协议 (IP) 地址租约的作用域，对名为 DHCP1 的 DHCP 服务器进行配置。 如果在路由器上配置了 DHCP 转发，还可以将 DHCP 服务器配置为具有向其他子网上的计算机提供 IP 地址租约的其他作用域。

##### <a name="client-computers"></a>客户端计算机
默认情况下，运行 Windows 客户端操作系统的计算机配置为 DHCP 客户端，该客户端自动从 DHCP 服务器获得 IP 地址和 DHCP 选项。

## <a name="BKMK_planning"></a>核心网络规划
部署核心网络之前，必须规划以下项目。

-   [规划子网](#bkmk_NetFndtn_Pln_Subnt)

-   [规划所有服务器的基本配置](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [规划 DC1 的部署](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [规划域访问](#bkmk_NetFndtn_Pln_DomAccess)

-   [规划 DHCP1 的部署](#bkmk_NetFndtn_Pln_DHCP-01)

以下部分提供有关这些项目中每个项目的详细信息。

> [!NOTE]
> 有关规划部署的帮助，另请参阅[附录 E-核心网络规划准备工作表](#BKMK_E)。

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>规划子网
在传输控制协议/Internet 协议 (TCP/IP) 网络中，使用路由器互连在不同物理网段（称为子网）上使用的硬件和软件。 还使用路由器在每个子网之间转发 IP 数据包。 继续按本指南中的说明进行操作之前，请确定网络的物理布局，包括所需的路由器和子网数量。

此外，若要将网络上的服务器配置为具有静态 IP 地址，必须确定要用于核心网络服务器所在子网的 IP 地址范围。 在本指南中，专用 IP 地址范围为10.0.0.254，10.0.1.1-10.0.1.254 用作示例，但你可以使用你喜欢的任何专用 IP 地址范围。

> [!IMPORTANT]
> 当选择了想用于每个子网的 IP 地址范围之后，确保用来配置你的路由器的 IP 地址，来自与安装了该路由器的子网所使用的相同的 IP 地址范围。 例如，如果你的路由器默认用 IP 地址 192.168.1.1 进行配置，但你打算将其安装在 IP 地址范围为 10.0.0.0/24 的子网上，你必须重新配置路由器，让它使用来自 10.0.0.0/24 IP 地址范围的一个 IP 地址。

按照 Internet 征求意见文档 (RFC) 1918 指定以下可以识别的专用 IP 地址范围：

-   10.0.0.0-10.255.255.255

-   172.16.0.0-172.31.255.255

-   192.168.0.0-192.168.255.255

当使用 RFC 1918 中指定的专用 IP 地址范围时，无法使用专用 IP 地址直接连接到 Internet，因为 Internet 服务提供商 (ISP) 路由器会自动丢弃传入这些地址或从中传出的请求。 若要稍后向核心网络中添加 Internet 连接，必须与 ISP 签约以获得公用 IP 地址。

> [!IMPORTANT]
> 当使用专用 IP 地址时，必须使用某些类型的代理或网络地址转换 (NAT) 服务器，将本地网络上的专用 IP 地址范围转换为可以在 Internet 上路由的公用 IP 地址。 大多数路由器提供 NAT 服务，所以选择一个提供 NAT 的路由器应该相当简单。

有关详细信息，请参阅[规划 DHCP1 的部署](#bkmk_NetFndtn_Pln_DHCP-01)。

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>规划所有服务器的基本配置
对于核心网络的每个服务器，你应该重命名计算机并为计算机分配和配置一个静态的 IPv4 地址和其他 TCP/IP 属性。

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>规划计算机和设备的命名约定
为了实现网络之间的一致性，最好对服务器、打印机和其他设备使用一致的名称。 计算机名称可用来帮助用户和管理员轻松确定服务器、打印机或其他设备的用途和位置。 例如，如果你有三个 DNS 服务器，一个在洛杉矶，一个在洛杉矶，一个在芝加哥，你可以使用命名约定*服务器函数*-*位置*-*号*：

-   DNS-DEN-01。 此名称代表了科罗拉多州丹佛市的 DNS 服务器。 如果在丹佛添加了其他 DNS 服务器，则可以递增名称中的数值，如 DNS-DEN-02 和 DNS-DEN-03。

-   DNS-SPAS-01。 此名称代表了加利福尼亚州南帕萨迪纳市的 DNS 服务器。

-   DNS-ORL-01。 此名称代表了佛罗里达州奥兰多市的 DNS 服务器。

对这个向导来说，服务器命名约定非常简单，由主服务器函数和一个数值组成。 例如，域控制器被命名为 DC1 而 DHCP 服务器被命名为 DHCP1。

建议你在使用这个向导安装核心网络前选择一个命名约定。

#### <a name="planning-static-ip-addresses"></a>规划静态 IP 地址
将每台计算机配置为具有静态 IP 地址之前，必须规划子网和 IP 地址范围。 此外，还必须确定 DNS 服务器的 IP 地址。 如果计划安装可以访问其他网络（如其他子网或 Internet）的路由器，则必须知道该路由器的 IP 地址（也称为默认网关）才能进行静态 IP 地址配置。

下表提供了静态 IP 地址配置的示例值。

|配置项|示例值|
|-----------------------|------------------|
|IP 地址|10.0.0.2|
|子网掩码|255.255.255.0|
|默认网关（路由器 IP 地址）|10.0.0.1|
|首选 DNS 服务器|10.0.0.2|

> [!NOTE]
> 如果你规划部署一台以上 DNS 服务器，则也可规划替代的 DNS 服务器 IP 地址。

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>规划 DC1 的部署
下面是在 DC1 上安装 Active Directory 域服务（AD DS）和 DNS 之前的主要规划步骤。

#### <a name="planning-the-name-of-the-forest-root-domain"></a>规划目录林根级域的名称
AD DS 设计过程的第一步是确定组织所需的林数量。 林是顶级 AD DS 容器，由共享公用架构和全局编录的一个或多个域组成。 一个组织可以拥有多个林，但是对于大多数组织来说，单一林设计是首选模型，而且也最便于管理。

在组织中创建第一个域控制器时，便会创建第一个域（也称为目录林根级域）和第一个林。 但在使用本指南执行该操作之前，必须确定适合你组织的最佳域名。 大多数情况下，将使用组织名称作为域名，并且在很多情况下注册该域名。 如果你打算规划部署面向外部的基于 Internet 的 Web 服务器，为你的客户与伙伴提供信息和服务，选择一个尚未使用的域名，然后登记该域名以使你的组织拥有它。

#### <a name="planning-the-forest-functional-level"></a>规划林功能级别
在安装 AD DS 时，必须选择要使用的林功能级别。 Windows Server 2003 Active Directory 中引入的域和林功能提供了一种在网络环境中启用域或林范围 Active Directory 功能的方法。 不同的环境，可以使用不同级别的域功能和林功能级别。

林功能启用了林中所有域上的功能。 可以使用以下林功能级别：

-    Windows Server 2008。 此林功能级别仅支持运行 Windows Server 2008 和更高版本的 Windows Server 操作系统的域控制器。

-    Windows Server 2008 R2。 此林功能级别支持 Windows Server 2008 R2 域控制器和运行更高版本 Windows Server 操作系统的域控制器。

-    Windows Server 2012。 此林功能级别支持 Windows Server 2012 域控制器和运行更高版本的 Windows Server 操作系统的域控制器。

-    Windows Server 2012 R2。 此林功能级别支持 Windows Server 2012 R2 域控制器和运行更高版本 Windows Server 操作系统的域控制器。

-    Windows Server 2016。 此林功能级别仅支持运行 windows Server 操作系统的更高版本的 Windows Server 2016 域控制器和域控制器。

如果要在新林中部署新域，并且所有域控制器都将运行 Windows Server 2016，则建议在 AD DS 安装过程中使用 Windows Server 2016 林功能级别配置 AD DS。

> [!IMPORTANT]
> 提升林功能级别后，无法将运行早期版本操作系统的域控制器引入该林。 例如，如果将林功能级别提升到 Windows Server 2016，则无法将运行 Windows Server 2012 R2 或 Windows Server 2008 的域控制器添加到该林。

下表提供了 AD DS 的示例配置项目。

|配置项：|示例值：|
|------------------------|-------------------|
|完整 DNS 名|例如：<br /><br />-corp.contoso.com<br />-example.com|
|林功能级别|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />-Windows Server 2012 R2 <br />-Windows Server 2016|
|Active Directory 域服务数据库文件夹位置|E:\Configuration @ no__t-0<br /><br />或接受默认位置。|
|Active Directory 域服务日志文件文件夹位置|E:\Configuration @ no__t-0<br /><br />或接受默认位置。|
|Active Directory 域服务 SYSVOL 文件夹位置|E:\Configuration @ no__t-0<br /><br />或接受默认位置|
|目录还原模式的管理员密码|**J @ no__t-1p2leO4 $ F**|
|答案文件名（可选）|**AD DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>规划 DNS 区域
在初级的集成了 Active Directory 的 DNS 服务器上，默认在 DNS 服务器角色安装期间创建一个正向查找区域。 正向查找区域允许计算机和设备根据其 DNS 名称查询其他计算机或设备的 IP 地址。 除了正向查找区域之外，建议你创建 DNS 反向查找区域。 借助 DNS 反向查找查询，计算机或设备可以使用其 IP 地址发现其他计算机或设备的名称。 部署反向查找区域通常会提高 DNS 的性能，而且会大大增加 DNS 查询的成功率。

创建反向查找区域时，会在 DNS 中配置 in-addr.arpa 域，该域在 DNS 标准中定义且保留在 Internet DNS 命名空间中，以提供一种用于执行反向查询的实用可靠的方式。 为创建反向命名空间，通过对 IP 地址点分十进制表示法形式的数字进行反向排序，可以形成 in-addr.arpa 域中的子域。

Arpa 域适用于基于 Internet 协议版本4（IPv4）寻址的所有 TCP/IP 网络。 新建区域向导会自动假定你在创建新的反向查找区域时使用此域。

运行新建区域向导时，建议进行以下选择：

|配置项|示例值|
|-----------------------|------------------|
|区域类型|选择 **“主要区域”** 和 **“在 Active Directory 中存储区域”**|
|Active Directory 区域传送作用域|**此域中的所有 DNS 服务器**|
|第一个反向查找区域名称向导页|**IPv4 反向查找区域**|
|第二个反向查找区域名称向导页|网络 ID = 10.0.0.|
|动态更新|**仅允许安全动态更新**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>规划域访问
若要登录到域，计算机必须是域成员计算机，并且必须在登录尝试之前 AD DS 中创建用户帐户。

> [!NOTE]
> 运行 Windows 的单个计算机有一个本地用户与组的用户帐户数据库，被称为被称为安全帐户管理器 (SAM) 用户帐户数据库。 当在 SAM 数据库中的本地计算机上创建用户帐户的时候，你可以登录到本地计算机，但无法登录到域。 用域控制器上的 Active Directory 用户和计算机 Microsoft 管理控制台 (MMC)，而不是用本地计算机上的本地用户和组来创建域用户帐户。

使用域登录凭据第一次成功登录之后，登录设置将持续存在，除非从域中删除该计算机或者手动更改登录设置。

登录到域之前：

-   在“Active Directory 用户和计算机”中创建用户帐户。 每个用户必须在“Active Directory 用户和计算机”中拥有一个 Active Directory 域服务用户帐户。 有关详细信息，请参阅[在“Active Directory 用户和计算机”中创建用户帐户](#BKMK_createUA)。

-   确保 IP 地址配置正确。 若要使计算机加入域，该计算机必须具有 IP 地址。 本指南中，服务器配置为具有静态 IP 地址并且客户端计算机从 DHCP 服务器接收 IP 地址租约。 为此，必须在将客户端加入域之前部署 DHCP 服务器。 有关详细信息，请参阅[部署 DHCP1](#BKMK_deployDHCP01)。

-   将计算机加入域。 提供或者访问网络资源的任何计算机都必须加入域。 有关详细信息，请参阅[将服务器计算机加入到域并登录](#BKMK_joinlogserver)和[将客户端计算机加入到域并登录](#BKMK_joinlogclients)。

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>规划 DHCP1 的部署
下面是在 DHCP1 上安装 DHCP 服务器角色之前的主要规划步骤。

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>规划 DHCP 服务器和 DHCP 转发
由于 DHCP 消息是广播消息，因此路由器不会在子网之间转发这些消息。 如果你有多个子网并且想为每个子网提供 DHCP 服务，则必须执行以下操作之一：

-   在每个子网上安装 DHCP 服务器

-   将路由器配置为跨子网转发 DHCP 广播消息，并且在 DHCP 服务器上配置多个作用域，每个子网一个作用域。

大多数情况下，将路由器配置为转发 DHCP 广播消息比在网络的每个物理网段上部署 DHCP 服务器更经济有效。

#### <a name="planning-ip-address-ranges"></a>规划 IP 地址范围
每个子网必须具有其自己的独特 IP 地址范围。 在 DHCP 服务器上用作用域表示这些范围。

作用域是为了便于管理而对子网上使用 DHCP 服务的计算机 IP 地址进行的分组。 管理员首先为每个物理子网创建一个作用域，然后使用此作用域定义客户端所用的参数。

作用域具有下列属性：

-   IP 地址的范围，可在其中包含或排除用于提供 DHCP 服务租用的地址。

-   子网掩码，它确定给定 IP 地址的子网前缀。

-   作用域名称，在创建作用域时指定该名称。

-   租用期限值，这些值被分配到接收动态分配的 IP 地址的 DHCP 客户端。

-   为向 DHCP 客户端进行分配而配置的所有 DHCP 作用域选项，例如 DNS 服务器 IP 地址和路由器/默认网关 IP 地址。

-   保留，可以选择用于确保 DHCP 客户端始终接收相同的 IP 地址。

部署服务器之前，请列出子网以及要用于每个子网的 IP 地址范围。

#### <a name="planning-subnet-masks"></a>规划子网掩码
使用子网掩码来区分 IP 地址中的网络 ID 和主机 ID。 每个子网掩码都是一个 32 位的数字，它使用全部为 1 的连续位组标识网络 ID，使用全部为 0 的连续位组标识 IP 地址的主机 ID 部分。

例如，用于 IP 地址 131.107.16.200 的子网掩码通常是以下 32 位二进制数字：

```
11111111 11111111 00000000 00000000
```

此子网掩码号为 16 1 位后跟16个零位，这表明此 IP 地址的网络 ID 和主机 ID 部分的长度都是16位。 通常，该子网掩码采用点分隔的十进制表示法显示为 255.255.0.0。

下表显示了 Internet 地址类别的子网掩码。

|地址类别|子网掩码的位|子网掩码|
|-----------------|------------------------|---------------|
|A 类|11111111 00000000 00000000 00000000|255.0.0.0|
|B 类|11111111 11111111 00000000 00000000|255.255.0.0|
|C 类|11111111 11111111 11111111 00000000|255.255.255.0|

在 DHCP 中创建作用域并且为该作用域输入 IP 地址范围时，DHCP 提供这些默认的子网掩码值。 通常，默认的子网掩码值对于没有特殊要求的大多数网络来说都是可以接受的，其中每个 IP 网段对应于一个物理网络。

某些情况下，可以使用自定义子网掩码实现 IP 子网。 借助 IP 子网，可以细分 IP 地址的默认主机 ID 部分以指定子网，这些子网是基于原始类别的网络 ID 的细分。

通过自定义子网掩码长度，可以减少用于实际主机 ID 的位数。

为了防止出现寻址和路由问题，应该确保网段上的所有 TCP/IP 计算机使用同一子网掩码并且每台计算机或设备具有唯一的 IP 地址。

#### <a name="planning-exclusion-ranges"></a>规划排除范围
当你在 DHCP 服务器上创建一个作用域的时候，你指定一个 IP 地址范围，这个范围包括允许 DHCP 服务器租用给 DHCP 客户端（如计算机和其他设备）的全部 IP 地址。 如果你随后进行尝试，并使用来自 DHCP 服务器所用的 IP 地址范围的静态 IP 地址，手动配置一些服务器和其他设备，你可能碰巧会造成一个 IP 地址冲突，在冲突中，你和 DHCP 服务器向不同的设备分配了同样的 IP 地址。

若要解决这个问题，你可以为 DHCP 作用域创建一个排除范围。 排除范围是不允许 DHCP 服务器使用的作用域的 IP 地址范围内的连续 IP 地址范围。 如果你创建了排除范围，DHCP 服务器将不会分配那个范围内的地址，这允许你手动分配这些地址，而不会造成 IP 地址冲突。

可以通过为每个作用域创建一个排除范围，从 DHCP 服务器的分发中排除 IP 地址。 应该对配置有静态 IP 地址的所有设备使用排除。 所排除的地址应包括所有手动分配给其他服务器、非 DHCP 客户端、无盘工作站或路由和远程访问及 PPP 客户端的 IP 地址。

建议你将排除范围配置为具有额外地址，以适应未来网络的扩容。 下表提供了一个 IP 地址范围为 10.0.0.1-10.0.0.254、子网掩码为255.255.255.0 的作用域的示例排除范围。

|配置项|示例值|
|-----------------------|------------------|
|排除范围起始 IP 地址|10.0.0.1|
|排除范围结束 IP 地址|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>规划 TCP/IP 静态配置
某些设备（如路由器、DHCP 服务器和 DNS 服务器）必须配置有静态 IP 地址。 此外，你可能还拥有其他设备（如打印机），你希望确保这些设备始终具有相同的 IP 地址。 针对每个子网，列出要静态配置的设备，然后规划要在 DHCP 服务器上使用的排除范围，以确保 DHCP 服务器不会租用静态配置的设备的 IP 地址。 排除范围是作用域中有限的 IP 地址序列，这些地址不提供 DHCP 服务。 排除范围确保服务器不会将这些范围中的任何地址提供给网络上的 DHCP 客户端。

例如，如果子网的 IP 地址范围为192.168.0.1 到192.168.0.254，并且你有10个要配置静态 IP 地址的设备，则可以为192.168.0 创建排除范围。*x*范围包含10个或更多 IP 地址：192.168.0.1 到192.168.0.15。

在本例中，使用 10 个排除的 IP 地址将服务器和其他设备配置为静态 IP 地址，剩下的其他 5 个 IP 地址用于将来可能希望添加的新设备的静态配置。 对于此排除范围，DHCP 服务器将保留 192.168.0.16 到 192.168.0.254 的地址池。

下表提供了 AD DS 和 DNS 的其他示例配置项目。

|配置项|示例值|
|-----------------------|------------------|
|网络连接绑定|Ethernet|
|DNS 服务器设置|DC1.corp.contoso.com|
|首选 DNS 服务器 IP 地址|10.0.0.2|
|“添加作用域”对话框值<br /><br />1.作用域名称<br />2.起始 IP 地址<br />3.结束 IP 地址<br />4.子网掩码<br />5.默认网关（可选）<br />6.租用期限|1.主要子网<br />2. 10.0.0。1<br />3. 10.0.0.254<br />4. 255.255.255。0<br />5. 10.0.0。1<br />6 8 天|
|IPv6 DHCP 服务器操作模式|未启用|

## <a name="BKMK_deployment"></a>核心网络部署
若要部署核心网络，请按照以下基本步骤执行操作：

1.  [配置所有服务器](#BKMK_configuringAll)

2.  [部署 DC1](#BKMK_deployADDNS01)

3.  [将服务器计算机加入域并登录](#BKMK_joinlogserver)

4.  [部署 DHCP1](#BKMK_deployDHCP01)

5.  [将客户端计算机加入域并登录](#BKMK_joinlogclients)

6.  [为网络访问身份验证和 Web 服务部署可选功能](#BKMK_optionalfeatures)

> [!NOTE]
> -   本指南中的大多数过程都提供有等价的 Windows PowerShell 命令。 在运行 Windows PowerShell 的这些 cmdlets之前，用适合于你的网络部署的值替换示例值。 此外，在 Windows PowerShell 中，你必须在一个单独的行中输入每个 cmdlet。 本指南中，由于格式化约束和你浏览器或其他应用程序对文档的显示，单独的 cmdlets 可能显示在好几行中。
> -   对于打开 **“用户帐户控制”** 对话框以请求你允许继续操作的情况，本指南中的过程不做说明。 如果在执行本指南中的过程时出现了此对话框，并且该对话框是因响应你的操作而出现的，则请单击 **“继续”** 。

### <a name="BKMK_configuringAll"></a>配置所有服务器
安装其他技术（如 Active Directory 域服务或 DHCP）之前，配置以下项非常重要。

-   [重命名计算机](#BKMK_rename)

-   [配置静态 IP 地址](#BKMK_ip)

可以使用以下部分为每台服务器执行这些操作。

Administrators组成员或同等身份是执行这些过程的最低要求。

#### <a name="BKMK_rename"></a>重命名计算机
您可以使用本部分中的过程来更改计算机的名称。 在操作系统自动创建了一个你不想使用的计算机名的情况下，重命名计算机是件有用的事情。

> [!NOTE]
> 要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并在不同的行中键入下列 cmdlet，然后按 ENTER 键。 你同样必须将 *ComputerName* 替换为你要使用的名字。
>
> `Rename-Computer`*ComputerName*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>重命名运行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的计算机

1.  在服务器管理器中，单击 **“本地服务器”** 。 计算机 **“属性”** 显示在细节窗格中。

2.  在**属性**中，在 **“计算机名”** 中单击现有计算机名。 这将打开“系统属性”对话框。 单击“更改”。 这将打开“计算机名/域更改”对话框。

3.  在 **“计算机名/域更改”** 对话框的 **“计算机名称”** 框中，键入计算机的新名称。 例如，如果要将计算机命名为 DC1，请键入 **DC1**。

4.  单击两次“确定” ，再单击“关闭”。 如果你想立即重启计算机完成名称更改，单击 **“立即重新启动”** 。 否则，单击 **“稍后重新启动”** 。

> [!NOTE]
> 有关如何重命名运行其他 Microsoft 操作系统的计算机的信息，请参阅[附录 A-重命名计算机](#BKMK_A)。

#### <a name="BKMK_ip"></a>配置静态 IP 地址
你可以使用本主题中的过程为运行 Windows Server 2016 的计算机配置具有静态 IP 地址的网络连接的 Internet 协议版本4（IPv4）属性。

> [!NOTE]
> 要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并在不同的行中键入下列 cmdlet，然后按 ENTER 键。 也必须用你想要用来配置计算机的值，替换这个例子里的接口名称和 IP 地址。
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>在运行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的计算机上配置静态 IP 地址

1.  在任务栏里，右键单击“网络”图表，然后单击 **“打开网络和共享中心”** 。

2.  在 "**网络和共享中心**" 中，单击 "**更改适配器设置**"。 **“网络连接”** 文件夹打开并显示可用的网络连接。

3.  在 **“网络连接”** 中，右键单击要配置的连接，然后单击 **“属性”** 。 打开网络连接 **“属性”** 对话框。

4.  在网络连接 **“属性”** 对话框的 **“此连接使用下列项目”** 中，选择 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。 这将打开“Internet 协议版本 4 (TCP/IPv4) 属性”对话框。

5.  在“Internet 协议版本 4 (TCP/IPv4) 属性”中的“常规”选项卡上，单击“使用下面的 IP 地址”。 在 **“IP 地址”** 中，键入你要使用的 IP 地址。

6.  按 Tab 以将光标移到“子网掩码”中。 子网掩码的默认值将自动输入。 可以接受该默认的子网掩码，也可以键入要使用的子网掩码。

7.  在 **“默认网关”** 中，键入默认网关的 IP 地址。

    > [!NOTE]
    > 必须用你路由器的局域网 (LAN) 界面使用的同一 IP 地址配置 **“默认网关”** 。 例如，如果你有一个不仅连接到你的 LAN，也连接到如 Internet 这样的广域网 (WAN) 的路由器，使用你随后将指定为 **“默认网关”** 的同一个 IP 地址，配置 LAN 界面。 在另一个例子里，如果你有一个连接到两个 LAN 的路由器，其中 LAN A 使用地址范围 10.0.0.0/24 而 LAN B 使用地址范围 192.168.0.0/24，使用来自那个地址范围的一个地址（如 10.0.0.1）配置 LAN A 路由器 IP 地址。 另外，在这个地址范围的 DHCP 作用域里，用 IP 地址 10.0.0.1 配置 **“默认网关”** 。 对 LAN B 而言，使用来自那个地址范围的一个地址（如 192.168.0.1）配置 LAN B 路由器界面，然后将 LAN B 作用域配置为 192.168.0.0/24， **“默认网关”** 值为 192.168.0.1。

8.  在 **“首选 DNS 服务器”** 中，键入 DNS 服务器的 IP 地址。 如果计划使用本地计算机作为首选 DNS 服务器，则键入本地计算机的 IP 地址。

9. 在 **“备用 DNS 服务器”** 中，键入备用 DNS 服务器的 IP 地址（如果有）。 如果计划使用本地计算机作为备用 DNS 服务器，则键入本地计算机的 IP 地址。

10. 单击“确定”，然后单击“关闭”。

> [!NOTE]
> 有关如何在运行其他 Microsoft 操作系统的计算机上配置静态 IP 地址的信息，请参阅[附录 B-配置静态 ip 地址](#BKMK_B)。

#### <a name="BKMK_deployADDNS01"></a>部署 DC1
若要部署 DC1 （即运行 Active Directory 域服务（AD DS）和 DNS 的计算机，你必须按以下顺序完成这些步骤：

-   执行[配置所有服务器](#BKMK_configuringAll)部分中描述的步骤。

-   [为新林安装 AD DS 和 DNS](#BKMK_installAD-DNS)

-   [在 Active Directory 用户和计算机中创建用户帐户](#BKMK_createUA)

-   [分配组成员身份](#BKMK_assigngroup)

-   [配置 DNS 反向查找区域](#BKMK_reverse)

**管理权限**

如果要安装小型网络并且你是该网络的唯一管理员，则建议你为自己创建一个用户帐户，然后将该用户帐户同时添加为 Enterprise Admins 和 Domain Admins 的成员。 这样做将便于你充当所有网络资源的管理员。 还建议你仅当需要执行管理任务时才使用该帐户登录，并且创建一个单独的用户帐户，用于执行与 IT 不相关的任务。

如果你的组织有多个管理员，请参阅 AD DS 文档来确定组织员工的最佳组成员身份。

**本地计算机上的域用户帐户和用户帐户之间的差异**

基于域的基础结构的一个优势是不需要在域中的每台计算机上创建用户帐户。 无论该计算机是客户端计算机还是服务器都是如此。

因此，不应在域中的每台计算机上创建用户帐户。 而应在“Active Directory 用户和计算机”中创建所有用户帐户并使用前面所述步骤分配组成员身份。 默认情况下，所有用户帐户都是 Domain Users 组的成员。

域用户组的所有成员在被加入到域之后，都可以登录到任意客户端计算机。

可以配置用户帐户以指定允许用户登录到计算机的日期和时间。 还可以指定允许每个用户使用的计算机。 若要配置这些设置，请打开“Active Directory 用户和计算机”，找到要配置的用户帐户，然后双击该帐户。 在用户帐户 **“属性”** 中，单击 **“帐户”** 选项卡，然后单击 **“登录时间”** 或 **“登录到”** 。

#### <a name="BKMK_installAD-DNS"></a>为新林安装 AD DS 和 DNS

你可以使用以下过程之一来安装 Active Directory 域服务（AD DS）和 DNS 并在新林中创建新域。 

第一个过程提供了使用 Windows PowerShell 执行这些操作的说明，而第二个过程演示如何使用服务器管理器安装 AD DS 和 DNS。

>[!IMPORTANT]
>完成此过程中的步骤后，计算机会自动重新启动。

**使用 Windows PowerShell 安装 AD DS 和 DNS**

你可以使用以下命令来安装和配置 AD DS 和 DNS。 必须将此示例中的域名替换为要用于该域的值。

>[!NOTE]
>有关这些 Windows PowerShell 命令的详细信息，请参阅以下参考主题。
>- [Add-windowsfeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps)
>- [安装-Install-addsforest](https://docs.microsoft.com/powershell/module/addsdeployment/install-addsforest?view=win10-ps)

**Administrators** 中的成员身份是执行此过程所需的最低要求。

- 以管理员身份运行 Windows PowerShell，键入以下命令，然后按 ENTER：  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

安装成功完成后，Windows PowerShell 中将显示以下消息。


    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...


- 在 Windows PowerShell 中，键入以下命令，将 " **corp.contoso.com** " 文本替换为你的域名，然后按 enter：

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- 在 Windows PowerShell 窗口顶部显示的安装和配置过程中，将显示以下提示。 出现后，键入密码，然后按 ENTER。

    **SafeModeAdministratorPassword**

- 键入密码并按 ENTER 后，将显示以下确认提示。 键入相同的密码，然后按 ENTER。

    **确认 SafeModeAdministratorPassword：**

- 出现以下提示时，键入字母**Y** ，然后按 enter。


~~~
The target server will be configured as a domain controller and restarted when this operation is complete.
Do you want to continue with this operation?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
~~~

- 如果需要，你可以阅读 AD DS 和 DNS 的正常安装过程中显示的警告消息。 这些消息是正常的，并不表示安装失败。

- 安装成功后，将显示一条消息，指出您将从计算机注销，以便计算机可以重新启动。 如果单击 "**关闭**"，则会立即从计算机注销，计算机将重新启动。 如果不单击 "**关闭**"，计算机将在默认的时间段后重新启动。

- 重新启动服务器后，可以验证 Active Directory 域服务和 DNS 的安装是否成功。 打开 Windows PowerShell，键入以下命令，然后按 ENTER。

````
Get-WindowsFeature
````

此命令的结果将显示在 Windows PowerShell 中，应类似于下图所示的结果。 对于已安装的技术，技术名称左侧的括号包含字符**X** **，安装**状态的值为**Install** 。

![Get-help 命令的结果](../media/Core-Network-Guide/server-roles-installed.jpg)

**使用服务器管理器安装 AD DS 和 DNS**

1.  在 DC1 上，在 **“服务器管理器”** 中，单击 **“管理”** ，然后单击 **“添加角色和功能”** 。 将打开“添加角色和功能向导”。

2.  在“开始之前”中单击“下一步”。

    > [!NOTE]
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”** ，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。

3.  在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。

4.  在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”** 。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步”。

5.  在 **“选择服务器角色”** 的 **“角色”** 中，单击 **“Active Directory 域服务”** 。 在 **“添加 Active Directory 域服务所需的功能”** 中，单击 **“添加功能”** 。 单击“下一步”。

6.  在 **“选择功能**”中，单击 **“下一步”** ，然后在 **“Active Directory 域服务”** 中查看提供的信息，然后单击 **“下一步”** 。

7.  在 **“确认安装选择”** 中，单击 **“安装”** 。 在安装进程期间，安装进度页显示状态。 进程完成时，在详细信息消息中，单击 **“将这台服务器提升为域控制器”** 。 打开“Active Directory 域服务配置向导”。

8.  在 **“部署配置”** 中，选择 **“添加一个新林”** 。 在 **“根域名”** 中，为你的域键入完全限定的域名 (FQDN)。 例如，如果你的 FQDN 为 corp.contoso.com，则键入 **corp.contoso.com**。 单击“下一步”。

9. 在 **“域控制器选项”** 的 **“选择新林和根域的功能级别”** ，选择要使用的林功能级别和域功能级别。 在 **“指定域控制器功能”** 中，确保选中了 **“域名系统 (DNS)”服务器**和 **“全局编录 (GC)”** 。 在 **“密码”** 和 **“确认密码”** 中，键入要使用的“目录服务还原模式 (DSRM)”密码。 单击“下一步”。

10. 在 **“DNS 选项”** 中单击 **“下一步”** 。

11. 在 **“其他选项”** 中，验证指派给域的 NetBIOS 名称，并且只要需要，就更改它。 单击“下一步”。

12. 在 **“路径”** 的 **“指定 AD DS 数据库、日志文件和 SYSVOL 的位置”** 中，执行下列操作之一：

    -   接受默认值。

    -   键入要用于 **“数据库文件夹”** 、 **“日志文件文件夹”** 和 **“SYSVOL 文件夹”** 的文件夹位置。

13. 单击“下一步”。

14. 在 **“查看选项”** 中，查看你的选择。

15. 如果要将设置导出至一个 Windows PowerShell 脚本，单击 **“查看脚本”** 。 脚本在“记事本”中被打开，可将它保存到你期望的文件夹位置。 单击“下一步”。 在 **“先决条件检查”** 中验证你的选择。 检查完成时，单击 **“安装”** 。 当 Windows 提示你的时候，单击 **“关闭”** 。 服务器将重新启动以完成安装 AD DS 和 DNS。

16. 若要验证安装是否成功，请在服务器重新启动后查看服务器管理器控制台。 AD DS 和 DNS 都应该出现在左窗格中，如下面的图像中突出显示的项目。

![服务器管理器中的 AD DS 和 DNS](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>在 Active Directory 用户和计算机中创建用户帐户
可以使用此过程在“Active Directory 用户和计算机”Microsoft 管理控制台 (MMC) 中新建域用户帐户。

“Domain Admins”成员身份或同等身份是执行此过程的最低要求。

> [!NOTE]
> 要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并在一行中键入下面的 cmdlet，然后按 ENTER 键。 你也必须用你想使用的值替换这个例子里的用户帐户名。
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> 按 ENTER 键。，键入用户帐户的密码。 创建了帐户，并且默认授予其“域用户”组的成员资格。
>
> 使用下面的 cmdlet，你可以给新用户帐户分配更多的组成员资格。 下面的例子将 User1 添加到“Domain Admins”和“Enterprise Admins”组。 运行此命令前确保你更改了用户帐户名、域名和组，以匹配你的要求。
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>创建用户帐户的步骤

1.  在 DC1 的服务器管理器中，单击 **“工具”** ，然后单击 **“Active Directory 用户和计算机”** 。 将打开“Active Directory 用户和计算机”MMC。 单击域的节点（如果尚未选定）。 例如，如果你的域是 corp.contoso.com，则单击 **corp.contoso.com**。

2.  在细节窗格中，右键单击要向其中添加用户帐户的文件夹。

    **其中?**

    -   Active Directory 用户和计算机/*域节点*/*文件夹*

3.  指向“新建”，然后单击“用户”。 此时将打开 "**新建对象-用户**" 对话框。

4.  在 **“名字”** 中，键入用户的名字。

5.  在 **“姓名缩写”** 中，键入用户的姓名首字母缩写。

6.  在“姓”中，键入用户的姓氏。

7.  修改 **“全名”** 以添加姓名首字母缩写或颠倒名字和姓氏的顺序。

8.  在 **“用户登录名”** 中，键入用户的登录名。 单击“下一步”。

9. 在 **“新建对象 - 用户”** 的 **“密码”** 和 **“确认密码”** 中，键入该用户的密码，然后选择相应的密码选项。

10. 单击 **“下一步”** ，查看新用户帐户设置，然后单击 **“完成”** 。

##### <a name="BKMK_assigngroup"></a>分配组成员身份
可以使用以下过程在 Active Directory 用户和计算机的 Microsoft 管理控制台 (MMC) 中将用户、计算机或组添加到另一个组中。

“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。

###### <a name="to-assign-group-membership"></a>分配组成员身份的步骤

1.  在 DC1 的服务器管理器中，单击 **“工具”** ，然后单击 **“Active Directory 用户和计算机”** 。 将打开“Active Directory 用户和计算机”MMC。 单击域的节点（如果尚未选定）。 例如，如果你的域是 corp.contoso.com，则单击 **corp.contoso.com**。

2.  在细节窗格中，双击包含要添加成员的组的文件夹。

    位置：

    -   **Active Directory 用户和计算机**/*域节点*/*包含组的文件夹*

3.  在细节窗格中，右键单击你要添加到组的对象（如用户或计算机），然后单击 **“属性”** 。 对象的 "**属性**" 对话框将打开。 单击 **“隶属于”** 选项卡。

4.  在 **“隶属于”** 选项卡上，单击 **“添加”** 。

5.  在 **“输入要选择的对象名称”** 中，键入要添加对象的组名，然后单击 **“确定”** 。

6.  若要将组成员身份分配到其他用户、组或计算机，请重复此过程中的步骤 4 和 5。

##### <a name="BKMK_reverse"></a>配置 DNS 反向查找区域
可以使用此过程在“域名系统 (DNS)”中配置反向查找区域。

**“Domain Admins”** 中的成员身份是执行此过程所需的最低要求。

> [!NOTE]
> -   对于中型和大型组织，建议你在 Active Directory 用户和计算机中配置和使用 DNSAdmins 组。 有关详细信息，请参阅[其他技术资源](#BKMK_resources)。
> -   要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并在一行中键入下面的 cmdlet，然后按 ENTER 键。 你也必须用你想使用的值替换这个例子里的 DNS 反向查找区域和区域文件名。 确保你是将网络 ID 反过来作为反向区域名称的。 例如，如果网络 ID 是192.168.0，请创建**0.168.192.in-arpa**的反向查找区域名称。
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>配置 DNS 反向查找区域的步骤

1.  在 DC1 的服务器管理器中，单击 **“工具”** ，然后单击 **“DNS”** 。 此时将打开 DNS MMC。

2.  在 DNS 中，如果它尚未展开，请双击服务器名称展开树。 例如，如果 DNS 服务器名为 DC1，则双击 **DC1**。

3.  选择 **“反向查找区域”** ，右键单击 **“反向查找区域”** ，然后单击 **“新建区域”** 。 将会打开新建区域向导。

4.  在 **“欢迎使用新建区域向导”** 中，单击 **“下一步”** 。

5.  在 **“区域类型”** 中，选择 **“主要区域”** 。

6.  如果 DNS 服务器是可写域控制器，确保选中了 **“在 Active Directory 中存储区域”** 。 单击“下一步”。

7.  在 **“Active Directory 区域复制作用域”** 中，选择 **“到在本域内域控制器上运行的所有 DNS 服务器”** ，除非你有特别的原因，要选择不同的选项。 单击“下一步”。

8.  在第一个 **“反向查找区域名称”** 页中，选择 **“IPv4 反向查找区域”** 。 单击“下一步”。

9. 在第二个 **“反向查找区域名称”** 页中，执行下列操作之一：

    -   在 **“网络 ID”** 中，键入 IP 地址范围的网络 ID。 例如，如果你的 IP 地址范围是 10.0.0.1 到 10.0.0.254，那么键入 **10.0.0**。

    -   在**反向查找区域名称**中，自动添加了你的 IPv4 反向查找区域名称。 单击“下一步”。

10. 在 **“动态更新”** 中，选择希望允许的动态更新类型。 单击“下一步”。

11. 在 **“完成新建区域向导”** 中，查看你的选择，然后单击 **“完成”** 。

#### <a name="BKMK_joinlogserver"></a>将服务器计算机加入域并登录
安装 Active Directory 域服务（AD DS）并创建一个或多个具有将计算机加入域的权限的用户帐户后，可以将核心网络服务器加入域并登录到服务器，以便安装其他技术，如动态主机配置协议（DHCP）。

在要部署的所有服务器上（运行 AD DS 的服务器除外），执行以下操作：

1.  完成 [“配置所有服务器”](#BKMK_configuringAll)中提供的过程。

2.  使用以下两个过程中的说明，将服务器加入域并登录到服务器，以执行其他部署任务：

> [!NOTE]
> 要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并键入下面的 cmdlet，然后按 ENTER 键。 你同样必须将域名替换为你要使用的名字。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 按照提示，为一个有权限将计算机加入域的帐户键入用户名和密码。 若要重新启动计算机，键入以下命令，然后按 ENTER 键。
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>将运行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的计算机加入到域中

1.  在服务器管理器中，单击 **“本地服务器”** 。 在详细信息窗格中，单击 **“工作组”** 。 这将打开“系统属性”对话框。

2.  在“系统属性” 对话框中，单击“更改”。 这将打开“计算机名/域更改”对话框。

3.  在 **“计算机名称”** 中的 **“隶属于”** 中，单击 **“域”** ，然后键入要加入的域的名称。 例如，如果域名为 corp.contoso.com，则键入 **corp.contoso.com**。

4.  单击 **“确定”** 。 这将打开“Windows 安全性”对话框。

5.  在 **“计算机名/域更改”** 中的 **“用户名”** 中，键入用户名，并在 **“密码”** 中键入密码，然后单击 **“确定”** 。 将会打开 **“计算机名/域更改”** 对话框，欢迎你加入域。 单击 **“确定”** 。

6.  **“计算机名/域更改”** 对话框显示一条消息，表明必须重新启动计算机才能应用更改。 单击 **“确定”** 。

7.  在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“关闭”** 。 将会打开 **Microsoft Windows** 对话框并显示一条消息，再次表明必须重新启动计算机才能应用更改。 单击“立即重新启动”。

> [!NOTE]
> 有关如何将运行其他 Microsoft 操作系统的计算机加入域的信息，请参阅[附录 C-将计算机加入到域](#BKMK_C)。

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>使用运行 Windows Server 2016 的计算机登录到域

1.  注销计算机，或重新启动计算机。

2.  按 Ctrl + Alt + Delete。 将会出现登录屏幕。

3.  在左下角，单击 "**其他用户**"。

4.  在 "**用户名**" 中，键入你的用户名。

5.  在 **“密码”** 中，键入域密码，然后单击箭头，或按 ENTER 键。

> [!NOTE]
> 有关如何使用运行其他 Microsoft 操作系统的计算机登录到域的信息，请参阅[附录 D-登录到域](#BKMK_D)。

#### <a name="BKMK_deployDHCP01"></a>部署 DHCP1
在部署核心网络的此组件之前，必须执行以下操作：

-   执行[配置所有服务器](#BKMK_configuringAll)部分中描述的步骤。

-   执行[将服务器计算机加入域并登录](#BKMK_joinlogserver)部分中描述的步骤。

若要部署 DHCP1 [运行“动态主机配置协议 (DHCP)”服务器角色的计算机]，必须按照以下顺序完成这些步骤：

-   [安装动态主机配置协议（DHCP）](#BKMK_installDHCP)

-   [创建并激活一个新的 DHCP 作用域](#BKMK_newscopeDHCP)

> [!NOTE]
> 要使用 Windows PowerShell 执行这些过程，打开 PowerShell 并在不同的行中键入下列 cmdlet，然后按 ENTER 键。 你也必须将作用域名称、IP 地址起始和结束范围、子网掩码和此例子中的其他值替换为要使用的值。
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

##### <a name="BKMK_installDHCP"></a>安装动态主机配置协议（DHCP）
可以使用此过程，通过“添加角色和功能向导”安装和配置 DHCP 服务器角色。

“Domain Admins”成员身份或同等身份是执行此过程的最低要求。

###### <a name="to-install-dhcp"></a>安装 DHCP 的步骤

1.  在 DHCP1 上，在 “服务器管理器”中，单击 **“管理”** ，然后单击 **“添加角色和功能”** 。 将打开“添加角色和功能向导”。

2.  在“开始之前”中单击“下一步”。

    > [!NOTE]
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”** ，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。

3.  在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。

4.  在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”** 。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步”。

5.  在 "**选择服务器角色**" 的 "**角色**" 中，选择**DHCP 服务器**。 在 **“添加DHCP 服务器所需的功能”** 中，单击 **“添加功能”** 。 单击“下一步”。

6.  在 **“选择功能”** 中，单击 **“下一步”** ，然后在 **“DHCP 服务器”** 中查看提供的信息，然后单击 **“下一步”** 。

7.  在 **“确认安装选择”** 中，单击 **“如果需要，自动重启目标服务器”** 。 当提示你确认这一选择时，单击 **“是”** ，然后单击 **“安装”** 。 安装**进度**页面显示安装过程中的状态。 此过程完成后，消息 "需要配置。 *Computername*上的安装成功 "，其中*ComputerName*是安装了 DHCP 服务器的计算机的名称。 在消息窗口中，单击 **“完成 DHCP 配置”** 。 这将打开DHCP 安装后配置向导。 单击“下一步”。

8.  在 **“授权”** 中，指定要用来给“Active Directory 域服务”中的 DHCP 服务器授权的凭据，然后单击 **“确认”** 。 授权完成后，请单击 **“关闭”** 。

##### <a name="BKMK_newscopeDHCP"></a>创建并激活一个新的 DHCP 作用域
可以使用此过程通过 DHCP Microsoft 管理控制台 (MMC) 新建 DHCP 作用域。 当完成这个过程后，作用域被激活，你创建的排除范围将阻止 DHCP 服务器出租你用来静态配置需要静态 IP 地址的服务器和其他设备。

**DHCP 管理员** 中的成员身份或等效身份是执行此过程所需的最低要求。

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>创建并激活一个新的 DHCP 作用域的步骤

1.  在 DHCP1 的服务器管理器中，单击 **“工具”** ，然后单击 **“DHCP”** 。 此时将打开 DHCP MMC。

2.  在**DHCP**中，展开服务器名称。 例如，如果 DHCP 服务器名称是 DHCP1.corp.contoso.com，请单击**DHCP1.corp.contoso.com**旁边的向下箭头。

3.  在 "服务器名称" 下，右键单击 " **IPv4**"，然后单击 "**新建作用域**"。 此时将打开新建作用域向导。

4.  在 **“欢迎使用新建作用域向导”** 中，单击 **“下一步”** 。

5.  在 **“作用域名称”** 的 **“名称”** 中，键入作用域的名称。 例如，键入 **子网 1**。

6.  在 **“描述”** 中，键入对新作用域的描述，然后单击 **“下一步”** 。

7.  在 **“IP 地址范围”** 中，执行以下操作：

    1.  在 **“起始 IP 地址”** 中，键入为范围中第一个 IP 地址的 IP 地址。 例如，键入 **10.0.0.1**。

    2.  在 **“结束 IP 地址”** 中，键入为范围中最后一个 IP 地址的 IP 地址。 例如，键入 " **10.0.0.254**"。 根据为 **“起始 IP 地址”** 输入的 IP 地址，会自动输入 **“长度”** 和 **“子网掩码”** 的值。

    3.  如有必要，修改 **“长度”** 或 **“子网掩码”** 的值以适合你的寻址方案。

    4.  单击“下一步”。

8.  在 **“添加排除”** 中，执行以下操作：

    1.  在 **“起始 IP 地址”** 中，键入为排除范围中第一个 IP 地址的 IP 地址。 例如，键入 **10.0.0.1**。

    2.  在 **“结束 IP 地址”** 中，键入排除范围中最后一个 IP 地址，例如键入 **10.0.0.15**。

9. 单击 **“添加”** ，然后单击 **“下一步”** 。

10. 在 **“租用期限”** 中，修改 **“天”** 、 **“小时”** 和 **“分钟”** 的默认值以适合你的网络，然后单击 **“下一步”** 。

11. 在 **“配置 DHCP 选项”** 中，选择 **“是，我想现在配置这些选项”** ，然后单击 **“下一步”** 。

12. 在 **“路由器(默认网关)”** 中，执行以下操作之一：

    -   如果网络上没有路由器，则单击 **“下一步”** 。

    -   在 **“IP 地址”** 中，键入路由器或默认网关的 IP 地址。 例如，键入 **10.0.0.1**。 单击 **“添加”** ，然后单击 **“下一步”** 。

13. 在 **“域名称和 DNS 服务器”** 中，执行以下操作之一：

    1.  在 **“父域”** 中，键入客户端用于名称解析的 DNS 域的名称。 例如，键入 **corp.contoso.com**。

    2.  在 **“服务器名称”** 中，键入客户端用于名称解析的 DNS 计算机的名称。 例如，键入 **DC1**。

    3.  单击 **“解析”** 。 DNS 服务器的 IP 地址便添加到 **“IP 地址”** 中。 单击 **“添加”** ，等候 DNS 服务器 IP 地址验证完成，然后单击 **“下一步”** 。

14. 如果网络上没有 WINS 服务器，则在 **“WINS 服务器”** 中单击 **“下一步”** 。

15. 在 **“激活作用域”** 中，选择 **“是，我要立刻激活这个作用域”** 。

16. 单击“下一步”，然后单击“完成”。

> [!IMPORTANT]
> 要为更多的子网创建新作用域，重复这一过程。 为要部署的每个子网使用不同的 IP 地址，并确保所有导向其他子网的路由器上，都启用了 DHCP 消息转发。

### <a name="BKMK_joinlogclients"></a>将客户端计算机加入域并登录

> [!NOTE]
> 要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并键入下面的 cmdlet，然后按 ENTER 键。 你同样必须将域名替换为你要使用的名字。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 按照提示，为一个有权限将计算机加入域的帐户键入用户名和密码。 若要重新启动计算机，键入以下命令，然后按 ENTER 键。
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>将运行 Windows 10 的计算机加入到域中

1.  使用本地 Administrator 帐户登录到计算机。

2.  在 **"搜索 web 和 Windows**" 中键入 " **System**"。 在搜索结果中，单击 "**系统（控制面板）** "。 这将打开“系统”对话框。

3.  在**系统**中，单击 "**高级系统设置**"。 这将打开“系统属性”对话框。 单击 "**计算机名称**" 选项卡。

4.  在 "**计算机名**" 中，单击 "**更改**"。 这将打开“计算机名/域更改”对话框。

5.  在 "**计算机名/域更改**" 的 "**成员**" 中，单击 "**域**"，然后键入要加入的域的名称。 例如，如果域名为 corp.contoso.com，则键入 **corp.contoso.com**。

6.  单击 **“确定”** 。 这将打开“Windows 安全性”对话框。

7.  在 **“计算机名/域更改”** 中的 **“用户名”** 中，键入用户名，并在 **“密码”** 中键入密码，然后单击 **“确定”** 。 将会打开 **“计算机名/域更改”** 对话框，欢迎你加入域。 单击 **“确定”** 。

8.  **“计算机名/域更改”** 对话框显示一条消息，表明必须重新启动计算机才能应用更改。 单击 **“确定”** 。

9. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“关闭”** 。 将会打开 **Microsoft Windows** 对话框并显示一条消息，再次表明必须重新启动计算机才能应用更改。 单击“立即重新启动”。

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>将运行 Windows 8.1 的计算机加入到域中

1.  使用本地 Administrator 帐户登录到计算机。

2.  右键单击 "**开始**"，然后单击 "**系统**"。 这将打开“系统”对话框。

3.  在**系统**中，单击 "**高级系统设置**"。 这将打开“系统属性”对话框。 单击 "**计算机名称**" 选项卡。

4.  在 "**计算机名**" 中，单击 "**更改**"。 这将打开“计算机名/域更改”对话框。

5.  在 "**计算机名/域更改**" 的 "**成员**" 中，单击 "**域**"，然后键入要加入的域的名称。 例如，如果域名为 corp.contoso.com，则键入 **corp.contoso.com**。

6.  单击 **“确定”** 。 这将打开“Windows 安全性”对话框。

7.  在 **“计算机名/域更改”** 中的 **“用户名”** 中，键入用户名，并在 **“密码”** 中键入密码，然后单击 **“确定”** 。 将会打开 **“计算机名/域更改”** 对话框，欢迎你加入域。 单击 **“确定”** 。

8.  **“计算机名/域更改”** 对话框显示一条消息，表明必须重新启动计算机才能应用更改。 单击 **“确定”** 。

9. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“关闭”** 。 将会打开 **Microsoft Windows** 对话框并显示一条消息，再次表明必须重新启动计算机才能应用更改。 单击“立即重新启动”。

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>使用运行 Windows 10 的计算机登录到域

1.  注销计算机，或重新启动计算机。

2.  按 Ctrl + Alt + Delete。 将会出现登录屏幕。

3.  在左下角，单击 "**其他用户**"。

4.  在 **“用户名”** 中，以 *域名\域用户名* 格式键入域和用户名。 例如，若要使用名为 **User-01** 的帐户登录到域 corp.contoso.com，则键入 **CORP\User-01**。

5.  在 **“密码”** 中，键入域密码，然后单击箭头，或按 ENTER 键。

### <a name="BKMK_optionalfeatures"></a>为网络访问身份验证和 Web 服务部署可选功能
如果要部署网络访问服务器（如无线访问点或 VPN 服务器），则在安装核心网络后，建议同时部署 NPS 和 Web 服务器。 对于网络访问部署，建议使用基于安全证书的身份验证方法。 可以用 NPS 管理网络访问策略并部署安全身份验证方法。 可以用 Web 服务器发布提供安全身份验证所需证书的证书颁发机构 (CA) 的证书吊销列表 (CRL)。

> [!NOTE]
> 可以使用“核心网络助理指南”部署服务器证书和其他附加功能。 有关详细信息，请参阅[其他技术资源](#BKMK_resources)。

下图显示了添加了 NPS 和 Web 服务器的 Windows Server Core 网络拓扑。

![添加了 NPS 和 Web 服务器的 Windows Server Core 网络拓扑](../media/Core-Network-Guide/cng16_overview_2.jpg)

下面的部分提供了向网络添加 NPS 和 Web 服务器的有关信息。

-   [部署 NPS1](#BKMK_deployNPS1)

-   [部署 WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>部署 NPS1
安装网络策略服务器 (NPS) 服务器，将其作为部署其他网络访问技术 [如虚拟专用网络 (VPN) 服务器、无线访问点以及 802.1X 身份验证交换机] 的预备步骤。

网络策略服务器（NPS）允许你通过以下功能集中配置和管理网络策略：远程身份验证拨入用户服务（RADIUS）服务器和 RADIUS 代理。

NPS 是核心网络的可选组件，但如果下列任何一项为真，则应安装 NPS：

-   你计划将网络扩展为包含与 RADIUS 协议兼容的远程访问服务器，例如运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的计算机，以及路由和远程访问服务、终端服务网关或远程桌面网关。


-   你计划为有线或无线访问部署 802.1 X 身份验证。

部署此角色服务之前，必须在要配置为 NPS 的计算机上执行以下步骤。

-   执行[配置所有服务器](#BKMK_configuringAll)部分中描述的步骤。

-   执行[“将服务器计算机加入域并登录”](#BKMK_joinlogserver)部分中描述的步骤。

若要部署 NPS1 [运行“网络策略和访问服务”服务器角色的“网络策略服务器 (NPS)”角色服务的计算机]，必须完成此步骤：

-   [规划 NPS1 的部署](#bkmk_NetFndtn_Pln_NPS-01)

-   [安装网络策略服务器（NPS）](#BKMK_installNPS)

-   [在默认域中注册 NPS](#BKMK_registerNPS)

> [!NOTE]
> 本指南说明如何在名为 NPS1 的独立服务器或 VM 上部署 NPS。  另一种建议的部署模型是在域控制器上安装 NPS。 如果希望在域控制器而不是独立服务器上安装 NPS，请在 DC1 上安装 NPS。

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>规划 NPS1 的部署
如果想部署网络访问服务器（如无线访问点或 VPN 服务器），则在部署基础网络之后，建议你部署 NPS。

将 NPS 用作远程身份验证拨入用户服务 (RADIUS) 服务器时，NPS 通过网络访问服务器对连接请求执行身份验证和授权。 NPS 还允许你集中配置和管理网络策略，这些网络策略确定谁可以访问网络、他们如何访问网络以及何时可以访问网络。

下面是安装 NPS 之前的主要规划步骤。

- 规划用户帐户数据库。 默认情况下，如果将运行 NPS 的服务器加入到 Active Directory 域中，则 NPS 将使用 AD DS 用户帐户数据库执行身份验证和授权。 某些情况下，例如将 NPS 用作 RADIUS 代理将连接请求转发给其他 RADIUS 服务器的大型网络，可能希望将 NPS 安装在非域成员的计算机上。

- 规划 RADIUS 记帐。 NPS 允许你将记帐数据记录到 SQL Server 数据库或记录到本地计算机上的文本文件。 如果想使用 SQL Server 日志记录，请规划运行 SQL Server 的服务器的安装和配置。

##### <a name="BKMK_installNPS"></a>安装网络策略服务器（NPS）
您可以使用此过程通过 "添加角色和功能向导" 来安装网络策略服务器（NPS）。 NPS 是网络策略和访问服务服务器角色的一个角色服务。

> [!NOTE]
> 默认情况下，NPS 在所有已安装的网络适配器上侦听端口 1812、1813、1645 和 1646 上的 RADIUS 流量。 如果在安装 NPS 时启用了具有高级安全性的 Windows 防火墙，则在安装过程中会自动为这些端口创建防火墙例外，同时为 Internet 协议版本 6 @no__t 0IPv6 @ no__t 和 IPv4 通信。 如果网络访问服务器被配置为通过这些默认端口之外的端口发送 RADIUS 流量，请在安装 NPS 期间删除在 "高级安全 Windows 防火墙" 中创建的例外，并为用于的端口创建例外。RADIUS 流量。

**管理凭据**

若要完成此过程，你必须是**Domain Admins**组的成员。

> [!NOTE]
> 要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并键入下列内容，然后按 ENTER 键。
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>安装 NPS 的步骤

1.  在 NPS1 上，在 “服务器管理器”中，单击 **“管理”** ，然后单击 **“添加角色和功能”** 。 将打开“添加角色和功能向导”。

2.  在“开始之前”中单击“下一步”。

    > [!NOTE]
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”** ，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。

3.  在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。

4.  在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”** 。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步”。

5.  在 "**选择服务器角色**" 的 "**角色**" 中，选择 "**网络策略和访问服务**"。 此时将打开一个对话框，询问是否应添加网络策略和访问服务所需的功能。 单击“添加功能”，然后单击“下一步”。

6.  在**选择功能**中，单击 **“下一步”** ，然后在 **“网络策略和访问服务”** 中查看提供的信息，然后单击 **“下一步”** 。

7.  在**选择角色服务**中，单击 **“网络策略服务器”** 。  在**添加“网络策略服务器”需要的功能**中，单击 **“添加功能”** 。 单击“下一步”。

8.  在 **“确认安装选择”** 中，单击 **“如果需要，自动重启目标服务器”** 。 当提示你确认这一选择时，单击 **“是”** ，然后单击 **“安装”** 。 在安装进程期间，安装进度页显示状态。 此过程完成后，将显示消息 " *computername*上安装成功"，其中*ComputerName*是安装了网络策略服务器的计算机的名称。 单击 **“关闭”** 。

##### <a name="BKMK_registerNPS"></a>在默认域中注册 NPS
您可以使用此过程在服务器是域成员的域中注册 NPS。

必须在 Active Directory 中注册 NPSs，使其在授权过程中有权读取用户帐户的拨入属性。 注册 NPS 会将服务器添加到 Active Directory 中的 " **RAS 和 IAS 服务器**" 组。

**管理凭据**

若要完成此过程，你必须是**Domain Admins**组的成员。

> [!NOTE]
> 若要在 Windows PowerShell 中使用 network shell （Netsh）命令执行此过程，请打开 PowerShell 并键入以下命令，然后按 ENTER。
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-in-its-default-domain"></a>在其默认域中注册 NPS

1.  在 NPS1 的服务器管理器中，单击“工具”，然后单击 **“网络策略服务器”** 。 打开“网络策略服务器”MMC。

2.  右键单击 **“NPS（本地）”** ，然后单击 **“在 Active Directory 中注册服务器”** 。 打开 **“网络策略服务器”** 对话框。

3.  在 **“网络策略服务器”** 中，单击 **“确定”** ，然后再次单击 **“确定”** 。

有关网络策略服务器的详细信息，请参阅[网络策略服务器（NPS）](../technologies/nps/nps-top.md)。

#### <a name="BKMK_IIS"></a>部署 WEB1

Windows Server 2016 中的 Web 服务器（IIS）角色提供一个安全、易于管理的模块化和可扩展的平台，以可靠地托管网站、服务和应用程序。 使用 Internet Information Services （IIS），您可以与 Internet、intranet 或 extranet 上的用户共享信息。 IIS 是一个集成了 IIS、ASP.NET、FTP 服务、PHP 和 Windows Communication Foundation （WCF）的统一 web 平台。

除了允许您发布 CRL 以供域成员计算机访问以外，Web 服务器（IIS）服务器角色还允许您设置和管理多个网站、Web 应用程序和 FTP 站点。 IIS 还提供以下优势：

-   通过减少服务器资源占用和自动应用程序隔离，最大程度地保证 web 安全。

-   轻松地在同一个服务器上部署和运行 ASP.NET、经典 ASP 和 PHP web 应用程序。

-   通过默认情况下赋予工作进程唯一的标识和带沙盒安全机制的配置，实现应用程序隔离，进一步降低安全风险。

-   能够轻松添加、删除、甚至使用自定义模块替换内置的 IIS 组件，特别符合客户需求。

-   通过内置的动态缓存和增强压缩提高网站的速度。

要部署运行“Web 服务器 (IIS)”服务器角色的计算机 WEB1，必须执行以下操作：

-   执行[配置所有服务器](#BKMK_configuringAll)部分中描述的步骤。

-   执行[“将服务器计算机加入域并登录”](#BKMK_joinlogserver)部分中描述的步骤。

-   [安装 Web 服务器（IIS）服务器角色](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>安装 Web 服务器（IIS）服务器角色
若要完成此过程，你必须是**Administrators**组的成员。

> [!NOTE]
> 要使用 Windows PowerShell 执行这一过程，打开 PowerShell 并键入下列内容，然后按 ENTER 键。
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  在 **“服务器管理器”** 中，单击 **“管理”** ，然后单击 **“添加角色和功能”** 。 将打开“添加角色和功能向导”。

2.  在“开始之前”中单击“下一步”。

    > [!NOTE]
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”** ，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。

3.  在 "**选择安装类型**" 页上，单击 "**下一步**"。

4.  在 "**选择目标服务器**" 页上，确保选中 "本地计算机"，然后单击 "**下一步**"。

5.  在 "**选择服务器角色**" 页上，滚动到，然后选择 " **WEB 服务器（IIS）** "。 此时将打开 " **Web 服务器（IIS）所需的添加功能**" 对话框。 单击“添加功能”，然后单击“下一步”。

6.  单击 **“下一步”** 直到接受了所有默认的 web 服务器设置，然后单击 **“安装”** 。

7.  验证是否所有安装都已成功，然后单击“关闭”。

## <a name="BKMK_resources"></a>其他技术资源
有关本指南中讨论的技术的详细信息，请参见以下资源：

 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 技术库资源

-   [Windows Server 2016 Active Directory 域服务（AD DS）中的新增功能](https://technet.microsoft.com/library/mt163897.aspx)

-   [Active Directory 域服务概述](https://technet.microsoft.com/library/hh831484.aspx)@no__t。

-   @No__t-1 的[域名系统（DNS）概述](https://technet.microsoft.com/library/hh831667.aspx)。

-   [实现 DNS 管理员角色](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [动态主机配置协议（DHCP）概述](https://technet.microsoft.com/library/hh831825.aspx)@no__t 为-1。

-   [网络策略和访问服务概述](https://technet.microsoft.com/library/hh831683.aspx)https://technet.microsoft.com/library/hh831683.aspx 。

-   @No__t-1 的[Web 服务器（IIS）概述](https://technet.microsoft.com/library/hh831725.aspx)。

## <a name="BKMK_appendix"></a>附录 A 到 E
对于运行 Windows Server 2016、Windows 10、Windows Server 2012 和 Windows 8 以外的操作系统的计算机，以下部分包含其他配置信息。 此外，还提供了一个网络准备工作表来帮助您进行部署。

1.  [附录 A-重命名计算机](#BKMK_A)

2.  [附录 B-配置静态 IP 地址](#BKMK_B)

3.  [附录 C-将计算机加入域](#BKMK_C)

4.  [附录 D-登录到域](#BKMK_D)

5.  [附录 E-核心网络规划准备工作表](#BKMK_E)

## <a name="BKMK_A"></a>附录 A-重命名计算机
你可以使用本部分中的过程来向运行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的计算机提供不同的计算机名称。

-   [Windows Server 2008 R2 和 Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 和 Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 和 Windows 7
Administrators组成员或同等身份是执行这些过程的最低要求。

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>重命名运行 Windows Server 2008 R2 和 Windows 7 的计算机的步骤

1.  单击“开始”、右键单击“计算机”，然后单击“属性”。 这将打开“系统”对话框。

2.  在“计算机名称、域和工作组设置”下，单击“更改设置”。 这将打开“系统属性”对话框。

    > [!NOTE]
    > 在运行 Windows 7 的计算机上，在 "**系统属性**" 对话框打开之前，将打开 "**用户帐户控制**" 对话框，并请求权限继续。 单击“继续”以继续。

3.  单击“更改”。 这将打开“计算机名/域更改”对话框。

4.  在 **“计算机名称”** 中，键入计算机的名称。 例如，如果要将计算机命名为 DC1，请键入 **DC1**。

5.  单击两次“确定”，单击“关闭”，然后单击“立即重新启动”以重新启动计算机。

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 和 Windows Vista
Administrators组成员或同等身份是执行这些过程的最低要求。

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>重命名运行 Windows Server 2008 和 Windows Vista 的计算机

1.  单击“开始”、右键单击“计算机”，然后单击“属性”。 这将打开“系统”对话框。

2.  在“计算机名称、域和工作组设置”下，单击“更改设置”。 这将打开“系统属性”对话框。

    > [!NOTE]
    > 在运行 Windows Vista 的计算机上，在 "**系统属性**" 对话框打开之前，将打开 "**用户帐户控制**" 对话框，并请求权限继续。 单击“继续”以继续。

3.  单击“更改”。 这将打开“计算机名/域更改”对话框。

4.  在 **“计算机名称”** 中，键入计算机的名称。 例如，如果要将计算机命名为 DC1，请键入 **DC1**。

5.  单击两次“确定”，单击“关闭”，然后单击“立即重新启动”以重新启动计算机。

## <a name="BKMK_B"></a>附录 B-配置静态 IP 地址
本主题提供在运行以下操作系统的计算机上配置静态 IP 地址的过程：

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
Administrators组成员或同等身份是执行此过程的最低要求。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>在运行 Windows Server 2008 R2 的计算机上配置静态 IP 地址的步骤

1.  单击 **「开始」** ，然后单击 **“控制面板”** 。

2.  在“控制面板”中，单击“网络和 Internet”。 这将打开“网络和 Internet”。

    在“网络和 Internet”中，单击“网络和共享中心”。 这将打开“网络和共享中心”。

3.  在“网络和共享中心”中，单击“更改适配器设置”。 这将打开“网络连接”。

4.  在“网络连接”中，右键单击要配置的网络连接，然后单击“属性”。

5.  在“本地连接属性”的“此连接使用下列项目”中，选择“Internet 协议版本 4 (TCP/IPv4)”，然后单击“属性”。 这将打开“Internet 协议版本 4 (TCP/IPv4) 属性”对话框。

6.  在“Internet 协议版本 4 (TCP/IPv4) 属性”中的“常规”选项卡上，单击“使用下面的 IP 地址”。 在 **“IP 地址”** 中，键入你要使用的 IP 地址。

7.  按 Tab 以将光标移到“子网掩码”中。 子网掩码的默认值将自动输入。 可以接受该默认的子网掩码，也可以键入要使用的子网掩码。

8.  在 **“默认网关”** 中，键入默认网关的 IP 地址。

9. 在 **“首选 DNS 服务器”** 中，键入 DNS 服务器的 IP 地址。 如果计划使用本地计算机作为首选 DNS 服务器，则键入本地计算机的 IP 地址。

10. 在 **“备用 DNS 服务器”** 中，键入备用 DNS 服务器的 IP 地址（如果有）。 如果计划使用本地计算机作为备用 DNS 服务器，则键入本地计算机的 IP 地址。

11. 单击“确定”，然后单击“关闭”。

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
Administrators组成员或同等身份是执行这些过程的最低要求。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>在运行 Windows Server 2008 的计算机上配置静态 IP 地址的步骤

1.  单击 **「开始」** ，然后单击 **“控制面板”** 。

2.  在 **“控制面板”** 中，验证是否选择了 **“经典视图”** ，然后双击 **“网络和共享中心”** 。

3.  在 **“网络和共享中心”** 的 **“任务”** 中，单击 **“管理网络连接”** 。

4.  在“网络连接”中，右键单击要配置的网络连接，然后单击“属性”。

5.  在“本地连接属性”的“此连接使用下列项目”中，选择“Internet 协议版本 4 (TCP/IPv4)”，然后单击“属性”。 这将打开“Internet 协议版本 4 (TCP/IPv4) 属性”对话框。

6.  在“Internet 协议版本 4 (TCP/IPv4) 属性”中的“常规”选项卡上，单击“使用下面的 IP 地址”。 在 **“IP 地址”** 中，键入你要使用的 IP 地址。

7.  按 Tab 以将光标移到“子网掩码”中。 子网掩码的默认值将自动输入。 可以接受该默认的子网掩码，也可以键入要使用的子网掩码。

8.  在 **“默认网关”** 中，键入默认网关的 IP 地址。

9. 在 **“首选 DNS 服务器”** 中，键入 DNS 服务器的 IP 地址。 如果计划使用本地计算机作为首选 DNS 服务器，则键入本地计算机的 IP 地址。

10. 在 **“备用 DNS 服务器”** 中，键入备用 DNS 服务器的 IP 地址（如果有）。 如果计划使用本地计算机作为备用 DNS 服务器，则键入本地计算机的 IP 地址。

11. 单击“确定”，然后单击“关闭”。

## <a name="BKMK_C"></a>附录 C-将计算机加入域
你可以使用这些过程将运行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的计算机加入域。

-   [Windows Server 2008 R2 和 Windows 7](#BKMK_c1)

-   [Windows Server 2008 和 Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> 若要将计算机加入域，必须使用本地 Administrator 帐户登录到计算机，或者，如果使用不具有本地计算机管理凭据的用户帐户登录到计算机，则必须在将计算机加入域的过程中提供本地 Administrator 帐户的凭据。 此外，还必须在要将计算机加入其中的域中拥有一个用户帐户。 在将计算机加入域的过程中，系统将提示你提供域帐户凭据（用户名和密码）。

### <a name="BKMK_c1"></a>Windows Server 2008 R2 和 Windows 7
**Domain Users** 中的成员身份或同等身份是执行此过程所需的最低要求。

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>将运行 Windows Server 2008 R2 和 Windows 7 的计算机加入到域中

1.  使用本地 Administrator 帐户登录到计算机。

2.  单击“开始”、右键单击“计算机”，然后单击“属性”。 这将打开“系统”对话框。

3.  在“计算机名称、域和工作组设置”下，单击“更改设置”。 这将打开“系统属性”对话框。

    > [!NOTE]
    > 在运行 Windows 7 的计算机上，在 "**系统属性**" 对话框打开之前，将打开 "**用户帐户控制**" 对话框，并请求权限继续。 单击“继续”以继续。

4.  单击“更改”。 这将打开“计算机名/域更改”对话框。

5.  在 **“计算机名称”** 中的 **“隶属于”** 中，选择 **“域”** ，然后键入要加入的域的名称。 例如，如果域名为 corp.contoso.com，则键入 **corp.contoso.com**。

6.  单击 **“确定”** 。 这将打开“Windows 安全性”对话框。

7.  在 **“计算机名/域更改”** 中的 **“用户名”** 中，键入用户名，并在 **“密码”** 中键入密码，然后单击 **“确定”** 。 将会打开 **“计算机名/域更改”** 对话框，欢迎你加入域。 单击 **“确定”** 。

8.  **“计算机名/域更改”** 对话框显示一条消息，表明必须重新启动计算机才能应用更改。 单击 **“确定”** 。

9. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“关闭”** 。 将会打开 **Microsoft Windows** 对话框并显示一条消息，再次表明必须重新启动计算机才能应用更改。 单击“立即重新启动”。

### <a name="BKMK_c2"></a>Windows Server 2008 和 Windows Vista
**Domain Users** 中的成员身份或同等身份是执行此过程所需的最低要求。

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>将运行 Windows Server 2008 和 Windows Vista 的计算机加入到域中

1.  使用本地 Administrator 帐户登录到计算机。

2.  单击“开始”、右键单击“计算机”，然后单击“属性”。 这将打开“系统”对话框。

3.  在“计算机名称、域和工作组设置”下，单击“更改设置”。 这将打开“系统属性”对话框。

4.  单击“更改”。 这将打开“计算机名/域更改”对话框。

5.  在 **“计算机名称”** 中的 **“隶属于”** 中，选择 **“域”** ，然后键入要加入的域的名称。 例如，如果域名为 corp.contoso.com，则键入 **corp.contoso.com**。

6.  单击 **“确定”** 。 这将打开“Windows 安全性”对话框。

7.  在 **“计算机名/域更改”** 中的 **“用户名”** 中，键入用户名，并在 **“密码”** 中键入密码，然后单击 **“确定”** 。 将会打开 **“计算机名/域更改”** 对话框，欢迎你加入域。 单击 **“确定”** 。

8.  **“计算机名/域更改”** 对话框显示一条消息，表明必须重新启动计算机才能应用更改。 单击 **“确定”** 。

9. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“关闭”** 。 将会打开 **Microsoft Windows** 对话框并显示一条消息，再次表明必须重新启动计算机才能应用更改。 单击“立即重新启动”。

## <a name="BKMK_D"></a>附录 D-登录到域
你可以使用这些过程，使用运行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的计算机登录到域。

-   [Windows Server 2008 R2 和 Windows 7](#BKMK_d1)

-   [Windows Server 2008 和 Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 和 Windows 7
**Domain Users** 中的成员身份或同等身份是执行此过程所需的最低要求。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>使用运行 Windows Server 2008 R2 和 Windows 7 的计算机登录到域

1.  注销计算机，或重新启动计算机。

2.  按 Ctrl + Alt + Delete。 将会出现登录屏幕。

3.  单击 **“切换用户”** ，然后单击 **“其他用户”** 。

4.  在 **“用户名”** 中，以 *域名\域用户名* 格式键入域和用户名。 例如，若要使用名为 **User-01** 的帐户登录到域 corp.contoso.com，则键入 **CORP\User-01**。

5.  在 **“密码”** 中，键入域密码，然后单击箭头，或按 ENTER 键。

### <a name="BKMK_d2"></a>Windows Server 2008 和 Windows Vista
**Domain Users** 中的成员身份或同等身份是执行此过程所需的最低要求。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>使用运行 Windows Server 2008 和 Windows Vista 的计算机登录到域

1.  注销计算机，或重新启动计算机。

2.  按 Ctrl + Alt + Delete。 将会出现登录屏幕。

3.  单击 **“切换用户”** ，然后单击 **“其他用户”** 。

4.  在 **“用户名”** 中，以 *域名\域用户名* 格式键入域和用户名。 例如，若要使用名为 **User-01** 的帐户登录到域 corp.contoso.com，则键入 **CORP\User-01**。

5.  在 **“密码”** 中，键入域密码，然后单击箭头，或按 ENTER 键。

## <a name="BKMK_E"></a>附录 E-核心网络规划准备工作表
可以使用此网络规划准备页收集安装核心网络所需的信息。 本主题提供了几个表，这些表包含每台服务器计算机的各个配置项，在安装或配置过程中必须为它们提供相关信息或特定值。 为每个配置项提供了示例值。

为了进行规划和跟踪，每个表中都为你提供了输入部署时所使用值的空间。 如果在这些表中记录与安全有关的值，则应将此信息存储在安全位置。

以下链接会将你引导至本主题中的各个部分，这些部分将提供与本指南中所提供的部署过程关联的配置项和示例值。

1.  [安装 Active Directory 域服务和 DNS](#BKMK_FndtnPrep_InstallAD)

    -   [配置 DNS 反向查找区域](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [安装 DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [在 DHCP 中创建排除范围](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [创建新的 DHCP 作用域](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [安装网络策略服务器（可选）](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>安装 Active Directory 域服务和 DNS
此部分中的表列出了用于预安装和安装 Active Directory 域服务（AD DS）和 DNS 的配置项目。

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>AD DS 和 DNS 的预安装配置项目
下列表格列出了[“配置所有服务器”](#BKMK_configuringAll)中所述的预安装配置项：

-   [配置静态 IP 地址](#BKMK_ip)

|配置项|示例值|值|
|-----------------------|------------------|----------|
|IP 地址|10.0.0.2||
|子网掩码|255.255.255.0||
|默认网关|10.0.0.1||
|首选 DNS 服务器|127.0.0.1||
|备用 DNS 服务器|10.0.0.15||

-   [重命名计算机](#BKMK_rename)

|配置项|示例值|ReplTest1|
|----------------------|-----------------|---------|
|计算机名称|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>AD DS 和 DNS 安装配置项目
Windows Server 核心网络部署过程[为新林安装 AD DS 和 DNS](#BKMK_installAD-DNS) 的配置项：

|配置项|示例值|值|
|-----------------------|------------------|----------|
|完整 DNS 名|corp.contoso.com||
|林功能级别|Windows Server 2003||
|“Active Directory 域服务”数据库文件夹位置|E:\Configuration @ no__t-0<br /><br />或接受默认位置。||
|“Active Directory 域服务”日志文件文件夹位置|E:\Configuration @ no__t-0<br /><br />或接受默认位置。||
|Active Directory 域服务 SYSVOL 文件夹位置|E:\Configuration @ no__t-0<br /><br />或接受默认位置||
|目录还原模式的管理员密码|J*p2leO4$F||
|答案文件名（可选）|AD DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>配置 DNS 反向查找区域

|配置项|示例值|值|
|-----------------------|------------------|----------|
|区域类型：|-主区域<br />-辅助区域<br />-存根区域||
|区域类型<br /><br />**将区域存储在 Active Directory**|-选定<br />-未选择||
|Active Directory 区域传送作用域|-到此林中的所有 DNS 服务器<br />-到此域中的所有 DNS 服务器<br />-到此域中的所有域控制器<br />-到在此目录分区范围内指定的所有域控制器||
|反向查找区域名称<br /><br />（IP 类型）|-IPv4 反向查找区域<br />-IPv6 反向查找区域||
|反向查找区域名称<br /><br />（网络 ID）|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>安装 DHCP
此部分中的表列出了 DHCP 的预安装配置项和安装配置项。

##### <a name="pre-installation-configuration-items-for-dhcp"></a>DHCP 的预安装配置项
下列表格列出了[“配置所有服务器”](#BKMK_configuringAll)中所述的预安装配置项：

-   [配置静态 IP 地址](#BKMK_ip)

|配置项|示例值|值|
|-----------------------|------------------|----------|
|IP 地址|10.0.0.3||
|子网掩码|255.255.255.0||
|默认网关|10.0.0.1||
|首选 DNS 服务器|10.0.0.2||
|备用 DNS 服务器|10.0.0.15||

-   [重命名计算机](#BKMK_rename)

|配置项|示例值|ReplTest1|
|----------------------|-----------------|---------|
|计算机名称|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>DHCP 安装配置项
Windows Server 核心网络部署过程[安装动态主机配置协议 (DHCP)](#BKMK_installDHCP) 的配置项：

|配置项|示例值|值|
|-----------------------|------------------|----------|
|网络连接绑定|Ethernet||
|DNS 服务器设置|DC1||
|首选 DNS 服务器 IP 地址|10.0.0.2||
|备用 DNS 服务器 IP 地址|10.0.0.15||
|作用域名称|Corp1||
|起始 IP 地址|10.0.0.1||
|结束 IP 地址|10.0.0.254||
|子网掩码|255.255.255.0||
|默认网关（可选）|10.0.0.1||
|租用期限|8天||
|IPv6 DHCP 服务器操作模式|未启用||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>在 DHCP 中创建排除范围
配置项以在 DHCP 中创建作用域时，创建一个排除范围。

|配置项|示例值|值|
|-----------------------|------------------|----------|
|作用域名称|Corp1||
|作用域描述|主办公室子网 1||
|排除范围起始 IP 地址|10.0.0.1||
|排除范围结束 IP 地址|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>创建新的 DHCP 作用域
Windows Server 核心网络部署过程[新建并激活 DHCP 作用域](#BKMK_newscopeDHCP)的配置项：

|配置项|示例值|值|
|-----------------------|------------------|----------|
|新作用域名称|Corp2||
|作用域描述|总公司子网2||
|（IP 地址范围）<br /><br />起始 IP 地址|10.0.1.1||
|（IP 地址范围）<br /><br />结束 IP 地址|10.0.1.254||
|长度|8||
|子网掩码|255.255.255.0||
|（排除范围）起始 IP 地址|10.0.1.1||
|排除范围结束 IP 地址|10.0.1.15||
|租用期限<br /><br />天<br /><br />小时<br /><br />分钟|-8<br />-   0<br />-   0||
|路由器（默认网关）<br /><br />IP 地址|10.0.1.1||
|DNS 父域|corp.contoso.com||
|DNS 服务器<br /><br />IP 地址|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>安装网络策略服务器（可选）
此部分中的表列出了 NPS 的预安装配置项和安装配置项。

##### <a name="pre-installation-configuration-items"></a>预安装配置项
以下三个表列出了[配置所有服务器](#BKMK_configuringAll)中所述的预安装配置项：

-   [配置静态 IP 地址](#BKMK_ip)

|配置项|示例值|值|
|-----------------------|------------------|----------|
|IP 地址|10.0.0.4||
|子网掩码|255.255.255.0||
|默认网关|10.0.0.1||
|首选 DNS 服务器|10.0.0.2||
|备用 DNS 服务器|10.0.0.15||

-   [重命名计算机](#BKMK_rename)

|配置项|示例值|ReplTest1|
|----------------------|-----------------|---------|
|计算机名称|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>网络策略服务器安装配置项
Windows Server 核心网络 NPS 部署过程的配置项目[安装网络策略服务器（NPS）](#BKMK_installNPS)并[在默认域中注册 NPS](#BKMK_registerNPS)。

-   安装和注册 NPS 不需要其他配置项。

