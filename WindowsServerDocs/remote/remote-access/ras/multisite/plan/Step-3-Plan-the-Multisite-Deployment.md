---
title: 步骤 3 计划多站点部署
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29d52e57a18bf956d135179b503255efd256b35e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446851"
---
# <a name="step-3-plan-the-multisite-deployment"></a>步骤 3 计划多站点部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在计划的多站点基础结构之后, 计划的任何其他证书要求，客户端计算机如何选择入口点，并在部署中分配的 IPv6 地址。  

以下部分提供详细的规划信息。
  
## <a name="bkmk_3_1_IPHTTPS"></a>3.1 规划 IP-HTTPS 证书  
在配置的入口点，需要通过特定的 ConnectTo 地址配置每个入口点。 每个入口点的 IP-HTTPS 证书必须与 ConnectTo 地址匹配。 获取证书时，请注意以下事项：  
  
-   在多站点部署中你不能使用自签名证书。  
  
-   建议使用公共 CA，以便可以随时使用 CRL。  
  
-   在主题字段中，指定远程访问服务器 （如果 ConnectTo 地址已被指定为 IP 地址而不是 DNS 名称），外部适配器的 IPv4 地址或 IP-HTTPS URL 的 FQDN。  
  
-   证书的公用名应与 IP-HTTPS 网站的名称匹配。 此外支持使用通配符 URL ConnectTo DNS 名称相匹配。  
  
-   IP-HTTPS 证书可以使用通配符使用者名称中。 相同的通配符证书可用于所有入口点。  
  
-   对于“增强型密钥使用”字段，请使用服务器身份验证对象标识符 (OID)。  
  
-   如果要支持在多站点部署中，运行 Windows 7 的客户端计算机中的 CRL 分发点字段中，指定连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。 这不是必需的 （默认情况下对 IP-HTTPS 这些客户端上禁用 CRL 吊销检查） 中运行 Windows 8 的客户端。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   IP-HTTPS 证书必须导入直接到的计算机，而不是用户的个人存储中。  
  
## <a name="bkmk_3_2_NLS"></a>3.2 规划网络位置服务器  
可以托管网络位置服务器网站，远程访问服务器上或在你的组织中的另一台服务器上。 如果托管网络位置服务器远程访问服务器上的，自动创建网站时部署远程访问。 如果托管在你的组织中运行 Windows 操作系统的另一台服务器上的网络位置服务器，必须确保 Internet 信息服务 (IIS) 安装以创建网站。  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 证书用于网络位置服务器的要求  
请确保网络位置服务器网站满足证书部署的以下要求：  
  
-   它需要 HTTPS 服务器证书。  
  
-   如果网络位置服务器位于远程访问服务器，并且你已选择使用自签名的证书部署单一远程访问服务器时，必须重新配置为使用内部 CA 颁发的证书的单一服务器部署。  
  
-   DirectAccess 客户端计算机必须信任将服务器证书颁发给网络位置服务器网站的 CA。  
  
-   内部网络上的 DirectAccess 客户端计算机必须能够解析网络位置服务器网站的名称。  
  
-   网络位置服务器网站必须是内部网络上的计算机对高度可用。  
  
-   网络位置服务器不可以由 Internet 上的 DirectAccess 客户端计算机访问。  
  
-   必须针对证书吊销列表 (CRL) 检查服务器证书。  
  
-   当远程访问服务器上托管网络位置服务器时，不支持通配符证书。  
  
获取要用于网络位置服务器网站证书，请注意以下事项：  
  
1.  在“使用者”字段中，请指定网络位置服务器的 Intranet 接口的 IP 地址，或网络位置 URL 的 FQDN。 请注意，是否网络位置服务器托管在远程访问服务器上，应指定 IP 地址。 这是因为网络位置服务器必须使用相同的使用者名称的所有入口点，并且不是所有入口点都具有相同的 IP 地址。  
  
2.  对于“增强型密钥使用”字段，请使用服务器身份验证 OID。  
  
3.  对于 CRL 分发点字段中，使用连接到 intranet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
### <a name="322dns-for-the-network-location-server"></a>用于网络位置服务器 3.2.2DNS  
如果托管网络位置服务器远程访问服务器上的，则必须在部署中添加每个入口点的网络位置服务器网站的 DNS 条目。 请注意以下事项：  
  
-   在多站点部署中的第一个网络位置服务器证书的使用者名称用作所有入口点的网络位置服务器 URL，因此使用者名称和网络位置服务器 URL 不能是相同的计算机名称在部署中的第一个远程访问服务器。 它必须是专用于网络位置服务器的 FQDN。  
  
-   个入口点使用 DNS，平衡的网络位置服务器流量提供的服务，因此应该同时存在具有相同 URL 的每个入口点，使用的入口点的内部 IP 地址配置 DNS 条目。  
  
-   所有入口点必须都配置网络位置服务器证书具有相同的使用者名称 （相匹配的网络位置服务器 URL）。  
  
-   必须添加入口点之前创建的网络位置服务器基础结构 （DNS 和证书设置） 的入口点。  
  
## <a name="bkmk_3_3_IPsec"></a>3.3 计划的所有远程访问服务器的 IPsec 根证书  
规划用于 IPsec 多站点部署中的客户端身份验证时，请注意以下：  
  
1.  如果你已选择使用内置 Kerberos 代理进行计算机身份验证设置单个远程访问服务器时，必须更改设置，以使用 CA 颁发的内部，由于多站点不支持 Kerberos 代理的计算机证书部署。  
  
2.  如果使用自签名的证书，必须重新配置为使用内部 CA 颁发的证书的单一服务器部署。  
  
3.  若要在客户端身份验证过程成功执行 IPsec 身份验证，所有远程访问服务器必须具有颁发的 IPsec 根或中间 CA 和客户端身份验证 OID 的增强型密钥使用的证书。  
  
4.  所有的多站点部署中的远程访问服务器上，必须安装相同的 IPsec 根或中间证书。  
  
## <a name="bkmk_3_4_GSLB"></a>3.4 规划全局服务器负载平衡  
在多站点部署中，可以额外配置全局服务器负载均衡器。 如果你的部署涵盖大型的地理位置分布，因为它可以分配入口点之间的流量负载，全局服务器负载均衡器可以是适用于你的组织。  全局服务器负载均衡器可以配置为 DirectAccess 客户端提供的最接近的入口点的入口点信息。 过程的工作，如下所示：  
  
1.  运行 Windows 10 或 Windows 8 客户端计算机有全局服务器负载均衡器 IP 地址的列表，每个入口点相关联。  
  
2.  Windows 10 或 Windows 8 客户端计算机尝试在公共 DNS 中的全局服务器负载均衡器 FQDN 解析为 IP 地址。 如果已解析的 IP 地址被列为全局服务器负载均衡器 IP 地址的入口点，客户端计算机将自动选择该入口点，并将连接到其的 IP-HTTPS URL （ConnectTo 地址），或它的 Teredo 服务器 IP 地址。 请注意，全局服务器负载均衡器的 IP 地址不需要为 ConnectTo 地址或入口点，Teredo 服务器地址相同，因为客户端计算机永远不会尝试连接到全局服务器负载均衡器 IP 地址。  
  
3.  如果客户端计算机位于 web 代理 （并且不能使用 DNS 解析），或者如果全局服务器负载均衡器 FQDN 未解析为任何配置全局服务器负载均衡器 IP 地址，则将自动使用 HTTPS 探测器到选择的入口点所有入口点的 IP-HTTPS Url。 客户端将连接到服务器首先响应。  
  
全局服务器负载平衡支持远程访问的设备的列表，请转到查找合作伙伴页面[Microsoft 服务器和云平台](https://www.microsoft.com/server-cloud/)。  
  
## <a name="bkmk_3_5_EP_Selection"></a>3.5 规划 DirectAccess 客户端入口点选择  
默认情况下，Windows 10 配置的多站点部署中，并在 Windows 8 客户端计算机上配置了信息所需连接到部署中的所有入口点并将自动连接到基于所选内容上的单一入口点算法。 此外可以配置部署，以允许 Windows 10 和 Windows 8 客户端计算机，以手动选择该条目指向它们将连接。 如果启用了自动入口点选择，如果美国入口点变得无法访问 Windows 10 或 Windows 8 的客户端计算机当前连接到美国入口点，在几分钟后客户端计算机将尝试连接通过欧洲入口点。 建议使用自动入口点选择;但是，允许手动入口点选择使最终用户能够连接到基于当前网络条件的不同入口点。 例如，如果一台计算机连接到美国入口点和到内部网络连接变得比预期慢得多。 在此情况下，最终用户可以手动选择要连接到欧洲的入口点，以提高到内部网络的连接。  
  
> [!NOTE]  
> 最终用户手动选择的入口点后，客户端计算机将恢复为自动入口点的选择。 也就是说，如果手动选择的入口点变得无法访问，最终用户必须恢复为自动入口点的选择，或手动选择另一个入口点。  
  
 Windows 7 客户端计算机上配置了连接到多站点部署中的单一入口点所需的信息。 它们不能同时存储多个入口点的信息。 例如，Windows 7 客户端计算机可以配置为连接到美国入口点，但不是欧洲入口点。 如果无法访问美国入口点时，Windows 7 客户端计算机将无法连接到内部网络，直到的入口点是可访问。 最终用户不能进行任何更改，以尝试连接到欧洲的入口点。  
  
## <a name="bkmk_3_6_IPv6"></a>3.6 计划前缀和路由  
  
### <a name="internal-ipv6-prefix"></a>内部 IPv6 前缀  
在单个远程访问部署过程中 server 计划的内部网络 IPv6 前缀，请注意多站点部署中的以下：  
  
1.  如果您配置了单一服务器的远程访问部署时包括了所有 Active Directory 站点，将已在远程访问管理控制台中定义的内部网络 IPv6 前缀。  
  
2.  如果创建多站点部署的其他 Active Directory 站点，必须规划对更多站点的新 IPv6 前缀和远程访问中定义它们。 请注意，仅可以使用远程访问管理控制台或 PowerShell cmdlet，如果在企业内部网络中部署 IPv6 配置 IPv6 前缀。  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>DirectAccess 客户端计算机 （的 IP-HTTPS 前缀） 的 IPv6 前缀  
  
1.  如果在内部公司网络中部署 IPv6，必须计划要将分配给你的部署中的任何其他入口点中的 DirectAccess 客户端计算机的 IPv6 前缀。  
  
2.  请确保要分配给每个入口点中的 DirectAccess 客户端计算机的 IPv6 前缀不同，并且不存在中的 IPv6 前缀重叠。  
  
3.  如果未在企业网络中部署 IPv6，添加的入口点时，将自动选择每个入口点的 IP-HTTPS 前缀。  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>VPN 客户端的 IPv6 前缀  
如果在单个远程访问服务器上部署 VPN，请注意以下各项：  
  
1.  添加 IPv6 VPN 前缀的入口点是只需要你想要允许 IPv6 连接到公司网络的 VPN 客户端。  
  
2.  只能使用远程访问管理控制台或 PowerShell cmdlet，如果在内部公司网络中，部署 IPv6 和入口点上启用 VPN 的入口点上配置 VPN 前缀。  
  
3.  VPN 前缀中每个入口点应唯一，并且不应与其他 VPN 或 IP-HTTPS 前缀重叠。  
  
4.  如果未在企业网络中部署 IPv6，连接到的入口点的 VPN 客户端将不会分配 IPv6 地址。  
  
### <a name="routing"></a>路由  
在多站点部署中对称路由将强制使用 Teredo 和 IP-HTTPS。 在企业网络中部署 IPv6，请注意以下：  
  
1. 在企业网络到其关联的远程访问服务器，必须可路由的每个入口点的 Teredo 和的 IP-HTTPS 前缀。  
  
2. 必须在企业网络路由基础结构中配置路由。  
  
3. 每个入口点应该有一到三个路由的内部网络中：  
  
   1. IP-HTTPS 前缀此前缀选择由管理员在添加入口点向导。  
  
   2. VPN IPv6 前缀 （可选）。 为入口点启用 VPN 后，可以选择此前缀  
  
   3. Teredo 前缀 （可选）。 此前缀是只使用两个连续公用 IPv4 地址的外部适配器上配置远程访问服务器时才适用。 前缀基于的地址对的第一个公共 IPv4 地址。 例如，如果外部地址：  
  
      1. www.xxx.yyy.zzz  
  
      2. www.xxx.yyy.zzz+1  
  
      若要配置的 Teredo 前缀为 2001:0:WWXX:YYZZ:: / 64，其中 WWXX:YYZZ 是 IPv4 地址 www.xxx.yyy.zzz 的十六进制表示形式。  
  
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
  
   4. 所有上述路由必须路由到内部适配器的远程访问服务器上的 IPv6 地址 （或到的内部虚拟 IP (VIP) 地址在负载平衡入口点）。  
  
> [!NOTE]  
> IPv6 中的企业网络和远程访问服务器管理的部署时通过 DirectAccess，Teredo 的路由远程执行，并且必须将添加到每台远程访问服务器的 IP-HTTPS 前缀的所有其他入口点，以便将流量转发到内部网络。  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory 的特定于站点 IPv6 前缀  
当运行 Windows 10 或 Windows 8 的客户端计算机连接到的入口点时，客户端计算机将立即与入口点，在 Active Directory 站点关联并且配置有与入口点相关的 IPv6 前缀。 首选方法是为客户端计算机连接到使用这些 IPv6 前缀，因为它们动态的 IPv6 前缀策略表中使用配置较高的优先级时连接到的入口点的资源。  
  
如果你的组织具有特定于站点 IPv6 前缀 （例如内部资源 FQDN app.corp.com 承载于北美和欧洲具有特定于站点的 IP 地址，每个位置中） 使用的 Active Directory 拓扑结构，这不是配置的每个入口点未配置默认使用的远程访问控制台中和特定于站点 IPv6 前缀。 如果你确实想要启用此可选方案，需要具有特定客户端计算机连接到特定入口点应首选的 IPv6 前缀配置每个入口点。 执行此操作，如下所示：  
  
1.  对于 Windows 10 或 Windows 8 客户端计算机使用的每个 GPO，请运行集 DAEntryPointTableItem PowerShell cmdlet  
  
2.  带有特定于站点 IPv6 前缀的 cmdlet 的 EntryPointRange 参数设置。 例如，若要添加特定于站点前缀 2001:db8:1:1:: / 64 和 2001:db:1:2:: / 64 到名为欧洲的入口点运行以下命令  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  在修改 EntryPointRange 参数，请确保不要删除属于 IPsec 隧道终结点和 DNS64 地址的现有 128 位前缀。  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3.7 计划转换为 IPv6 部署多站点远程访问时  
许多组织在企业网络上使用 IPv4 协议。 使用可用的 IPv4 前缀耗尽，许多组织将从仅使用 IPv4 过渡到仅限 IPv6 的网络。  
  
此转换是最有可能发生在两个阶段：  
  
1.  从仅使用 IPv4 到 IPv6 + IPv4 公司网络。  
  
2.  从 IPv6 + IPv4 到仅支持 IPv6 的企业网络。  
  
在每个部分中，可能会分阶段执行转换。 在每个阶段中只有一个子网的网络可能会更改为新的网络配置。 因此，DirectAccess 多站点部署时需要支持混合部署位置，例如，一些入口点属于仅使用 IPv4 的子网和其他人属于 IPv6 + IPv4 子网。 此外，在转换进程期间的配置更改必须不会中断通过 DirectAccess 客户端连接。  
  
### <a name="TransitionIPv4toMixed"></a>从仅使用 IPv4 过渡到 IPv6 + IPv4 公司网络  
当将 IPv6 地址添加到仅使用 IPv4 的公司网络，你可以将 IPv6 地址添加到已部署的 DirectAccess 服务器。 此外，你可能想要添加到负载平衡群集使用 IPv4 和 IPv6 地址到 DirectAccess 部署的入口点或节点。  
  
远程访问允许你将使用 IPv4 和 IPv6 地址的服务器添加到最初的配置仅使用 IPv4 地址使用的部署。 这些服务器添加为仅使用 IPv4 的服务器和 DirectAccess; 将忽略其 IPv6 地址因此，你的组织不能充分利用本机 IPv6 连接的优势，这些新服务器上。  
  
若要转换到 IPv6 + IPv4 的部署的部署并充分利用本机 IPv6 功能，必须重新安装 DirectAccess。 若要维护整个在重新安装客户端连接请参阅转换从仅使用 IPv4 到仅支持 IPv6 的部署使用双 DirectAccess 部署。  
  
> [!NOTE]  
> 与仅使用 IPv4 的网络，在混合 IPv4 + IPv6 网络中，如 DNS64 部署在远程访问服务器本身，而不是公司 DNS，必须配置用于解析的 DNS 客户端请求的 DNS 服务器的地址。  
  
### <a name="TransitionMixedtoIPv6"></a>从 IPv6 + IPv4 过渡到仅支持 IPv6 的企业网络  
DirectAccess 可以添加仅支持 IPv6 的入口点仅当部署中的第一个远程访问服务器最初有两个 IPv4 和 IPv6 地址或 IPv6 地址。 也就是说，不能从转换仅使用 IPv4 的网络到单个步骤中仅支持 IPv6 的网络而无需重新安装 DirectAccess。 若要直接从仅使用 IPv4 的网络转换到仅支持 IPv6 的网络，请参阅转换从仅使用 IPv4 使用双 DirectAccess 部署的仅支持 IPv6 的部署。  
  
完成从仅使用 IPv4 的部署过渡到 IPv6 + IPv4 的部署后，你可以过渡到仅支持 IPv6 的网络。 期间和转换完成后，请注意以下事项：  
  
-   如果任何仅使用 IPv4 的后端服务器保留在公司网络，它们不会向客户端连接通过仅支持 IPv6 的入口点的可访问。  
  
-   当将仅支持 IPv6 的入口点添加到 IPv4 + IPv6 部署中，DNS64 和 NAT64 将不启用新的服务器上。 连接到这些入口点的客户端将自动配置为使用公司的 DNS 服务器。  
  
-   如果需要从已部署的服务器中删除 IPv4 地址，必须从 DirectAccess 部署中删除服务器，删除其 IPv4 公司网络地址，并再次将其添加到部署。  
  
若要支持客户端连接到公司网络，必须确保网络位置服务器可通过公司 DNS 中为其 IPv6 地址来解决。 附加的 IPv4 地址可以设置，但不是必需的。  
  
### <a name="DualDeployment"></a>从仅使用 IPv4 过渡到使用双 DirectAccess 部署的仅支持 IPv6 的部署  
从仅使用 IPv4 过渡到仅支持 IPv6 的企业网络不能完成而无需重新安装 DirectAccess 部署。 若要在转换期间维护客户端连接，可以使用另一个 DirectAccess 部署。 当第一个转换阶段完成 （升级到 IPv4 + IPv6 仅使用 IPv4 的网络），并且你想要准备的本机 IPv6 连接优势将来转换为仅支持 IPv6 的企业网络或充分利用双部署需要。 双部署所述的以下常规步骤：  
  
1.  安装第二个的 DirectAccess 部署。 可以在新服务器上安装 DirectAccess 或从第一个部署中删除服务器并将其用于第二个部署。  
  
    > [!NOTE]  
    > 在安装时以及当前的一个其他 DirectAccess 部署，请确保没有两个入口点共享相同的客户端前缀。  
    >   
    > 如果安装 DirectAccess 使用开始向导或 cmdlet `Install-RemoteAccess`，远程访问将自动设置为默认值为部署中的第一个入口点的客户端前缀 < IPv6 子网\_前缀 >: 1000年:: /64。 如果需要则必须更改前缀。  
  
2.  从第一个部署中删除所选客户端安全组。  
  
3.  将客户端安全组添加到第二个部署。  
  
    > [!IMPORTANT]  
    > 为了保持在整个过程中的客户端连接，必须将安全组添加到第二个部署后立即删除它们从第一个中。 这可确保客户端将不会使用两个或零个 DirectAccess Gpo 进行更新。 客户端将开始使用第二个部署后它们检索和更新其客户端 GPO。  
  
4.  可选：从第一个部署中删除的 DirectAccess 入口点并在第二个中将这些服务器添加为新入口点。  
  
完成转换后，你可以卸载第一次的 DirectAccess 部署。 卸载时，可能会发生以下问题：  
  
-   如果部署被配置为支持移动的计算机上只有客户端，将删除 WMI 筛选器。 如果第二个部署的客户端安全组包括台式计算机，DirectAccess 客户端 GPO 不会筛选台式计算机，并对它们可能会导致问题。 如果需要移动计算机筛选器，请按照的说明重新创建它[gpo 创建 WMI 筛选器](https://technet.microsoft.com/library/cc947846.aspx)。  
  
-   如果在同一 Active Directory 域上最初创建这两个部署，DNS 将探测的项，用于指向 localhost 将被删除，并且可能会导致客户端连接问题。 例如，客户端可能会使用 IP HTTPS 而不是 Teredo 进行连接或 DirectAccess 多站点入口点之间进行切换。 在这种情况下，必须将以下 DNS 条目添加到企业 DNS:  
  
    -   区域： 域名称  
  
    -   名称： directaccess corpConnectivityHost  
  
    -   IP 地址::: 1  
  
    -   键入：AAAA  
  
  
  


