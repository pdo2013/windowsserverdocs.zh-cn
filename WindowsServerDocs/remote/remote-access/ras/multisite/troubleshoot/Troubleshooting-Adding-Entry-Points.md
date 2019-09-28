---
title: 添加入口点疑难解答
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7e93c972dbbe2971796c12cdeea27474723a80ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404468"
---
# <a name="troubleshooting-adding-entry-points"></a>添加入口点疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍与 `Add-DAEntryPoint` 命令相关的问题疑难解答信息。 要确认你收到的错误是否与添加入口点有关，请检查 Windows 事件日志的事件 ID 10067。  
  
## <a name="missing-remoteaccessserver-parameter"></a>缺少 RemoteAccessServer 参数  
**接收到错误**。 必须提供参数 RemoteAccessServer 的值。  
  
**原因**  
  
当向多站点部署中添加新的入口点时，你必须指定 *RemoteAccessServer* 参数，该参数是你要添加为新入口点的服务器的名称。  
  
**解决方案**  
  
运行该命令并确保为 *RemoteAccessServer* 参数指定要添加作为入口点的服务器的名称。  
  
## <a name="remote-access-is-not-configured"></a>未配置远程访问  
**接收到错误**。 < Server_name > 上未配置远程访问。 请指定属于多站点部署的服务器的名称。  
  
**原因**  
  
参数 *ComputerName* 指定的计算机或运行该命令的计算机上未配置远程访问。  
  
向多站点部署添加新的入口点时，必须指定两个参数：*ComputerName*和*RemoteAccessServer*。 参数 *ComputerName* 是多站点部署中的一台服务器的名称，参数 *RemoteAccessServer* 是你要添加为新入口点的服务器的名称。 如果你运行的计算机是多站点部署中的一部分，则不需要 ComputerName 参数。  
  
**解决方案**  
  
运行该命令并确保为 *ComputerName* 参数指定已经配置为多站点部署的一部分的服务器的名称，或从多站点部署的一台计算机运行该命令。  
  
## <a name="multisite-not-enabled"></a>未启用多站点  
**接收到错误**。 必须先启用多站点部署，然后才能执行此操作。 请使用 `Enable-DAMultiSite` cmdlet 完成此操作。  
  
**原因**  
  
参数 *ComputerName* 指定的服务器未启用多站点。 要向远程访问部署添加新的入口点，你必须先启用多站点。  
  
**解决方案**  
  
使用 `Enable-DaMultiSite` cmdlet 启用多站点。 有关详细信息，请参阅[部署多站点远程访问](https://technet.microsoft.com/library/hh831664.aspx)。  
  
## <a name="ipv6-prefix-issues"></a>IPv6 前缀问题  
  
-   **问题1**  
  
    **接收到错误**。 IPv6 已部署在内部网络中，但未指定客户端 IPv6 前缀。  
  
    **原因**  
  
    IPv6 已部署在企业网络中，但需要 IP-HTTPS 前缀。 不过，*ClientIPv6Prefix* 参数中没有为新入口点指定前缀。  
  
    **解决方案**  
  
    1.  为新入口点分配一个唯一的 IP-HTTPS 前缀，并确保向此前缀下的 IP 地址发送的数据包将被传送到你正在添加的服务器。  
  
    2.  运行 `Add-DAEntryPoint` cmdlet 并指定 *ClientIPv6Prefix* 参数中的 IP-HTTPS 前缀。  
  
-   **问题2**  
  
    **接收到错误**。 其他入口点已在使用客户端 IPv6 前缀。 请指定其他值。  
  
    **原因**  
  
    在 *ClientIPv6Prefix* 参数中指定的 IP-HTTPS 前缀已被其他入口点使用  
  
    **解决方案**  
  
    1.  为新入口点分配一个唯一的 IP-HTTPS 前缀，并确保向此前缀下的 IP 地址发送的数据包将被传送到你正在添加的服务器。  
  
    2.  运行 `Add-DAEntryPoint` cmdlet 并指定 *ClientIPv6Prefix* 参数中的 IP-HTTPS 前缀。  
  
## <a name="connectto-address"></a>ConnectTo 地址  
**接收到错误**。 远程访问服务器上 DirectAccess 客户端连接到的地址（< connect_to_address >）与网络位置服务器地址相同。 请指定其他值。  
  
**原因**  
  
ConnectTo 地址与网络位置服务器地址相同。  
  
**解决方案**  
  
ConnectTo 地址应当可以通过 Internet 解析，以允许客户端计算机通过 IP-HTTPS 实现连接。 网络位置服务器地址应当可以通过企业网络解析，但不可以通过 Internet 解析。 确保网络位置服务器地址不同于 ConnectTo 地址。 选择其他地址并重试。  
  
## <a name="directaccess-or-vpn-already-installed"></a>DirectAccess 或 VPN 已安装  
**接收到错误**。 在服务器 < server_name > 上检测到 VPN 安装。 请指定其他未安装远程访问的服务器或删除该服务器的 VPN 配置。  
  
或  
  
远程访问已安装在服务器 < server_name > 上。 请指定其他未运行 DirectAccess 的服务器或删除该服务器现有的 DirectAccess 配置。  
  
**原因**  
  
新入口点上已配置了 DirectAccess 或 VPN。 你无法向多站点部署中添加配置的入口点。  
  
**解决方案**  
  
要向多站点部署添加服务器，你必须在该服务器上安装远程访问角色，但不应配置 DirectAccess 和 VPN。  
  
运行该命令并确保你在 *RemoteAccessServer* 参数中指定的服务器未配置 DirectAccess 或 VPN。  
  
## <a name="ipsec-root-certificate"></a>IPsec 根证书  
**接收到错误**。 在服务器 < server_name > 上找不到配置的 IPsec 根证书。  
  
**原因**  
  
无法在你要加入部署的服务器上找到根证书或颁发计算机证书的中间证书颁发机构 (CA)。  
  
**解决方案**  
  
在远程访问管理控制台的 **“步骤 2 远程访访问服务器”** 中，单击 **“编辑”** ，然后在 **“身份验证”** 页的 **“使用计算机证书”** 中确保所选的证书是有效的。 如果证书有效，请确保其位于你要添加的服务器的受信根 CA 下并重试一次。  
  
> [!NOTE]  
> 证书必须相同，并有着同样的指纹。  
  
如果证书无效，请选择所有远程访问服务器上配置的有效受信根 CA。  
  
## <a name="mixing-ipv6-and-ipv4-entry-points"></a>同时混有 IPv6 和 IPv4 入口点  
首次安装 DirectAccess 时，会检查内部网络适配器，以确定网络是仅包含 IPv4 地址（仅限 IPv4 网络）、同时包含 IPv6 和 IPv4 地址还是仅包含 IPv6 地址（仅限 IPv6 网络）。 该信息用于确定部署类型（仅限 IPv4、IPv6+IPv4 或仅限 IPv6）。  
  
-   **问题1**  
  
    **收到警告**。 要添加的远程访问服务器配置了 IPv4 和 IPv6 地址。 这是仅限 IPv4 的部署，远程访问将忽略 IPv6 地址。  
  
    **原因**  
  
    首次安装此部署时，内部网络被检测为 IPv4 网络。 在多站点部署中，假定不同入口点位于具有不同特征的不同子网中。 因此，虽然部署配置为仅限 IPv4 的部署，它仍包含位于 IPv6+IPv4 子网中的入口点。 但是，尽管入口点将添加到部署，但 DirectAccess 将忽略在新入口点的内部接口上配置的 IPv6 地址。  
  
    **解决方案**  
  
    如果整个内部网络配置为包含 IPv6 和 IPv4 地址，可考虑转换为 IPv6+IPv4 部署，以利用 IPv6 技术。 请参阅 [Step 3 中的 "从纯 IPv4 转换为 IPv6 + IPv4 公司网络"：计划多站点部署 @ no__t。  
  
-   **问题2**  
  
    **接收到错误**。 此多站点部署中的远程访问服务器的内部网络适配器配置了 IPv4 地址。 你要添加的入口点的内部网络适配器上也必须配置为 IPv4 地址。  
  
    **原因**  
  
    首次安装此部署时，内部网络被检测为 IPv4 网络。 远程访问发现你正试图添加的入口点的内部网络配置为 IPv6 地址。 这在 IPv4 部署中是不允许的。  
  
    **解决方案**  
  
    如果整个网络已配置为 IPv6 地址，你应当将其转换为 IPv6+IPv4 或 IPv6 部署。 请参阅 "计划在部署多站点远程访问时转换为 IPv6"。  
  
-   **问题3**  
  
    **接收到错误**。 此入口点位于 IPv4 网络中，但以前的入口点位于 IPv6 网络中。 请在将此入口点添加到同一多站点部署前，将其接入 IPv6 网络。  
  
    **原因**  
  
    首次安装此部署时，检测到内部网络为 IPv6+IPv4 或 IPv6。 检测到你正在尝试添加的新入口点上的内部网络配置为 IPv4 地址。 这在 IPv6+IPv4 或 IPv6 部署中是不允许的。  
  
    **解决方案**  
  
    将新入口点配置为 IPv6 地址，然后将其添加到多站点部署中。  
  
-   **问题4**  
  
    **收到警告**。 远程访问服务器上的内部网络适配器未配置 IPv4 地址。 将不会在此服务器上配置 DNS64 和 NAT64。 DirectAccess 客户端只能访问 IPv6 内部服务器。  
  
    **原因**  
  
    首次安装此部署时，检测到内部网络为 IPv6+IPv4。 在此部署模式中，DNS64 和 NAT64 被启用，以允许客户端计算机访问配置为 IPv4 地址的内部网络上的计算机。  
  
    当添加新入口点时，远程访问检测到新计算机上的内部接口只有 IPv6 地址。 要配置 DNS64 和 NAT64，需要 IPv4 地址才能将数据包从远程访问服务器发送到仅使用 IPv4 的计算机。 由于新计算机中不存在此类 IP，将不会在远程访问服务器上配置 NAT64 和 DNS64。 因此，利用此入口点通过 DirectAccess 访问企业网络的客户端计算机将无法访问内部网络上仅使用 IPv4 的服务器。 有关如何转换为 IPv6 + IPv4 网络或仅限 IPv6 的网络的信息，请参阅 "在部署多站点远程访问时计划转换为 IPv6"。  
  
    **解决方案**  
  
    向新的远程访问服务器添加 IPv4 地址，以确保 DNS64 和 NAT64 正确运行。  
  
## <a name="domain-issues-with-the-servergponame"></a>ServerGpoName 的域问题  
  
-   **问题1**  
  
    **接收到错误**。 在 ServerGpoName 参数中指定的域 < server_GPO > 不存在。 改为指定域 < domain_name >。  
  
    **原因**  
  
    未找到管理员发送的服务器 GPO 名称的域名部分。  
  
    **解决方案**  
  
    确保你正确键入了域名。 如果域名拼写错误，使用完全限定的域名 (FQDN) 重试一次。  
  
-   **问题2**  
  
    **接收到错误**。 服务器 GPO 必须位于远程访问服务器域中。 在 ServerGpoName 参数中指定域 < domain_name >。  
  
    **原因**  
  
    服务器 GPO 的域与远程访问服务器所属的域不同。  
  
    **解决方案**  
  
    服务器 GPO 应当位于远程访问服务器所属的域中。 针对服务器 GPO 采用服务器的域名并重试一次。  
  
## <a name="split-brain-dns"></a>拆分式 DNS  
**收到警告**。 DNS 后缀 < DNS_suffix > 的 NRPT 条目包含客户端计算机连接到远程访问服务器时所使用的公共名称。 在 NRPT 中将名称 < connect_to_address > 添加为免除。  
  
**原因**  
  
你正在使用拆分式 DNS。 要允许客户端使用 IP-HTTPS 进行连接，你应当确保所选的 ConnectTo 地址是 NRPT 规则中的免除条目。  
  
**解决方案**  
  
如果你使用多站点部署，请确保不同入口点的所有连接地址均是 NRPT 规则的免除条目。  
  
要免除 NRPT 规则中的地址，请执行以下操作：  
  
1.  在远程访问管理控制台的 **“步骤 3 基础结构服务器”** 中，单击 **“编辑”** 。  
  
2.  在 **“基础结构服务器设置”** 向导的 **“DNS”** 页面上，双击表格，输入新的名称后缀。  
  
3.  在 **“DNS 服务器地址”** 对话框中的 DNS 后缀中，输入入口点的 ConnectTo 地址，然后单击 **“应用”** 。  
  
如果你在未指定服务器地址的情况下添加名称后缀，该后缀便会被视为 NRPT 免除条目。  
  
## <a name="saving-server-gpo-settings"></a>保存服务器 GPO 设置  
**接收到错误**。 将远程访问设置保存到 GPO < GPO_name > 时出错。  
  
若要解决此错误，请参阅在[启用多站点疑难解答](https://technet.microsoft.com/library/jj591658.aspx)中保存服务器 GPO 设置。  
  
## <a name="gpo-updates-cannot-be-applied"></a>无法应用 GPO 更新  
**收到警告**。 无法在 < server_name > 上应用 GPO 更新。 进行下次策略刷新后改动才能生效。  
  
**原因**  
  
尝试在指定的计算机上刷新策略时出错。 因此，进行下次策略刷新后改动才能生效。  
  
**解决方案**  
  
要强制执行策略刷新，请在指定的计算机上运行 `gpupdate /force`。  
  


