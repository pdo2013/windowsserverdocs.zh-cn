---
title: 步骤 1 规划 DirectAccess 基础结构
description: 本主题是指南的 Windows Server 2016 的现有远程访问 (VPN) 部署添加 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ca50ea8-6987-4081-acd5-5bf9ead62acd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a761f644fbae489124392f195465bf2acf8e66f
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805088"
---
# <a name="step-1-plan-directaccess-infrastructure"></a>步骤 1 规划 DirectAccess 基础结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在单个服务器上规划基本远程访问部署的第一步是规划部署所需的基础结构。 本主题介绍基础结构规划步骤：  
  
|任务|描述|  
|----|--------|  
|规划网络拓扑和设置|确定放置远程访问服务器的位置（在边缘，或者在网络地址转换 (NAT) 设备或防火墙后面），并规划 IP 寻址和路由。|  
|规划防火墙要求|规划允许远程访问通过边缘防火墙。|  
|规划证书要求|远程访问可以使用 Kerberos 或证书进行客户端身份验证。 在此基本远程访问部署中，将自动配置 Kerberos 并使用远程访问服务器自动颁发的自签名证书来完成身份验证。|  
|规划 DNS 要求|规划用于远程访问服务器、基础结构服务器、本地名称解析选项和客户端连接的 DNS 设置。|  
|规划 Active Directory|规划域控制器和 Active Directory 要求。|  
|规划组策略对象|确定你的组织中需要哪些 GPO，以及如何创建或编辑这些 GPO。|  
  
不需要按照特定顺序完成这些规划任务。  
  
## <a name="plan-network-topology-and-settings"></a>规划网络拓扑和设置  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>规划网络适配器和 IP 寻址  
  
1. 标识你打算使用的网络适配器拓扑。 可以使用下列任一方法设置远程访问：  
  
    - 具有两个网络适配器：可以使用一个网络适配器连接到 Internet 和其他到内部网络，或在 NAT 后面的边缘，防火墙或路由器设备，使用一个网络适配器将连接到外围网络和其他到内部网络。  
  
    - 后面使用一个网络适配器的 NAT 设备：NAT 设备后面安装远程访问服务器和单个网络适配器连接到内部网络。  
  
2. 标识 IP 寻址要求：  
  
    DirectAccess 使用 IPv6 和 IPsec 在 DirectAccess 客户端计算机和内部企业网络之间创建安全连接。 但是，DirectAccess 不一定需要连接到 IPv6 Internet 或内部网络上的本机 IPv6 支持。 相反，它会自动配置并使用 IPv6 转换技术在 IPv4 Internet 上（6to4、Teredo、IP-HTTPS）和仅支持 IPv4 的 Intranet 上（NAT64 或 ISATAP）对 IPv6 通信进行隧道传送。 有关这些转换技术的概述，请参阅以下资源：  
  
    - [IPv6 转换技术](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [IP-HTTPS 隧道协议规范](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3. 按下表配置所需的适配器和寻址。 对于使用单个网络适配器在 NAT 设备后面部署，配置您使用仅内部网络适配器列的 IP 地址。  
  
    |IP 地址类型|外部网络适配器|内部网络适配器|路由要求|  
    |-|--------------|--------------------|------------|  
    |IPv4 Intranet 和 IPv4 Internet|配置以下内容：<br/><br/>-一个静态公用 IPv4 地址带有相应子网掩码。<br/>默认网关的 Internet 防火墙或本地 Internet 服务提供商 (ISP) 路由器的 IPv4 地址。|配置以下内容：<br/><br/>的带有相应子网掩码 IPv4 intranet 地址。<br/>的 intranet 命名空间特定于连接的 DNS 后缀。 还必须在内部接口上配置 DNS 服务器。<br/>-不要在任何 intranet 接口上配置默认网关。|若要配置远程访问服务器以访问内部 IPv4 网络上的所有子网，请执行以下操作：<br/><br/>1.列出 Intranet 上所有位置的 IPv4 地址空间。<br/>2.使用 **route add -p** 或 **netsh interface ipv4 add route** 命令将 IPv4 地址空间添加为远程访问服务器 IPv4 路由表中的静态路由。|  
    |IPv6 Internet 和 IPv6 Intranet|配置以下内容：<br/><br/>-使用您的 ISP 提供的自动配置地址配置。<br/>-使用**路由打印**命令，以确保指向 ISP 路由器的默认 IPv6 路由存在 IPv6 路由表中。<br/>-确定 ISP 和 intranet 路由器是否正在使用 RFC 4191 中所述，使用比本地 intranet 路由器更高版本的默认首选项的默认路由器首选项。 如果两个结果都为“是”，则默认路由不需要任何其他配置。 ISP 路由器的更高首选等级可确保远程访问服务器的活动默认 IPv6 路由指向 IPv6 Internet。<br/><br/>因为远程访问服务器是一个 IPv6 路由器，所以如果你具有本机 IPv6 基础结构，则 Internet 接口也可以访问 Intranet 上的域控制器。 在这种情况下，将数据包筛选器添加到外围网络中的域控制器，这些数据包筛选器可阻止连接到远程访问服务器面向 Internet 的接口的 IPv6 地址。|配置以下内容：<br/><br/>-如果不使用默认首选等级，配置您的 intranet 接口**netsh 接口 ipv6 set InterfaceIndex ignoredefaultroutes = 启用**命令。 此命令可确保不会将指向 Intranet 路由器的其他默认路由添加到 IPv6 路由表。 你可以从 netsh 接口显示接口命令的显示中获得 Intranet 接口的 InterfaceIndex。|如果你拥有 IPv6 Intranet，若要配置远程访问服务器以访问所有的 IPv6 位置，请执行以下操作：<br/><br/>1.列出你 Intranet 上所有位置的 IPv6 地址空间。<br/>2.使用 **netsh interface ipv6 add route** 命令将 IPv6 地址空间添加为远程访问服务器的 IPv6 路由表中的静态路由。|  
    |IPv6 Internet 和 IPv4 Intranet|远程访问服务器使用 Microsoft 6to4 适配器接口将 IPv6 路由流量转发到 IPv4 Internet 上的 6to4 中继。 可以使用以下命令为 IPv4 Internet 上的 Microsoft 6to4 中继的 IPv4 地址配置远程访问服务器（在企业网络中未部署本机 IPv6 时使用）：netsh interface ipv6 6to4 set relay name=192.88.99.1 state=enabled 命令。|||  
  
    > [!NOTE]
    > 1. 如果已为 DirectAccess 客户端分配公用 IPv4 地址，则它将使用 6to4 转换技术连接到 Intranet。 如果 DirectAccess 客户端无法使用 6to4 连接到 DirectAccess 服务器，则它将使用 IP-HTTPS。  
    > 2. 本机 IPv6 客户端计算机可以通过本机 IPv6 连接到远程访问服务器，而无需任何转换技术。  
  
### <a name="plan-firewall-requirements"></a>规划防火墙要求

如果远程访问服务器位于边缘防火墙后面，则当远程访问服务器位于 IPv4 Internet 上时，远程访问通信还需要以下例外：  
  
- 6to4 通信 — IP 协议 41 入站和出站。  
  
- IP-HTTPS — 传输控制协议 (TCP) 目标端口 443，以及 TCP 源端口 443 出站。  
  
- 如果你使用单个网络适配器部署远程访问，并且在远程访问服务器上安装网络位置服务器，则还应免除 TCP 端口 62000。  
  
当远程访问服务器位于 IPv6 Internet 上时，远程访问通信将需要以下例外：  
  
- IP 协议 50  
  
- UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
当使用其它防火墙时，应用远程访问通信的以下内部网络防火墙例外情况：  
  
- ISATAP — 协议 41 入站和出站  
  
- 所有 IPv4/IPv6 通信的 TCP/UDP  
  
### <a name="plan-certificate-requirements"></a>规划证书要求

IPsec 的证书要求包括：在客户端和远程访问服务器之间建立 IPsec 连接时，DirectAccess 客户端计算机使用的计算机证书，以及在建立与 DirectAccess 客户端的 IPsec 连接时，远程访问服务器使用的计算机证书。 Windows Server 2012 中的 DirectAccess 使用这些 IPsec 证书不是必需的。 启用 DirectAccess 向导将远程访问服务器配置为充当 Kerberos 代理，以便无需证书即可执行 IPsec 身份验证。  
  
1. **IP-HTTPS 服务器**:在配置远程访问时，远程访问服务器自动配置为充当 IP-HTTPS web 侦听器。 IP-HTTPS 站点需要网站证书，并且客户端计算机必须能够联系该证书的证书吊销列表 (CRL) 站点。 启用 DirectAccess 向导会尝试使用 SSTP VPN 证书。 如果未配置 SSTP，它会检查计算机个人存储中是否存在 IP-HTTPS 的证书。 如果没有可用证书，它会自动创建自签名证书。  
  
2. **网络位置服务器**:网络位置服务器是一个用于检测客户端计算机是否位于企业网络中的网站。 网络位置服务器需要网站证书。 DirectAccess 客户端必须能够联系该证书的 CRL 站点。 启用 DirectAccess 向导会检查计算机个人存储区中是否存在网络位置服务器的证书。 如果不存在，则它会自动创建自签名证书。  
  
下表中总结了其中每项的证书要求：  
  
|IPsec 身份验证|IP-HTTPS 服务器|网络位置服务器|  
|------------|----------|--------------|  
|在未使用 Kerberos 代理进行身份验证时需要向远程访问服务器和 IPsec 身份验证的客户端颁发计算机证书的内部 CA|公共 CA：我们建议使用公共 CA 颁发 IP-HTTPS 证书，这可确保在已外部可用的 CRL 分发点。|内部 CA：可以使用内部 CA 颁发网络位置服务器网站证书。 请确保 CRL 分发点在内部网络中高度可用。|  
||内部 CA：可以使用内部 CA 颁发 IP-HTTPS 证书；但是，必须确保 CRL 分发点在外部可用。|自签名证书：可将自签名的证书用于网络位置服务器网站;但是，您不能在多站点部署中使用自签名的证书。|  
||自签名证书：可以对 IP-HTTPS 服务器使用自签名证书；但是，必须确保 CRL 分发点在外部可用。 无法在多站点部署中使用自签名证书。||  
  
#### <a name="plan-certificates-for-ip-https"></a>规划 IP-HTTPS 的证书

远程访问服务器充当 IP-HTTPS 侦听器，而且你必须在服务器上手动安装 HTTPS 网站证书。 在规划时请注意以下事项：  
  
- 建议使用公共 CA，以便可以随时使用 CRL。  
  
- 在“使用者”字段中，请指定远程访问服务器的 Internet 适配器的 IPv4 地址，或 IP-HTTPS URL（ConnectTo 地址）的 FQDN。 如果远程访问服务器位于 NAT 设备后面，应指定公用名或 NAT 设备的地址。  
  
- 该证书的公用名应与 IP-HTTPS 站点的名称相匹配。  
  
- 对于“增强型密钥使用”字段，请使用服务器身份验证对象标识符 (OID)。  
  
- 对于“CRL 分发点”字段，请指定已连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
- IP-HTTPS 证书必须包含私钥。  
  
- 必须直接将 IP-HTTPS 证书导入到个人存储中。  
  
- IP-HTTPS 证书的名称中可以包含通配符。  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>规划用于网络位置服务器的网站证书

在规划网络位置服务器网站时，请注意以下事项：  
  
- 在“使用者”字段中，请指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。  
  
- 对于“增强型密钥使用”字段，请使用服务器身份验证 OID。  
  
- 对于“CRL 分发点”字段，请指定连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。 不应从内部网络之外访问此 CRL 分发点。  
  
- 如果稍后你打算配置多站点或群集部署，则证书的名称不应该与远程访问服务器的内部名称相匹配。  

    > [!NOTE]  
    > 确保 IP-HTTPS 和网络位置服务器的证书包含“使用者名称”  。 如果该证书不包含  “使用者名称”但包含“备用名称”  ，则远程访问向导将不会接受它。  
  
#### <a name="plan-dns-requirements"></a>规划 DNS 要求

在远程访问部署中，以下项将需要 DNS：  
  
- **DirectAccess 客户端请求**:DNS 用于解析来自不位于内部网络上的 DirectAccess 客户端计算机的请求。 DirectAccess 客户端尝试连接到 DirectAccess 网络位置服务器，以确定它们是位于 Internet 上，还是位于企业网络上。如果连接成功，则确定客户端在 Intranet 上且未使用 DirectAccess，并使用客户端计算机的网络适配器上配置的 DNS 服务器解析客户端请求。 如果该连接不成功，则假定客户端在 Internet 上。 DirectAccess 客户端将使用名称解析策略表 (NRPT) 来确定在解析名称请求时使用哪个 DNS 服务器。 你可以指定客户端应使用 DirectAccess DNS64 或备用的内部 DNS 服务器来解析名称。 在执行名称解析时，将由 DirectAccess 客户端使用 NRPT 来确定如何处理请求。 客户端请求 FQDN 或单标签名称，如<https://internal>。 如果请求单标签名称，则将追加 DNS 后缀以产生 FQDN。 如果 DNS 查询与 NRPT 中某个条目匹配，并且为该条目指定了 DNS4 或 Intranet DNS 服务器，则将使用指定的服务器发送用于名称解析的 DNS 查询。 如果存在匹配条目，但未指定 DNS 服务器，这表示存在一条免除规则，且将应用普通的名称解析。  
  
    请注意，将新后缀添加远程访问管理控制台中的 NRPT 后，可通过单击  “检测”按钮自动发现该后缀对应的默认 DNS 服务器。 自动检测的工作原理如下：  
  
    1. 如果企业网络基于 IPv4 或者是 IPv4 和 IPv6，则默认地址是远程访问服务器上内部适配器的 DNS64 地址。  
  
    2. 如果企业网络基于 IPv6，则默认地址是企业网络中 DNS 服务器的 IPv6 地址。  
  
-  **基础结构服务器**  
  
    1. **网络位置服务器**:DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析该网络位置服务器的名称，但当它们位于 Internet 上时，必须阻止它们解析该名称。 为了确保这一点，默认情况下，网络位置服务器的 FQDN 将作为免除规则添加到 NRPT。 此外，在配置远程访问时，将自动创建以下规则：  
  
        1. 一条适用于远程访问服务器的根域或域名，以及对应远程访问服务器上配置的 Intranet DNS 服务器的 IPv6 地址的 DNS 后缀规则。 例如，如果远程访问服务器是 corp.contoso.com 域的成员，则会为 corp.contoso.com DNS 后缀创建一条规则。  
  
        2. 用于网络位置服务器的 FQDN 的免除规则。 例如，如果网络位置服务器 URL 为<https://nls.corp.contoso.com>，为 FQDN nls.corp.contoso.com 创建一条免除规则。  
  
        **IP-HTTPS 服务器**:远程访问服务器充当 IP HTTPS 侦听器，并使用其服务器证书对 IP-HTTPS 客户端进行身份验证。 IP-HTTPS 名称必须可由使用公用 DNS 服务器的 DirectAccess 客户端解析。  
  
        **连接性验证程序**:远程访问将创建由 DirectAccess 客户端计算机用来验证到内部网络的连接的默认 web 探测。 若要确保探测按预期运行，则必须在 DNS 中手动注册以下名称：  
  
        1.  directaccess webprobehost 应解析为远程访问服务器的内部 IPv4 地址或仅 IPv6 环境中的 IPv6 地址。  
  
        2.  directaccess corpconnectivityhost 应解析为本地主机 （环回） 地址。 应创建 A 和 AAAA 记录，A 记录具有值 127.0.0.1，AAAA 记录具有由 NAT64 前缀构造的值，且最后 32 位类似于 127.0.0.1。 可以通过运行 cmdlet get-netnattransitionconfiguration 来检索 NAT64 前缀。  
  
            > [!NOTE]  
            > 这是仅在仅使用 IPv4 的环境中有效。 在 IPv4 + IPv6 或仅支持 IPv6 环境中，只有 AAAA 记录应使用环回 IP 地址 ::1 创建。  
  
        你可以通过 HTTP 或 PING 使用其他 Web 地址创建其他连接性验证程序。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
#### <a name="dns-server-requirements"></a>DNS 服务器要求  
  
- 对于 DirectAccess 客户端，必须使用运行 Windows Server 2003、 Windows Server 2008、 Windows Server 2008 R2、 Windows Server 2012 或支持 IPv6 的任何 DNS 服务器的 DNS 服务器。  
  
### <a name="plan-active-directory"></a>规划 Active Directory

远程访问使用 Active Directory 和 Active Directory 组策略对象，如下所示：  
  
- **身份验证**:Active Directory 用于身份验证。 Intranet 隧道使用 Kerberos 身份验证以使用户可以访问内部资源。  
  
- **组策略对象**:远程访问将配置设置收集到应用于远程访问服务器、 客户端，以及内部应用程序服务器的组策略对象。  
  
- **安全组**：远程访问使用安全组来收集到一起并标识 DirectAccess 客户端计算机和远程访问服务器。 组策略将应用于所需的安全组。  
  
- **扩展的 IPsec 策略**:远程访问可以使用 IPsec 身份验证和客户端和远程访问服务器之间的加密。 可以将 IPsec 身份验证和加密扩展到指定的内部应用程序服务器。   
  
#### <a name="active-directory-requirements"></a>Active Directory 要求  
  
在规划远程访问部署的 Active Directory 时，需要满足以下条件：  
  
- 在 Windows Server 2012、 Windows Server 2008 R2 Windows Server 2008 或 Windows Server 2003 操作系统上安装至少一个域控制器。  
  
    如果域控制器位于外围网络上（并因此可从远程访问服务器的面向 Internet 的网络适配器中访问），请通过在域控制器上添加数据包筛选器来阻止远程访问服务器访问它，进而阻止连接到 Internet 适配器的 IP 地址。  
  
- 远程访问客户端必须使用 V.90 调制解调器。  
  
- DirectAccess 客户端必须是域成员。 客户端可以属于：  
  
    - 与远程访问服务器位于同一林中的任何域。  
  
    - 与远程访问服务器域具有双向信任的任何域。  
  
    - 与远程访问域所属的林之间具有双向信任的林中任何域。  
  
> [!NOTE]  
> - 远程访问服务器不可以是域控制器。  
> - 不可从远程访问服务器的外部 Internet 适配器中访问用于远程访问的 Active Directory 域控制器（该适配器不可位于 Windows 防火墙的域配置文件中）。  
  
### <a name="plan-group-policy-objects"></a>规划组策略对象

将配置远程访问时配置的 DirectAccess 设置收集到组策略对象 (GPO) 中。 使用 DirectAccess 设置填充三个不同的 GPO 并按如下方式分发它们：  
  
- **DirectAccess 客户端 GPO**:此 GPO 包含客户端设置，包括 IPv6 转换技术设置、NRPT 条目和高级安全 Windows 防火墙连接安全规则。 将 GPO 应用于为客户端计算机指定的安全组。  
  
- **DirectAccess 服务器 GPO**:此 GPO 包含应用于配置为你的部署中的远程访问服务器的任一服务器的 DirectAccess 配置设置。 它还包含高级安全 Windows 防火墙连接安全规则。  
  
- **应用程序服务器 GPO**:此 GPO 包含用于所选应用程序服务器的设置，你选择性地将身份验证和加密从 DirectAccess 客户端扩展到这些服务器。 如果未扩展身份验证和加密，则不会使用此 GPO。  
  
启用 DirectAccess 向导将自动创建 GPO 并为每个 GPO 指定默认名称。  
  
> [!CAUTION]  
> 在执行 DirectAccess cmdlet 之前，请使用以下过程来备份所有远程访问组策略对象：[备份和还原远程访问配置](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
可以通过两种方式配置 GPO：  
  
1. **自动**:可以指定自动创建它们。 为每个 GPO 指定默认名称。  
  
2. **手动**:可以使用已由 Active Directory 管理员预定义的 GPO。  
  
请注意，将 DirectAccess 配置为使用特定的 GPO 后，无法将它配置为使用不同的 GPO。  
  
#### <a name="automatically-created-gpos"></a>自动创建的 GPO

使用自动创建的 GPO 时，请注意以下事项：  
  
将根据位置和链接目标参数应用自动创建的 GPO，如下所示：  
  
- 对于 DirectAccess 服务器 GPO，位置和链接参数都指向包含远程访问服务器的域。  
  
- 创建客户端 GPO 后，将该位置设置为将在其中创建 GPO 的单个域。 在每个域中查找 GPO 名称，如果存在，则使用 DirectAccess 设置对其进行填充。 将链接目标设置为在其中创建了 GPO 的域的根。 为每个包含客户端计算机或应用程序服务器的域创建一个 GPO，并将该 GPO 链接到其各自域的根。  
  
在使用自动创建的 GPO 时，若要应用 DirectAccess 设置，远程访问服务器管理员需要以下权限：  
  
- 每个域的 GPO 创建权限。  
  
- 所有选定客户端域根的链接权限。  
  
- 服务器 GPO 域根的链接权限。  
  
- GPO 需要创建、编辑、删除和修改安全权限。  
  
- 我们建议远程访问管理员具有对每个所需的域的 GPO 读取权限。 这使远程访问可以验证在创建 GPO 时不存在具有相同名称的 GPO。  
  
请注意，如果不存在用于链接 GPO 的正确权限，则会发出警告。 远程访问操作将继续进行，但不会出现链接。 如果发出此警告，则即使在添加了权限后，也不会自动创建链接。 管理员将需要手动创建链接。  
  
#### <a name="manually-created-gpos"></a>手动创建的 GPO

使用手动创建的 GPO 时，请注意以下事项：  
  
- 在运行远程访问安装向导之前，这些 GPO 应存在。  
  
- 在使用手动创建的 GPO 时，若要应用 DirectAccess 设置，远程访问管理员需要对手动创建的 GPO 具有完全 GPO 权限（编辑、删除、修改安全性）。  
  
- 在使用手动创建的 GPO 时，将在整个域中搜索到 GPO 的链接。 如果未在域中链接 GPO，将在域的根中自动创建链接。 如果未提供创建链接所需的权限，则会发出警告。  
  
请注意，如果不存在用于链接 GPO 的正确权限，则会发出警告。 远程访问操作将继续进行，但不会出现链接。 如果发出此警告，则即使之后添加了权限，也不会自动创建链接。 管理员将需要手动创建链接。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>从已删除的 GPO 中恢复

如果已意外删除远程访问服务器、客户端或应用程序服务器 GPO，并且没有可用备份，你必须删除配置设置并再次重新配置。 如果有可用的备份，则可以从备份中还原 GPO。  
  
“远程访问管理”  将显示以下错误消息：**找不到 GPO （组策略对象名称）** 。 若要删除配置设置，请执行以下步骤：  
  
1. 运行 PowerShell cmdlet **Uninstall-remoteaccess**。  
  
2. 重新打开**远程访问管理**。  
  
3. 你将看到关于未找到 GPO 的错误消息。 单击“删除配置设置”  。 完成操作后，服务器将还原到未配置状态。  

