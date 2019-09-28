---
title: 步骤1规划高级 DirectAccess 基础结构
description: 本主题是 "使用 Windows Server 2016 的高级设置部署单个 DirectAccess 服务器" 指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa3174f3-42af-4511-ac2d-d8968b66da87
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9fa6fe4de0c8723c17f6a61717281d0a38d1b579
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388664"
---
# <a name="step-1-plan-the-advanced-directaccess-infrastructure"></a>步骤1规划高级 DirectAccess 基础结构

>适用于：Windows Server（半年频道）、Windows Server 2016

在单个服务器上规划高级 DirectAccess 部署的第一步是规划部署所需的基础结构。 本主题介绍基础结构规划步骤。 不需要按照特定顺序完成这些规划任务。  
  
|任务|描述|
|----|--------|  
|[1.1 规划网络拓扑和设置](#11-plan-network-topology-and-settings)|确定放置 DirectAccess 服务器的位置（在边缘，或者在网络地址转换 (NAT) 设备或防火墙后面），并规划 IP 寻址、路由和强制隧道。|  
|[1.2 计划防火墙要求](#12-plan-firewall-requirements)|规划允许 DirectAccess 通信通过边缘防火墙。|  
|[1.3 规划证书要求](#13-plan-certificate-requirements)|确定你打算使用 Kerberos 还是证书进行客户端身份验证，并规划你的网站证书。 IP-HTTPS 是一种转换协议，DirectAccess 客户端使用该协议在 IPv4 网络上对 IPv6 通信进行隧道传送。 确定是使用由证书颁发机构 (CA) 颁发的证书，还是使用由 DirectAccess 服务器自动颁发的自签名证书对 IP-HTTPS 服务器进行身份验证。|  
|[1.4 规划 DNS 要求](#14-plan-dns-requirements)|规划用于 DirectAccess 服务器、基础结构服务器、本地名称解析选项和客户端连接的域名系统 (DNS) 设置。|  
|[1.5 规划网络位置服务器](#15-plan-the-network-location-server)|DirectAccess 客户端使用网络位置服务器来确定它们是否位于内部网络上。 确定网络位置服务器网站放置在组织中的位置（在 DirectAccess 服务器上或备用服务器上）；如果网络位置服务器位于 DirectAccess 服务器上，则会规划证书要求。|  
|[1.6 计划管理服务器](#16-plan-management-servers)|你可以在 Internet 上远程管理位于企业网络之外的 DirectAccess 客户端计算机。 规划在远程客户端管理过程中使用的管理服务器（如更新服务器）。|  
|[1.7 计划 Active Directory 域服务](#17-plan-active-directory-domain-services)|规划你的域控制器、Active Directory 要求、客户端身份验证和多个域。|  
|[1.8 计划组策略对象](#18-plan-group-policy-objects)|确定你的组织中需要哪些 GPO，以及如何创建或编辑这些 GPO。|  
  
## <a name="11-plan-network-topology-and-settings"></a>1.1 规划网络拓扑和设置

本部分介绍如何规划网络，包括：  
  
- [1.1.1 规划网络适配器和 IP 寻址](#111-plan-network-adapters-and-ip-addressing)  
  
- [1.1.2 规划 IPv6 intranet 连接](#112-plan-ipv6-intranet-connectivity)  
  
- [强制隧道的1.1.3 计划](#113-plan-for-force-tunneling)  
  
### <a name="111-plan-network-adapters-and-ip-addressing"></a>1.1.1 规划网络适配器和 IP 寻址  
  
1. 标识你打算使用的网络适配器拓扑。 可以使用下列任一拓扑设置 DirectAccess：  
  
    - **两个网络适配器** 可以使用一个连接到 Internet 的网络适配器和另一个连接到内部网络的网络适配器，在边缘安装 DirectAccess 服务器；也可以使用一个连接到外围网络的网络适配器和另一个连接到内部网络的网络适配器，在 NAT、防火墙或路由器设备后面安装 DirectAccess 服务器。  
  
    - **一个网络适配器** 在 NAT 设备后面安装 DirectAccess 服务器，单个网络适配器将连接到内部网络。  
  
2. 标识 IP 寻址要求：  
  
    DirectAccess 使用 IPv6 和 IPsec 在 DirectAccess 客户端计算机和内部企业网络之间创建安全连接。 但是，DirectAccess 不一定需要连接到 IPv6 Internet 或内部网络上的本机 IPv6 支持。 相反，它会自动配置并使用 IPv6 转换技术在 IPv4 Internet 上（通过使用 6to4、Teredo 或 IP-HTTPS）和仅支持 IPv4 的 Intranet 上（通过使用 NAT64 或 ISATAP）对 IPv6 通信进行隧道传送。 有关这些转换技术的概述，请参阅以下资源：  
  
    - [IPv6 转换技术](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [Ip-https 隧道协议规范](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3. 按下表配置所需的适配器和地址。 对于使用单个网络适配器并在 NAT 设备后面设置的部署，仅使用**内部网络适配器**列配置 IP 地址。  
  
    ||外部网络适配器|内部网络适配器|路由要求|  
    |-|--------------|--------------|------------|  
    |IPv4 Internet 和 IPv4 Intranet|配置带有相应子网掩码的两个静态连续公用 IPv4 地址（仅 Teredo 要求)。<br/><br/>此外，配置 Internet 防火墙或本地 Internet 服务提供商 (ISP) 路由器的默认网关 IPv4 地址。 **注意：** DirectAccess 服务器需要两个连续的公用 IPv4 地址，以便它可用作 Teredo 服务器，而且 NAT 设备后面的基于 Windows 的客户端可以使用 DirectAccess 服务器检测该 NAT 设备的类型。|配置以下内容：<br/><br/>-具有相应子网掩码的 IPv4 intranet 地址。<br/>-Intranet 命名空间的特定于连接的 DNS 后缀。 还应在内部接口上配置 DNS 服务器。 **警告：** 不要在任何 Intranet 接口上配置默认网关。|若要配置 DirectAccess 服务器以访问内部 IPv4 网络上的所有子网，请执行以下操作：<br/><br/>-列出 intranet 上所有位置的 IPv4 地址空间。<br/>-使用**route add-p**或**netsh interface ipv4 add Route**命令将 ipv4 地址空间添加为 DirectAccess 服务器 ipv4 路由表中的静态路由。|  
    |IPv6 Internet 和 IPv6 Intranet|配置以下内容：<br/><br/>-使用你的 ISP 提供的地址配置。<br/>-使用**Route Print**命令，以确保默认 ipv6 路由存在并指向 IPv6 路由表中的 ISP 路由器。<br/>-确定 ISP 和 intranet 路由器是否使用 RFC 4191 中所述的默认路由器首选项，并使用比本地 intranet 路由器更高的默认首选项。<br/>    如果两个结果都为“是”，则默认路由不需要任何其他配置。 用于 ISP 路由器的更高级首选项可确保 DirectAccess 服务器的活动默认 IPv6 路由指向 IPv6 Internet。<br/><br/>因为 DirectAccess 服务器是一个 IPv6 路由器，所以如果你具有本机 IPv6 基础结构，则 Internet 接口也可以访问 Intranet 上的域控制器。 在这种情况下，将数据包筛选器添加到外围网络中的域控制器，这些数据包筛选器可阻止连接到 DirectAccess 服务器面向 Internet 的接口的 IPv6 地址。|配置以下内容：<br/><br/>-如果不使用默认首选等级，则可以使用以下命令**netsh interface ipv6 Set InterfaceIndex ignoredefaultroutes = enabled**来配置 intranet 接口。<br/>    这一命令可确保不会将指向 Intranet 路由器的其他默认路由添加到 IPv6 路由表。 你可以使用以下命令获取 Intranet 接口的接口索引：**netsh interface ipv6 show interface**。|如果你拥有 IPv6 Intranet，若要配置 DirectAccess 服务器以访问所有的 IPv6 位置，请执行以下操作：<br/><br/>-列出 intranet 上所有位置的 IPv6 地址空间。<br/>-使用**netsh interface ipv6 add route**命令将 ipv6 地址空间添加为 DirectAccess 服务器的 ipv6 路由表中的静态路由。|  
    |IPv4 Internet 和 IPv6 Intranet|在 IPv4 Internet 上，DirectAccess 服务器通过 Microsoft 6to4 适配器将 IPv6 路由通信转发到 6to4 中继。 可以使用以下命令，为 Microsoft 6to4 适配器的 IPv4 地址配置 DirectAccess 服务器：`netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`。|||  
  
    > [!NOTE]  
    > - 如果已为 DirectAccess 客户端分配公用 IPv4 地址，则它将使用 6to4 转换技术连接到 Intranet。 如果给它分配了专用 IPv4 地址，则它将使用 Teredo。 如果 DirectAccess 客户端无法使用 6to4 或 Teredo 连接到 DirectAccess 服务器，则它将使用 IP-HTTPS。  
    > - 若要使用 Teredo，必须在面向外部的网络适配器上配置两个连续的 IP 地址。  
    > - 如果 DirectAccess 服务器只有一个网络适配器，则无法使用 Teredo。  
    > - 本机 IPv6 客户端计算机可以通过本机 IPv6 连接到 DirectAccess 服务器，而无需转换技术。  
  
### <a name="112-plan-ipv6-intranet-connectivity"></a>1.1.2 规划 IPv6 Intranet 连接

若要管理远程 DirectAccess 客户端，IPv6 是必需的。 为了进行远程管理，IPv6 允许 DirectAccess 管理服务器连接到位于 Internet 上的 DirectAccess 客户端。  
  
> [!NOTE]  
> - 无需在你的网络上使用 IPv6，即可支持由 DirectAccess 客户端计算机发起且到你组织网络上的 IPv4 资源的连接。 为此，将使用 NAT64/DNS64。  
> - 如果你不管理远程 DirectAccess 客户端，则无需部署 IPv6。  
> - DirectAccess 部署不支持站内自动隧道寻址协议 (ISATAP)。  
> - 使用 IPv6 时，你可以使用以下 Windows PowerShell 命令启用 DNS64 的 IPv6 主机 (AAAA) 资源记录查询： **Set-NetDnsTransitionConfiguration -OnlySendAQuery $false**。  
  
### <a name="113-plan-for-force-tunneling"></a>1.1.3 规划强制隧道

在使用 IPv6 和名称解析策略表 (NRPT) 时，默认情况下 DirectAccess 客户端会通过以下方式将其 Intranet 通信和 Internet 通信分开：  
  
- 对于 Intranet 完全限定的域名 (FQDN) 的 DNS 名称查询和所有 Intranet 通信将通过隧道进行交换，这些隧道使用 DirectAccess 服务器或直接使用 Intranet 服务器创建。 来自 DirectAccess 客户端的 Intranet 通信是 IPv6 通信。  
  
- 对于符合免除规则或与 Intranet 命名空间不匹配的 FQDN 的 DNS 名称查询（以及到 Internet 服务器的所有通信）将通过连接到 Internet 的物理接口进行交换。 来自 DirectAccess 客户端的 Internet 通信通常为 IPv4 通信。  
  
相反，在默认情况下，某些远程访问虚拟专用网络 (VPN) 实现（包括 VPN 客户端）通过远程访问 VPN 连接发送其所有 Intranet 和 Internet 通信。 Internet 绑定的通信会由 VPN 服务器路由到 Intranet IPv4 Web 代理服务器以访问 IPv4 Internet 资源。 可以使用拆分隧道分离远程访问 VPN 客户端的 Intranet 和 Internet 通信。 该过程包括在 VPN 客户端上配置 Internet 协议 (IP) 路由表，以便到 Intranet 位置的通信会通过 VPN 连接发送，而到所有其他位置的通信会使用连接到 Internet 的物理接口发送。  
  
你可以使用强制隧道将 DirectAccess 客户端配置为通过隧道将其所有通信发送到 DirectAccess 服务器。 如果配置了强制隧道，则 DirectAccess 客户端将检测到它们位于 Internet 上，并且会删除它们的 IPv4 默认路由。 除本地子网通信外，DirectAccess 客户端发送的所有通信均为 IPv6 通信，它们将通过隧道发送到 DirectAccess 服务器。  
  
> [!IMPORTANT]  
> 如果你打算使用强制隧道（或者以后可能添加它），则你应该部署双隧道配置。 因为出于安全考虑，不支持单一隧道配置中的强制隧道。  
  
启用强制隧道具有以下结果：  
  
- DirectAccess 客户端只能使用基于安全超文本传输协议的 Internet 协议 (IP-HTTPS) 获取 IPv4 Internet 上到 DirectAccess 服务器的 IPv6 连接。  
  
- 默认情况下，DirectAccess 客户端可通过 IPv4 通信访问的唯一位置为其本地子网上的位置。 由 DirectAccess 客户端上运行的应用程序和服务发送的所有其他通信为 IPv6 通信，它们通过 DirectAccess 连接发送。 因此，无法使用 DirectAccess 客户端上仅支持 IPv4 的应用程序访问 Internet 资源，本地子网上的此类应用程序除外。  
  
> [!IMPORTANT]  
> 若要通过 DNS64 和 NAT64 进行强制隧道，必须实现 IPv6 Internet 连接。 实现此目的的一种方法是使 IP-HTTPS 前缀全局可路由，以便可通过 IPv6 访问 ipv6.msftncsi.com，并且能够通过 DirectAccess 服务器返回从 Internet 服务器到 IP-HTTPS 客户端的响应。  
>
> 因为在大多数情况下这一方法并不可行，所以最好的选择是在企业网络内部创建虚拟 NCSI 服务器，如下所示：  
>
> 1. 为 ipv6.msftncsi.com 添加 NRPT 条目，并针对 DNS64 将它解析到内部网站（可以是 IPv4 网站）。  
> 2. 为 dns.msftncsi.com 添加 NRPT 条目，并针对企业 DNS 服务器对其进行解析，以返回 IPv6 主机 (AAAA) 资源记录 fd3e:4f5a:5b81::1。 （使用 DNS64 仅发送此 FQDN 的主机 (A) 资源记录查询可能不起作用，因为已将它在仅支持 IPv4 部署中配置，所以你应将其配置为直接针对企业 DNS 进行解析。）  
  
## <a name="12-plan-firewall-requirements"></a>1.2 规划防火墙要求

如果 DirectAccess 服务器位于边缘防火墙后面，则当 DirectAccess 服务器位于 IPv4 Internet 上时，要进行远程访问通信还需要以下例外：  
  
- Teredo 流量-用户数据报协议（UDP）目标端口3544入站，UDP 源端口3544出站。  
  
- 6to4 流量-IP 协议41入站和出站。  
  
- Ip-https-传输控制协议（TCP）目标端口443和 TCP 源端口443出站。  
  
- 如果你要使用单个网络适配器部署远程访问，并且要在 DirectAccess 服务器上安装网络位置服务器，则还应免除 TCP 端口 62000。  
  
    > [!NOTE]  
    > 这一免除位于 DirectAccess 服务器上，其他所有免除位于边缘防火墙上。  
  
对于 Teredo 和 6to4 通信，这些例外应适用于 DirectAccess 服务器上两个面向 Internet 的连续公用 IPv4 地址。 对于 IP-HTTPS，需要将这些例外应用于公用 DNS 服务器上注册的地址。  
  
当 DirectAccess 服务器位于 IPv6 Internet 上时，要进行远程访问通信需要以下例外：  
  
- IP 协议 ID 50  
  
- UDP 目标端口 500 入站，以及 UDP 源端口 500 出站  
  
- ICMPv6 通信入站和出站（仅使用 Teredo 时）  
  
当你使用其他防火墙时，请为远程访问通信应用以下内部网络防火墙例外：  
  
- ISATAP-协议41入站和出站  
  
- 用于所有 IPv4 和 IPv6 通信的 TCP/UDP  
  
- 用于所有 IPv4 和 IPv6 通信的 ICMP（仅使用 Teredo 时）  
  
## <a name="13-plan-certificate-requirements"></a>1.3 规划证书要求

部署单个 DirectAccess 服务器时，以下三种方案需要证书：  
  
- [1.3.1 计划用于 IPsec 身份验证的计算机证书](#131-plan-computer-certificates-for-ipsec-authentication)  
  
    IPsec 的证书要求包括 DirectAccess 客户端计算机在客户端和 DirectAccess 服务器之间建立 IPsec 连接时使用的计算机证书，以及 DirectAccess 服务器用于建立与 DirectAccess 客户端的 IPsec 连接的计算机证书。  
  
    对于 Windows Server 2012 中的 DirectAccess，不强制使用这些 IPsec 证书。 作为备用服务器，DirectAccess 服务器可以充当 Kerberos 代理执行 IPsec 身份验证，而无需证书。 如果使用 Kerberos 协议，则它通过 SSL 工作，并且 Kerberos 代理将使用基于此目的为 IP-HTTPS 配置的证书。 某些企业方案（包括多站点部署和一次性密码 (OTP) 客户端身份验证）需要使用证书身份验证而非 Kerberos 协议。  
  
-   [1.3.2 计划 IP-HTTPS 的证书](#132-plan-certificates-for-ip-https)  
  
    配置远程访问时，DirectAccess 服务器将自动配置为充当 IP-HTTPS 侦听器。 IP-HTTPS 站点需要网站证书，并且客户端计算机必须能够联系该证书的证书吊销列表 (CRL) 站点。  
  
-   [1.3.3 规划网络位置服务器的网站证书](#133-plan-website-certificates-for-the-network-location-server)  
  
    网络位置服务器是一个用于检测客户端计算机是否位于企业网络中的网站。 网络位置服务器需要网站证书。 DirectAccess 客户端必须能够联系该证书的 CRL 站点。  
  
下表中总结了每个方案的证书颁发机构 (CA) 要求。  
  
|IPsec 身份验证|IP-HTTPS 服务器|网络位置服务器|  
|------------|----------|--------------|  
|当你未使用 Kerberos 代理进行身份验证时，需要内部 CA 向 DirectAccess 服务器和客户端颁发计算机证书，以进行 IPsec 身份验证|内部 CA：<br/><br/>可以使用内部 CA 颁发 IP-HTTPS 证书；但是，必须确保 CRL 分发点在外部可用。|内部 CA：<br/><br/>可以使用内部 CA 颁发网络位置服务器网站证书。 请确保 CRL 分发点在内部网络中高度可用。|  
||自签名证书：<br/><br/>可以对 IP-HTTPS 服务器使用自签名证书；但是，必须确保 CRL 分发点在外部可用。<br/><br/>无法在多站点部署中使用自签名证书。|自签名证书：<br/><br/>可以对网络位置服务器网站使用自签名证书。<br/><br/>无法在多站点部署中使用自签名证书。|  
||**您**<br/><br/>公共 CA：<br/><br/>建议使用公用 CA 颁发 IP-HTTPS 证书。 这可确保 CRL 分发点在外部可用。|  
  
### <a name="131-plan-computer-certificates-for-ipsec-authentication"></a>1.3.1 规划用于 IPsec 身份验证的计算机证书  
如果使用基于证书的 IPsec 身份验证，则 DirectAccess 服务器和客户端需要获得计算机证书。 安装证书的最简单的方法是为计算机证书配置基于组策略的自动注册。 此方法将确保所有域成员都从企业 CA 获取证书。 如果你的组织中未设置企业 CA，请参阅 [Active Directory 证书服务](https://technet.microsoft.com/library/cc770357.aspx)。  
  
此证书具有以下要求：  
  
-   该证书应包含客户端身份验证扩展密钥用法 (EKU)。  
  
-   客户端证书和服务器证书应链接到相同的根证书。 必须在 DirectAccess 配置设置中选择该根证书。  
  
### <a name="132-plan-certificates-for-ip-https"></a>1.3.2 规划用于 IP-HTTPS 的证书  
DirectAccess 服务器充当 IP-HTTPS 侦听器，而且必须在服务器上手动安装 HTTPS 网站证书。 规划时，请考虑以下内容：  
  
-   建议使用公共 CA，以便可以随时使用证书吊销列表 (CRL)。  
  
-   在“使用者”字段中，指定 DirectAccess 服务器的 Internet 适配器的 IPv4 地址，或 IP-HTTPS URL（ConnectTo 地址）的 FQDN。 如果 DirectAccess 服务器位于 NAT 设备后面，则应指定 NAT 设备的公用名或地址。  
  
-   该证书的公用名应与 IP-HTTPS 站点的名称相匹配。  
  
-   对于“增强型密钥使用”字段，请使用服务器身份验证对象标识符 (OID)。  
  
-   对于“CRL 分发点”字段，请指定已连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   必须直接将 IP-HTTPS 证书导入到个人存储中。  
  
-   IP-HTTPS 证书可以在名称中包含通配符符。  
  
如果你计划在非标准端口上使用 IP-HTTPS，请在 DirectAccess 服务器上执行以下步骤：  
  
1.  删除 0.0.0.0:443 的现有证书绑定，并将其替换为所选端口的证书绑定。 为了达到此示例的目的，将使用端口 44500。 在删除证书绑定之前，显示并复制 **appid**。  
  
    1.  若要删除证书绑定，请输入：  
  
        ```  
        netsh http delete ssl ipport=0.0.0.0:443  
        ```  
  
    2.  若要添加新的证书绑定，请输入：  
  
        ```  
        netsh http add ssl ipport=0.0.0.0:44500 certhash=<use the thumbprint from the DirectAccess server SSL cert> appid=<use the appid from the binding that was deleted>  
        ```  
  
2.  若要在服务器上修改 IP-HTTPS URL，请输入：  
  
    ```  
    Netsh int http set int url=https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS  
    ```  
  
    ```  
    Net stop iphlpsvc & net start iphlpsvc  
    ```  
  
3.  更改 kdcproxy 的 URL 保留项。  
  
    1.  若要删除现有的 URL 保留项，请输入：  
  
        ```  
        netsh http del urlacl url=https://+:443/KdcProxy/  
        ```  
  
    2.  若要添加新的 URL 保留项，请输入：  
  
        ```  
        netsh http add urlacl url=https://+:44500/KdcProxy/ sddl=D:(A;;GX;;;NS)  
        ```  
  
4.  添加该设置以使 kppsvc 在非标准端口上进行侦听。 若要添加注册表项，请输入：  
  
    ```  
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\KPSSVC\Settings /v HttpsUrlGroup /t REG_MULTI_SZ /d +:44500 /f  
    ```  
  
5.  若要在域控制器上重启 kdcproxy 服务，请输入：  
  
    ```  
    net stop kpssvc & net start kpssvc  
    ```  
  
若要在非标准端口上使用 IP-HTTPS，请在域控制器上执行以下步骤：  
  
1.  修改客户端 GPO 中的 IP-HTTPS 设置。  
  
    1.  打开组策略编辑器。  
  
    2.  导航到“计算机配置”=>“策略”=>“管理模板”=>“网络”=>“TCPIP 设置”=>“IPv6 转换技术”。  
  
    3.  打开 IP-HTTPS 状态设置并将 URL 更改为 **https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS**。  
  
    4.  单击 **“应用”** 。  
  
2.  修改客户端 GPO 中的 Kerberos 代理客户端设置。  
  
    1.  在组策略编辑器中，导航到“计算机配置”=>“策略”=>“管理模板”=>“系统”=>“Kerberos”=>“指定 Kerberos 客户端的 KDC 代理服务器”。  
  
    2.  打开 IPHTTPS 状态设置并将 URL 更改为 **https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS**。  
  
    3.  单击 **“应用”** 。  
  
3.  修改客户端 IPsec 策略设置以使用 ComputerKerb 和 UserKerb。  
  
    1.  在组策略编辑器中，导航到“计算机配置”=>“策略”=>“Windows 设置”=>“安全设置”=>“高级安全 Windows 防火墙”。  
  
    2.  单击“连接安全规则”，然后双击“IPsec 规则”。  
  
    3.  在“身份验证”选项卡上，单击“高级”。  
  
    4.  对于 Auth1：删除现有的身份验证方法，并将它替换为 ComputerKerb。 对于 Auth2：删除现有的身份验证方法，并将其替换为 UserKerb。  
  
    5.  单击“应用”，然后单击“确定”。  
  
若要完成使用 IP-HTTPS 非标准端口的手动过程，请在客户端计算机和 DirectAccess 服务器上运行 **gpupdate /force**。  
  
### <a name="133-plan-website-certificates-for-the-network-location-server"></a>1.3.3 规划用于网络位置服务器的网站证书  
规划网络位置服务器网站时，请考虑以下方面：  
  
-   在“使用者”字段中，指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。  
  
-   在“增强型密钥用法”字段中，使用服务器身份验证 OID。  
  
-   在“CRL 分发点”字段中，使用已连接到 Intranet 的 DirectAccess 客户端可访问的 CRL 分发点。 不应从内部网络之外访问此 CRL 分发点。  
  
-   如果以后你打算配置多站点或群集部署，证书的名称不应该与将添加到部署的任一 DirectAccess 服务器的内部名称相匹配。  
  
    > [!NOTE]  
    > 确保用于 IP-HTTPS 和网络位置服务器的证书包含“使用者名称”。 如果该证书不包含“使用者名称”，但它包含“备用名称”，则远程访问向导将不会接受它。  
  
## <a name="14-plan-dns-requirements"></a>1.4 规划 DNS 要求  
本部分介绍远程访问部署中关于 DirectAccess 客户端请求和基础结构服务器的 DNS 要求。 它包括了以下几节：  
  
-   [DNS 服务器要求的1.4.1 计划](#141-plan-for-dns-server-requirements)  
  
-   [1.4.2 规划本地名称解析](#142-plan-for-local-name-resolution)  
  
**DirectAccess 客户端请求**  
  
使用 DNS 来解析来自不位于内部（或企业）网络上的 DirectAccess 客户端计算机的请求。 DirectAccess 客户端尝试连接到 DirectAccess 网络位置服务器，以确定它们是位于 Internet 上，还是位于内部网络上。  
  
-   如果连接成功，则将客户端标识为位于内部网络上，不会使用 DirectAccess，并且将使用客户端计算机的网络适配器上配置的 DNS 服务器解析客户端请求。  
  
-   如果该连接不成功，则假定客户端位于 Internet 上，DirectAccess 客户端将使用名称解析策略表 (NRPT) 来确定解析名称请求时使用哪个 DNS 服务器。  
  
你可以指定客户端使用 DirectAccess DNS64（或备用的内部 DNS 服务器）来解析名称。 在执行名称解析时，将由 DirectAccess 客户端使用 NRPT 来确定如何处理请求。 客户端请求 FQDN 或单标签名称，例如 <https://internal>。 如果请求单标签名称，则将追加 DNS 后缀以产生 FQDN。 如果 DNS 查询与 NRPT 中某个条目匹配，并且为该条目指定了内部网络上的 DNS64 或 DNS 服务器，则将使用指定的服务器发送用于名称解析的 DNS 查询。 如果存在匹配条目，但未指定 DNS 服务器，则这表示存在一条免除规则，并且将应用正常的名称解析。  
  
> [!NOTE]  
> 请注意，在远程访问管理控制台将新后缀添加到 NRPT 时，可通过单击“检测”自动发现该后缀的默认 DNS 服务器。  
  
自动检测的工作原理如下：  
  
-   如果企业网络基于 IPv4 或者使用 IPv4 和 IPv6，则默认地址是 DirectAccess 服务器上内部适配器的 DNS64 地址。  
  
-   如果企业网络基于 IPv6，则默认地址是企业网络中 DNS 服务器的 IPv6 地址。  
  
**基础结构服务器**  
  
-   **网络位置服务器**  
  
    DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析该网络位置服务器的名称，但当它们位于 Internet 上时，必须阻止它们解析该名称。 为了确保这一点，默认情况下，将网络位置服务器的 FQDN 作为免除规则添加到 NRPT。 此外，在配置远程访问时，将自动创建以下规则：  
  
    -   根域或 DirectAccess 服务器的域名对应的 DNS 后缀规则，以及与 DNS64 地址相对应的 IPv6 地址。 在仅支持 IPv6 的企业网络中，将在 DirectAccess 服务器上配置 Intranet DNS 服务器。 例如，如果 DirectAccess 服务器是 corp.contoso.com 域的成员，则会为 corp.contoso.com DNS 后缀创建一条规则。  
  
    -   用于网络位置服务器的 FQDN 的免除规则。 例如，如果网络位置服务器 URL <https://nls.corp.contoso.com>，则会为 FQDN nls.corp.contoso.com 创建例外规则。  
  
-   **Ip-https 服务器**  
  
    DirectAccess 服务器充当 IP-HTTPS 侦听器，并使用其服务器证书对 IP-HTTPS 客户端进行身份验证。 IP-HTTPS 名称必须可由使用公用 DNS 服务器的 DirectAccess 客户端解析。  
  
-   **CRL 吊销检查**  
  
    DirectAccess 将对以下连接使用证书吊销检查：DirectAccess 客户端和 DirectAccess 服务器之间的 IP-HTTPS 连接，以及 DirectAccess 客户端和网络位置服务器之间基于 HTTPS 的连接。 在这两种情况下，DirectAccess 客户端都必须能够解析和访问 CRL 分发点位置。  
  
-   **ISATAP**  
  
    ISATAP 可使企业计算机获取 IPv6 地址，并将 IPv6 数据包封装在 IPv4 标头中。 DirectAccess 服务器使用它提供整个 Intranet 上到 ISATAP 主机的 IPv6 连接。 在非本机 IPv6 网络环境中，DirectAccess 服务器会自动将其自身配置为 ISATAP 路由器。  
  
    因为 DirectAccess 中不再支持 ISATAP，所以你必须确保将你的 DNS 服务器配置为不响应 ISATAP 查询。 默认情况下，DNS 服务器服务通过 DNS 全局查询阻止列表阻止 ISATAP 名称的名称解析。 请勿从全局查询阻止列表中删除 ISATAP 名称。  
  
-   **连接性验证**  
  
    远程访问将创建可由 DirectAccess 客户端计算机用来验证到内部网络的连接的默认 Web 探测。 若要确保探测按预期方式运行，必须在 DNS 中手动注册以下名称：  
  
    -   **directaccess-webprobehost**-应解析为 directaccess 服务器的内部 IPv4 地址，或解析为仅限 ipv6 的环境中的 ipv6 地址。  
  
    -   **directaccess-corpconnectivityhost**-应解析为本地主机（环回）地址。 应创建以下主机 (A) 和 (AAAA) 资源记录：具有值 127.0.0.1 的主机 (A) 资源记录，以及具有由 NAT64 前缀构造且最后 32 位类似于 127.0.0.1 的值的主机 (AAAA) 资源记录。 可以通过运行 Windows PowerShell 命令 **get-netnattransitionconfiguration** 来检索 NAT64 前缀。  
  
        > [!NOTE]  
        > 这仅在仅适用于 IPv4 的环境中有效。 在支持 IPv4 和 IPv6 或者仅支持 IPv6 的环境中，仅应使用环回 IP 地址 ::1 创建主机 (AAAA) 资源记录。  
  
    你可以通过 HTTP 使用其他 Web 地址或使用 **ping** 来创建其他连接性验证程序。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
### <a name="141-plan-for-dns-server-requirements"></a>1.4.1 规划 DNS 服务器要求  
以下是部署 DirectAccess 时的 DNS 要求。  
  
-   对于 DirectAccess 客户端，必须使用运行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 或支持 IPv6 的任何其他 DNS 服务器的 DNS 服务器。  
  
    > [!NOTE]  
    > 部署 DirectAccess 时，不建议使用运行 Windows Server 2003 的 DNS 服务器。 尽管 Windows Server 2003 DNS 服务器支持 IPv6 记录，但 Microsoft 不再支持 Windows Server 2003。 此外，如果域控制器因为文件复制服务的问题而运行 Windows Server 2003，则不应部署 DirectAccess。 有关详细信息，请参阅 [DirectAccess 不受支持的配置](https://technet.microsoft.com/library/dn464274.aspx)。  
  
-   使用支持动态更新的 DNS 服务器。 你也可以使用不支持动态更新的 DNS 服务器，但你必须在这些服务器上手动更新条目。  
  
-   必须可以通过使用 Internet DNS 服务器解析可访问 Internet 的 CRL 分发点的 FQDN。 例如，如果 URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> 位于 DirectAccess 服务器的 ip-https 证书的 " **CRL 分发点**" 字段中，则必须确保可以使用 Internet DNS 服务器解析 FQDN crld.contoso.com。  
  
### <a name="142-plan-for-local-name-resolution"></a>1.4.2 规划本地名称解析  
在规划本地名称解析时，请考虑以下问题：  
  
**NRPT**  
  
在以下情况中，你可能需要创建其他 NRPT 规则：  
  
-   如果你需要为 Intranet 命名空间添加更多的 DNS 后缀。  
  
-   如果你的 CRL 分发点的完全限定的域名 (FQDN) 基于你的 Intranet 命名空间，则你必须为 CRL 分发点的 FQDN 添加免除规则。  
  
-   如果你拥有拆分式 DNS 环境，并且你希望位于 Internet 上的 DirectAccess 客户端访问 Internet 版本而非 Intranet 版本的资源，则你必须为这些资源的名称添加免除规则。  
  
-   如果你通过 Intranet Web 代理服务器将通信重定向到外部网站，则仅可从 Intranet 访问外部网站，并且它将使用你的 Web 代理服务器的地址以允许入站请求。 在这种情况下，为该外部网站的 FQDN 添加一条免除规则，并指定规则使用你的 Intranet Web 代理服务器，而不是 Intranet DNS 服务器的 IPv6 地址。  
  
    例如，如果你要测试名为 test.contoso.com 的外部网站，此名称不可通过 Internet DNS 服务器进行解析，但 Contoso Web 代理服务器知道如何解析该名称，以及如何将网站的请求指向外部 Web 服务器。 为了阻止非 Contoso Intranet 上的用户访问该站点，外部网站将仅允许来自 Contoso Web 代理的 IPv4 Internet 地址的请求。 这样，因为 Intranet 用户使用 Contoso Web 代理，所以他们可以访问该网站，但因为 DirectAccess 用户不使用 Contoso Web 代理，所以他们无法访问该网站。 通过为使用 Contoso Web 代理的 test.contoso.com 配置 NRPT 免除规则，可在 IPv4 Internet 上将 test.contoso.com 的网页请求路由到 Intranet Web 代理服务器。  
  
**单标签名称**  
  
单个标签名称（例如 <https://paycheck>）有时用于 intranet 服务器。 如果请求单标签名称并配置了 DNS 后缀搜索列表，则列表中的 DNS 后缀将追加到单标签名称。 例如，当计算机上的用户在 web 浏览器中是 corp.contoso.com 域类型的成员 <https://paycheck> 时，作为名称构造的 FQDN 为 paycheck.corp.contoso.com。 默认情况下，附加的后缀基于客户端计算机的主 DNS 后缀。  
  
> [!NOTE]  
> 在不互连的命名空间方案中（其中一个或多个域计算机具有的 DNS 后缀与计算机所属的 Active Directory 域不匹配），应确保将搜索列表自定义为包括所有必需的后缀。 默认情况下，远程访问向导会将 Active Directory DNS 名称配置为客户端上的主 DNS 后缀。 请确保添加客户端用于名称解析的 DNS 后缀。  
  
如果你的组织中部署了多个域和 Windows Internet 名称服务 (WINS)，并且你要进行远程连接，则可以按如下所示解析单个名称：  
  
-   在 DNS 中部署 WINS 前向查找区域。 在尝试解析 computername.dns.zone1.corp.contoso.com 时，将请求定向到仅使用 computername 的 WINS 服务器。 客户端会认为它颁发的是常规 DNS 主机 (A) 资源记录，但它实际上是 NetBIOS 请求。 有关详细信息，请参阅“管理前向查找区域”。  
  
-   将 DNS 后缀（例如 dns.zone1.corp.contoso.com）添加到默认域策略 GPO。  
  
**拆分式 DNS**  
  
拆分式 DNS 指的是对 Internet 和 Intranet 资源使用同一 DNS 域。  
  
对于拆分式 DNS 部署，你必须列出 Internet 和 intranet 上重复的 Fqdn，并确定 DirectAccess 客户端应访问的资源-intranet 或 Internet 版本。 对于你希望 DirectAccess 客户端访问的公共版本资源所对应的每个名称，必须将相应 FQDN 作为免除规则添加到 DirectAccess 客户端的 NRPT 中。  
  
在拆分式 DNS 环境中，如果你希望可以使用两个版本的资源，请使用与 Internet 上使用的名称不重复的备用名称配置 Intranet 资源，并指示用户在位于 Intranet 上时使用此备用名称。 例如，为内部名称 www.contoso.com 配置备用名称 www.internal.contoso.com。  
  
在非拆分式 DNS 环境中，Internet 命名空间不同于 Intranet 命名空间。 例如，Contoso 公司在 Internet 上使用 contoso.com，在 Intranet 上使用 corp.contoso.com。 因为所有 Intranet 资源都使用 corp.contoso.com DNS 后缀，所以 corp.contoso.com 的 NRPT 规则会将针对所有 Intranet 资源的 DNS 名称查询都路由到 Intranet DNS 服务器。 带有 contoso.com 后缀的名称的 DNS 名称查询与 NRPT 中的 corp.contoso.com Intranet 命名空间规则不匹配，因此系统会将其发送到 Internet DNS 服务器。 对于非拆分式 DNS 部署，由于 Intranet 和 Internet 资源的 FQDN 互不重复，因此无需对 NRPT 进行其他配置。 DirectAccess 客户端可访问其组织的 Internet 和 Intranet 资源。  
  
**DirectAccess 客户端的本地名称解析行为**  
  
如果无法使用 DNS 解析名称，则为了解析本地子网上的名称，Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows 8 和 Windows 7 中的 DNS 客户端服务可结合使用本地名称解析以及链路本地多播名称 Resolution （LLMNR）和 TCP/IP 上的 NetBIOS 协议。  
  
当计算机位于专用网络（如单个子网家庭网络）上时，对等连接通常需要使用本地名称解析。 如果 DNS 客户端服务对 Intranet 服务器名称执行本地名称解析，并且计算机连接到 Internet 上的共享子网，恶意用户则可以捕获 LLMNR 和 TCP/IP 上的 NetBIOS 消息来确定 Intranet 服务器名称。 在基础结构服务器安装向导的 DNS 页上，你可以根据从 Intranet DNS 服务器接收到的响应类型配置本地名称解析行为。 你可使用以下选项：  
  
-   **如果 DNS 中不存在该名称，则使用本地名称解析**。 由于 DirectAccess 客户端仅对 Intranet DNS 服务器无法解析的服务器名称执行本地名称解析，因此此选项的安全性最高。 如果可以访问 Intranet DNS 服务器，则将解析 Intranet 服务器的名称。 如果无法访问 Intranet DNS 服务器，或者存在其他类型的 DNS 错误，则不会通过本地名称解析将 Intranet 服务器名称泄漏到子网中。  
  
-   **如果 DNS 中不存在该名称，或客户端计算机位于专用网络上时无法访问 DNS 服务器，则使用本地名称解析（推荐）** 。 因为仅当无法访问 Intranet DNS 服务器时，此选项才允许在专用网络上使用本地名称解析，因此推荐使用此选项。  
  
-   **无论出现何种 DNS 解析错误，都使用本地名称解析（安全性最低）** 。 由于 Intranet 网络服务器的名称可能通过本地名称解析泄漏到本地子网，因此此选项的安全性最低。  
  
## <a name="15-plan-the-network-location-server"></a>1.5 规划网络位置服务器  
网络位置服务器是一个用于检测 DirectAccess 客户端是否位于企业网络中的网站。 企业网络中的客户端不会使用 DirectAccess 访问内部资源，相反，它们会直接进行连接。  
  
可以在 DirectAccess 服务器或组织中的另一台服务器上托管网络位置服务器网站。 如果将网络位置服务器托管在 DirectAccess 服务器上，则安装远程访问服务器角色时将自动创建该网站。 如果你将网络位置服务器托管在组织中另一台运行 Windows 操作系统的服务器上，则必须确保已在该服务器上安装 Internet 信息服务 (IIS)，且已创建该网站。 DirectAccess 无法在远程网络位置服务器上配置设置。  
  
请确保网络位置服务器网站符合以下要求：  
  
-   它是一个具有 HTTPS 服务器证书的网站。  
  
-   DirectAccess 客户端计算机必须信任将服务器证书颁发给网络位置服务器网站的 CA。  
  
-   内部网络上的 DirectAccess 客户端计算机必须能够解析网络位置服务器网站的名称。  
  
-   对于内部网络上的计算机，网络位置服务器站点必须高度可用。  
  
-   网络位置服务器不可以由 Internet 上的 DirectAccess 客户端计算机访问。  
  
-   必须根据 CRL 检查服务器证书。  
  
### <a name="151-plan-certificates-for-the-network-location-server"></a>1.5.1 规划用于网络位置服务器的证书  
若打算获得用于网络位置服务器的网站证书，请考虑以下方面：  
  
1.  在“使用者”字段中，指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。  
  
2.  在“增强型密钥用法”字段中，使用服务器身份验证 OID。  
  
3.  在“CRL 分发点”字段中，使用已连接到 Intranet 的 DirectAccess 客户端可访问的 CRL 分发点。 不应从内部网络之外访问此 CRL 分发点。  
  
### <a name="152-plan-dns-for-the-network-location-server"></a>1.5.2 规划用于网络位置服务器的 DNS  
DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析该网络位置服务器的名称，但当它们位于 Internet 上时，必须阻止它们解析该名称。 为了确保这一点，默认情况下，将网络位置服务器的 FQDN 作为免除规则添加到 NRPT。  
  
## <a name="16-plan-management-servers"></a>1.6 规划管理服务器  
DirectAccess 客户端将启动与提供服务（如 Windows 更新和防病毒更新）的管理服务器之间的通信。 DirectAccess 客户端还使用 Kerberos 协议联系域控制器，以在其访问内部网络之前进行身份验证。 在 DirectAccess 客户端的远程管理期间，管理服务器与客户端计算机进行通信以执行管理功能，例如，软件或硬件清单评估。 远程访问可以自动发现某些管理服务器，包括：  
  
-   域控制器-对与 DirectAccess 服务器和客户端计算机位于同一林中的所有域执行自动发现域控制器。  
  
-   System Center Configuration Manager 服务器-对与 DirectAccess 服务器和客户端计算机位于同一林中的所有域执行自动发现 System Center Configuration Manager 服务器。  
  
首次配置 DirectAccess 时，会自动检测域控制器和 System Center Configuration Manager 服务器。 检测到的域控制器不会显示在控制台中，但可以使用 Windows PowerShell cmdlet **get-damgmtserver-Type All**来检索设置。 如果修改了域控制器或 System Center Configuration Manager 服务器，则可通过单击远程访问管理控制台中的“刷新管理服务器”来刷新管理服务器列表。  
  
**管理服务器要求**  
  
-   必须可通过第一个（基础结构）隧道访问管理服务器。 在你配置远程访问时，将服务器添加到管理服务器列表将自动使它们可以通过此隧道进行访问。  
  
-   不管是通过使用本机 IPv6 地址，还是通过某个由 ISATAP 分配的 IPv6 地址，启动到 DirectAccess 客户端连接的管理服务器都必须完全支持 IPv6。  
  
## <a name="17-plan-active-directory-domain-services"></a>1.7 规划 Active Directory 域服务  
本部分介绍了 DirectAccess 如何使用 Active Directory 域服务 (AD DS)，它包括以下各小节：  
  
-   [1.7.1 计划客户端身份验证](#171-plan-client-authentication)  
  
-   [1.7.2 规划多个域](#172-plan-multiple-domains)  
  
DirectAccess 使用 AD DS 和 Active Directory 组策略对象（Gpo），如下所示：  
  
-   **身份验证**  
  
    AD DS 用于身份验证。 基础结构隧道将 NTLMv2 身份验证用于连接到 DirectAccess 服务器的计算机帐户，且必须在 Active Directory 域中列出该帐户。 Intranet 隧道使用 Kerberos 身份验证，以便用户创建第二条隧道。  
  
-   **组策略对象**  
  
    DirectAccess 将配置设置收集到 GPO 中，GPO 将应用于 DirectAccess 服务器、客户端和内部应用程序服务器。  
  
-   **安全组**  
  
    DirectAccess 使用安全组来收集和标识 DirectAccess 客户端计算机。 GPO 将应用于所需的安全组。  
  
-   **扩展的 IPsec 策略**  
  
    DirectAccess 可以在客户端与 DirectAccess 服务器之间使用 IPsec 身份验证和加密。 可以将 IPsec 身份验证和加密从客户端扩展到指定的内部应用程序服务器。 若要执行这一操作，请将所需的应用程序服务器添加到安全组中。  
  
**AD DS 要求**  
  
规划用于 DirectAccess 部署的 AD DS 时，请考虑以下要求：  
  
-   至少一个域控制器必须随 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 操作系统一起安装。  
  
    如果域控制器位于外围网络上（因此可从来自 DirectAccess 服务器的面向 Internet 的网络适配器进行访问），则你必须阻止 DirectAccess 服务器访问它，方法是在域控制器上添加数据包筛选器，以阻止 Internet 适配器连接到 IP 地址。  
  
-   DirectAccess 服务器必须是域成员。  
  
-   DirectAccess 客户端必须是域成员。 客户端可以属于：  
  
    -   与 DirectAccess 服务器位于同一林中的任何域。  
  
    -   与 DirectAccess 服务器域具有双向信任的任何域。  
  
    -   与 DirectAccess 域所属的林之间具有双向信任的林中的任何域。  
  
> [!NOTE]  
> -   DirectAccess 服务器不可以是域控制器。  
> -   不可从 DirectAccess 服务器的外部 Internet 适配器访问用于 DirectAccess 的 AD DS 域控制器（即，该适配器不可位于 Windows 防火墙的域配置文件中）。  
  
### <a name="171-plan-client-authentication"></a>1.7.1 规划客户端身份验证  
DirectAccess 允许你在使用证书进行 IPsec 计算机身份验证，以及使用内置 Kerberos 代理（使用用户名和密码进行身份验证）之间进行选择。  
  
在选择使用 AD DS 凭据进行身份验证时，DirectAccess 将使用一条安全隧道，该安全隧道使用计算机 Kerberos 进行第一次身份验证，使用用户 Kerberos 进行第二次身份验证。 将这个模式用于身份验证时，DirectAccess 将使用单条安全隧道，该隧道提供对 DNS 服务器、域控制器和内部网络上的其他服务器的访问权限。  
  
在选择使用双重身份验证还是网络访问保护时，DirectAccess 将使用两条安全隧道。 远程访问设置向导配置高级安全 Windows 防火墙连接安全规则，在协商到 DirectAccess 服务器的隧道的 IPsec 安全关联时，这些规则将指定对以下类型凭据的使用：  
  
-   基础结构隧道使用计算机 Kerberos 凭据进行第一次身份验证，使用用户 Kerberos 进行第二次身份验证。  
  
-   Intranet 隧道使用计算机证书凭据进行第一次身份验证，使用用户 Kerberos 进行第二次身份验证。  
  
当 DirectAccess 选择允许访问运行 Windows 7 或多站点部署中的客户端时，它将使用两个安全隧道。 远程访问设置向导配置高级安全 Windows 防火墙连接安全规则，在协商到 DirectAccess 服务器的隧道的 IPsec 安全关联时，这些规则将指定对以下类型凭据的使用：  
  
-   基础结构隧道使用计算机证书凭据进行第一次身份验证，使用 NTLMv2 进行第二次身份验证。 NTLMv2 凭据强制使用已验证 Internet 协议 (AuthIP)，并在 DirectAccess 客户端可为 Intranet 隧道使用 Kerberos 凭据之前，提供对 DNS 服务器和域控制器的访问权限。  
  
-   Intranet 隧道使用计算机证书凭据进行第一次身份验证，使用用户 Kerberos 进行第二次身份验证。  
  
### <a name="172-plan-multiple-domains"></a>1.7.2 规划多个域  
管理服务器列表应包括来自所有域的域控制器，这些域所包含的安全组中具有 DirectAccess 客户端计算机。 它应该包括所有包含用户帐户的域，这些帐户可能使用配置为 DirectAccess 客户端的计算机。 这可确保通过用户域中的域控制器，可以对未与用户使用的客户端计算机位于同一域中的用户进行身份验证。 如果这些域位于同一个林中，则可自动完成这一操作。  
  
> [!NOTE]  
> 如果某些计算机位于用于不同林中的客户端计算机或应用程序服务器的安全组，则不会自动检测这些林的域控制器。 你可以在远程访问管理控制台中运行“刷新管理服务器”任务以检测这些域控制器。  
  
在远程访问部署过程中，如果可能，应将常见域名后缀添加到名称解析策略表 (NRPT)。 例如，如果你有两个域（domain1.corp.contoso.com 和 domain2.corp.contoso.com），你可以添加一个常见的 DNS 后缀条目（其中域名后缀是 corp.contoso.com），而不是将两个条目都添加到 NRPT 中。 将自动为同一根中的域进行添加，但必须为不在同一根中的域进行手动添加。  
  
如果在多域环境中部署 Windows Internet 名称服务 (WINS)，则你必须在 DNS 中部署 WINS 前向查找区域。 有关详细信息，请参阅本文档前面的[1.4.2 规划本地名称解析](#142-plan-for-local-name-resolution)部分中的**单标签名称**。  
  
## <a name="18-plan-group-policy-objects"></a>1.8 规划组策略对象  
本部分介绍远程访问基础结构中的组策略对象 (GPO) 的角色，包括以下各小节：  
  
-   [1.8.1 配置自动创建的 Gpo](#181-configure-automatically-created-gpos)  
  
-   [1.8.2 配置手动创建的 Gpo](#182-configure-manually-created-gpos)  
  
-   [1.8.3 在多域控制器环境中管理 Gpo](#183-manage-gpos-in-a-multi-domain-controller-environment)  
  
-   [1.8.4 管理具有有限权限的远程访问 Gpo](#184-manage-remote-access-gpos-with-limited-permissions)  
  
-   [1.8.5 从已删除的 GPO 中恢复](#185-recover-from-a-deleted-gpo)  
  
配置远程访问时配置的 DirectAccess 设置将收集到 GPO 中。 以下类型的 GPO 会使用 DirectAccess 设置填充并进行分发，如下所示：  
  
-   **DirectAccess 客户端 GPO**  
  
    此 GPO 包含客户端设置，包括 IPv6 转换技术设置、NRPT 条目和高级安全 Windows 防火墙连接安全规则。 此 GPO 将应用于为客户端计算机指定的安全组。  
  
-   **DirectAccess 服务器 GPO**  
  
    此 GPO 包含的 DirectAccess 配置设置适用于在部署中配置为 DirectAccess 服务器的任一服务器。 它还包含高级安全 Windows 防火墙连接安全规则。  
  
-   **应用程序服务器 GPO**  
  
    此 GPO 包含用于所选应用程序服务器的设置，你选择性地将身份验证和加密从 DirectAccess 客户端扩展到这些服务器。 如果未扩展身份验证和加密，则不会使用此 GPO。  
  
可以通过两种方式配置 GPO：  
  
-   **自动**-你可以指定自动创建它们。 为每个 GPO 指定默认名称。  
  
-   **手动**-你可以使用已由 Active Directory 管理员预定义的 gpo。  
  
> [!NOTE]  
> 将 DirectAccess 配置为使用特定的 GPO 后，无法将它配置为使用不同的 GPO。  
  
无论你使用自动或手动配置的 GPO，如果你的客户端将使用 3G 网络，你需要添加用于慢速链接检测的策略。 @No__t 的路径-0Policy：配置组策略慢速链接检测 @ no__t 为：“计算机配置”/“策略”/“管理模板”/“系统”/“组策略”。  
  
> [!CAUTION]  
> 运行 DirectAccess cmdlet 之前，请使用以下过程备份所有远程访问 GPO：[备份和还原远程访问配置](https://go.microsoft.com/fwlink/?LinkID=257928)。  
  
如果用于链接 GPO 的正确权限（将在以下部分中列出）不存在，则会发出警告。 远程访问操作将继续进行，但不会出现链接。 如果发出此警告，则即使之后添加了权限，也不会自动创建链接。 相反，管理员必须手动创建链接。  
  
### <a name="181-configure-automatically-created-gpos"></a>1.8.1 配置自动创建的 GPO  
使用自动创建的 GPO 时，请考虑以下内容。  
  
将根据位置和链接目标参数应用自动创建的 GPO，如下所示：  
  
-   对于 DirectAccess 服务器 GPO，位置参数和链接参数指向包含 DirectAccess 服务器的域。  
  
-   在创建客户端和应用程序服务器 GPO 时，该位置将设置为要在其中创建 GPO 的单个域。 在每个域中查找 GPO 名称，如果存在，则使用了 DirectAccess 设置对其进行填充。 将链接目标设置为在其中创建了 GPO 的域的根。 为每个包含客户端计算机或应用程序服务器的域创建一个 GPO，并将该 GPO 链接到其各自域的根。  
  
在使用自动创建的 GPO 应用 DirectAccess 设置时，远程访问管理员需要以下权限：  
  
-   每个域的 GPO 创建权限  
  
-   所有选定客户端域根的链接权限  
  
-   服务器 GPO 域根的链接权限  
  
此外，还需要以下权限：  
  
-   GPO 需要创建、编辑、删除和修改安全权限。  
  
-   我们建议远程访问管理员具有针对每个所需的域的 GPO 读取权限。 这使远程访问可以验证在创建 GPO 时不存在具有相同名称的 GPO。  
  
### <a name="182-configure-manually-created-gpos"></a>1.8.2 配置手动创建的 GPO  
使用手动创建的 GPO 时，请考虑以下内容：  
  
-   在运行远程访问安装向导之前，这些 GPO 应存在。  
  
-   若要应用 DirectAccess 设置，远程访问管理员需要对手动创建的 GPO 具有完全的 GPO 权限（编辑、删除、修改安全权限）。  
  
-   在整个域中对指向 GPO 的链接进行搜索。 如果未在域中链接 GPO，将在域的根中自动创建链接。 如果未提供创建链接所需的权限，则会发出警告。  
  
### <a name="183-manage-gpos-in-a-multi-domain-controller-environment"></a>1.8.3 在多域控制器环境中管理 GPO  
每个 GPO 都由某个特定的域控制器管理，如下所示：  
  
-   服务器 GPO 由一个与服务器相关联的 Active Directory 站点中的域控制器管理。 如果该站点中的域控制器为只读控制器，则服务器 GPO 由最接近 DirectAccess 服务器且已启用写入权限的域控制器进行管理。  
  
-   客户端和应用程序服务器 GPO 由作为主域控制器 (PDC) 运行的域控制器管理。  
  
如果你想要手动修改 GPO 设置，请考虑以下内容：  
  
-   对于服务器 GPO，若要确定哪个域控制器与 DirectAccess 服务器相关联，请在 DirectAccess 服务器上通过提升的命令提示符运行 **nltest /dsgetdc: /writable**。  
  
-   默认情况下，当你使用网络 Windows PowerShell cmdlet 作出更改，或从组策略管理控制台作出更改时，将使用充当 PDC 的域控制器。  
  
此外，如果要在未与 DirectAccess 服务器（适用于服务器 GPO）或 PDC（适用于客户端和应用程序服务器 GPO）相关联的域控制器上修改设置，请考虑以下内容：  
  
-   在修改这些设置之前，请确保已使用最新的 GPO 复制域控制器，并备份 GPO 设置。 有关详细信息，请参阅[备份和还原远程访问配置](https://go.microsoft.com/fwlink/?LinkID=257928)。 如果未更新 GPO，在复制期间可能会发生合并冲突，这可能导致远程访问配置损坏。  
  
-   在修改这些设置之后，你必须等待更改复制到与 GPO 相关联的域控制器。 在复制完成之前，请勿使用远程访问管理控制台或远程访问 PowerShell cmdlet 作出其他更改。 如果在复制完成之前，在两个域控制器上编辑同一个 GPO，则可能会发生合并冲突，这可导致远程访问配置损坏。  
  
或者，可以使用组策略管理控制台中的“更改域控制器”对话框或者使用 Windows PowerShell cmdlet“Open-netgpo”来更改默认设置，这样更改将使用你指定的域控制器。  
  
-   若要在组策略管理控制台中执行这一操作，请右键单击域或站点容器，然后单击“更改域控制器”。  
  
-   若要在 Windows PowerShell 中执行这一操作，请为 **Open-NetGPO** cmdlet 指定 **DomainController** 参数。 例如，若要使用名为 europe-dc.corp.contoso.com 的域控制器在名为 domain1\DA_Server_GPO_Europe 的 GPO 上启用 Windows 防火墙中的专用和公用配置文件，请输入以下内容：  
  
    ```powershell
    $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
    Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
    Save-NetGPO -GpoSession $gpoSession  
    ```  
  
### <a name="184-manage-remote-access-gpos-with-limited-permissions"></a>1.8.4 管理具有有限权限的远程访问 GPO  
若要管理远程访问部署，远程访问管理员需要对部署中使用的 GPO 具有完全的 GPO 权限（读取、编辑、删除和修改安全权限）。 这是因为远程访问管理控制台和远程访问 PowerShell 模块将在远程访问 GPO（即，客户端、服务器和应用程序服务器 GPO）中读取和写入配置。  
  
在许多组织中，负责 GPO 操作的域管理员与负责远程访问配置的远程访问管理员不是同一个人。 这些组织可能包含限制远程访问管理员在域中对 GPO 具有完全权限的策略。 在将策略配置应用于域中任何计算机之前，可能还要求域管理员查看策略配置。  
  
为了满足这些需求，域管理员应为每个 GPO 创建两个副本：过渡和生产。 授予远程访问管理员对过渡 GPO 的完全权限。 远程访问管理员在远程访问管理控制台和 Windows PowerShell cmdlet 中将过渡 GPO 指定为用于远程访问部署的 GPO。 这使远程访问管理员能够随时按需读取和修改远程访问配置。  
  
域管理员必须确保过渡 GPO 未链接到域中的任何管理范围，且远程访问管理员在域中不具有 GPO 链接权限。 这可确保由远程访问管理员对过渡 GPO 所作的更改不会对域中的计算机产生任何影响。  
  
域管理员将生产 GPO 链接到所需的管理范围，并应用相应的安全筛选。 这可确保对这些 GPO 所作的更改应用于域中的计算机（客户端计算机、DirectAccess 服务器和应用程序服务器）。 远程访问管理员不具有对生产 GPO 的权限。  
  
当对过渡 GPO 作出更改时，域管理员可以查看这些 GPO 中的策略配置，以确保它满足组织中的安全要求。 然后，域管理员将通过使用备份功能从过渡 GPO 导出这些设置，并将这些设置导入到相应的生产 GPO，它们将应用于域中的计算机。  
  
下图显示了这一配置。  
  
![管理远程访问 Gpo](../../../media/Step-1-Plan-the-DirectAccess-Infrastructure/DA_Plan_Advanced_Step1_GPOS.png)  
  
### <a name="185-recover-from-a-deleted-gpo"></a>1.8.5 从已删除的 GPO 中恢复  
如果已意外删除客户端、DirectAccess 服务器或应用程序服务器 GPO，并且没有可用的备份，则你必须删除配置设置并重新配置它们。 如果有可用的备份，则可以从备份中还原 GPO。  
  
远程访问管理控制台将显示以下错误消息：**找不到 gpo （gpo 名称）** 。 若要删除配置设置，请执行以下步骤：  
  
1.  运行 Windows PowerShell cmdlet **Uninstall-remoteaccess**。  
  
2.  打开远程访问管理控制台。  
  
3.  你将看到关于未找到 GPO 的错误消息。 单击“删除配置设置”。 完成操作后，服务器将还原到未配置状态。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [步骤 2：规划 DirectAccess 部署 @ no__t-0  
  


