---
title: 步骤1规划基本 DirectAccess 基础结构
description: 本主题是使用 Windows Server 2016 的入门向导部署单个 DirectAccess 服务器指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f4c727dc8f7905502d47119bd0e911537e827aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404873"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>步骤1规划基本 DirectAccess 基础结构
在单台服务器上进行基本 DirectAccess 部署的第一步是针对部署所需的基础结构进行规划。 本主题介绍基础结构规划步骤：  
  
|任务|描述|  
|----|--------|  
|规划网络拓扑和设置|确定在何处放置 DirectAccess 服务器 \(at 边缘，或在网络地址转换后 \(NAT @ no__t，并规划 IP 寻址和路由。|  
|规划防火墙要求|规划允许 DirectAccess 通过边缘防火墙。|  
|规划证书要求|DirectAccess 可以使用 Kerberos 或证书进行客户端身份验证。 在这一基础 DirectAccess 部署中，将自动配置 Kerberos 代理并使用 Active Directory 凭据完成身份验证。|  
|规划 DNS 要求|规划 DirectAccess 服务器、基础结构服务器和客户端连接性的 DNS 设置。|  
|规划 Active Directory|规划域控制器和 Active Directory 要求。|  
|规划组策略对象|确定你的组织中需要哪些 GPO，以及如何创建或编辑这些 GPO。|  
  
不需要按照特定顺序完成这些规划任务。  
  
## <a name="bkmk_1_1_Network_svr_top_settings"></a>规划网络拓扑和设置  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>规划网络适配器和 IP 寻址  
  
1.  标识你打算使用的网络适配器拓扑。 可以使用下列任一操作来设置 DirectAccess：  
  
    -   具有两个网络适配器-其中一个网络适配器连接到 Internet，另一个网络适配器连接到内部网络，另一个网络适配器连接到外围网络，另一个网络适配器连接到外围网络，另一个网络适配器连接到外围网络，另一个网络适配器连接到内部网络网桥.  
  
    -   在具有一个网络适配器的 NAT 设备后面-DirectAccess 服务器安装在 NAT 设备的后面，并且单个网络适配器连接到内部网络。  
  
2.  标识 IP 寻址要求：  
  
    DirectAccess 使用 IPv6 和 IPsec 在 DirectAccess 客户端计算机和内部企业网络之间创建安全连接。 但是，DirectAccess 不一定需要连接到 IPv6 Internet 或内部网络上的本机 IPv6 支持。 相反，它会自动配置并使用 IPv6 转换技术在 IPv4 Internet \(6to4、Teredo、IP @ no__t-1HTTPS @ no__t 之间以及 IPv4 @ no__t-3only intranet \(NAT64 或 ISATAP @ no__t-5 之间建立 IPv6 流量的隧道。 有关这些转换技术的概述，请参阅以下资源：  
  
    -   [IPv6 转换技术](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Ip-https 隧道协议规范](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  按下表配置所需的适配器和寻址。 对于使用单个网络适配器的 NAT 设备后面的部署，仅使用**内部网络适配器**列配置 IP 地址。  
  
    ||外部网络适配器|内部网络适配器<sup>1</sup>|路由要求|  
    |-|--------------|--------------------|------------|  
    |IPv4 Intranet 和 IPv4 Internet|配置以下内容：<br /><br />-一个具有相应子网掩码的静态公用 IPv4 地址。<br />-Internet 防火墙或本地 Internet 服务提供商的默认网关 IPv4 地址 \(ISP @ no__t 路由器。|配置以下内容：<br /><br />-具有相应子网掩码的 IPv4 intranet 地址。<br />-A connection @ no__t-0specific DNS 后缀的 intranet 命名空间。 还必须在内部接口上配置 DNS 服务器。<br />-不要在任何 intranet 接口上配置默认网关。|若要配置 DirectAccess 服务器以访问内部 IPv4 网络上的所有子网，请执行以下操作：<br /><br />1.列出 Intranet 上所有位置的 IPv4 地址空间。<br />2.使用**路由 add \- p**或**netsh interface ipv4 add Route**命令将 ipv4 地址空间添加为 DirectAccess 服务器 ipv4 路由表中的静态路由。|  
    |IPv6 Internet 和 IPv6 Intranet|配置以下内容：<br /><br />-使用 ISP 提供的自动配置地址配置。<br />-使用**route print**命令，以确保指向 ISP 路由器的默认 ipv6 路由存在于 IPv6 路由表中。<br />-确定 ISP 和 intranet 路由器是否使用 RFC 4191 中所述的默认路由器首选项，并使用比本地 intranet 路由器更高的默认首选项。 如果两个结果都为“是”，则默认路由不需要任何其他配置。 用于 ISP 路由器的更高级首选项可确保 DirectAccess 服务器的活动默认 IPv6 路由指向 IPv6 Internet。<br /><br />因为 DirectAccess 服务器是一个 IPv6 路由器，所以如果你具有本机 IPv6 基础结构，则 Internet 接口也可以访问 Intranet 上的域控制器。 在这种情况下，将数据包筛选器添加到外围网络中的域控制器，阻止连接到 DirectAccess 服务器的 Internet @ no__t-0facing 接口的 IPv6 地址。|配置以下内容：<br /><br />-如果使用的不是默认首选项级别，请使用**netsh interface ipv6 Set InterfaceIndex ignoredefaultroutes @ no__t-1enabled**命令来配置 intranet 接口。 此命令可确保不会将指向 Intranet 路由器的其他默认路由添加到 IPv6 路由表。 你可以从 netsh 接口显示接口命令的显示中获得 Intranet 接口的 InterfaceIndex。|如果你拥有 IPv6 Intranet，若要配置 DirectAccess 服务器以访问所有的 IPv6 位置，请执行以下操作：<br /><br />1.列出你 Intranet 上所有位置的 IPv6 地址空间。<br />2.使用 **netsh interface ipv6 add route** 命令将 IPv6 地址空间添加为 DirectAccess 服务器的 IPv6 路由表中的静态路由。|  
    |IPv4 Internet 和 IPv6 Intranet|在 IPv4 Internet 上，DirectAccess 服务器使用 Microsoft 6to4 适配器接口将默认 IPv6 路由通信转发到 6to4 中继。 你可以在 IPv4 Internet 上为 Microsoft 6to4 中继的 IPv4 地址配置 DirectAccess 服务器 @no__t-在未使用以下命令在企业网络中部署本机 IPv6 时 0used-1： netsh interface IPv6 6to4 set 中继 name @ no__2192.88.99.1 state @ no__t-3enabled 命令。|||  
  
    > [!NOTE]  
    > 请注意以下事项：  
    >   
    > 1.  如果已为 DirectAccess 客户端分配公用 IPv4 地址，则它将使用 6to4 转换技术连接到 Intranet。 如果 DirectAccess 客户端无法使用6to4 连接到 DirectAccess 服务器，则它将使用 IP @ no__t-0HTTPS。  
    > 2.  本机 IPv6 客户端计算机可以通过本机 IPv6 连接到 DirectAccess 服务器，而无需转换技术。  
  
### <a name="ConfigFirewalls"></a>规划防火墙要求  
如果 DirectAccess 服务器位于边缘防火墙后面，则当 DirectAccess 服务器位于 IPv4 Internet 上时，要进行 DirectAccess 通信还需要以下例外：  
  
-   6to4 流量-IP 协议41入站和出站。  
  
-   IP @ no__t-0HTTPS-传输控制协议 \(TCP @ no__t-2 目标端口443，TCP 源端口443出站。  
  
-   如果你要使用单个网络适配器部署 DirectAccess，并且要在 DirectAccess 服务器上安装网络位置服务器，则还应免除 TCP 端口 62000。  
  
    > [!NOTE]  
    > 此例外位于 DirectAccess 服务器上。 所有其他例外都位于边缘防火墙上。  
  
当 DirectAccess 服务器位于 IPv6 Internet 上时，要进行 DirectAccess 通信将需要以下例外：  
  
-   IP 协议 50  
  
-   UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
当使用其他防火墙时，为 DirectAccess 通信应用以下内部网络防火墙例外：  
  
-   ISATAP-协议41入站和出站  
  
-   所有 IPv4 @ no__t-1IPv6 流量的 TCP @ no__t-0UDP  
  
### <a name="bkmk_1_2_CAs_and_certs"></a>规划证书要求  
IPsec 的证书要求包括 DirectAccess 客户端计算机在客户端和 DirectAccess 服务器之间建立 IPsec 连接时使用的计算机证书，以及 DirectAccess 服务器用于建立与 DirectAccess 客户端的 IPsec 连接的计算机证书。 对于 Windows Server 2012 R2 和 Windows Server 2012 中的 DirectAccess，不强制使用这些 IPsec 证书。 入门向导将 DirectAccess 服务器配置为充当 Kerberos 代理来执行 IPsec 身份验证，而无需证书。
  
1.  **IP @ no__t-1HTTPS 服务器**。 配置 DirectAccess 时，DirectAccess 服务器将自动配置为充当 IP @ no__t-0HTTPS web 侦听器。 IP @ no__t-0HTTPS 站点需要网站证书，并且客户端计算机必须能够联系证书的证书吊销列表 \(CRL @ no__t 站点。 启用 DirectAccess 向导会尝试使用 SSTP VPN 证书。 如果未配置 SSTP，它会检查计算机个人存储区中是否存在 IP @ no__t-0HTTPS 的证书。 如果没有可用的，它会自动创建自 @ no__t-0signed 证书。
  
2.  **网络位置服务器**。 网络位置服务器是一个用于检测客户端计算机是否位于企业网络中的网站。 网络位置服务器需要网站证书。 DirectAccess 客户端必须能够联系该证书的 CRL 站点。 "启用远程访问" 向导会检查计算机个人存储区中是否存在网络位置服务器的证书。 如果不存在，它将自动创建一个自 @ no__t-0signed 证书。
  
下表中总结了其中每项的证书要求：  
  
|IPsec 身份验证|IP @ no__t-0HTTPS 服务器|网络位置服务器|  
|------------|----------|--------------|  
|当你未使用 Kerberos 代理进行身份验证时，需要内部 CA 向 DirectAccess 服务器和客户端颁发计算机证书，以进行 IPsec 身份验证|公共 CA-建议使用公共 CA 颁发 IP @ no__t-0HTTPS 证书，这可确保 CRL 分发点在外部可用。|内部 CA-可以使用内部 CA 颁发网络位置服务器网站证书。 请确保 CRL 分发点在内部网络中高度可用。|  
||内部 CA-可以使用内部 CA 颁发 IP @ no__t-0HTTPS 证书;但是，必须确保 CRL 分发点在外部可用。|自行 @ no__t-0signed 证书-你可以为网络位置服务器网站使用自 @ no__t-1signed 证书;但是，不能在多站点部署中使用自 @ no__t-2signed 证书。|  
||Self @ no__t-0signed 证书-你可以对 IP @ no__t-2HTTPS 服务器使用一个自 @ no__t-1signed 证书;但是，必须确保 CRL 分发点在外部可用。 无法在多站点部署中使用自 @ no__t-0signed 证书。||  
  
#### <a name="bkmk_website_cert_IPHTTPS"></a>规划 IP @ no__t-1HTTPS 和网络位置服务器的证书  
如果你想要针对这些目的设置证书，请参阅[使用高级设部署置单个 DirectAccess 服务器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 如果没有可用的证书，入门向导会自动为这些目的创建 no__t-0signed 证书。
  
> [!NOTE]
> 如果手动预配 IP @ no__t-0HTTPS 和网络位置服务器的证书，请确保证书具有使用者名称。 如果该证书不包含使用者名称，但它包含备用名称，则 DirectAccess 向导将不会接受它。  
  
#### <a name="plan-dns-requirements"></a>规划 DNS 要求  
在 DirectAccess 部署中，以下方面将需要 DNS：  
  
-   **DirectAccess 客户端请求**。 DNS 用于解析来自不位于内部网络上的 DirectAccess 客户端计算机的请求。 DirectAccess 客户端尝试连接到 DirectAccess 网络位置服务器，以确定它们是位于 Internet 上，还是位于企业网络上。如果连接成功，则确定客户端在 Intranet 上且未使用 DirectAccess，并使用客户端计算机的网络适配器上配置的 DNS 服务器解析客户端请求。 如果该连接不成功，则假定客户端在 Internet 上。 DirectAccess 客户端将使用名称解析策略表 \(NRPT @ no__t）来确定在解析名称请求时使用哪个 DNS 服务器。 你可以指定客户端应使用 DirectAccess DNS64 或备用的内部 DNS 服务器来解析名称。 在执行名称解析时，将由 DirectAccess 客户端使用 NRPT 来确定如何处理请求。 客户端请求 FQDN 或单个 @ no__t-0label 名称，如 http： \/ @ no__t-2internal。 如果单个 @ no__t-0label 名称为 "请求"，则将追加 DNS 后缀以生成 FQDN。 如果 DNS 查询与 NRPT 中某个条目匹配，并且为该条目指定了 DNS4 或 Intranet DNS 服务器，则将使用指定的服务器发送用于名称解析的 DNS 查询。 如果存在匹配条目，但未指定 DNS 服务器，这表示存在一条免除规则，且将应用普通的名称解析。  
  
    将新后缀添加到 DirectAccess 管理控制台中的 NRPT 后，可通过单击“检测”按钮自动发现该后缀的默认 DNS 服务器。 自动检测的工作原理如下：  
  
    1.  如果企业网络是 IPv4 @ no__t-0based，或者是 IPv4 和 IPv6，则默认地址是 DirectAccess 服务器上内部适配器的 DNS64 地址。  
  
    2.  如果企业网络是 IPv6 @ no__t-0based，则默认地址是企业网络中 DNS 服务器的 IPv6 地址。  
  
-   **基础结构服务器**
  
    1.  **网络位置服务器**。 DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析该网络位置服务器的名称，但当它们位于 Internet 上时，必须阻止它们解析该名称。 为了确保这一点，默认情况下，网络位置服务器的 FQDN 将作为免除规则添加到 NRPT。 此外，在你配置 DirectAccess 时，将自动创建以下规则：  
  
        1.  根域的 DNS 后缀规则或 DirectAccess 服务器的域名，以及与 DirectAccess 服务器上配置的 Intranet DNS 服务器相对应的 IPv6 地址。 例如，如果 DirectAccess 服务器是 corp.contoso.com 域的成员，则会为 corp.contoso.com DNS 后缀创建一条规则。  
  
        2.  用于网络位置服务器的 FQDN 的免除规则。 例如，如果网络位置服务器 URL 是 https： \/\/nls.corp.contoso.com，则会为 FQDN nls.corp.contoso.com 创建一条免除规则。  
  
        **IP @ no__t-1HTTPS 服务器**。 DirectAccess 服务器充当 IP @ no__t-0HTTPS 侦听器，并使用其服务器证书向 IP @ no__t-1HTTPS 客户端进行身份验证。 使用公共 DNS 服务器的 DirectAccess 客户端必须能够解析 IP @ no__t-0HTTPS 名称。  
  
        **连接性验证程序**。 DirectAccess 将创建可由 DirectAccess 客户端计算机用来验证到内部网络的连接的默认 Web 探测。 若要确保探测按预期运行，则必须在 DNS 中手动注册以下名称：  
  
        1.  directaccess @ no__t-应解析为 DirectAccess 服务器的内部 IPv4 地址，或解析为 IPv6 @ no__t 1only 环境中的 IPv6 地址。  
  
        2.  directaccess @ no__t-0corpconnectivityhost-应解析为 localhost \(loopback @ no__t-2 地址。 应创建 A 和 AAAA 记录，A 记录具有值 127.0.0.1，AAAA 记录具有由 NAT64 前缀构造的值，且最后 32 位类似于 127.0.0.1。 可以通过运行 cmdlet get @ no__t-0netnattransitionconfiguration 来检索 NAT64 前缀。  
  
        你可以通过 HTTP 或 PING 使用其他 Web 地址创建其他连接性验证程序。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
#### <a name="dns-server-requirements"></a>DNS 服务器要求  
  
-   对于 DirectAccess 客户端，必须使用运行 Windows Server 2008、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2、Windows Server 2016 或支持 IPv6 的任何 DNS 服务器的 DNS 服务器。  
  
> [!NOTE]  
> 部署 DirectAccess 时，不建议使用运行 Windows Server 2003 的 DNS 服务器。 尽管 Windows Server 2003 DNS 服务器支持 IPv6 记录，但 Microsoft 不再支持 Windows Server 2003。 此外，如果域控制器因为文件复制服务的问题而运行 Windows Server 2003，则不应部署 DirectAccess。 有关详细信息，请参阅 [DirectAccess 不受支持的配置](../DirectAccess-Unsupported-Configurations.md)。  
  
### <a name="bkmk_1_4_NLS"></a>规划网络位置服务器  
网络位置服务器是一个用于检测 DirectAccess 客户端是否位于企业网络中的网站。 企业网络中的客户端不会使用 DirectAccess 访问内部资源，相反，它们会直接进行连接。  
  
入门向导会自动设置 DirectAccess 服务器上的网络位置服务器，而且当你部署 DirectAccess 时会自动创建网站。 这样，无需使用证书基础结构即可进行简单安装。
  
如果要部署网络位置服务器而不使用 no__t-0signed 证书，请参阅[使用高级设置部署单个 DirectAccess 服务器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。
  
### <a name="bkmk_1_6_AD"></a>规划 Active Directory  
DirectAccess 使用 Active Directory 和 Active Directory 组策略对象，如下所示：
  
-   **身份验证**。 Active Directory 用于身份验证。 DirectAccess 隧道使用 Kerberos 身份验证以使用户访问内部资源。
  
-   **组策略对象**。 DirectAccess 将配置设置收集到已应用于 DirectAccess 服务器和客户端的组策略对象中。
  
-   **安全组**。 DirectAccess 使用安全组将 DirectAccess 客户端计算机和 DirectAccess 服务器收集到一起并标识它们。 组策略将应用于所需的安全组。

**Active Directory 要求**  
  
在规划用于 DirectAccess 部署的 Active Directory 时，需要满足以下条件：  
  
-   至少在 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 上安装了一个域控制器。 
  
    如果域控制器位于外围网络中 \(and 因此可从 DirectAccess 服务器 @ no__t 的 Internet @ no__t-1facing 网络适配器访问，通过在域上添加数据包筛选器来阻止 DirectAccess 服务器访问它控制器，用于阻止连接到 Internet 适配器的 IP 地址。  
  
-   DirectAccess 服务器必须是域成员。  
  
-   DirectAccess 客户端必须是域成员。 客户端可以属于：  
  
    -   与 DirectAccess 服务器位于同一林中的任何域。  
  
    -   与 DirectAccess 服务器域具有两个 @ no__t-0way 信任的任何域。  
  
    -   与 DirectAccess 域所属的林之间具有两个 @ no__t-0way 信任的林中的任何域。  
  
> [!NOTE]
> - DirectAccess 服务器不可以是域控制器。  
> - 无法从 DirectAccess 服务器的外部 Internet 适配器访问用于 DirectAccess 的 Active Directory 域控制器 \(the 适配器不得位于 Windows 防火墙 @ no__t 的域配置文件中。  
  
### <a name="bkmk_1_7_GPOs"></a>规划组策略对象  
在配置 DirectAccess 时配置的 DirectAccess 设置将收集到组策略对象 \(GPO @ no__t-1 中。 使用 DirectAccess 设置填充两个不同的 GPO，并通过以下方式进行分发：  
  
-   **DirectAccess 客户端 GPO**。 此 GPO 包含客户端设置，包括 IPv6 转换技术设置、NRPT 条目和高级安全 Windows 防火墙连接安全规则。 将 GPO 应用于为客户端计算机指定的安全组。  
  
-   **DirectAccess 服务器 GPO**。 此 GPO 包含的 DirectAccess 配置设置适用于在部署中配置为 DirectAccess 服务器的任一服务器。 它还包含高级安全 Windows 防火墙连接安全规则。  
  
可以通过两种方式配置 GPO：  
  
1.  **自动**。 可以指定自动创建它们。 为每个 GPO 指定默认名称。 通过入门向导自动创建 GPO。
  
2.  **手动**。 可以使用已由 Active Directory 管理员预定义的 GPO。
  
请注意，将 DirectAccess 配置为使用特定的 GPO 后，无法将它配置为使用不同的 GPO。
  
> [!IMPORTANT]
> 无论你使用自动或手动配置的 GPO，只要你的客户端将使用 3G，你就必须添加用于慢速链接检测的策略。 @No__t 0Policy 的组策略路径：配置组策略慢速链接检测 @ no__t 为：**计算机配置 \/ 策略 \/ 管理模板 \/ 系统**@no__t 组策略。  
  
> [!CAUTION]  
> 在执行 DirectAccess cmdlet 之前，请使用以下过程来备份所有 DirectAccess 组策略对象：[备份和还原 DirectAccess 配置](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>自动 no__t-0created Gpo  
使用 "自动 no__t-0created" Gpo 时，请注意以下事项：  
  
将根据位置和链接目标参数应用自动创建的 GPO，如下所示：  
  
-   对于 DirectAccess 服务器 GPO，位置和链接参数都指向包含 DirectAccess 服务器的域。  
  
-   创建客户端 GPO 后，将该位置设置为将在其中创建 GPO 的单个域。 在每个域中查找 GPO 名称，如果存在，则使用 DirectAccess 设置对其进行填充。 将链接目标设置为在其中创建了 GPO 的域的根。 为每个包含客户端计算机的域创建一个 GPO，并且该 GPO 将链接到各自域的根。  
  
在使用自动创建的 GPO 时，若要应用 DirectAccess 设置，DirectAccess 服务器管理员需要以下权限：  
  
-   每个域的 GPO 创建权限。  
  
-   所有选定客户端域根的链接权限。  
  
-   服务器 GPO 域根的链接权限。  
  
-   GPO 需要创建、编辑、删除和修改安全权限。  
  
-   我们建议 DirectAccess 管理员具有对每个所需的域的 GPO 读取权限。 这使 DirectAccess 可在创建 GPO 时验证不存在具有相同名称的 GPO。  
  
请注意，如果不存在用于链接 GPO 的正确权限，则会发出警告。 DirectAccess 操作将继续进行，但不会发生链接。 如果发出此警告，则即使之后添加了权限，也不会自动创建链接。 管理员将需要手动创建链接。  
  
#### <a name="manually-created-gpos"></a>手动 no__t-0created Gpo  
使用手动 @ no__t-0created Gpo 时，请注意以下事项：  
  
-   在运行远程访问入门向导之前，应存在这些 GPO。  
  
-   使用手动 @ no__t-0created Gpo 时，若要应用 DirectAccess 设置，DirectAccess 管理员需要对手动 @ no__t-3created Gpo 使用完全 GPO 权限，\(Edit、Delete、Modify security @ no__t。  
  
-   在使用手动创建的 GPO 时，将在整个域中搜索到 GPO 的链接。 如果未在域中链接 GPO，将在域的根中自动创建链接。 如果未提供创建链接所需的权限，则会发出警告。  
  
请注意，如果不存在用于链接 GPO 的正确权限，则会发出警告。 DirectAccess 操作将继续进行，但不会发生链接。 如果发出此警告，则即使之后添加了权限，也不会自动创建链接。 管理员将需要手动创建链接。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>从已删除的 GPO 中恢复  
如果已意外删除 DirectAccess 服务器、客户端或应用程序服务器 GPO，并且没有可用的备份，则必须删除配置设置，并再次重试 @ no__t-0configure。 如果有可用的备份，则可以从备份中还原 GPO。  
  
“DirectAccess 管理”将显示以下错误消息：**找不到 GPO <GPO name>** 。 若要删除配置设置，请执行以下步骤：  
  
1.  运行 PowerShell cmdlet **Uninstall @ no__t-1remoteaccess**。  
  
2.  Re @ no__t-0open **DirectAccess Management**。  
  
3.  你将看到关于未找到 GPO 的错误消息。 单击“删除配置设置”。 完成后，服务器将还原到 "非 @ no__t-0configured" 状态。  
  
### <a name="BKMK_Links"></a>下一步  
  
-   [步骤 2：规划基础 DirectAccess 部署](da-basic-plan-s2-deployment.md)  
  
