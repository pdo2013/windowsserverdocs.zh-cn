---
title: 步骤1规划远程访问基础结构
description: 本主题是在 Windows Server 2016 中远程管理 DirectAccess 客户端的指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c8db30d3c5512fc72648c7894d66b715850fb619
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367315"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>步骤1规划远程访问基础结构

>适用于：Windows Server（半年频道）、Windows Server 2016

> [!NOTE]
> Windows Server 2016 将 DirectAccess 和路由和远程访问服务（RRAS）合并到单个远程访问角色中。  
  
本主题介绍规划基础结构的步骤，你可以使用这些步骤设置单个远程访问服务器以远程管理 DirectAccess 客户端。 下表列出了这些步骤，但不需要按特定顺序完成这些规划任务。  
  
|任务|描述|  
|----|--------|  
|[规划网络拓扑和服务器设置](#plan-network-topology-and-settings)|确定远程访问服务器的位置（在边缘或网络地址转换（NAT）设备或防火墙后面），并规划 IP 寻址和路由。|  
|[规划防火墙要求](#plan-firewall-requirements)|规划允许远程访问通过边缘防火墙。|  
|[规划证书要求](#plan-certificate-requirements)|决定是使用 Kerberos 协议还是证书进行客户端身份验证，并规划网站证书。<br/><br/>IP-HTTPS 是一种转换协议，DirectAccess 客户端使用该协议在 IPv4 网络上对 IPv6 通信进行隧道传送。 决定是使用由证书颁发机构（CA）颁发的证书，还是使用远程访问服务器自动颁发的自签名证书对服务器的 ip-https 进行身份验证。|  
|[规划 DNS 要求](#plan-dns-requirements)|规划远程访问服务器、基础结构服务器、本地名称解析选项和客户端连接的域名系统（DNS）设置。| 
|[规划网络位置服务器配置](#plan-the-network-location-server-configuration)|确定将网络位置服务器网站放置在组织中的位置（在远程访问服务器上或备用服务器上），如果网络位置服务器位于远程访问服务器上，则规划证书要求。 **注意：** DirectAccess 客户端使用网络位置服务器来确定它们是否位于内部网络上。|  
|[规划管理服务器的配置](#plan-management-servers-configuration)|规划在远程客户端管理过程中使用的管理服务器（如更新服务器）。 **注意：** 管理员可以使用 Internet 远程管理位于企业网络之外的 DirectAccess 客户端计算机。|  
|[规划 Active Directory 要求](#plan-active-directory-requirements)|规划你的域控制器、你的 Active Directory 要求、客户端身份验证和多个域结构。|  
|[规划组策略对象创建](#plan-group-policy-object-creation)|确定你的组织中需要哪些 Gpo，以及如何创建和编辑 Gpo。|  
  
## <a name="plan-network-topology-and-settings"></a>规划网络拓扑和设置  
规划网络时，需要考虑网络适配器拓扑、IP 寻址设置和 ISATAP 要求。  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>规划网络适配器和 IP 寻址  
  
1.  确定要使用的网络适配器拓扑。 可以使用下列任一拓扑设置远程访问：  
  
    -   具有两个网络适配器：远程访问服务器安装在边缘，其中一个网络适配器连接到 Internet，另一个网络适配器连接到内部网络。  
  
    -   具有两个网络适配器：远程访问服务器安装在 NAT 设备、防火墙或路由器后面，其中一个网络适配器连接到外围网络，另一个网络适配器连接到内部网络。  
  
    -   具有一个网络适配器：远程访问服务器安装在 NAT 设备的后面，并且单个网络适配器连接到内部网络。  
  
2.  标识 IP 寻址要求：  
  
    DirectAccess 使用 IPv6 和 IPsec 在 DirectAccess 客户端计算机和内部企业网络之间创建安全连接。 但是，DirectAccess 不一定需要连接到 IPv6 Internet 或内部网络上的本机 IPv6 支持。 相反，它会自动配置并使用 IPv6 转换技术在 IPv4 Internet 上（6to4、Teredo 或 ip-https）和仅限 IPv4 的 intranet （NAT64 或 ISATAP）对 IPv6 通信进行隧道传输。 有关这些转换技术的概述，请参阅以下资源：  
  
    -   [IPv6 转换技术](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Ip-https 隧道协议规范](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  按下表配置所需的适配器和寻址。 对于使用单个网络适配器的 NAT 设备后面的部署，仅使用**内部网络适配器**列配置 IP 地址。  
  
    ||外部网络适配器|内部网络适配器<sup>1，以上</sup>|路由要求|  
    |-|--------------|------------------------|------------|  
    |IPv4 Internet 和 IPv4 Intranet|配置以下内容：<br/><br/>-具有相应子网掩码的两个静态连续公用 IPv4 地址（仅对 Teredo 是必需的）。<br/>-Internet 防火墙或本地 Internet 服务提供商（ISP）路由器的默认网关 IPv4 地址。 **注意：** 远程访问服务器需要两个连续的公用 IPv4 地址，以便它可用作 Teredo 服务器，基于 Windows 的 Teredo 客户端可以使用远程访问服务器来检测 NAT 设备的类型。|配置以下内容：<br/><br/>-具有相应子网掩码的 IPv4 intranet 地址。<br/>-Intranet 命名空间的特定于连接的 DNS 后缀。 还应在内部接口上配置 DNS 服务器。 **警告：** 不要在任何 Intranet 接口上配置默认网关。|若要配置远程访问服务器以访问内部 IPv4 网络上的所有子网，请执行以下操作：<br/><br/>-列出 intranet 上所有位置的 IPv4 地址空间。<br/>-使用 `route add -p` 或 `netsh interface ipv4 add route` 命令将 IPv4 地址空间添加为远程访问服务器的 IPv4 路由表中的静态路由。|  
    |IPv6 Internet 和 IPv6 Intranet|配置以下内容：<br/><br/>-使用 ISP 提供的自动配置地址配置。<br/>-使用 `route print` 命令可确保指向 ISP 路由器的默认 IPv6 路由存在于 IPv6 路由表中。<br/>-确定 ISP 和 intranet 路由器是否使用 RFC 4191 中所述的默认路由器首选项，以及它们使用的默认首选项比本地 intranet 路由器更高。 如果两个结果都为“是”，则默认路由不需要任何其他配置。 ISP 路由器的更高首选等级可确保远程访问服务器的活动默认 IPv6 路由指向 IPv6 Internet。<br/><br/>因为远程访问服务器是一个 IPv6 路由器，所以如果你具有本机 IPv6 基础结构，则 Internet 接口也可以访问 Intranet 上的域控制器。 在这种情况下，将数据包筛选器添加到外围网络中的域控制器，阻止连接到远程访问服务器的 Internet 接口的 IPv6 地址。|配置以下内容：<br/><br/>如果使用的不是默认首选项级别，请使用 `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled` 命令来配置 intranet 接口。 这一命令可确保不会将指向 Intranet 路由器的其他默认路由添加到 IPv6 路由表。 你可以从 @no__t 的显示中获得 intranet 接口的 InterfaceIndex。|如果你拥有 IPv6 Intranet，若要配置远程访问服务器以访问所有的 IPv6 位置，请执行以下操作：<br/><br/>-列出 intranet 上所有位置的 IPv6 地址空间。<br/>-使用 `netsh interface ipv6 add route` 命令将 IPv6 地址空间添加为远程访问服务器的 IPv6 路由表中的静态路由。|  
    |IPv4 Internet 和 IPv6 Intranet|远程访问服务器使用 Microsoft 6to4 适配器接口将默认的 IPv6 路由流量转发到 IPv4 Internet 上的6to4 中继。 在企业网络中未部署本机 IPv6 时，可以使用以下命令为 IPv4 Internet 上的 Microsoft 6to4 中继的 IPv4 地址配置远程访问服务器： `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`。|||  
  
    > [!NOTE]  
    > -   如果已为 DirectAccess 客户端分配公用 IPv4 地址，则它将使用6to4 中继技术连接到 intranet。 如果为客户端分配了专用 IPv4 地址，则它将使用 Teredo。 如果 DirectAccess 客户端无法使用 6to4 或 Teredo 连接到 DirectAccess 服务器，则它将使用 IP-HTTPS。  
    > -   若要使用 Teredo，必须在面向外部的网络适配器上配置两个连续的 IP 地址。  
    > -   如果远程访问服务器只有一个网络适配器，则不能使用 Teredo。  
    > -   本机 IPv6 客户端计算机可以通过本机 IPv6 连接到远程访问服务器，而无需任何转换技术。  
  
### <a name="plan-isatap-requirements"></a>规划 ISATAP 要求  
远程管理 DirectAccessclients 需要 ISATAP，以便 DirectAccess 管理服务器可以连接到位于 Internet 上的 DirectAccess 客户端。 ISATAP 不需要支持 DirectAccess 客户端计算机启动到企业网络上的 IPv4 资源的连接。 为此，将使用 NAT64/DNS64。 如果你的部署需要 ISATAP，请使用下表来确定你的要求。  
  
|ISATAP 部署方案|要求|  
|---------------|--------|  
|现有的本机 IPv6 intranet （无需 ISATAP）|使用现有的本机 IPv6 基础结构，可以在远程访问部署过程中指定组织的前缀，远程访问服务器不会将自身配置为 ISATAP 路由器。 请执行以下操作：<br/><br/>1.若要确保可从 intranet 访问 DirectAccess 客户端，必须修改 IPv6 路由，以便将默认路由流量转发到远程访问服务器。 如果 intranet IPv6 地址空间使用的地址不是单个48位 IPv6 地址前缀，则必须在部署过程中指定相关的组织 IPv6 前缀。<br/>2.如果当前已连接到 IPv6 Internet，则必须配置默认路由流量，使其转发到远程访问服务器，然后在远程访问服务器上配置适当的连接和路由，以便默认路由流量将转发到连接到 IPv6 Internet 的设备。|  
|现有 ISATAP 部署|如果你有现有的 ISATAP 基础结构，则在部署过程中，系统会提示你输入组织的48位前缀，并且远程访问服务器不会将自身配置为 ISATAP 路由器。 若要确保可从 intranet 访问 DirectAccess 客户端，必须修改 IPv6 路由基础结构，以便将默认路由流量转发到远程访问服务器。 需要在要将默认流量转发到 intranet 客户端的现有 ISATAP 路由器上完成此更改。|  
|无现有 IPv6 连接|当远程访问设置向导检测到服务器没有基于本机或 ISATAP 的 IPv6 连接时，它会自动为 intranet 派生基于6to4 的48位前缀，并将远程访问服务器配置为 ISATAP 路由器以提供 IPv6跨 intranet 连接到 ISATAP 主机。 （仅当服务器具有公用地址时才使用基于6to4 的前缀，否则将从唯一的本地地址范围自动生成前缀。）<br/><br/>若要使用 ISATAP，请执行以下操作：<br/><br/>1.在 DNS 服务器上为你要启用基于 ISATAP 的连接的每个域注册 ISATAP 名称，以便内部 DNS 服务器可将该 ISATAP 名称解析为远程访问服务器的内部 IPv4 地址。<br/>2.默认情况下，通过使用全局查询块列表，运行 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 块解析的 DNS 服务器。 若要启用 ISATAP，你必须从阻止列表中删除 ISATAP 名称。 有关详细信息，请参阅[从 DNS 全局查询阻止列表中删除 ISATAP](https://go.microsoft.com/fwlink/p/?LinkId=168593)。<br/><br/>可以解析 ISATAP 名称的基于 Windows 的 ISATAP 主机自动使用远程访问服务器配置一个地址，如下所示：<br/><br/>1.ISATAP 隧道接口上基于 ISATAP 的 IPv6 地址<br/>2.向 intranet 上的其他 ISATAP 主机提供连接的64位路由<br/>3.指向远程访问服务器的默认 IPv6 路由。 默认路由确保 intranet ISATAP 主机可以访问 DirectAccess 客户端<br/><br/>当基于 Windows 的 ISATAP 主机获得基于 ISATAP 的 IPv6 地址时，如果目标也是 ISATAP 主机，则它们会开始使用 ISATAP 封装的流量进行通信。 由于 ISATAP 对整个 intranet 使用单个64位子网，因此，通信将从分段的 IPv4 通信模型到使用 IPv6 的单个子网通信模型。 这可能会影响某些 Active Directory 域服务（AD DS）和依赖于 Active Directory 站点和服务配置的应用程序的行为。 例如，如果使用 "Active Directory 站点和服务" 管理单元来配置站点、基于 IPv4 的子网，以及用于将请求转发到站点中的服务器的站点间传输，则 ISATAP 主机不使用此配置。<br/><br/><ol><li>若要配置 Active Directory 站点和服务，以便在 ISATAP 主机内的站点中转发，请为每个 IPv4 子网对象配置一个等效的 IPv6 子网对象，在该对象中，子网的 IPv6 地址前缀表示同一范围的 ISATAP 主机作为 IPv4 子网的地址。 例如，对于 IPv4 子网 192.168.99.0/24 和64位 ISATAP 地址前缀2002：836b：1：8000：：/64，IPv6 子对象的等效 IPv6 地址前缀为2002：836b：1：8000：0：5efe： 192.168.99.0/120。 对于任意 IPv4 前缀长度（在示例中设置为24），可以从公式 96 + IPv4PrefixLength 确定相应的 IPv6 前缀长度。</li><li>对于 DirectAccess 客户端的 IPv6 地址，请添加以下内容：<br/><br/><ul><li>对于基于 Teredo 的 DirectAccess 客户端：2001年范围的 IPv6 子网：0： WWXX： YYZZ：：/64，其中 WWXX： YYZZ 是远程访问服务器的第一个面向 Internet 的 IPv4 地址的冒号十六进制版本。 .</li><li>对于基于 IP-HTTPS 的 DirectAccess 客户端：用于2002范围的 IPv6 子网： WWXX： YYZZ：8100：：/56，其中 WWXX： YYZZ 是远程访问服务器的第一个面向 Internet 的 IPv4 地址（w.x.y.z）的冒号十六进制版本。 .</li><li>对于基于6to4 的 DirectAccess 客户端：以2002开头的一系列基于6to4 的 IPv6 前缀，表示由 Internet 编号分配机构（IANA）和区域注册表管理的区域公用 IPv4 地址前缀。 公用 IPv4 地址前缀的基于6to4 的前缀为2002： WWXX： YYZZ：：/[16 + n]，其中 WWXX： YYZZ 是的冒号十六进制版本<br/><br/>        例如，7.0.0.0/8 范围由美国注册表管理，用于北美的 Internet 号码（ARIN）。 此公共 IPv6 地址范围对应的基于6to4 的前缀为2002:700::/24。 有关 IPv4 公用地址空间的信息，请参阅[IANA IPv4 地址空间注册表](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml)。 .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> 确保 DirectAccess 服务器的内部接口上没有公共 IP 地址。 如果内部接口上有公共 IP 地址，则通过 ISATAP 的连接可能会失败。  
  
### <a name="plan-firewall-requirements"></a>规划防火墙要求  
如果远程访问服务器位于边缘防火墙后面，则当远程访问服务器位于 IPv4 Internet 上时，远程访问通信还需要以下例外：  
  
-   对于 IP-HTTPS：传输控制协议（TCP）目标端口443和 TCP 源端口443出站。  
  
-   对于 Teredo 流量：用户数据报协议（UDP）目标端口3544入站，UDP 源端口3544出站。  
  
-   对于6to4 流量：IP 协议41入站和出站。  
  
    > [!NOTE]  
    > 对于 Teredo 和 6to4 通信，这些例外应适用于远程访问服务器上两个面向 Internet 的连续公用 IPv4 地址。  
    >   
    > 对于 IP-HTTPS，需要将例外应用于在公共 DNS 服务器上注册的地址。  
  
-   如果要使用单个网络适配器部署远程访问，并在远程访问服务器上安装网络位置服务器，请使用 TCP 端口62000。  
  
    > [!NOTE]  
    > 此例外位于远程访问服务器上，而以前的免除位于边缘防火墙上。  
  
当远程访问服务器位于 IPv6 Internet 上时，远程访问通信需要以下例外：  
  
-   IP 协议 50  
  
-   UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
-   ICMPv6 流量入站和出站（仅在使用 Teredo 时）。  
  
使用其他防火墙时，请为远程访问通信应用以下内部网络防火墙例外：  
  
-   对于 ISATAP：协议41入站和出站  
  
-   对于所有 IPv4/IPv6 通信：TCP/UD  
  
-   对于 Teredo：所有 IPv4/IPv6 通信的 ICMP  
  
### <a name="plan-certificate-requirements"></a>规划证书要求  
部署单个远程访问服务器时，有三种方案需要证书。  
  
-   **IPsec 身份验证**：IPsec 的证书要求包括 DirectAccess 客户端计算机在与远程访问服务器建立 IPsec 连接时使用的计算机证书，以及由远程访问服务器用于建立连接的计算机证书。与 DirectAccess 客户端的 IPsec 连接。  
  
    对于 Windows Server 2012 中的 DirectAccess，不强制使用这些 IPsec 证书。 作为替代方法，远程访问服务器可以充当 Kerberos 身份验证的代理，而无需证书。 如果使用 Kerberos 身份验证，则它通过 SSL 工作，并且 Kerberos 协议使用为 ip-https 配置的证书。 某些企业方案（包括多站点部署和一次性密码客户端身份验证）需要使用证书身份验证而不是 Kerberos 身份验证。  
  
-   Ip-https**服务器**：当你配置远程访问时，远程访问服务器将自动配置为充当 IP-HTTPS web 侦听器。 IP-HTTPS 站点需要网站证书，并且客户端计算机必须能够联系该证书的证书吊销列表 (CRL) 站点。  
  
-   **网络位置服务器**：网络位置服务器是一个用于检测客户端计算机是否位于企业网络中的网站。 网络位置服务器需要网站证书。 DirectAccess 客户端必须能够联系该证书的 CRL 站点。  
  
下表总结了每个方案的证书颁发机构（CA）要求。  
  
|IPsec 身份验证|IP-HTTPS 服务器|网络位置服务器|  
|------------|----------|--------------|  
|当你未使用 Kerberos 协议进行身份验证时，需要内部 CA 向远程访问服务器和客户端颁发计算机证书，以进行 IPsec 身份验证。|内部 CA：可以使用内部 CA 颁发 IP-HTTPS 证书；但是，必须确保 CRL 分发点在外部可用。|内部 CA：可以使用内部 CA 颁发网络位置服务器网站证书。 请确保 CRL 分发点在内部网络中高度可用。|  
||自签名证书：可以为 ip-https 服务器使用自签名证书。 无法在多站点部署中使用自签名证书。|自签名证书：你可以为网络位置服务器网站使用自签名证书;但是，不能在多站点部署中使用自签名证书。|  
||公共 CA：建议使用公共 CA 颁发 IP-HTTPS 证书，这可确保 CRL 分发点在外部可用。||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>规划用于 IPsec 身份验证的计算机证书  
如果你使用的是基于证书的 IPsec 身份验证，则需要使用远程访问服务器和客户端来获取计算机证书。 若要安装证书，最简单的方法是使用组策略来配置计算机证书的自动注册。 此方法将确保所有域成员都从企业 CA 获取证书。 如果你的组织中未设置企业 CA，请参阅 [Active Directory 证书服务](https://technet.microsoft.com/library/cc770357.aspx)。  
  
此证书具有以下要求：  
  
-   该证书应具有客户端身份验证扩展密钥用法（EKU）。  
  
-   客户端和服务器证书应关联到相同的根证书。 必须在 DirectAccess 配置设置中选择该根证书。  
  
#### <a name="plan-certificates-for-ip-https"></a>规划 IP-HTTPS 的证书  
远程访问服务器充当 IP-HTTPS 侦听器，而且你必须在服务器上手动安装 HTTPS 网站证书。 规划时，请考虑以下内容：  
  
-   建议使用公共 CA，以便可以随时使用 CRL。  
  
-   在 "使用者" 字段中，指定远程访问服务器的 Internet 适配器的 IPv4 地址，或 IP-HTTPS URL （ConnectTo 地址）的 FQDN。 如果远程访问服务器位于 NAT 设备后面，则应指定 NAT 设备的公用名或地址。  
  
-   该证书的公用名应与 IP-HTTPS 站点的名称相匹配。  
  
-   对于 "**增强型密钥用法**" 字段，请使用服务器身份验证对象标识符（OID）。  
  
-   对于“CRL 分发点”字段，请指定已连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
    > [!NOTE]  
    > 这仅适用于运行 Windows 7 的客户端。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   必须直接将 IP-HTTPS 证书导入到个人存储中。  
  
-   IP-HTTPS 证书可以在名称中包含通配符符。  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>规划用于网络位置服务器的网站证书  
在规划网络位置服务器网站时，请注意以下事项：  
  
-   在“使用者”字段中，指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。  
  
-   对于 "**增强型密钥用法**" 字段，请使用服务器身份验证 OID。  
  
-   对于 " **CRL 分发点**" 字段，请使用连接到 Intranet 的 DirectAccess 客户端可访问的 crl 分发点。 不应从内部网络之外访问此 CRL 分发点。  
  
> [!NOTE]  
> 确保 ip-https 和网络位置服务器的证书具有使用者名称。 如果证书使用备用名称，则远程访问向导将不接受该证书。  
  
#### <a name="plan-dns-requirements"></a>规划 DNS 要求  
本部分介绍远程访问部署中的客户端和服务器的 DNS 要求。  
  
##### <a name="directaccess-client-requests"></a>DirectAccess 客户端请求  
DNS 用于解析来自不位于内部网络上的 DirectAccess 客户端计算机的请求。 DirectAccess 客户端尝试连接到 DirectAccess 网络位置服务器，以确定它们是位于 Internet 上还是位于企业网络上。  
  
-   如果连接成功，则确定客户端在 intranet 上，不会使用 DirectAccess，并且通过使用在客户端计算机的网络适配器上配置的 DNS 服务器解析客户端请求。  
  
-   如果该连接不成功，则假定客户端在 Internet 上。 DirectAccess 客户端将使用名称解析策略表 (NRPT) 来确定在解析名称请求时使用哪个 DNS 服务器。 你可以指定客户端应使用 DirectAccess DNS64 或备用的内部 DNS 服务器来解析名称。  
  
在执行名称解析时，将由 DirectAccess 客户端使用 NRPT 来确定如何处理请求。 客户端请求 FQDN 或单标签名称，例如 <https://internal>。 如果请求单标签名称，则将追加 DNS 后缀以产生 FQDN。 如果 DNS 查询与 NRPT 和并且了 DNS4 中的条目相匹配，或者为该条目指定了 intranet DNS 服务器，则通过使用指定的服务器为该查询发送名称解析。 如果存在匹配项，但未指定 DNS 服务器，则应用免除规则和正常名称解析。  
  
在远程访问管理控制台中将新的后缀添加到 NRPT 时，可通过单击 "**检测**" 按钮自动发现该后缀的默认 DNS 服务器。 自动检测的工作原理如下所示：  
  
-   如果企业网络基于 IPv4，或者使用 IPv4 和 IPv6，则默认地址是远程访问服务器上内部适配器的 DNS64 地址。  
  
-   如果企业网络基于 IPv6，则默认地址是企业网络中 DNS 服务器的 IPv6 地址。  
  
##### <a name="infrastructure-servers"></a>基础结构服务器  
  
-   **网络位置服务器**  
  
    DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析网络位置服务器的名称，并且在这些客户端位于 Internet 上时，必须阻止它们解析该名称。 为了确保这一点，默认情况下，将网络位置服务器的 FQDN 作为免除规则添加到 NRPT。 此外，在配置远程访问时，将自动创建以下规则：  
  
    -   根域或远程访问服务器的域名的 DNS 后缀规则，以及与远程访问服务器上配置的 intranet DNS 服务器相对应的 IPv6 地址。 例如，如果远程访问服务器是 corp.contoso.com 域的成员，则会为 corp.contoso.com DNS 后缀创建一条规则。  
  
    -   用于网络位置服务器的 FQDN 的免除规则。 例如，如果网络位置服务器 URL <https://nls.corp.contoso.com>，则会为 FQDN nls.corp.contoso.com 创建例外规则。  
  
-   **Ip-https 服务器**  
  
    远程访问服务器充当 ip-https 侦听器，并使用其服务器证书对 ip-https 客户端进行身份验证。 Ip-https 名称必须可由使用公用 DNS 服务器的 DirectAccess 客户端解析。  
  
##### <a name="connectivity-verifiers"></a>连接性验证程序  
远程访问将创建可由 DirectAccess 客户端计算机用来验证到内部网络的连接的默认 Web 探测。 若要确保探测按预期方式运行，必须在 DNS 中手动注册以下名称：  
  
-   **directaccess-directaccess-webprobehost**应解析为远程访问服务器的内部 IPv4 地址，或解析为仅限 ipv6 的环境中的 ipv6 地址。  
  
-   **directaccess-corpconnectivityhost**应解析为本地主机（环回）地址。 你应创建 A 和 AAAA 记录。 A 记录的值为127.0.0.1，AAAA 记录的值从带有最后32位的 NAT64 前缀构建为127.0.0.1。 可以通过运行**Get-netnattransitionconfiguration 来**Windows PowerShell cmdlet 来检索 NAT64 前缀。  
  
    > [!NOTE]  
    > 这仅在仅适用于 IPv4 的环境中有效。 在 IPv4 + IPv6 或仅使用 IPv6 的环境中，仅创建具有环回 IP 地址：：1的 AAAA 记录。  
  
可以通过 HTTP 或 PING 使用其他 web 地址来创建其他连接性验证程序。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
##### <a name="dns-server-requirements"></a>DNS 服务器要求  
  
-   对于 DirectAccess 客户端，必须使用运行 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 或支持 IPv6 的任何 DNS 服务器的 DNS 服务器。  
  
-   应使用支持动态更新的 DNS 服务器。 你可以使用不支持动态更新的 DNS 服务器，但必须手动更新条目。  
  
-   CRL 分发点的 FQDN 必须可以通过使用 Internet DNS 服务器解析。 例如，如果 URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> 位于远程访问服务器的 ip-https 证书的 " **CRL 分发点**" 字段中，则必须确保可以使用 Internet DNS 服务器解析 FQDN crld.contoso.com。  
  
#### <a name="plan-for-local-name-resolution"></a>规划本地名称解析  
在规划本地名称解析时，请考虑以下事项：  
  
##### <a name="nrpt"></a>NRPT  
在以下情况下，可能需要创建其他名称解析策略表（NRPT）规则：  
  
-   需要为 intranet 命名空间添加更多的 DNS 后缀。  
  
-   如果你的 CRL 分发点的 Fqdn 基于你的 intranet 命名空间，则你必须为 CRL 分发点的 Fqdn 添加例外规则。  
  
-   如果使用的是裂脑 DNS 环境，则必须为要在 Internet 上访问 Internet 版本的资源的名称（而不是 intranet 版本）添加例外规则。  
  
-   如果要通过 intranet web 代理服务器将流量重定向到外部网站，则仅可从 intranet 访问外部网站。 它使用 web 代理服务器的地址以允许入站请求。 在这种情况下，请为外部网站的 FQDN 添加例外规则，并指定规则使用 intranet web 代理服务器，而不是 intranet DNS 服务器的 IPv6 地址。  
  
    例如，假设要测试名为 test.contoso.com 的外部网站。 此名称不可通过 Internet DNS 服务器解析，但 Contoso web 代理服务器知道如何解析该名称，以及如何将网站请求定向到外部 web 服务器。 为了阻止非 Contoso Intranet 上的用户访问该站点，外部网站将仅允许来自 Contoso Web 代理的 IPv4 Internet 地址的请求。 因为 intranet 用户使用 Contoso web 代理，所以他们可以访问该网站，但因为 DirectAccess 用户不使用 Contoso web 代理，所以他们无法访问该网站。 通过为使用 Contoso Web 代理的 test.contoso.com 配置 NRPT 免除规则，可在 IPv4 Internet 上将 test.contoso.com 的网页请求路由到 Intranet Web 代理服务器。  
  
##### <a name="single-label-names"></a>单标签名称  
单个标签名称（例如 <https://paycheck>）有时用于 intranet 服务器。 如果请求单标签名称并配置了 DNS 后缀搜索列表，则列表中的 DNS 后缀将追加到单标签名称。 例如，当计算机上的用户在 web 浏览器中是 corp.contoso.com 域类型的成员 <https://paycheck> 时，作为名称构造的 FQDN 为 paycheck.corp.contoso.com。 默认情况下，附加的后缀基于客户端计算机的主 DNS 后缀。  
  
> [!NOTE]  
> 在不相互连接的命名空间方案中（其中一个或多个域计算机具有与计算机所属的 Active Directory 域不匹配的 DNS 后缀），应确保将搜索列表自定义为包括所有必需的后缀。 默认情况下，远程访问向导会将 Active Directory DNS 名称配置为客户端上的主 DNS 后缀。 请确保添加客户端用于名称解析的 DNS 后缀。  
  
如果你的组织中部署了多个域和 Windows Internet 名称服务（WINS），并且你远程连接，则可以按如下所示解析单个名称：  
  
-   在 DNS 中部署 WINS 前向查找区域。 尝试解析 computername.dns.zone1.corp.contoso.com 时，请求会定向到仅使用计算机名称的 WINS 服务器。 客户端认为它正在发出常规 DNS A 记录请求，但它实际上是 NetBIOS 请求。  
  
    有关详细信息，请参阅[管理正向查找区域](https://technet.microsoft.com/library/cc816891(WS.10).aspx)。  
  
-   将 DNS 后缀（例如，dns.zone1.corp.contoso.com）添加到默认域 GPO。  
  
##### <a name="split-brain-dns"></a>拆分式 DNS  
拆分式 DNS 指的是对 Internet 和 intranet 名称解析使用同一个 DNS 域。  
  
对于拆分式 DNS 部署，你必须列出 Internet 和 intranet 上重复的 Fqdn，并确定 DirectAccess 客户端应访问的资源-intranet 或 Internet 版本。 当你希望 DirectAccess 客户端访问 Internet 版本时，必须将相应的 FQDN 作为免除规则添加到每个资源的 NRPT 中。  
  
在拆分式 DNS 环境中，如果希望资源的两个版本都可用，请使用与 Internet 上使用的名称不重复的名称配置 intranet 资源。 然后，当用户访问 intranet 上的资源时，指示用户使用替代名称。 例如，将 www\.internal.contoso.com 配置为 www\.contoso.com 的内部名称。  
  
在非拆分式 DNS 环境中，Internet 命名空间不同于 Intranet 命名空间。 例如，Contoso 公司在 Internet 上使用 contoso.com，在 Intranet 上使用 corp.contoso.com。 因为所有 Intranet 资源都使用 corp.contoso.com DNS 后缀，所以 corp.contoso.com 的 NRPT 规则会将针对所有 Intranet 资源的 DNS 名称查询都路由到 Intranet DNS 服务器。 具有 contoso.com 后缀的名称的 DNS 查询与 NRPT 中的 corp.contoso.com intranet 命名空间规则不匹配，并将其发送到 Internet DNS 服务器。 对于非拆分式 DNS 部署，由于 Intranet 和 Internet 资源的 FQDN 互不重复，因此无需对 NRPT 进行其他配置。 DirectAccess 客户端可以访问其组织的 Internet 和 intranet 资源。  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>规划 DirectAccess 客户端的本地名称解析行为  
如果无法使用 DNS 解析名称，Windows Server 2012、Windows 8、Windows Server 2008 R2 和 Windows 7 中的 DNS 客户端服务可结合使用本地名称解析以及链路本地多播名称解析（LLMNR）和 TCP/IP 上的 NetBIOS 协议，来解析 本地子网上的名称。 当计算机位于专用网络（如单个子网家庭网络）上时，对等连接通常需要使用本地名称解析。  
  
当 DNS 客户端服务对 intranet 服务器名称执行本地名称解析，并且计算机连接到 Internet 上的共享子网时，恶意用户可以捕获 LLMNR 和 TCP/IP 上的 NetBIOS 消息来确定 intranet 服务器名称。 在基础结构服务器安装向导的 "DNS" 页上，你可以根据从 intranet DNS 服务器收到的响应类型来配置本地名称解析行为。 你可使用以下选项：  
  
-   **如果 DNS 中不存在该名称，则使用本地名称解析**：由于 DirectAccess 客户端仅对 Intranet DNS 服务器无法解析的服务器名称执行本地名称解析，因此此选项的安全性最高。 如果可以访问 Intranet DNS 服务器，则将解析 Intranet 服务器的名称。 如果无法访问 Intranet DNS 服务器，或者存在其他类型的 DNS 错误，则不会通过本地名称解析将 Intranet 服务器名称泄漏到子网中。  
  
-   **如果 dns 中不存在该名称，或者当客户端计算机位于专用网络上时无法访问 dns 服务器，则使用本地名称解析（推荐）** ：因为仅当无法访问 Intranet DNS 服务器时，此选项才允许在专用网络上使用本地名称解析，因此推荐使用此选项。  
  
-   **对任何类型的 DNS 解析错误使用本地名称解析（安全性最低）** ：由于 Intranet 网络服务器的名称可能通过本地名称解析泄漏到本地子网，因此此选项的安全性最低。  
  
#### <a name="plan-the-network-location-server-configuration"></a>规划网络位置服务器配置  
网络位置服务器是一个用于检测 DirectAccess 客户端是否位于企业网络中的网站。 企业网络中的客户端不使用 DirectAccess 访问内部资源;而是直接连接。  
  
网络位置服务器网站可以托管在远程访问服务器上，也可以托管在组织中的另一个服务器上。 如果将网络位置服务器托管在远程访问服务器上，则部署远程访问时将自动创建该网站。 如果将网络位置服务器托管在另一台运行 Windows 操作系统的服务器上，则必须确保在该服务器上安装了 Internet Information Services （IIS），并且已创建该网站。 远程访问不会在网络位置服务器上配置设置。  
  
请确保网络位置服务器网站满足以下要求：  
  
-   具有 HTTPS 服务器证书。  
  
-   内部网络上的计算机具有高可用性。  
  
-   在 Internet 上，DirectAccess 客户端计算机无法访问。  
  
-  
  
此外，在设置网络位置服务器网站时，请考虑客户端的以下要求：  
  
-   DirectAccess 客户端计算机必须信任将服务器证书颁发给网络位置服务器网站的 CA。  
  
-   内部网络上的 DirectAccess 客户端计算机必须能够解析网络位置服务器网站的名称。  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>规划网络位置服务器的证书  
获取用于网络位置服务器的网站证书时，请考虑以下事项：  
  
-   在“使用者”字段中，指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。  
  
-   对于 "**增强型密钥用法**" 字段，请使用服务器身份验证 OID。  
  
-   必须对照证书吊销列表（CRL）检查网络位置服务器证书。 对于 " **CRL 分发点**" 字段，请使用连接到 Intranet 的 DirectAccess 客户端可访问的 crl 分发点。 不应从内部网络之外访问此 CRL 分发点。  
  
##### <a name="plan-dns-for-the-network-location-server"></a>规划网络位置服务器的 DNS  
DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析该网络位置服务器的名称，但当它们位于 Internet 上时，必须阻止它们解析该名称。 为了确保这一点，默认情况下，网络位置服务器的 FQDN 将作为免除规则添加到 NRPT。  
  
### <a name="plan-management-servers-configuration"></a>规划管理服务器的配置  
DirectAccess 客户端启动与管理服务器的通信，这些服务器提供 Windows 更新和防病毒更新等服务。 DirectAccess 客户端还使用 Kerberos 协议对域控制器进行身份验证，然后才能访问内部网络。 在 DirectAccess 客户端的远程管理期间，管理服务器与客户端计算机进行通信以执行管理功能，例如，软件或硬件清单评估。 远程访问可以自动发现某些管理服务器，包括：  
  
-   域控制器：对于包含客户端计算机的域以及与远程访问服务器位于同一林中的所有域，会自动发现域控制器。  
  
-   System Center Configuration Manager 服务器  
  
首次配置 DirectAccess 时，会自动检测域控制器和 System Center Configuration Manager 服务器。 检测到的域控制器不会显示在控制台中，但可以使用 Windows PowerShell cmdlet 来检索设置。 如果修改了域控制器或 System Center Configuration Manager 服务器，请单击控制台中的 "**更新管理服务器**" 将刷新管理服务器列表。  
  
**管理服务器要求**  
  
-   必须可通过基础结构隧道访问管理服务器。 在你配置远程访问时，将服务器添加到管理服务器列表将自动使它们可以通过此隧道进行访问。  
  
-   启动与 DirectAccess 客户端的连接的管理服务器必须完全支持 IPv6，方法是使用本机 IPv6 地址或使用由 ISATAP 分配的地址。  
  
### <a name="plan-active-directory-requirements"></a>规划 Active Directory 要求  
远程访问使用 Active Directory，如下所示：  
  
-   **身份验证**：基础结构隧道对连接到远程访问服务器的计算机帐户使用 NTLMv2 身份验证，并且该帐户必须位于 Active Directory 域中。 Intranet 隧道使用 Kerberos 身份验证，以便用户创建 intranet 隧道。  
  
-   **组策略对象**：远程访问将配置设置收集到应用于远程访问服务器、客户端和内部应用程序服务器的组策略对象（Gpo）中。  
  
-   **安全组**：远程访问使用安全组来收集和标识 DirectAccess 客户端计算机。 Gpo 应用于所需的安全组。  
  
规划远程访问部署的 Active Directory 环境时，请考虑以下要求：  
  
-   Windows Server 2012、Windows Server 2008 R2 Windows Server 2008 或 Windows Server 2003 操作系统上至少安装了一个域控制器。  
  
    如果域控制器位于外围网络上（因此可从远程访问服务器的面向 Internet 的网络适配器中访问），则阻止远程访问服务器访问它。 你需要在域控制器上添加数据包筛选器，以防止连接到 Internet 适配器的 IP 地址。  
  
-   远程访问客户端必须使用 V.90 调制解调器。  
  
-   DirectAccess 客户端必须是域成员。 客户端可以属于：  
  
    -   与远程访问服务器位于同一林中的任何域。  
  
    -   与远程访问服务器域具有双向信任的任何域。  
  
    -   林中与远程访问服务器域的林有双向信任关系的任何域。  
  
> [!NOTE]  
> -   远程访问服务器不可以是域控制器。  
> -   无法从远程访问服务器的外部 Internet 适配器访问用于远程访问的 Active Directory 域控制器（该适配器不得位于 Windows 防火墙的域配置文件中）。  
  
#### <a name="plan-client-authentication"></a>规划客户端身份验证  
在 Windows Server 2012 中的远程访问中，可以选择使用内置 Kerberos 身份验证（使用用户名和密码），也可以选择使用证书进行 IPsec 计算机身份验证。  
  
**Kerberos 身份验证**：当你选择使用 Active Directory 凭据进行身份验证时，DirectAccess 首先将 Kerberos 身份验证用于计算机，然后使用用户的 Kerberos 身份验证。 使用这种身份验证模式时，DirectAccess 使用单个安全隧道，该隧道提供对 DNS 服务器、域控制器和内部网络上的任何其他服务器的访问权限。  
  
**IPsec 身份验证**：当你选择使用双因素身份验证或网络访问保护时，DirectAccess 将使用两个安全隧道。 远程访问设置向导配置高级安全 Windows 防火墙中的连接安全规则。 在将 IPsec 安全协商到远程访问服务器时，这些规则将指定以下凭据：  
  
-   基础结构隧道使用计算机证书凭据进行第一次身份验证，使用用户（NTLMv2）凭据进行第二次身份验证。 用户凭据强制使用已验证 Internet 协议（AuthIP），并提供对 DNS 服务器和域控制器的访问权限，然后 DirectAccess 客户端才能对 intranet 隧道使用 Kerberos 凭据。  
  
-   Intranet 隧道使用计算机证书凭据进行第一次身份验证，使用用户（Kerberos V5）凭据进行第二次身份验证。  
  
#### <a name="plan-multiple-domains"></a>规划多个域  
管理服务器列表应包括来自所有域的域控制器，这些域所包含的安全组中具有 DirectAccess 客户端计算机。 它应包含所有域，其中包含可能使用配置为 DirectAccess 客户端的计算机的用户帐户。 这可确保通过用户域中的域控制器，可以对未与用户使用的客户端计算机位于同一域中的用户进行身份验证。  
  
如果域在同一林中，则此身份验证是自动进行的。 如果有一个安全组具有不同林中的客户端计算机或应用程序服务器，则不会自动检测这些林的域控制器。 也不会自动检测林。 你可以在**远程访问管理**中运行任务**更新管理服务器**，以检测这些域控制器。  
  
如果可能，应在远程访问部署过程中将公用域名后缀添加到 NRPT。 例如，如果你有两个域（domain1.corp.contoso.com 和 domain2.corp.contoso.com），你可以添加一个常见的 DNS 后缀条目（其中域名后缀是 corp.contoso.com），而不是将两个条目都添加到 NRPT 中。 对于同一根中的域，会自动发生这种情况。 必须手动添加不在同一根中的域。  
  
### <a name="plan-group-policy-object-creation"></a>规划组策略对象创建  
配置远程访问时，DirectAccess 设置将收集到组策略对象（Gpo）中。 两个 Gpo 都是用 DirectAccess 设置填充的，它们的分发方式如下：  
  
-   **DirectAccess 客户端 GPO**：此 GPO 包含客户端设置，包括 IPv6 转换技术设置、NRPT 条目和高级安全 Windows 防火墙连接安全规则。 此 GPO 将应用于为客户端计算机指定的安全组。  
  
-   **DirectAccess 服务器 GPO**：此 GPO 包含的 DirectAccess 配置设置适用于在部署中配置为远程访问服务器的任何服务器。 它还包含高级安全 Windows 防火墙的连接安全规则。  
  
> [!NOTE]  
> 远程管理 DirectAccess 客户端不支持应用程序服务器的配置，因为客户端无法访问应用程序服务器所在的 DirectAccess 服务器的内部网络。 远程访问设置配置屏幕中的步骤4不适用于此类型的配置。  
  
你可以自动或手动配置 Gpo。  
  
**自动**：指定自动创建 Gpo 时，将为每个 GPO 指定默认名称。  
  
**手动**：你可以使用由 Active Directory 管理员预定义的 Gpo。  
  
配置 Gpo 时，请考虑以下警告：  
  
-   将 DirectAccess 配置为使用特定的 GPO 后，无法将它配置为使用不同的 GPO。  
  
-   运行 DirectAccess cmdlet 之前，请使用以下过程备份所有远程访问组策略对象：  
  
    [备份和还原远程访问配置](https://go.microsoft.com/fwlink/?LinkID=257928)。  
  
-   无论你使用的是自动还是手动配置的 Gpo，如果你的客户端将使用3G，则需要添加用于慢速链接检测的策略。 @No__t 的路径-0Policy：配置组策略慢速链接检测 @ no__t 为：  
  
    “计算机配置”/“策略”/“管理模板”/“系统”/“组策略”。  
  
-   如果链接 Gpo 的正确权限不存在，则会发出警告。 远程访问操作将继续，但不会出现链接。 如果发出此警告，则不会自动创建链接，即使稍后添加了权限也是如此。 相反，管理员必须手动创建链接。  
  
#### <a name="automatically-created-gpos"></a>自动创建的 Gpo  
使用自动创建的 Gpo 时，请注意以下事项：  
  
将根据位置和链接目标应用自动创建的 GPO，如下所示：  
  
-   对于 DirectAccess 服务器 GPO，位置和链接目标指向包含远程访问服务器的域。  
  
-   在创建客户端和应用程序服务器 Gpo 时，该位置将设置为单一域。 将在每个域中查找 GPO 名称，如果存在，则用 DirectAccess 设置填充域。  
  
-   将链接目标设置为在其中创建了 GPO 的域的根。 为每个包含客户端计算机或应用程序服务器的域创建一个 GPO，并将该 GPO 链接到其各自域的根。  
  
当使用自动创建的 Gpo 应用 DirectAccess 设置时，远程访问服务器管理员需要以下权限：  
  
-   为每个域创建 Gpo 的权限。  
  
-   用于链接到所有选定客户端域根的权限。  
  
-   链接到服务器 GPO 域根的权限。  
  
-   用于创建、编辑、删除和修改 Gpo 的安全权限。  
  
-   每个所需的域的 GPO 读取权限。 此权限不是必需的，但建议使用它，因为它允许远程访问以验证在创建 Gpo 时不存在具有相同名称的 Gpo。  
  
#### <a name="manually-created-gpos"></a>手动创建的 Gpo  
使用手动创建的 GPO 时，请考虑以下内容：  
  
-   在运行远程访问安装向导之前，这些 GPO 应存在。  
  
-   若要应用 DirectAccess 设置，远程访问服务器管理员需要完全安全权限才能创建、编辑、删除和修改手动创建的 Gpo。  
  
-   在整个域中对 GPO 的链接进行搜索。 如果未在域中链接 GPO，将在域的根中自动创建链接。 如果未提供创建链接所需的权限，则会发出警告。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>从已删除的 GPO 中恢复  
如果远程访问服务器、客户端或应用程序服务器上的 GPO 已被意外删除，将显示以下错误消息：**找不到 gpo （gpo 名称）** 。  
  
如果有可用的备份，则可以从备份中还原 GPO。 如果没有可用的备份，则必须删除配置设置并重新配置它们。  
  
###### <a name="to-remove-configuration-settings"></a>删除配置设置  
  
1.  运行 Windows PowerShell cmdlet **Uninstall**。  
  
2.  打开 "**远程访问管理**"。  
  
3.  你将看到关于未找到 GPO 的错误消息。 单击“删除配置设置”。 完成后，服务器将还原到未配置状态，你可以重新配置这些设置。  
  


