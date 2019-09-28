---
title: 步骤3规划多站点部署
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1320a5c8b8c267f270dae43e764533d9289006a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404455"
---
# <a name="step-3-plan-the-multisite-deployment"></a>步骤3规划多站点部署

>适用于：Windows Server（半年频道）、Windows Server 2016

规划多站点基础结构后，计划任何其他证书要求、客户端计算机如何选择入口点，以及在部署中分配的 IPv6 地址。  

以下各节提供了详细的规划信息。
  
## <a name="bkmk_3_1_IPHTTPS"></a>3.1 计划 IP-HTTPS 证书  
配置入口点时，会使用特定的 ConnectTo 地址配置每个入口点。 每个入口点的 ip-https 证书必须与 ConnectTo 地址匹配。 获取证书时，请注意以下事项：  
  
-   在多站点部署中你不能使用自签名证书。  
  
-   建议使用公共 CA，以便可以随时使用 CRL。  
  
-   在 "使用者" 字段中，指定远程访问服务器的外部适配器的 IPv4 地址（如果已将 ConnectTo 地址指定为 IP 地址而不是 DNS 名称）或 IP-HTTPS URL 的 FQDN。  
  
-   证书的公用名应与 ip-https 网站的名称相匹配。 还支持使用与 ConnectTo DNS 名称匹配的通配符 URL。  
  
-   Ip-https 证书可以在使用者名称中使用通配符。 相同的通配符证书可用于所有入口点。  
  
-   对于“增强型密钥使用”字段，请使用服务器身份验证对象标识符 (OID)。  
  
-   如果你在多站点部署中支持运行 Windows 7 的客户端计算机，请在 "CRL 分发点" 字段中指定连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。 对于运行 Windows 8 的客户端，这并不是必需的（默认情况下，在这些客户端上对 ip-https 禁用 CRL 吊销检查）。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   Ip-https 证书必须直接导入到计算机的个人存储区中，而不是用户。  
  
## <a name="bkmk_3_2_NLS"></a>3.2 规划网络位置服务器  
网络位置服务器网站可以托管在远程访问服务器上，也可以托管在组织中的另一个服务器上。 如果将网络位置服务器托管在远程访问服务器上，则部署远程访问时将自动创建该网站。 如果将网络位置服务器托管在组织中另一台运行 Windows 操作系统的服务器上，则必须确保安装 Internet Information Services （IIS）以创建网站。  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>网络位置服务器的3.2.1 证书要求  
请确保网络位置服务器网站满足以下证书部署要求：  
  
-   它需要 HTTPS 服务器证书。  
  
-   如果网络位置服务器位于远程访问服务器上，而你在部署单个远程访问服务器时选择使用自签名证书，则必须重新配置单一服务器部署，才能使用内部 CA 颁发的证书。  
  
-   DirectAccess 客户端计算机必须信任将服务器证书颁发给网络位置服务器网站的 CA。  
  
-   内部网络上的 DirectAccess 客户端计算机必须能够解析网络位置服务器网站的名称。  
  
-   对于内部网络上的计算机，网络位置服务器网站必须高度可用。  
  
-   网络位置服务器不可以由 Internet 上的 DirectAccess 客户端计算机访问。  
  
-   必须对照证书吊销列表（CRL）检查服务器证书。  
  
-   如果网络位置服务器托管在远程访问服务器上，则不支持通配符证书。  
  
获取用于网络位置服务器的网站证书时，请注意以下事项：  
  
1.  在“使用者”字段中，请指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。 请注意，如果网络位置服务器托管在远程访问服务器上，则不应指定 IP 地址。 这是因为网络位置服务器必须对所有入口点使用相同的使用者名称，并且并非所有入口点都具有相同的 IP 地址。  
  
2.  对于“增强型密钥使用”字段，请使用服务器身份验证 OID。  
  
3.  对于 "CRL 分发点" 字段，请使用连接到 intranet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
### <a name="322dns-for-the-network-location-server"></a>网络位置服务器的 3.2.2 DNS  
如果将网络位置服务器托管在远程访问服务器上，则必须为部署中的每个入口点添加网络位置服务器网站的 DNS 条目。 请注意以下事项：  
  
-   多站点部署中第一个网络位置服务器证书的使用者名称将用作所有入口点的网络位置服务器 URL，因此使用者名称和网络位置服务器 URL 不能与部署中的第一台远程访问服务器。 它必须是专用于网络位置服务器的 FQDN。  
  
-   网络位置服务器流量提供的服务在使用 DNS 的入口点之间均衡，因此，每个入口点都应有一个具有相同 URL 的 DNS 条目，并使用入口点的内部 IP 地址进行配置。  
  
-   必须使用与网络位置服务器 URL 匹配的使用者名称的网络位置服务器证书配置所有入口点。  
  
-   在添加入口点之前，必须先创建入口点的网络位置服务器基础结构（DNS 和证书设置）。  
  
## <a name="bkmk_3_3_IPsec"></a>3.3 为所有远程访问服务器规划 IPsec 根证书  
在多站点部署中规划 IPsec 客户端身份验证时，请注意以下事项：  
  
1.  如果在设置单个远程访问服务器时选择使用内置 Kerberos 代理进行计算机身份验证，则必须将此设置更改为使用内部 CA 颁发的计算机证书，因为多站点不支持 Kerberos 代理部署.  
  
2.  如果你使用的是自签名证书，则必须重新配置单一服务器部署，才能使用内部 CA 颁发的证书。  
  
3.  若要在客户端身份验证过程中成功进行 IPsec 身份验证，所有远程访问服务器必须具有由 IPsec 根 CA 或中间 CA 颁发的证书，以及用于增强型密钥用法的客户端身份验证 OID。  
  
4.  必须在多站点部署中的所有远程访问服务器上安装相同的 IPsec 根证书或中间证书。  
  
## <a name="bkmk_3_4_GSLB"></a>3.4 计划全局服务器负载平衡  
在多站点部署中，你还可以配置全局服务器负载均衡器。 如果你的部署涉及到大地理分布，全局服务器负载均衡器对你的组织很有用，因为它可以在入口点之间分配流量负载。  全局服务器负载均衡器可配置为为 DirectAccess 客户端提供最近入口点的入口点信息。 此过程的工作原理如下所示：  
  
1.  运行 Windows 10 或 Windows 8 的客户端计算机具有全局服务器负载均衡器 IP 地址的列表，每个地址都与一个入口点相关联。  
  
2.  Windows 10 或 Windows 8 客户端计算机尝试将公共 DNS 中的全局服务器负载均衡器的 FQDN 解析为 IP 地址。 如果已解析的 IP 地址列为入口点的全局服务器负载均衡器 IP 地址，则客户端计算机会自动选择该入口点并连接到其 IP-HTTPS URL （ConnectTo 地址）或其 Teredo 服务器 IP 地址。 请注意，全局服务器负载均衡器的 IP 地址不需要与入口点的 ConnectTo 地址或 Teredo 服务器地址相同，因为客户端计算机从不尝试连接到全局服务器负载均衡器 IP 地址。  
  
3.  如果客户端计算机位于 web 代理后面（并且无法使用 DNS 解析），或者如果全局服务器负载平衡器 FQDN 未解析为任何已配置的全局服务器负载均衡器 IP 地址，则将使用 HTTPS 探测自动选择一个入口点所有入口点的 ip-https Url。 客户端将连接到首先响应的服务器。  
  
有关支持远程访问的全局服务器负载平衡设备的列表，请访问[Microsoft 服务器和云平台](https://www.microsoft.com/server-cloud/)上的查找合作伙伴页。  
  
## <a name="bkmk_3_5_EP_Selection"></a>3.5 计划 DirectAccess 客户端入口点选择  
配置多站点部署时，默认情况下，为 Windows 10 和 Windows 8 客户端计算机配置连接到部署中的所有入口点所需的信息，并根据选择自动连接到单个入口点算法. 你还可以配置你的部署以允许 Windows 10 和 Windows 8 客户端计算机手动选择它们将连接到的入口点。 如果 Windows 10 或 Windows 8 客户端计算机当前已连接到美国入口点并启用了自动入口点选择美国，则在几分钟后，客户端计算机将尝试连接通过欧洲入口点。 建议使用自动入口点选择;但是，允许手动输入点选择允许最终用户根据当前网络条件连接到不同的入口点。 例如，如果计算机连接到美国入口点，并且与内部网络的连接比预期慢得多。 在这种情况下，最终用户可以手动选择连接到欧洲入口点，以改善与内部网络的连接。  
  
> [!NOTE]  
> 最终用户手动选择入口点后，客户端计算机将不会恢复为自动入口点选择。 也就是说，如果手动选择的入口点变为不可访问，则最终用户必须恢复到自动入口点选择，或手动选择另一个入口点。  
  
 为 Windows 7 客户端计算机配置连接到多站点部署中的单个入口点所需的信息。 它们不能同时存储多个入口点的信息。 例如，可以将 Windows 7 客户端计算机配置为连接到美国入口点，但不能连接到欧洲入口点。 如果美国入口点不可访问，则 Windows 7 客户端计算机将失去与内部网络的连接，直到入口点可访问。 最终用户无法进行任何更改来尝试连接到欧洲入口点。  
  
## <a name="bkmk_3_6_IPv6"></a>3.6 计划前缀和路由  
  
### <a name="internal-ipv6-prefix"></a>内部 IPv6 前缀  
在部署单个远程访问服务器的过程中，你计划了内部网络 IPv6 前缀，请注意多站点部署中的以下内容：  
  
1.  如果在配置单一服务器远程访问部署时包括了所有 Active Directory 站点，则会在远程访问管理控制台中定义内部网络 IPv6 前缀。  
  
2.  如果为多站点部署创建其他 Active Directory 站点，则必须为其他站点规划新的 IPv6 前缀，并将其定义为远程访问。 请注意，如果在内部公司网络中部署了 IPv6，则只能使用远程访问管理控制台或 PowerShell cmdlet 来配置 IPv6 前缀。  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>DirectAccess 客户端计算机的 IPv6 前缀（ip-https 前缀）  
  
1.  如果 IPv6 部署在内部公司网络中，则必须计划在部署的任何其他入口点中分配给 DirectAccess 客户端计算机的 IPv6 前缀。  
  
2.  确保分配给每个入口点中 DirectAccess 客户端计算机的 IPv6 前缀都是不同的，并且 IPv6 前缀中没有重叠。  
  
3.  如果在企业网络中未部署 IPv6，则在添加入口点时，将自动选择每个入口点的 ip-https 前缀。  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>VPN 客户端的 IPv6 前缀  
如果在单个远程访问服务器上部署了 VPN，请注意以下事项：  
  
1.  仅当你想要允许与企业网络的 VPN 客户端 IPv6 连接时，才需要将 IPv6 VPN 前缀添加到入口点。  
  
2.  仅当在内部公司网络中部署了 IPv6，并且在入口点上启用了 VPN 的情况下，才可以使用远程访问管理控制台或 PowerShell cmdlet 在入口点上配置 VPN 前缀。  
  
3.  VPN 前缀在每个入口点中应该是唯一的，不应与其他 VPN 或 ip-https 前缀重叠。  
  
4.  如果在企业网络中未部署 IPv6，则不会为连接到入口点的 VPN 客户端分配 IPv6 地址。  
  
### <a name="routing"></a>路由  
在多站点部署中，使用 Teredo 和 ip-https 强制实施对称路由。 在公司网络中部署 IPv6 时，请注意以下事项：  
  
1. 每个入口点的 Teredo 和 ip-https 前缀必须可跨企业网络路由到其关联的远程访问服务器。  
  
2. 必须在企业网络路由基础结构中配置路由。  
  
3. 对于每个入口点，内部网络中应有一到三个路由：  
  
   1. Ip-https 前缀-管理员在 "添加入口点" 向导中选择此前缀。  
  
   2. VPN IPv6 前缀（可选）。 为入口点启用 VPN 后，可以选择此前缀  
  
   3. Teredo 前缀（可选）。 仅当在外部适配器上使用两个连续的公用 IPv4 地址配置远程访问服务器时，此前缀才适用。 前缀基于地址对的第一个公用 IPv4 地址。 例如，如果外部地址为：  
  
      1. www\.xxx.yyy.zzz  
  
      2. www\.xxx.yyy.zzz + 1  
  
      然后，要配置的 Teredo 前缀为2001：0： WWXX： YYZZ：：/64，其中 WWXX： YYZZ 是 IPv4 地址 www\.xxx.yyy.zzz 的十六进制表示形式。  
  
      请注意，可以使用以下脚本来计算 Teredo 前缀：  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. 所有上述路由都必须路由到远程访问服务器的内部适配器上的 IPv6 地址（或负载平衡入口点的内部虚拟 IP （VIP）地址）。  
  
> [!NOTE]  
> 如果在企业网络中部署了 IPv6，并且通过 DirectAccess 远程执行远程访问服务器管理，则必须将所有其他入口点的 Teredo 和 ip-https 前缀的路由添加到每个远程访问服务器，以便将流量转发到内部网络。  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory 特定于站点的 IPv6 前缀  
当运行 Windows 10 或 Windows 8 的客户端计算机连接到入口点时，客户端计算机会立即与入口点的 Active Directory 站点相关联，并且配置了与入口点关联的 IPv6 前缀。 首选项是为了让客户端计算机使用这些 IPv6 前缀连接到资源，因为在连接到入口点时，它们的优先级较高。  
  
如果你的组织使用带有特定于站点的 IPv6 前缀的 Active Directory 拓扑（例如，内部资源 FQDN app.corp.com 同时在每个位置使用特定于站点的 IP 地址托管在北美和欧洲），则不会配置此项使用远程访问控制台时，默认情况下不会为每个入口点配置站点特定的 IPv6 前缀。 如果确实要启用此可选方案，则需要将每个入口点配置为连接到特定入口点的客户端计算机应首选的特定 IPv6 前缀。 按如下所示执行此操作:  
  
1.  对于 Windows 10 或 Windows 8 客户端计算机使用的每个 GPO，请运行 DAEntryPointTableItem PowerShell cmdlet  
  
2.  将 cmdlet 的 EntryPointRange 参数设置为站点特定的 IPv6 前缀。 例如，若要添加特定于站点的前缀2001： db8：1：1：：/64 和2001：数据库：1：2：：/64 到名为欧洲的入口点，请运行以下  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  修改 EntryPointRange 参数时，请确保未删除属于 IPsec 隧道终结点和 DNS64 地址的现有128位前缀。  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3.7 计划在部署多站点远程访问时转换为 IPv6  
许多组织使用企业网络上的 IPv4 协议。 由于有可用的 IPv4 前缀，许多组织正在进行从 IPv4 到仅 IPv6 网络的转换。  
  
这种转换最有可能分两个阶段进行：  
  
1.  从 IPv4 到 IPv6 + IPv4 公司网络。  
  
2.  从 IPv6 + IPv4 到仅使用 IPv6 的公司网络。  
  
在每个部分中，转换可以分阶段执行。 在每个阶段中，只有一个网络子网可以更改为新的网络配置。 因此，支持混合部署需要 DirectAccess 多站点部署，例如，某些入口点属于仅支持 IPv4 的子网，其他入口点属于 IPv6 + IPv4 子网。 此外，在转换过程中进行的配置更改不得中断通过 DirectAccess 的客户端连接。  
  
### <a name="TransitionIPv4toMixed"></a>从 IPv4 转换为 IPv6 + IPv4 企业网络  
将 IPv6 地址添加到仅 IPv4 公司网络时，可能需要将 IPv6 地址添加到已部署的 DirectAccess 服务器。 此外，你可能需要将一个入口点或一个节点添加到负载平衡群集，同时将 IPv4 和 IPv6 地址添加到 DirectAccess 部署。  
  
远程访问允许将包含 IPv4 和 IPv6 地址的服务器添加到最初配置了 IPv4 地址的部署。 这些服务器添加为仅 IPv4 服务器，并且 DirectAccess 忽略其 IPv6 地址;因此，你的组织无法利用这些新服务器上的本机 IPv6 连接权益。  
  
若要将部署转换为 IPv6 + IPv4 部署并利用本机 IPv6 功能，你必须重新安装 DirectAccess。 若要在整个重新安装过程中保持客户端连接，请参阅使用双重 DirectAccess 部署从仅 IPv4 转换为仅限 IPv6 的部署。  
  
> [!NOTE]  
> 与仅使用 IPv4 的网络一样，在混合的 IPv4 + IPv6 网络中，用于解析客户端 DNS 请求的 DNS 服务器的地址必须使用部署在远程访问服务器本身上的 DNS64 来配置，而不是使用企业 DNS 进行配置。  
  
### <a name="TransitionMixedtoIPv6"></a>从 IPv6 + IPv4 过渡到仅 IPv6 企业网络  
仅当部署中的第一个远程访问服务器最初同时具有 IPv4 和 IPv6 地址或仅有 IPv6 地址时，DirectAccess 才允许添加仅限 IPv6 的入口点。 也就是说，不能在无需重新安装 DirectAccess 的情况下，在单个步骤中将仅从 IPv4 网络转换为仅 IPv6 网络。 若要直接从仅 IPv4 网络直接转换到仅 IPv6 网络，请参阅使用双重 DirectAccess 部署从仅 IPv4 转换为仅限 IPv6 的部署。  
  
完成从仅 IPv4 部署到 IPv6 + IPv4 部署的转换后，可以转换到仅包含 IPv6 的网络。 在转换期间和转换后，请注意以下事项：  
  
-   如果任何仅 IPv4 的后端服务器保留在公司网络中，则这些客户端将无法访问通过仅使用 IPv6 的入口点连接的客户端。  
  
-   将仅限 IPv6 的入口点添加到 IPv4 + IPv6 部署时，不会在新服务器上启用 DNS64 和 NAT64。 连接到这些入口点的客户端将自动配置为使用企业 DNS 服务器。  
  
-   如果需要从已部署的服务器中删除 IPv4 地址，则必须从 DirectAccess 部署中删除该服务器，删除其 IPv4 公司网络地址，并再次将其添加到部署中。  
  
若要支持到企业网络的客户端连接，必须确保将网络位置服务器解析为其 IPv6 地址。 还可以设置其他 IPv4 地址，但这不是必需的。  
  
### <a name="DualDeployment"></a>使用双重 DirectAccess 部署从 IPv4 转换为仅限 IPv6 的部署  
无法在不重新安装 DirectAccess 部署的情况下从仅从 IPv4 到仅 IPv6 的公司网络进行转换。 若要在转换期间维护客户端连接，可以使用其他 DirectAccess 部署。 当第一个转换阶段完成（仅 IPv4 网络升级到 IPv4 + IPv6），并且你打算准备将来过渡到仅使用 IPv6 的公司网络时，需要进行双重部署，orto 利用本机 IPv6 连接权益。 以下常规步骤介绍了双重部署：  
  
1.  安装第二个 DirectAccess 部署。 你可以在新服务器上安装 DirectAccess，或从第一个部署中删除服务器，并将其用于第二个部署。  
  
    > [!NOTE]  
    > 在同时安装其他 DirectAccess 部署时，请确保没有两个入口点共享相同的客户端前缀。  
    >   
    > 如果使用入门向导或 cmdlet @no__t 安装 DirectAccess，远程访问会自动将部署中第一个入口点的客户端前缀设置为 < IPv6 子网 @ no__t-1prefix >：1000：：/64。 如果需要，必须更改前缀。  
  
2.  从第一个部署中删除所选的客户端安全组。  
  
3.  向第二个部署添加客户端安全组。  
  
    > [!IMPORTANT]  
    > 若要在整个过程中保持客户端连接，必须在从第一个部署中删除安全组后立即将它们添加到第二个部署中。 这可确保不会用两个或零个 DirectAccess Gpo 更新客户端。 客户端将在检索和更新其客户端 GPO 后开始使用第二个部署。  
  
4.  可选：从第一个部署中删除 DirectAccess 入口点，并在第二个部署中将这些服务器添加为新的入口点。  
  
完成转换后，可以卸载第一个 DirectAccess 部署。 卸载时，可能会出现以下问题：  
  
-   如果部署配置为仅支持移动计算机上的客户端，则将删除 WMI 筛选器。 如果第二个部署的客户端安全组包括台式计算机，DirectAccess 客户端 GPO 将不会筛选台式计算机，并且可能会导致这些计算机出现问题。 如果需要移动计算机筛选器，请按照[为 GPO 创建 WMI 筛选器](https://technet.microsoft.com/library/cc947846.aspx)上的说明重新创建它。  
  
-   如果两个部署最初都是在同一个 Active Directory 域上创建的，则指向 localhost 的 DNS 探测条目将被删除，并且可能会导致客户端连接问题。 例如，客户端可以使用 IP-HTTPS 而不是 Teredo 进行连接，或在 DirectAccess 多站点入口点之间切换。 在这种情况下，必须将以下 DNS 条目添加到企业 DNS 中：  
  
    -   区域：域名  
  
    -   名称： directaccess-Directaccess-corpconnectivityhost  
  
    -   IP 地址：：：1  
  
    -   键入：AAAA  
  
  
  


