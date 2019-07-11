---
title: 步骤 1 规划远程访问基础结构
description: 本主题是指南的在 Windows Server 2016 中远程管理 DirectAccess 客户端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f3b1837145dee5767741052c548a4b44da56659b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792333"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>步骤 1 规划远程访问基础结构

>适用于：Windows Server（半年频道）、Windows Server 2016

> [!NOTE]
> Windows Server 2016 将 DirectAccess 以及路由和远程访问服务 (RRAS) 合并到单个远程访问角色。  
  
本主题介绍可用于设置远程管理 DirectAccess 客户端的单个远程访问服务器的基础结构规划步骤。 下表列出了这些步骤，但无需按特定顺序完成这些规划任务。  
  
|任务|描述|  
|----|--------|  
|[规划网络拓扑和服务器设置](#plan-network-topology-and-settings)|确定放置远程访问服务器 （在边缘或网络地址转换 (NAT) 设备或防火墙后面），并规划 IP 寻址和路由的位置。|  
|[规划防火墙要求](#plan-firewall-requirements)|规划允许远程访问通过边缘防火墙。|  
|[规划证书要求](#plan-certificate-requirements)|确定是否将使用 Kerberos 协议或证书进行客户端身份验证，并规划你的网站证书。<br/><br/>IP-HTTPS 是一种转换协议，DirectAccess 客户端使用该协议在 IPv4 网络上对 IPv6 通信进行隧道传送。 决定是否使用通过由远程访问服务器自动颁发自签名的证书或证书颁发机构 (CA) 颁发的证书进行 IP-HTTPS 身份验证服务器。|  
|[规划 DNS 要求](#plan-dns-requirements)|规划远程访问服务器、 基础结构服务器、 本地名称解析选项和客户端连接的域名系统 (DNS) 设置。| 
|[规划网络位置服务器配置](#plan-the-network-location-server-configuration)|决定要将网络位置服务器网站放置在你的组织 （在远程访问服务器或备用的服务器） 和规划证书要求，如果网络位置服务器将位于远程访问服务器上。 **注意：** DirectAccess 客户端使用网络位置服务器来确定它们是否位于内部网络上。|  
|[计划管理服务器配置](#plan-management-servers-configuration)|规划在远程客户端管理过程中使用的管理服务器（如更新服务器）。 **注意：** 管理员可以使用 Internet 远程管理位于企业网络之外的 DirectAccess 客户端计算机。|  
|[规划 Active Directory 要求](#plan-active-directory-requirements)|计划你的域控制器，Active Directory 的要求、 客户端身份验证，以及多个域结构。|  
|[计划组策略对象创建](#plan-group-policy-object-creation)|确定 Gpo 所需在你的组织以及如何创建和编辑 Gpo。|  
  
## <a name="plan-network-topology-and-settings"></a>规划网络拓扑和设置  
当你计划你的网络时，需要考虑网络适配器拓扑中，对于 IP 寻址，设置和 ISATAP 的要求。  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>规划网络适配器和 IP 寻址  
  
1.  标识你想要使用的网络适配器拓扑。 可以使用任何以下拓扑设置远程访问：  
  
    -   具有两个网络适配器：连接到 Internet，到内部网络将另一个网络适配器与远程访问服务器安装在边缘。  
  
    -   具有两个网络适配器：使用一个网络适配器连接到外围网络和到内部网络的其他情况下，远程访问服务器安装在 NAT 设备、 防火墙或路由器，后面。  
  
    -   使用一个网络适配器：NAT 设备后面安装远程访问服务器和单个网络适配器连接到内部网络。  
  
2.  标识 IP 寻址要求：  
  
    DirectAccess 使用 IPv6 和 IPsec 在 DirectAccess 客户端计算机和内部企业网络之间创建安全连接。 但是，DirectAccess 不一定需要连接到 IPv6 Internet 或内部网络上的本机 IPv6 支持。 相反，它会自动配置，并使用 IPv6 转换技术在 IPv4 internet （6to4、 Teredo 或 IP-HTTPS） 和仅使用 IPv4 的 intranet 上 （NAT64 或 ISATAP） 隧道 IPv6 通信。 有关这些转换技术的概述，请参阅以下资源：  
  
    -   [IPv6 转换技术](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [IP-HTTPS 隧道协议规范](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  按下表配置所需的适配器和寻址。 有关部署使用单个网络适配器，在 NAT 设备后面的使用仅配置你的 IP 地址**内部网络适配器**列。  
  
    ||外部网络适配器|内部网络适配器<sup>1、 更高版本</sup>|路由要求|  
    |-|--------------|------------------------|------------|  
    |IPv4 Internet 和 IPv4 Intranet|配置以下内容：<br/><br/>-两个静态连续公用 IPv4 地址 （仅 Teredo 要求） 的相应的子网掩码。<br/>默认网关的 Internet 防火墙或本地 Internet 服务提供商 (ISP) 路由器的 IPv4 地址。 **注意：** 远程访问服务器需要两个连续公用 IPv4 地址，以便它可以充当 Teredo 服务器和基于 Windows 的 Teredo 客户端可以使用远程访问服务器检测 NAT 设备的类型。|配置以下内容：<br/><br/>的带有相应子网掩码 IPv4 intranet 地址。<br/>的 intranet 命名空间特定于连接的 DNS 后缀。 还应在内部接口上配置 DNS 服务器。 **警告：** 不要在任何 Intranet 接口上配置默认网关。|若要配置远程访问服务器以访问内部 IPv4 网络上的所有子网，请执行以下操作：<br/><br/>-列出在 intranet 上的所有位置的 IPv4 地址空间。<br/>-使用`route add -p`或`netsh interface ipv4 add route`命令添加 IPv4 地址空间中的远程访问服务器 IPv4 路由表的静态路由。|  
    |IPv6 Internet 和 IPv6 Intranet|配置以下内容：<br/><br/>-使用您的 ISP 提供的自动配置地址配置。<br/>-使用`route print`命令，以确保指向 ISP 路由器的默认 IPv6 路由存在 IPv6 路由表中。<br/>-确定 ISP 和 intranet 路由器是否使用默认路由器首选项 RFC 4191 中所述，如果它们使用比本地 intranet 路由器更高版本的默认首选项。 如果两个结果都为“是”，则默认路由不需要任何其他配置。 ISP 路由器的更高首选等级可确保远程访问服务器的活动默认 IPv6 路由指向 IPv6 Internet。<br/><br/>因为远程访问服务器是一个 IPv6 路由器，所以如果你具有本机 IPv6 基础结构，则 Internet 接口也可以访问 Intranet 上的域控制器。 在这种情况下，将数据包筛选器添加到阻止连接到远程访问服务器的 Internet 接口的 IPv6 地址的外围网络中的域控制器。|配置以下内容：<br/><br/>如果不使用默认首选等级，通过配置 intranet 接口`netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled`命令。 这一命令可确保不会将指向 Intranet 路由器的其他默认路由添加到 IPv6 路由表。 你可以从显示中获取 intranet 接口的 InterfaceIndex`netsh interface show interface`命令。|如果你拥有 IPv6 Intranet，若要配置远程访问服务器以访问所有的 IPv6 位置，请执行以下操作：<br/><br/>-列出在 intranet 上的所有位置的 IPv6 地址空间。<br/>-使用`netsh interface ipv6 add route`命令将 IPv6 地址空间添加为远程访问服务器的 IPv6 路由表中的静态路由。|  
    |IPv4 Internet 和 IPv6 Intranet|远程访问服务器将默认 IPv6 路由通信转发到 6to4 中继的 Microsoft 6to4 适配器接口使用 IPv4 Internet 上。 不在企业网络中部署本机 IPv6，可以使用以下命令来配置 IPv4 Internet 上的 Microsoft 6to4 中继的 IPv4 地址的远程访问服务器： `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`。|||  
  
    > [!NOTE]  
    > -   如果 DirectAccess 客户端分配的公共 IPv4 地址，它将使用 6to4 中继技术连接到 intranet。 如果客户端分配了专用 IPv4 地址，它将使用 Teredo。 如果 DirectAccess 客户端无法使用 6to4 或 Teredo 连接到 DirectAccess 服务器，则它将使用 IP-HTTPS。  
    > -   若要使用 Teredo，必须在面向外部的网络适配器上配置两个连续的 IP 地址。  
    > -   如果远程访问服务器具有只有一个网络适配器，则无法使用 Teredo。  
    > -   本机 IPv6 客户端计算机可以通过本机 IPv6 连接到远程访问服务器，而无需任何转换技术。  
  
### <a name="plan-isatap-requirements"></a>规划 ISATAP 要求  
需要为远程管理的 DirectAccessclients，ISATAP，以便 DirectAccess 管理服务器可以连接到位于 Internet 上的 DirectAccess 客户端。 ISATAP 不需要支持由 DirectAccess 客户端计算机连接到公司网络上的 IPv4 资源启动的连接。 为此，将使用 NAT64/DNS64。 如果您的部署需要 ISATAP，使用下表来确定你的要求。  
  
|ISATAP 部署方案|要求|  
|---------------|--------|  
|（没有 ISATAP 是必需的） 现有的本机 IPv6 intranet|与现有本机 IPv6 基础结构，指定的远程访问在部署期间，组织中的前缀和远程访问服务器不会将自身配置为 ISATAP 路由器。 请执行以下操作：<br/><br/>1.若要确保 DirectAccess 客户端从 intranet 访问，则必须修改您 IPv6 路由，以便将默认路由通信转发到远程访问服务器。 如果您的 intranet IPv6 地址空间使用单个的 48 位 IPv6 地址前缀之外的地址，必须在部署期间指定的相关组织 IPv6 前缀。<br/>2.如果您已经连接到 IPv6 Internet，您必须配置默认路由流量以便将转发到远程访问服务器，，然后配置相应的连接和路由在远程访问服务器上，以便将默认路由流量转发到已连接到 IPv6 Internet 的设备。|  
|现有的 ISATAP 部署|如果有现有的 ISATAP 基础结构，在部署期间系统会提示输入组织的 48 位前缀和远程访问服务器不会将自身配置为 ISATAP 路由器。 若要确保 DirectAccess 客户端从 intranet 访问，必须修改将 IPv6 路由基础结构，以便默认路由通信转发到远程访问服务器。 此更改需要在现有的 intranet 客户端必须已被转发默认通信的 ISATAP 路由器上完成。|  
|没有现有的 IPv6 连接|当远程访问安装向导检测到的服务器具有任何本机或基于 ISATAP 的 IPv6 连接时，它会自动派生的 intranet，基于 6to4 的 48 位前缀和远程访问服务器配置为 ISATAP 路由器，用于提供 IPv6连接到 intranet 上的 ISATAP 主机。 （仅当服务器具有公共地址使用 6to4 基于前缀，否则前缀自动从生成的唯一本地地址范围。）<br/><br/>若要使用 ISATAP，请执行以下操作：<br/><br/>1.注册你想要对其启用基于 ISATAP 的连接，从而使 ISATAP 名称解析为远程访问服务器的内部 IPv4 地址的内部 DNS 服务器的每个域的 DNS 服务器上的 ISATAP 名称。<br/>2.默认情况下，运行 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的 DNS 服务器通过使用全局查询阻止列表阻止 ISATAP 名称的解析。 若要启用 ISATAP，必须从块列表中删除 ISATAP 名称。 有关详细信息，请参阅[从 DNS 全局查询阻止列表中删除 ISATAP](https://go.microsoft.com/fwlink/p/?LinkId=168593)。<br/><br/>可以自动解决 ISATAP 名称的基于 Windows 的 ISATAP 主机配置地址与远程访问服务器，如下所示：<br/><br/>1.在 ISATAP 隧道接口上基于 ISATAP 的 IPv6 地址<br/>2.一个 64 位路由，提供与 intranet 上的其他 ISATAP 主机的连接<br/>3.默认 IPv6 路由指向远程访问服务器。 默认路由可确保 intranet ISATAP 主机可以访问的 DirectAccess 客户端<br/><br/>当基于 Windows 的 ISATAP 主机获得基于 ISATAP 的 IPv6 地址时，他们开始使用 ISATAP 封装的流量进行通信如果目标也是 ISATAP 主机。 因为 ISATAP 的整个 intranet 使用单个 64 位的子网，通信将进入从分段 IPv4 模型的通信，具有 IPv6 的单个子网的通信模型。 这可能会影响某些 Active Directory 域服务 (AD DS) 的行为和应用程序依赖于 Active Directory 站点和服务配置的。 例如，如果使用 Active Directory 站点和服务管理单元来配置站点、 基于 IPv4 的子网和站点间传输将请求转发到站点内的服务器，此配置不是使用由 ISATAP 主机。<br/><br/><ol><li>若要为站点内的 ISATAP 主机，每个 IPv4 子网对象的转发配置 Active Directory 站点和服务必须配置一个等效的 IPv6 子网对象，在其中的子网的 IPv6 地址前缀表示相同的范围的 ISATAP 主机作为 IPv4 子网的地址。 例如，对于 IPv4 子网 192.168.99.0/24 和 64 位 ISATAP 地址前缀 2002:836b:1:8000:: / 64，IPv6 子网对象的等效 IPv6 地址前缀是 2002:836b:1:8000:0:5efe:192.168.99.0/120。 对于任意 IPv4 前缀长度 （设置为 24，在示例中），您可以确定相应的 IPv6 前缀长度，从公式 96 + IPv4PrefixLength。</li><li>DirectAccess 客户端的 IPv6 地址，添加以下代码：<br/><br/><ul><li>为基于 Teredo 的 DirectAccess 客户端：IPv6 子网范围 2001:0:WWXX:YYZZ:: / 64，哪些 WWXX:YYZZ 中的是远程访问服务器的第一个面向 Internet 的 IPv4 地址的冒号十六进制版本。 .</li><li>为基于 IP 的 HTTPS DirectAccess 客户端：IPv6 子网范围 2002:WWXX:YYZZ:8100:: / 56，哪些 WWXX:YYZZ 中的是冒号十六进制版本的远程访问服务器的第一个面向 Internet 的 IPv4 地址 (w.x.y.z)。 .</li><li>基于 6to4 的 DirectAccess 客户端：一系列基于 6to4 的 IPv6 前缀开头 2002年： 和表示的区域的公共 IPv4 地址前缀受 Internet 分配号机构 (IANA) 和区域的注册表。 公共 IPv4 地址前缀 w.x.y.z/n 的 6to4 基于前缀是 2002:WWXX:YYZZ:: / [16 + n]，在哪些 WWXX:YYZZ 是 w.x.y.z.的冒号十六进制版本<br/><br/>        例如，7.0.0.0/8 范围是由 American Registry 管理为 Internet 数字 (ARIN) 对于北美地区。 此公共 IPv6 地址范围内的相应 6to4 基于前缀是 2002:700:: / 24。 IPv4 公用地址空间有关的信息，请参阅[IANA IPv4 地址空间注册表](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml)。 .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> 确保不要在 DirectAccess 服务器的内部接口上具有公共 IP 地址。 如果在内部接口上有公共 IP 地址，通过 ISATAP 的连接可能会失败。  
  
### <a name="plan-firewall-requirements"></a>规划防火墙要求  
如果远程访问服务器位于边缘防火墙后面，则当远程访问服务器位于 IPv4 Internet 上时，远程访问通信还需要以下例外：  
  
-   用于 IP-HTTPS:传输控制协议 (TCP) 目标端口 443，以及 TCP 源端口 443 出站。  
  
-   Teredo 通信：用户数据报协议 (UDP) 目标端口 3544 入站，以及 UDP 源端口 3544 出站。  
  
-   6to4 通信：IP 协议 41 入站和出站。  
  
    > [!NOTE]  
    > 对于 Teredo 和 6to4 通信，这些例外应适用于远程访问服务器上两个面向 Internet 的连续公用 IPv4 地址。  
    >   
    > 对于 IP-HTTPS 异常需要应用于在公共 DNS 服务器注册的地址。  
  
-   如果要使用单个网络适配器和远程访问服务器上安装网络位置服务器部署远程访问，TCP 端口 62000。  
  
    > [!NOTE]  
    > 在远程访问服务器上，则可以进行免除和上一免除位于边缘防火墙上。  
  
当远程访问服务器位于 IPv6 Internet 上时，以下异常所需的远程访问通信：  
  
-   IP 协议 50  
  
-   UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
-   ICMPv6 通信入站和出站 （仅使用 Teredo 时）。  
  
当使用其他防火墙时，应用远程访问通信的以下内部网络防火墙例外情况：  
  
-   有关 ISATAP:协议 41 入站和出站  
  
-   对所有 IPv4/IPv6 流量：TCP/UD  
  
-   对于 Teredo:所有 IPv4/IPv6 通信的 ICMP  
  
### <a name="plan-certificate-requirements"></a>规划证书要求  
有三种部署单一远程访问服务器时需要证书的方案。  
  
-   **IPsec 身份验证**:IPsec 的证书要求包括 DirectAccess 客户端计算机它们建立与远程访问服务器的 IPsec 连接时使用的计算机证书和远程访问服务器用于建立的计算机证书与 DirectAccess 客户端 IPsec 连接。  
  
    对于 Windows Server 2012 中的 DirectAccess，不强制使用这些 IPsec 证书。 作为替代方法，远程访问服务器可以充当代理的 Kerberos 身份验证而无需证书。 如果使用 Kerberos 身份验证，则它可以通过 SSL 工作和 Kerberos 协议使用已配置用于 IP-HTTPS 的证书。 某些企业方案 （包括多站点部署和一次性密码客户端身份验证） 需要使用证书身份验证，并不是 Kerberos 身份验证。  
  
-   **IP-HTTPS 服务器**:在配置远程访问时，远程访问服务器自动配置为充当 IP-HTTPS web 侦听器。 IP-HTTPS 站点需要网站证书，并且客户端计算机必须能够联系该证书的证书吊销列表 (CRL) 站点。  
  
-   **网络位置服务器**:网络位置服务器是一个用于检测客户端计算机是否位于企业网络中的网站。 网络位置服务器需要网站证书。 DirectAccess 客户端必须能够联系该证书的 CRL 站点。  
  
下表总结了每种方案的证书颁发机构 (CA) 要求。  
  
|IPsec 身份验证|IP-HTTPS 服务器|网络位置服务器|  
|------------|----------|--------------|  
|在未使用 Kerberos 协议进行身份验证时，可以向远程访问服务器和 IPsec 身份验证的客户端颁发计算机证书需要内部 CA。|内部 CA：可以使用内部 CA 颁发 IP-HTTPS 证书；但是，必须确保 CRL 分发点在外部可用。|内部 CA：可以使用内部 CA 颁发网络位置服务器网站证书。 请确保 CRL 分发点在内部网络中高度可用。|  
||自签名证书：可以为 IP-HTTPS 服务器使用自签名的证书。 无法在多站点部署中使用自签名证书。|自签名证书：可将自签名的证书用于网络位置服务器网站;但是，您不能在多站点部署中使用自签名的证书。|  
||公共 CA：我们建议你使用公共 CA 颁发 IP-HTTPS 证书，这可确保 CRL 分发点可用外部。||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>规划用于 IPsec 身份验证的计算机证书  
如果使用基于证书的 IPsec 身份验证，获取计算机证书是所需的远程访问服务器和客户端。 若要安装证书的最简单方法是使用组策略来配置计算机证书自动注册。 此方法将确保所有域成员都从企业 CA 获取证书。 如果你的组织中未设置企业 CA，请参阅 [Active Directory 证书服务](https://technet.microsoft.com/library/cc770357.aspx)。  
  
此证书具有以下要求：  
  
-   该证书应具有客户端身份验证扩展密钥用法 (EKU)。  
  
-   客户端和服务器证书应与相同的根证书。 必须在 DirectAccess 配置设置中选择该根证书。  
  
#### <a name="plan-certificates-for-ip-https"></a>规划 IP-HTTPS 的证书  
远程访问服务器充当 IP-HTTPS 侦听器，而且你必须在服务器上手动安装 HTTPS 网站证书。 规划时，请考虑以下内容：  
  
-   建议使用公共 CA，以便可以随时使用 CRL。  
  
-   在主题字段中，指定远程访问服务器的 Internet 适配器或 IP-HTTPS URL （ConnectTo 地址） 的 FQDN 的 IPv4 地址。 如果远程访问服务器位于 NAT 设备后面，应指定公用名或 NAT 设备的地址。  
  
-   该证书的公用名应与 IP-HTTPS 站点的名称相匹配。  
  
-   有关**增强型密钥用法**字段中，使用服务器身份验证对象标识符 (OID)。  
  
-   对于“CRL 分发点”  字段，请指定已连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
    > [!NOTE]  
    > 这只是所需的运行 Windows 7 的客户端。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   必须直接将 IP-HTTPS 证书导入到个人存储中。  
  
-   IP-HTTPS 证书可以在名称中包含通配符符。  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>规划用于网络位置服务器的网站证书  
规划网络位置服务器网站时，请考虑以下方法：  
  
-   在  “使用者”字段中，指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。  
  
-   有关**增强型密钥用法**字段中，使用服务器身份验证 OID。  
  
-   有关**CRL 分发点**字段中，使用连接到 intranet 的 DirectAccess 客户端可访问的 CRL 分发点。 不应从内部网络之外访问此 CRL 分发点。  
  
> [!NOTE]  
> 确保用于 IP-HTTPS 和网络位置服务器证书包含使用者名称。 如果证书使用备用名称，远程访问向导将不会接受。  
  
#### <a name="plan-dns-requirements"></a>规划 DNS 要求  
本部分介绍远程访问部署中的服务器和客户端的 DNS 要求。  
  
##### <a name="directaccess-client-requests"></a>DirectAccess 客户端请求  
DNS 用于解析来自不位于内部网络上的 DirectAccess 客户端计算机的请求。 DirectAccess 客户端尝试连接到 DirectAccess 网络位置服务器，以确定它们是否位于 Internet 上，也可以在企业网络上。  
  
-   如果连接成功，确定客户端在 intranet 上，未使用 DirectAccess，并使用客户端计算机的网络适配器配置的 DNS 服务器解析客户端请求。  
  
-   如果该连接不成功，则假定客户端在 Internet 上。 DirectAccess 客户端将使用名称解析策略表 (NRPT) 来确定在解析名称请求时使用哪个 DNS 服务器。 你可以指定客户端应使用 DirectAccess DNS64 或备用的内部 DNS 服务器来解析名称。  
  
在执行名称解析时，将由 DirectAccess 客户端使用 NRPT 来确定如何处理请求。 客户端请求 FQDN 或单标签名称，如<https://internal>。 如果请求单标签名称，则将追加 DNS 后缀以产生 FQDN。 如果 DNS 查询与 NRPT 中的某个条目匹配的条目指定了 DNS4 或 intranet DNS 服务器，该查询是通过使用指定的服务器发送用于名称解析。 如果存在匹配项，但未指定任何 DNS 服务器，则免除规则和普通视图应用的名称解析。  
  
当新后缀添加到远程访问管理控制台中的 NRPT 后时，该后缀的默认 DNS 服务器可以自动发现通过单击**检测**按钮。 自动检测的工作方式如下：  
  
-   如果企业网络基于 IPv4 的或它使用 IPv4 和 IPv6，则默认地址是远程访问服务器上内部适配器的 DNS64 地址。  
  
-   如果企业网络基于 IPv6，则默认地址是企业网络中 DNS 服务器的 IPv6 地址。  
  
##### <a name="infrastructure-servers"></a>基础结构服务器  
  
-   **网络位置服务器**  
  
    DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析网络位置服务器的名称，它们必须阻止它们位于 Internet 上时，将名称解析。 为了确保这一点，默认情况下，将网络位置服务器的 FQDN 作为免除规则添加到 NRPT。 此外，在配置远程访问时，将自动创建以下规则：  
  
    -   DNS 后缀根域或远程访问服务器，以及对应于在远程访问服务器配置的 intranet DNS 服务器的 IPv6 地址的域名的规则。 例如，如果远程访问服务器是 corp.contoso.com 域的成员，则会为 corp.contoso.com DNS 后缀创建一条规则。  
  
    -   用于网络位置服务器的 FQDN 的免除规则。 例如，如果网络位置服务器 URL 为<https://nls.corp.contoso.com>，为 FQDN nls.corp.contoso.com 创建一条免除规则。  
  
-   **IP-HTTPS 服务器**  
  
    远程访问服务器充当 IP HTTPS 侦听器，并使用其服务器证书对 IP-HTTPS 客户端进行身份验证。 IP-HTTPS 名称必须是可由使用公用 DNS 服务器的 DirectAccess 客户端解析。  
  
##### <a name="connectivity-verifiers"></a>连接性验证程序  
远程访问将创建可由 DirectAccess 客户端计算机用来验证到内部网络的连接的默认 Web 探测。 若要确保探测按预期方式运行，必须在 DNS 中手动注册以下名称：  
  
-   **directaccess webprobehost**应解析为远程访问服务器的内部 IPv4 地址或仅 IPv6 环境中的 IPv6 地址解析。  
  
-   **directaccess corpconnectivityhost**应解析为本地主机 （环回） 地址。 应创建 A 和 AAAA 记录。 A 记录的值是 127.0.0.1，并从且最后 32 位类似于 127.0.0.1 的 NAT64 前缀构造 AAAA 记录的值。 可以通过运行来检索 NAT64 前缀**Get-netnattransitionconfiguration** Windows PowerShell cmdlet。  
  
    > [!NOTE]  
    > 这是仅在仅使用 IPv4 的环境中有效。 在 IPv4 和 IPv6 或仅支持 IPv6 的环境中，使用环回 IP 地址创建只有 AAAA 记录:: 1。  
  
可以通过 HTTP 或 PING 使用其他 web 地址来创建其他连接性验证程序。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
##### <a name="dns-server-requirements"></a>DNS 服务器要求  
  
-   对于 DirectAccess 客户端，必须使用运行 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003、 或支持 IPv6 的任何 DNS 服务器的 DNS 服务器。  
  
-   应使用支持动态更新的 DNS 服务器。 可以使用 DNS 服务器不支持动态更新，但然后必须手动更新条目。  
  
-   通过使用 Internet DNS 服务器，必须解析到 CRL 分发点的 FQDN。 例如，如果 URL<https://crl.contoso.com/crld/corp-DC1-CA.crl>处于**CRL 分发点**字段的 IP-HTTPS 证书的远程访问服务器，必须确保 FQDN crld.contoso.com 通过使用 Internet DNS 服务器进行解析。  
  
#### <a name="plan-for-local-name-resolution"></a>规划本地名称解析  
规划本地名称解析时，请考虑以下方法：  
  
##### <a name="nrpt"></a>NRPT  
您可能需要在以下情况下创建其他名称解析策略表 (NRPT) 规则：  
  
-   您需要添加更多的 DNS 后缀为 intranet 命名空间。  
  
-   如果到 CRL 分发点的 Fqdn 基于 intranet 命名空间，您必须为 CRL 分发点的 Fqdn 添加免除规则。  
  
-   如果必须拆分式 DNS 环境，你必须添加免除规则要为其 DirectAccess 客户端位于 Internet 来访问 Internet 版本，而不是 intranet 版本上的资源的名称。  
  
-   如果您将流量重定向至外部网站通过 intranet web 代理服务器，外部网站，则只能从 intranet。 它使用 web 代理服务器的地址以允许入站的请求。 在此情况下，该外部网站的 fqdn 添加免除规则，并指定该规则使用 intranet web 代理服务器而不是 intranet DNS 服务器的 IPv6 地址。  
  
    例如，假设您要测试名为 test.contoso.com 的外部网站。 此名称不可通过 Internet DNS 服务器解析，但 Contoso web 代理服务器知道如何解析名称，以及如何将请求定向到外部 web 服务器的网站以。 为了阻止非 Contoso Intranet 上的用户访问该站点，外部网站将仅允许来自 Contoso Web 代理的 IPv4 Internet 地址的请求。 因此，intranet 用户可以访问该网站，因为它们使用 Contoso web 代理，但 DirectAccess 用户不能因不使用 Contoso web 代理。 通过为使用 Contoso Web 代理的 test.contoso.com 配置 NRPT 免除规则，可在 IPv4 Internet 上将 test.contoso.com 的网页请求路由到 Intranet Web 代理服务器。  
  
##### <a name="single-label-names"></a>单标签名称  
单标签名称，如<https://paycheck>，有时用于 intranet 服务器。 如果请求单标签名称并配置了 DNS 后缀搜索列表，列表中的 DNS 后缀将追加到单标签名称。 例如，当用户在一台计算机上是 corp.contoso.com 域类型的成员<https://paycheck>在 web 浏览器中，构造为该名称的 FQDN 为 paycheck.corp.contoso.com。 默认情况下，附加的后缀基于客户端计算机的主 DNS 后缀。  
  
> [!NOTE]  
> 在相互连接的命名空间方案中 （其中一个或多个域计算机具有到该计算机是成员的 Active Directory 域不匹配的 DNS 后缀），应确保搜索列表自定义以包括所有必需的后缀。 默认情况下，远程访问向导将配置为在客户端上的主 DNS 后缀的 Active Directory DNS 名称。 请确保添加客户端用于名称解析的 DNS 后缀。  
  
如果在组织中部署多个域和 Windows Internet 名称服务 (WINS) 和远程连接，可以按如下所示解析单个名称：  
  
-   通过部署 DNS 中的 WINS 正向查找区域。 在尝试解析 computername.dns.zone1.corp.contoso.com 时，会将请求定向到仅使用计算机名称的 WINS 服务器。 客户端会认为它颁发的常规的 DNS A 记录请求，但它实际上是 NetBIOS 请求。  
  
    有关详细信息，请参阅[管理正向查找区域](https://technet.microsoft.com/library/cc816891(WS.10).aspx)。  
  
-   通过将 DNS 后缀 (例如 dns.zone1.corp.contoso.com) 添加到默认域 GPO。  
  
##### <a name="split-brain-dns"></a>拆分式 DNS  
拆分式 DNS 指同一 DNS 域用于 Internet 和 intranet 名称解析。  
  
对于拆分式 DNS 部署，则必须列出在 Internet 和 intranet，会有重复，并决定哪些资源的 Fqdn DirectAccess 客户端应市场宣传 intranet 或 Internet 版本。 当你想 DirectAccess 客户端访问 Internet 版本时，必须将相应 FQDN 作为免除规则添加到每个资源的 NRPT。  
  
在拆分式 DNS 环境中，如果你想要在可用的资源的两个版本，intranet 资源使用配置名称不重复使用在 Internet 的名称。 然后指示用户能够访问 intranet 上的资源时使用的备用名称。 例如，配置 www\.www 的内部名称的 internal.contoso.com\.contoso.com。  
  
在非拆分式 DNS 环境中，Internet 命名空间不同于 Intranet 命名空间。 例如，Contoso 公司在 Internet 上使用 contoso.com，在 Intranet 上使用 corp.contoso.com。 因为所有 Intranet 资源都使用 corp.contoso.com DNS 后缀，所以 corp.contoso.com 的 NRPT 规则会将针对所有 Intranet 资源的 DNS 名称查询都路由到 Intranet DNS 服务器。 带有 contoso.com 后缀的名称的 DNS 查询与 NRPT 中的 corp.contoso.com intranet 命名空间规则不匹配并向其发送到 Internet DNS 服务器。 对于非拆分式 DNS 部署，由于 Intranet 和 Internet 资源的 FQDN 互不重复，因此无需对 NRPT 进行其他配置。 DirectAccess 客户端可以访问其组织的 Internet 和 intranet 资源。  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>规划 DirectAccess 客户端的本地名称解析行为  
如果名称不能解析通过 DNS，DNS 客户端服务在 Windows Server 2012、 Windows 8、 Windows Server 2008 R2 和 Windows 7 可以使用本地名称解析，使用链路本地多播名称解析 (LLMNR) 和 TCP/IP 协议，若要解决上的 NetBIOS 本地子网的名称。 当计算机位于专用网络（如单个子网家庭网络）上时，对等连接通常需要使用本地名称解析。  
  
当 DNS 客户端服务对 intranet 服务器名称执行本地名称解析和的计算机连接到 Internet 上的共享子网时，则恶意用户可以捕获 LLMNR 和 NetBIOS 通过 TCP/IP 消息来确定 intranet 服务器名称。 在基础结构服务器安装向导的 DNS 页上，可以配置基于从 intranet DNS 服务器收到的响应类型的本地名称解析行为。 你可使用以下选项：  
  
-   **如果在 DNS 中不存在该名称，请使用本地名称解析**:由于 DirectAccess 客户端仅对 Intranet DNS 服务器无法解析的服务器名称执行本地名称解析，因此此选项的安全性最高。 如果可以访问 Intranet DNS 服务器，则将解析 Intranet 服务器的名称。 如果无法访问 Intranet DNS 服务器，或者存在其他类型的 DNS 错误，则不会通过本地名称解析将 Intranet 服务器名称泄漏到子网中。  
  
-   **如果 DNS 中不存在名称或 DNS 服务器都无法访问，当客户端计算机位于专用网络 （推荐） 时，请使用本地名称解析**:因为仅当无法访问 Intranet DNS 服务器时，此选项才允许在专用网络上使用本地名称解析，因此推荐使用此选项。  
  
-   **使用任何类型的 DNS 解析错误 （最不安全） 的本地名称解析**:由于 Intranet 网络服务器的名称可能通过本地名称解析泄漏到本地子网，因此此选项的安全性最低。  
  
#### <a name="plan-the-network-location-server-configuration"></a>规划网络位置服务器配置  
网络位置服务器是一个用于检测 DirectAccess 客户端是否位于企业网络中的网站。 企业网络中的客户端不会使用 DirectAccess 访问内部资源;但它们而是直接连接。  
  
可以托管网络位置服务器网站，远程访问服务器上或在你的组织中的另一台服务器上。 如果托管网络位置服务器远程访问服务器上的，自动创建网站时部署远程访问。 如果托管网络位置服务器上运行 Windows 操作系统的另一台服务器，必须确保 Internet 信息服务 (IIS) 在该服务器上安装和创建网站。 远程访问不配置网络位置服务器上的设置。  
  
请确保网络位置服务器网站符合以下要求：  
  
-   具有 HTTPS 服务器证书。  
  
-   对内部网络的计算机的高可用性。  
  
-   不能访问 Internet 上的 DirectAccess 客户端计算机。  
  
-  
  
当您设置了你的网络位置服务器网站时，此外，考虑以下要求的客户端：  
  
-   DirectAccess 客户端计算机必须信任将服务器证书颁发给网络位置服务器网站的 CA。  
  
-   内部网络上的 DirectAccess 客户端计算机必须能够解析网络位置服务器网站的名称。  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>规划用于网络位置服务器证书  
当获取要用于网络位置服务器网站证书时，请考虑以下方面：  
  
-   在  “使用者”字段中，指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。  
  
-   有关**增强型密钥用法**字段中，使用服务器身份验证 OID。  
  
-   必须根据证书吊销列表 (CRL) 检查网络位置服务器证书。 有关**CRL 分发点**字段中，使用连接到 intranet 的 DirectAccess 客户端可访问的 CRL 分发点。 不应从内部网络之外访问此 CRL 分发点。  
  
##### <a name="plan-dns-for-the-network-location-server"></a>规划网络位置服务器的 DNS  
DirectAccess 客户端尝试访问网络位置服务器，以确定它们是否位于内部网络上。 内部网络上的客户端必须能够解析该网络位置服务器的名称，但当它们位于 Internet 上时，必须阻止它们解析该名称。 为了确保这一点，默认情况下，网络位置服务器的 FQDN 将作为免除规则添加到 NRPT。  
  
### <a name="plan-management-servers-configuration"></a>规划管理服务器配置  
DirectAccess 客户端启动与提供服务，例如 Windows 更新和防病毒更新的管理服务器通信。 DirectAccess 客户端还使用 Kerberos 协议进行身份验证到域控制器，才能访问内部网络。 在 DirectAccess 客户端的远程管理期间，管理服务器与客户端计算机进行通信以执行管理功能，例如，软件或硬件清单评估。 远程访问可以自动发现某些管理服务器，包括：  
  
-   域控制器：包含客户端计算机的域和中与远程访问服务器位于同一林中所有域执行自动发现域控制器。  
  
-   System Center Configuration Manager 服务器  
  
配置域控制器和 System Center Configuration Manager 服务器将自动检测第一次 DirectAccess。 检测到的域控制器不会显示在控制台中，但可以使用 Windows PowerShell cmdlet 检索设置。 如果修改了域控制器或 System Center Configuration Manager 服务器，则单击**更新管理服务器**在控制台中刷新管理服务器列表。  
  
**管理服务器要求**  
  
-   管理服务器必须通过基础结构隧道进行访问。 在你配置远程访问时，将服务器添加到管理服务器列表将自动使它们可以通过此隧道进行访问。  
  
-   通过本机 IPv6 地址或使用由 ISATAP 分配的地址，启动到 DirectAccess 客户端的连接的管理服务器必须完全支持 IPv6。  
  
### <a name="plan-active-directory-requirements"></a>规划 Active Directory 要求  
远程访问使用 Active Directory，如下所示：  
  
-   **身份验证**:基础结构隧道将 NTLMv2 身份验证用于连接到远程访问服务器的计算机帐户和该帐户必须位于 Active Directory 域中。 Intranet 隧道使用 Kerberos 身份验证的用户创建 intranet 隧道。  
  
-   **组策略对象**:远程访问将配置设置收集到组策略对象 (Gpo) 应用于远程访问服务器、 客户端，以及内部应用程序服务器。  
  
-   **安全组**：远程访问使用安全组收集和标识 DirectAccess 客户端计算机。 Gpo 将应用到所需的安全组。  
  
当你计划用于远程访问部署的 Active Directory 环境时，请考虑以下要求：  
  
-   Windows Server 2012、 Windows Server 2008 R2 Windows Server 2008 或 Windows Server 2003 操作系统上安装至少一个域控制器。  
  
    如果域控制器是外围网络上 （并因此可从远程访问服务器的面向 Internet 的网络适配器），阻止远程访问服务器访问它。 您需要以阻止连接到 Internet 适配器的 IP 地址的域控制器上添加数据包筛选器。  
  
-   远程访问客户端必须使用 V.90 调制解调器。  
  
-   DirectAccess 客户端必须是域成员。 客户端可以属于：  
  
    -   与远程访问服务器位于同一林中的任何域。  
  
    -   与远程访问服务器域具有双向信任的任何域。  
  
    -   在具有双向信任的林的远程访问服务器域的林中任何域。  
  
> [!NOTE]  
> -   远程访问服务器不可以是域控制器。  
> -   不能从远程访问服务器 （适配器不可位于 Windows 防火墙的域配置文件中） 的外部 Internet 适配器访问用于远程访问的 Active Directory 域控制器。  
  
#### <a name="plan-client-authentication"></a>规划客户端身份验证  
在 Windows Server 2012 中的远程访问，可以使用内置 Kerberos 身份验证，使用用户名和密码，或使用证书进行 IPsec 计算机身份验证之间进行选择。  
  
**Kerberos 身份验证**:当你选择使用 Active Directory 凭据进行身份验证时，DirectAccess 对于计算机，将首先使用 Kerberos 身份验证，然后它使用 Kerberos 身份验证的用户。 使用此模式的身份验证时，DirectAccess 将使用单独的安全隧道的内部网络上提供的 DNS 服务器、 域控制器和任何其他服务器访问权限  
  
**IPsec 身份验证**:在您选择使用双因素身份验证或网络访问保护，DirectAccess 将使用两条安全隧道。 远程访问设置向导在具有高级安全性的 Windows 防火墙中配置连接安全规则。 协商 IPsec 安全远程访问服务器时，这些规则指定以下凭据：  
  
-   基础结构隧道使用计算机证书凭据进行第一身份验证和第二个身份验证的用户 (NTLMv2) 凭据。 用户凭据强制使用已验证 Internet 协议 (AuthIP)，并提供对 DNS 服务器和域控制器的访问之前 DirectAccess 客户端可为 intranet 隧道使用 Kerberos 凭据。  
  
-   Intranet 隧道使用计算机证书凭据进行第一身份验证和第二个身份验证的用户 (Kerberos V5) 凭据。  
  
#### <a name="plan-multiple-domains"></a>规划多个域  
管理服务器列表应包括来自所有域的域控制器，这些域所包含的安全组中具有 DirectAccess 客户端计算机。 它应包含所有域都包含可能使用配置为 DirectAccess 客户端的计算机的用户帐户。 这可确保通过用户域中的域控制器，可以对未与用户使用的客户端计算机位于同一域中的用户进行身份验证。  
  
此身份验证是如果在同一林中的域是自动的。 如果客户端计算机或在不同的林中的应用程序服务器的安全组，不会自动检测这些林的域控制器。 也不会自动检测林。 你可以运行任务**更新管理服务器**中**远程访问管理**来检测这些域控制器。  
  
在可能的情况，常见域名后缀应将添加到 NRPT 在远程访问部署过程。 例如，如果你有两个域（domain1.corp.contoso.com 和 domain2.corp.contoso.com），你可以添加一个常见的 DNS 后缀条目（其中域名后缀是 corp.contoso.com），而不是将两个条目都添加到 NRPT 中。 发生这种情况自动对在同一根域。 必须手动添加不在同一根的域。  
  
### <a name="plan-group-policy-object-creation"></a>计划组策略对象创建  
在配置远程访问时，DirectAccess 设置将收集到的组策略对象 (Gpo)。 使用 DirectAccess 设置填充两个 Gpo，并将它们分发，如下所示：  
  
-   **DirectAccess 客户端 GPO**:此 GPO 包含客户端设置，包括 IPv6 转换技术设置、 NRPT 条目和连接安全规则的高级安全 Windows 防火墙。 此 GPO 将应用于为客户端计算机指定的安全组。  
  
-   **DirectAccess 服务器 GPO**:此 GPO 包含应用于在部署中配置为远程访问服务器的任何服务器的 DirectAccess 配置设置。 它还包含高级安全 Windows 防火墙连接安全规则。  
  
> [!NOTE]  
> 在远程管理 DirectAccess 客户端不支持的应用程序服务器配置，因为客户端无法访问的应用程序服务器所在的 DirectAccess 服务器的内部网络。 在远程访问设置配置屏幕中的步骤 4 不适用于此类型的配置。  
  
你可以自动或手动配置的 Gpo。  
  
**自动**:当您指定自动创建 Gpo 时，为每个 GPO 指定默认名称。  
  
**手动**:你可以使用已由 Active Directory 管理员预定义的 Gpo。  
  
在配置您的 Gpo 时，请考虑以下警告：  
  
-   将 DirectAccess 配置为使用特定的 GPO 后，无法将它配置为使用不同的 GPO。  
  
-   使用以下过程运行 DirectAccess cmdlet 之前备份所有远程访问组策略对象：  
  
    [备份和还原远程访问配置](https://go.microsoft.com/fwlink/?LinkID=257928)。  
  
-   无论你使用自动或手动配置的 Gpo，您需要添加用于慢速链接检测的策略，如果你的客户端将使用 3g。 有关路径**策略：配置组策略慢速链接检测**是：  
  
    “计算机配置”/“策略”/“管理模板”/“系统”/“组策略”  。  
  
-   如果不存在用于链接 Gpo 的正确权限，则发出警告。 远程访问操作将继续，但不是会发生链接。 如果发出此警告，则将不会自动创建链接，即使更高版本添加了权限。 相反，管理员必须手动创建链接。  
  
#### <a name="automatically-created-gpos"></a>自动创建的 Gpo  
请考虑以下时使用自动创建的 Gpo:  
  
应用自动创建的 GPO 将根据位置和链接目标，如下所示：  
  
-   对于 DirectAccess 服务器 GPO，位置和链接目标指向包含远程访问服务器的域。  
  
-   在创建后客户端和应用程序服务器 Gpo，位置设置为单一域。 每个域中查找 GPO 名称，如果它存在，则使用 DirectAccess 设置填充域。  
  
-   将链接目标设置为在其中创建了 GPO 的域的根。 为每个包含客户端计算机或应用程序服务器的域创建一个 GPO，并将该 GPO 链接到其各自域的根。  
  
当使用自动创建的 Gpo 应用 DirectAccess 设置时，远程访问服务器管理员将需要以下权限：  
  
-   对于每个域中创建 Gpo 的权限。  
  
-   若要链接到所有选定的客户端域根的权限。  
  
-   若要链接到服务器 GPO 域根的权限。  
  
-   安全权限才能创建、 编辑、 删除和修改 Gpo。  
  
-   GPO 读取权限的每个所需的域。 此权限不是必需的但它建议，因为它使远程访问可以验证正在创建 Gpo 时，不存在具有相同名称的 Gpo。  
  
#### <a name="manually-created-gpos"></a>手动创建的 Gpo  
使用手动创建的 GPO 时，请考虑以下内容：  
  
-   在运行远程访问安装向导之前，这些 GPO 应存在。  
  
-   若要应用 DirectAccess 设置，远程访问服务器管理员需要完整的安全权限才能创建、 编辑、 删除和修改手动创建的 Gpo。  
  
-   对整个域中 GPO 的链接进行搜索。 如果未在域中链接 GPO，将在域的根中自动创建链接。 如果未提供创建链接所需的权限，则会发出警告。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>从已删除的 GPO 中恢复  
如果在远程访问服务器、 客户端或应用程序服务器 GPO 已意外删除，将显示以下错误消息：**找不到 GPO （组策略对象名称）** 。  
  
如果有可用的备份，则可以从备份中还原 GPO。 如果不没有可用的任何备份，则必须删除配置设置并再次对其进行配置。  
  
###### <a name="to-remove-configuration-settings"></a>若要删除的配置设置  
  
1.  运行 Windows PowerShell cmdlet **Uninstall-remoteaccess**。  
  
2.  打开**远程访问管理**。  
  
3.  你将看到关于未找到 GPO 的错误消息。 单击“删除配置设置”  。 完成后，服务器将还原到未配置状态，并可以重新配置设置。  
  


