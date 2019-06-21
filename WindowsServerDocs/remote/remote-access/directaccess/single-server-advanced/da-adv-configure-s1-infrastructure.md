---
title: 步骤 1 配置高级的 DirectAccess 基础结构
description: 本主题是指南部署单个 DirectAccess 服务器使用高级设置的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43abc30a-300d-4752-b845-10a6b9f32244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 42cec0d2e6ded443d24d787191bcb72a17a92306
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283533"
---
# <a name="step-1-configure-advanced-directaccess-infrastructure"></a>步骤 1 配置高级的 DirectAccess 基础结构

>适用于：Windows Server 2012 R2, Windows Server 2012

本主题介绍了如何在混合的 IPv4 和 IPv6 环境中配置高级远程访问部署所需的基础结构，该部署使用单个 DirectAccess 服务器。 开始执行部署步骤之前，请确保已完成中所述的规划步骤[规划高级 DirectAccess 部署](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md)。  
  
|任务|描述|  
|----|--------|  
|1.1 配置服务器网络设置|配置 DirectAccess 服务器上的服务器网络设置。|  
|1.2 配置强制隧道|配置强制隧道。|  
|1.3 在企业网络中配置路由|在企业网络中配置路由。|  
|1.4 配置防火墙|根据需要配置其他防火墙。|  
|1.5 配置 CA 和证书|如有必要，配置证书颁发机构 (CA)，以及部署中所需的任何其他证书模板。|  
|1.6 配置 DNS 服务器|配置 DirectAccess 服务器的域名系统 (DNS) 设置。|  
|1.7 配置 Active Directory|将客户端计算机和 DirectAccess 服务器加入到 Active Directory 域。|  
|1.8 配置 GPO|如果必要，为部署配置 GPO。|  
|1.9 配置安全组|配置将包含 DirectAccess 客户端计算机的安全组，以及部署中所需的任何其他安全组。|  
|1.10 配置网络位置服务器|配置网络位置服务器，包括安装网络位置服务器网站证书。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="ConfigNetworkSettings"></a>1.1 配置服务器网络设置  
在使用 IPv4 和 IPv6 的环境中，部署单个服务器需要下面的网络接口设置。 可使用  “Windows 网络和共享中心”中的  “更改适配器设置”配置所有 IP 地址。  
  
**边缘拓扑**  
  
-   两个面向 Internet 的连续公用静态 IPv4 或 IPv6 地址  
  
    > [!NOTE]  
    > Teredo 需要两个公用地址。 如果你不使用 Teredo，则可以配置单个公用静态 IPv4 地址。  
  
-   单个内部静态 IPv4 或 IPv6 地址  
  
**NAT 在设备后面 （具有两个网络适配器）**  
  
-   单个面向 Internet 的静态 IPv4 或 IPv6 地址  
  
-   单个面向内部网络的静态 IPv4 或 IPv6 地址  
  
**（使用一个网络适配器） 的 NAT 设备后面**  
  
-   单个面向内部网络的静态 IPv4 或 IPv6 地址  
  
> [!NOTE]  
> 如果使用单个网络适配器拓扑配置具有两个或多个网络适配器（一个在域配置文件中进行分类，另一个在公用或专用配置文件中进行分类）的 DirectAccess 服务器，我们的建议如下：  
>   
> -   确保第二个网络适配器和任何其他网络适配器在域配置文件中进行分类。  
> -   如果第二个网络适配器不能配置为域配置文件，将 DirectAccess IPsec 策略必须的范围手动扩展至所有配置文件配置 DirectAccess 后，使用以下 Windows PowerShell 命令：  
>   
>     ```  
>     $gposession = Open-NetGPO "PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule "DisplayName <Name of the IPsec policy> "GPOSession $gposession "Profile Any  
>     Save-NetGPO "GPOSession $gposession  
>     ```  
  
## <a name="BKMK_forcetunnel"></a>1.2 配置强制隧道  
可以通过远程访问设置向导配置强制隧道。 它显示为配置远程客户端向导中的一个复选框。 此设置仅影响 DirectAccess 客户端。 如果启用了 VPN，则 VPN 客户端将默认使用强制隧道。 管理员可通过客户端配置文件更改 VPN 客户端的设置。  
  
选中强制隧道对应的复选框将执行以下操作：  
  
-   启用 DirectAccess 客户端上的强制隧道  
  
-   在用于 DirectAccess 客户端的名称解析策略表 (NRPT) 中添加 **Any** 条目，这意味着所有的 DNS 通信都将转到内部网络 DNS 服务器  
  
-   将 DirectAccess 客户端配置为始终使用 IP-HTTPS 转换技术  
  
若要使 Internet 资源可供使用强制隧道的 DirectAccess 客户端使用，你可以使用代理服务器，它可以接收基于 IPv6 的 Internet 资源的请求并将其转换为对基于 IPv4 的 Internet 资源的请求。 若要为 Internet 资源配置代理服务器，你必须修改 NRPT 中的默认条目以添加该代理服务器。 可以通过使用远程访问 PowerShell cmdlet 或 DNS PowerShell cmdlet 来完成此操作。 例如，使用如下远程访问 PowerShell cmdlet：  
  
```  
Set-DAClientDNSConfiguration "DNSSuffix "." "ProxyServer <Name of the proxy server:port>  
```  
  
> [!NOTE]  
> 如果在同一服务器上启用了 DirectAccess 和 VPN，且 VPN 处于强制隧道模式下，并且在边缘拓扑或 NAT 后面的拓扑中部署该服务器（具有两个网络适配器，一个连接到域，一个连接到专用网络)，则无法通过 DirectAccess 服务器的外部接口转发 VPN Internet 通信。 若要实现这种方案，组织必须在单一网络适配器拓扑中防火墙后面的服务器上部署远程访问。 或者，组织可以在内部网络中使用单独的代理服务器转发来自 VPN 客户端的 Internet 通信。  
  
> [!NOTE]  
> 如果组织要使用 DirectAccess 客户端的 Web 代理访问 Internet 资源，并且企业代理不能够处理内部网络资源，则位于 Intranet 之外的 DirectAccess 客户端将无法访问内部资源。 在这种情况下，若要使 DirectAccess 客户端能够访问内部资源，请通过基础结构向导的 DNS 页手动为内部网络后缀创建 NRPT 条目。 请勿在这些 NRPT 后缀上应用代理设置。 应使用默认 DNS 服务器条目填充这些后缀。  
  
## <a name="ConfigRouting"></a>1.3 配置企业网络中的路由  
在企业网络中配置路由，如下所示：  
  
-   在组织中部署本机 IPv6 时，添加一个路由，以便内部网络上的路由器通过 DirectAccess 服务器将 IPv6 通信路由回来。  
  
-   手动配置的组织"s IPv4 和 IPv6 路由在 DirectAccess 服务器上。 添加已发布的路由，以便将所有具有组织 (/48) IPv6 前缀的通信都转发到内部网络。 对于 IPv4 通信，请添加显式路由，以便将 IPv4 通信转发到内部网络。  
  
## <a name="ConfigFirewalls"></a>1.4 配置防火墙  
在部署中使用其他防火墙的情况下，当 DirectAccess 服务器位于 IPv4 Internet 上时，为远程访问通信应用以下面向 Internet 的防火墙例外：  
  
-   Teredo 通信"用户数据报协议 (UDP) 目标端口 3544 入站，以及 UDP 源端口 3544 出站。  
  
-   6to4 通信"IP 协议 41 入站和出站。  
  
-   IP HTTPS"传输控制协议 (TCP) 目标端口 443，以及 TCP 源端口 443 出站。 当 DirectAccess 服务器配有单一网络适配器，且网络位置服务器位于 DirectAccess 服务器上时，还需要 TCP 端口 62000。  
  
    > [!NOTE]  
    > 必须在 DirectAccess 服务器上配置此例外，并且必须在边缘防火墙上配置所有其他例外。  
  
> [!NOTE]  
> 对于 Teredo 和 6to4 通信，这些例外应适用于 DirectAccess 服务器上两个面向 Internet 的连续公用 IPv4 地址。 对于 IP-HTTPS，仅需将例外情况应用于可解析服务器公用名称的地址。  
  
在使用其他防火墙的情况下，当 DirectAccess 服务器位于 IPv6 Internet 上时，为远程访问通信应用以下面向 Internet 的防火墙的例外：  
  
-   IP 协议 50  
  
-   UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
-   IPv6 的 Internet 控制消息协议 (ICMPv6) 流量入站和出站"适用于仅 Teredo 实施。  
  
当使用其它防火墙时，应用远程访问通信的以下内部网络防火墙例外情况：  
  
-   ISATAP"协议 41 入站和出站  
  
-   所有 IPv4/IPv6 通信的 TCP/UDP  
  
-   所有 IPv4/IPv6 通信的 ICMP  
  
## <a name="ConfigCAs"></a>1.5 配置 Ca 和证书  
Windows Server 2012 中的远程访问允许你使用证书进行计算机身份验证或使用的内置 Kerberos 代理服务器使用用户名和密码进行身份验证之间进行选择。 你还必须在 DirectAccess 服务器上配置 IP-HTTPS 证书。  
  
有关详细信息，请参阅 [Active Directory 证书服务](https://technet.microsoft.com/library/cc770357.aspx)。  
  
### <a name="151-configure-ipsec-authentication"></a>1.5.1 配置 IPsec 身份验证  
DirectAccess 服务器以及所有要使用 IPsec 身份验证的 DirectAccess 客户端上都需要计算机证书。 证书必须由内部证书颁发机构 (CA) 颁发，且 DirectAccess 服务器和 DirectAccess 客户端必须信任颁发根和中间证书的 CA 链。  
  
##### <a name="to-configure-ipsec-authentication"></a>配置 IPsec 身份验证  
  
1.  在内部 CA 中，确定你将使用**计算机**证书模板，或如果您将创建新的证书模板中所述[创建证书模板](https://technet.microsoft.com/library/cc731705.aspx)。  
  
    > [!NOTE]  
    > 如果你创建一个新的模板，则必须将其配置为可以进行客户端身份验证。  
  
2.  部署该证书模板（如有必要）。 有关详细信息，请参阅[部署证书模板](https://technet.microsoft.com/library/cc770794.aspx)。  
  
3.  如有必要，将该证书模板配置为自动注册。 有关详细信息，请参阅[配置证书自动注册](https://technet.microsoft.com/library/cc731522.aspx)。  
  
### <a name="ConfigCertTemp"></a>1.5.2 配置证书模板  
当你使用内部 CA 颁发证书时，必须为 IP-HTTPS 证书和网络位置服务器网站证书配置证书模板。  
  
##### <a name="to-configure-a-certificate-template"></a>配置证书模板的步骤  
  
1.  在内部 CA 中，根据[创建证书模板](https://technet.microsoft.com/library/cc731705.aspx)中所述创建一个证书模板。  
  
2.  根据 [部署证书模板](https://technet.microsoft.com/library/cc770794.aspx)中所述部署该证书模板。  
  
### <a name="153-configure-the-ip-https-certificate"></a>1.5.3 配置 IP-HTTPS 证书  
远程访问需要使用 IP-HTTPS 证书对到 DirectAccess 服务器的 IP-HTTPS 连接进行身份验证。 存在三个可用于 IP-HTTPS 身份验证的证书选择：  
  
**公共证书**  
  
由第三方提供公用证书。 如果证书使用者名称中不包含通配符，则它肯定是可在外部解析的完全限定的域名 (FQDN) URL，它仅用于 DirectAccess 服务器 IP-HTTPS 连接。  
  
**私有证书**  
  
在你使用专用证书时，以下内容是必需的（如果它们尚不存在）：  
  
-   用于进行 IP-HTTPS 身份验证的网站证书。 证书使用者应该是可从 Internet 访问的、可在外部解析的 FQDN。 1\.5.2 中的说明创建的证书模板上基于证书配置证书模板。  
  
-   能够从可公开解析的 FQDN 访问的证书吊销列表 (CRL) 分发点。  
  
**自签名的证书**  
  
使用自签名证书时，以下内容是必需的（如果它们尚不存在）：  
  
-   用于进行 IP-HTTPS 身份验证的网站证书。 证书使用者应该是可从 Internet 访问的、可在外部解析的 FQDN。  
  
-   能够从可公开解析的 FQDN 访问的 CRL 分发点。  
  
> [!NOTE]  
> 无法在多站点部署中使用自签名证书。  
  
请确保用于 IP-HTTPS 身份验证的网站证书符合以下要求：  
  
-   该证书的公用名应与 IP-HTTPS 站点的名称相匹配。  
  
-   在中**使用者**字段中，指定 IP-HTTPS URL 的 FQDN。  
  
-   对于  “增强型密钥使用”字段，请使用服务器身份验证对象标识符 (OID)。  
  
-   对于“CRL 分发点”  字段，请指定已连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   必须直接将 IP-HTTPS 证书导入到个人存储中。  
  
-   IP-HTTPS 证书可以在名称中包含通配符符。  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>安装内部 CA 颁发的 IP-HTTPS 证书  
  
1.  在 DirectAccess 服务器上：上**启动**屏幕上，键入**mmc.exe**，然后按 ENTER。  
  
2.  在 MMC 控制台中的“文件”  菜单上，单击“添加/删除管理单元”  。  
  
3.  在  “添加或删除管理单元”对话框中，依次单击  “证书”、  “添加”、  “计算机帐户”、  “下一步”、  “本地计算机”和  “完成”，然后单击  “确定”。  
  
4.  在证书管理单元的控制台树中，依次打开  “证书(本地计算机)\个人\证书”。  
  
5.  右键单击  “证书”，指向  “所有任务”，然后单击  “申请新证书”。  
  
6.  单击“下一步”  两次。  
  
7.  上**申请证书**页上，选中之前创建 （有关详细信息，请参阅 1.5.2 配置证书模板） 的证书模板的复选框。 如有必要，单击  “注册此证书需要详细信息”。  
  
8.  在  “证书属性”对话框的  “使用者”选项卡上，在  “使用者名称”区域的  “类型”中选择  “公用名”。  
  
9. 在  “值”中，指定 DirectAccess 服务器面向外部的适配器的 IPv4 地址，或者 IP-HTTPS URL 的 FQDN，然后单击  “添加”。  
  
10. 在  “备用名称”区域的  “类型”中，选择  “DNS”。  
  
11. 在  “值”中，指定 DirectAccess 服务器面向外部的适配器的 IPv4 地址，或者 IP-HTTPS URL 的 FQDN，然后单击  “添加”。  
  
12. 在  “常规”选项卡的  “友好名称”中，输入一个有助于标识证书的名称。  
  
13. 在  “扩展”选项卡上，单击“扩展密钥用法”  旁边的箭头，并确保  “服务器身份验证”出现在“选定的选项”  列表中。  
  
14. 依次单击  “确定”、  “注册”和  “完成”。  
  
15. 在证书管理单元的详细信息窗格中，通过“服务器身份验证的预期目的”验证是否注册了新证书。  
  
## <a name="ConfigDNS"></a>1.6 配置 DNS 服务器  
你必须为部署中的内部网络手动配置用于网络位置服务器网站的 DNS 条目。  
  
### <a name="NLS_DNS"></a>若要创建网络位置服务器  
  
1.  在内部网络 DNS 服务器上：上**启动**屏幕上，键入**dnsmgmt.msc**，然后按 ENTER。  
  
2.  在  “DNS 管理器”控制台的左窗格中，展开域的前向查找区域。 右键单击该域，然后单击  “新建主机(A 或 AAAA)”。  
  
3.  在  “新建主机”对话框的  “IP 地址”框中：  
  
    -   在  “名称(如果为空则使用父域名称)”框中，输入网络位置服务器网站的 DNS 名称（这是 DirectAccess 客户端用于连接到网络位置服务器的名称）。  
  
    -   输入网络位置服务器的 IPv4 或 IPv6 地址，然后依次单击  “添加主机”和  “确定”。  
  
4.  在  “新建主机”对话框中：  
  
    -   在“名称(如果为空则使用父域名称)”  框中，输入用于 Web 探测的 DNS 名称（用于默认 Web 探测的名称为 **directaccess-webprobehost**）。  
  
    -   在  “IP 地址框”中，输入 Web 探测的 IPv4 或 IPv6 地址，然后单击  “添加主机”。  
  
    -   为 **directaccess-corpconnectivityhost** 和任何手动创建的连接性验证程序重复此过程。  
  
5.  在  “DNS”对话框中，单击“确定”  ，然后单击“完成”  。  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
还必须为以下内容配置 DNS 条目：  
  
-   **IP-HTTPS 服务器**  
  
    DirectAccess 客户端必须能够解析来自 Internet 的 DirectAccess 服务器的 DNS 名称。  
  
-   **CRL 吊销检查**  
  
    DirectAccess 将对以下连接使用证书吊销检查：DirectAccess 客户端和 DirectAccess 服务器之间的 IP-HTTPS 连接，以及 DirectAccess 客户端和网络位置服务器之间基于 HTTPS 的连接。 在这两种情况下，DirectAccess 客户端都必须能够解析和访问 CRL 分发点位置。  
  
-   **ISATAP**  
  
    站点内自动隧道寻址协议 (ISATAP) 使用隧道以使 DirectAccess 客户端可以通过 IPv4 Internet 连接到 DirectAccess 服务器，方法是将 IPv6 数据包封装在 IPv4 标头中。 远程访问使用它提供通过 Intranet 到 ISATAP 主机的 IPv6 连接。 在非本机 IPv6 网络环境中，DirectAccess 服务器会自动将其自身配置为 ISATAP 路由器。 要求支持对 ISATAP 名称进行解析。  
  
## <a name="ConfigAD"></a>1.7 配置 Active Directory  
DirectAccess 服务器和所有 DirectAccess 客户端计算机都必须加入 Active Directory 域。 DirectAccess 客户端计算机必须是以下域类型之一的成员：  
  
-   与 DirectAccess 服务器位于同一林中的域。  
  
-   属于与 DirectAccess 服务器林具有双向信任关系的林的域。  
  
-   与 DirectAccess 服务器域具有双向域信任的域。  
  
#### <a name="to-join-the-directaccess-server-to-a-domain"></a>将 DirectAccess 服务器加入域  
  
1.  在服务器管理器中，单击 **“本地服务器”** 。 在详细信息窗格中，单击“计算机名”旁边的链接  。  
  
2.  在“系统属性”  对话框中，单击“计算机名”  选项卡，然后单击“更改”  。  
  
3.  如果在将服务器加入域时还要更改计算机名，请在“计算机名”中键入计算机的名称  。 在“隶属于”  下面单击“域”  ，键入服务器要加入到的域的名称（例如 corp.contoso.com），然后单击“确定”  。  
  
4.  当系统提示你输入用户名和密码时，请输入有权将计算机加入域的用户的用户名和密码，然后单击“确定”  。  
  
5.  在出现欢迎使用域的对话框时，单击  “确定”。  
  
6.  当系统提示你必须重新启动计算机时，请单击“确定”  。  
  
7.  在 **“系统属性”** 对话框中，单击 **“关闭”** 。  
  
8.  当系统提示你重新启动计算机时，请单击“立即重新启动”  。  
  
#### <a name="to-join-client-computers-to-the-domain"></a>将客户端计算机加入域  
  
1.  上**启动**屏幕上，键入**explorer.exe**，然后按 ENTER。  
  
2.  右键单击计算机图标，然后单击“属性”  。  
  
3.  在  “系统”页上，单击  “高级系统设置”。  
  
4.  在 **“系统属性”** 对话框中的 **“计算机名”** 选项卡上，单击 **“更改”** 。  
  
5.  如果在将服务器加入域时还要更改计算机名，请在“计算机名”中键入计算机的名称  。 在“隶属于”  下面单击“域”  ，键入服务器要加入到的域的名称（例如 corp.contoso.com），然后单击“确定”  。  
  
6.  当系统提示你输入用户名和密码时，请输入有权将计算机加入域的用户的用户名和密码，然后单击“确定”  。  
  
7.  在出现欢迎使用域的对话框时，单击  “确定”。  
  
8.  当系统提示你必须重新启动计算机时，请单击“确定”  。  
  
9. 在 **“系统属性”** 对话框中，单击 **“关闭”** 。  
  
10. 当系统提示你重新启动计算机时，请单击“立即重新启动”  。  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
> [!NOTE]  
> 在输入以下  “Add-computer”命令时，必须提供域凭据。  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>1.8 配置 Gpo  
部署远程访问需要至少两个组策略对象：  
  
-   一个包含用于 DirectAccess 服务器的设置  
  
-   一个包含用于 DirectAccess 客户端计算机的设置  
  
在配置远程访问时，该向导将自动创建所需的组策略对象。 但是，如果你的组织强制使用命名约定，你可以在远程访问管理控制台的 GPO 对话框中键入一个名称。 有关详细信息，请参阅 2.7。 配置摘要和备用 Gpo。 如果你已创建权限，则将创建 GPO。 如果你不具有创建 GPO 所需的权限，则必须在配置远程访问之前创建它们。  
  
若要创建组策略对象，请参阅[创建和编辑组策略对象](https://technet.microsoft.com/library/cc754740.aspx)。  
  
> [!IMPORTANT]  
> 管理员可以手动 DirectAccess 组策略对象链接到组织单位 (OU) 通过执行以下步骤：  
>   
> 1.  在配置 DirectAccess 之前，请将已创建的 GPO 链接到各自的 OU。  
> 2.  在你配置 DirectAccess 时，请为客户端计算机指定安全组。  
> 3.  远程访问管理员可能或可能不具有组策略对象链接到域的权限。 在任一情况下，都将自动配置组策略对象。 如果已将 GPO 链接到 OU，则将不会删除这些链接，并且不会将 GPO 链接到域。 对于服务器 GPO，OU 必须包含该服务器计算机对象，否则该 GPO 将链接到域的根。  
> 4.  如果你尚未链接到 OU，则配置完成后运行 DirectAccess 向导之前，域管理员可以将 DirectAccess 组策略对象链接到所需的 Ou。 可以删除指向域的链接。 有关详细信息，请参阅[链接组策略对象](https://technet.microsoft.com/library/cc732979.aspx)。  
  
> [!NOTE]  
> 如果手动创建组策略对象，则可能的组策略对象将不在可用在 DirectAccess 配置过程。 组策略对象可能不复制到最接近管理计算机的域控制器。 在这种情况下，管理员可以等待复制完成，或者强制进行复制。  
  
### <a name="181-configure-remote-access-gpos-with-limited-permissions"></a>1.8.1 配置具有有限权限的远程访问 GPO  
在使用过渡和生产 GPO 的部署中，域管理员应该执行以下操作：  
  
1.  从远程访问管理员处获取远程访问部署所需的 GPO 的列表。 有关详细信息，请参阅 1.8 规划组策略对象。  
  
2.  对于远程访问管理员所请求的每个 GPO，请使用不同的名称创建一对 GPO。 第一个 GPO 将用作过渡 GPO，第二个 GPO 将用作生产 GPO。  
  
    若要创建组策略对象，请参阅[创建和编辑组策略对象](https://technet.microsoft.com/library/cc754740.aspx)。  
  
3.  若要链接生产 GPO，请参阅[链接组策略对象](https://technet.microsoft.com/library/cc732979)。  
  
4.  授予远程访问管理员对所有过渡 GPO 的  “编辑设置、删除和修改安全性”权限。 有关详细信息，请参阅[为组或用户委派对组策略对象的权限](https://technet.microsoft.com/library/cc754542)。  
  
5.  拒绝所有域中的 Gpo 链接的远程访问管理员权限 (或验证远程访问管理员不是"t 具有此类权限)。 有关详细信息，请参阅[委派链接组策略对象的权限](https://technet.microsoft.com/library/cc755086)。  
  
当远程访问管理员配置远程访问时，他们应该始终仅指定过渡 GPO（非生产 GPO）。 在远程访问的初始配置中，以及在需要其他 GPO 的位置执行其他配置操作时（例如，在多站点部署中添加入口点，或在其他域中启用客户端计算机），该规则都适用。  
  
在远程访问管理员完成针对远程访问配置的任何更改后，域管理员应复查过渡 GPO 中的设置，并使用下列过程将这些设置复制到生产 GPO。  
  
> [!TIP]  
> 远程访问配置每次发生更改后，请执行下列过程。  
  
##### <a name="to-copy-settings-to-the-production-gpos"></a>将设置复制到生产 GPO  
  
1.  验证是否已将远程访问部署中的所有过渡 GPO 复制到域中的所有域控制器中。 为了确保已将最新的配置导入到生产 GPO 中，这一步骤是必需的。 有关详细信息，请参阅检查组策略基础结构状态。  
  
2.  通过备份远程访问部署中的所有过渡 GPO 来导出这些设置。 有关详细信息，请参阅重新启动组策略对象。  
  
3.  对于每个生产 GPO，更改安全筛选器以与相应的过渡 GPO 的安全筛选器相匹配。 有关详细信息，请参阅使用安全组筛选。  
  
    > [!NOTE]  
    > 因为  “导入设置”不会复制源 GPO 的安全筛选器，所以这一步骤是必需的。  
  
4.  对于每个生产 GPO，从相应过渡 GPO 的备份中导入设置，如下所示：  
  
    1.  在组策略管理控制台 (GPMC)，展开林和域包含的设置将导入到其中的生产组策略对象中的组策略对象节点。  
  
    2.  右键单击该 GPO，然后单击  “导入设置”。  
  
    3.  在  “导入设置向导”中的  “欢迎”页面上，单击“下一步”  。  
  
    4.  在上  “备份 GPO”页面上，单击  “备份”。  
  
    5.  在  “备份组策略对象”对话框的  “位置”框中，输入要存储 GPO 备份的位置所在的路径，或单击“浏览”  查找文件夹。  
  
    6.  在  “描述”框中，键入该生产 GPO 的描述，然后单击  “备份”。  
  
    7.  备份完成后，单击  “确定”，然后在  “备份 GPO”页面上，单击  “下一步”。  
  
    8.  在上  “备份位置”页面上的  “备份文件夹”框中，输入步骤 2 中相应过渡 GPO 备份所存储的位置的路径，或单击  “浏览”查找文件夹，然后单击  “下一步”。  
  
    9. 在  “源 GPO”页面上，选中  “仅显示每个 GPO 的最新版本”复选框以隐藏较早的备份，然后选择相应的过渡 GPO。 在将远程访问设置应用到生产 GPO 之前，单击  “视图设置”对其进行查看，然后单击  “下一步”。  
  
    10. 在“扫描备份”  页面上，单击“下一步”  ，然后单击“完成”  。  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
-   若要暂存的客户端 GPO"DirectAccess Client Settings-Staging"域"corp.contoso.com"中到备份文件夹"C:\Backups\":  
  
    ```  
    $backup = Backup-GPO "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "Path 'C:\Backups\'  
    ```  
  
-   若要查看过渡客户端 GPO"DirectAccess Client Settings-Staging"域"corp.contoso.com"中的安全筛选：  
  
    ```  
    Get-GPPermission "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "All | ?{ $_.Permission "eq 'GpoApply'}  
    ```  
  
-   若要将安全组"corp.contoso.com\DirectAccess clients"添加到生产客户端 GPO 的安全筛选器"DirectAccess 客户端设置"生产"域"corp.contoso.com"中：  
  
    ```  
    Set-GPPermission "Name 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com' "PermissionLevel GpoApply "TargetName 'corp.contoso.com\DirectAccess clients' "TargetType Group  
    ```  
  
-   若要从备份的设置导入到生产客户端 GPO"DirectAccess 客户端设置"生产"域"corp.contoso.com"中：  
  
    ```  
    Import-GPO "BackupId $backup.Id "Path $backup.BackupDirectory "TargetName 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com'  
    ```  
  
## <a name="ConfigSGs"></a>1.9 配置安全组  
客户端计算机的组策略对象中包含的 DirectAccess 设置仅应用于计算机是在配置远程访问时指定的安全组的成员。 此外，如果要使用安全组管理应用程序服务器，则为这些服务器创建安全组。  
  
### <a name="Sec_Group"></a>若要为 DirectAccess 客户端创建安全组  
  
1.  上**启动**屏幕上，键入**dsa.msc**，然后按 ENTER。 在  “Active Directory 用户和计算机”控制台的左窗格中，展开将包含安全组的域，右键单击  “用户”，指向  “新建”，然后单击  “组”。  
  
2.  在  “新建对象 – 组”对话框中的  “组名”下，输入该安全组的名称。  
  
3.  在  “组范围”下单击  “全局”，并在  “组类型”下单击“安全”  ，然后单击“确定”  。  
  
4.  双击 DirectAccess 客户端计算机安全组，然后在属性对话框中，单击  “成员”选项卡。  
  
5.  在“成员”  选项卡上，单击“添加”  。  
  
6.  在  “选择用户、联系人、计算机或服务帐户”对话框中，选择你希望为 DirectAccess 启用的客户端计算机，然后单击  “确定”。  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**Windows PowerShell 等效命令**  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="ConfigNLS"></a>1.10 配置网络位置服务器  
网络位置服务器应该是一个具有高可用性的服务器，它应具有 DirectAccess 客户端信任的有效 SSL 证书。 有两个网络位置服务器证书的证书选项：  
  
-   **私有证书**  
  
    此证书基于证书模板中的说明创建[1.5.2 配置证书模板](#ConfigCertTemp)。  
  
-   **自签名的证书**  
  
    > [!NOTE]  
    > 无法在多站点部署中使用自签名证书。  
  
任一类型的证书都需要以下内容（如果它们尚不存在）：  
  
-   用于网络位置服务器的网站证书。 证书使用者应该是网络位置服务器的 URL。  
  
-   在内部网络中高度可用的 CRL 分发点。  
  
> [!NOTE]  
> 如果网络位置服务器网站位于 DirectAccess 服务器上，则在你配置远程访问时，将自动创建一个网站。 此站点将绑定到你提供的服务器证书。  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>安装内部 CA 颁发的网络位置服务器证书  
  
1.  在将托管网络位置服务器网站的服务器上：上**启动**屏幕上，键入**mmc.exe**，然后按 ENTER。  
  
2.  在 MMC 控制台中的“文件”  菜单上，单击“添加/删除管理单元”  。  
  
3.  在  “添加或删除管理单元”对话框中，依次单击  “证书”、  “添加”、  “计算机帐户”、  “下一步”、  “本地计算机”和  “完成”，然后单击  “确定”。  
  
4.  在证书管理单元的控制台树中，依次打开  “证书(本地计算机)\个人\证书”。  
  
5.  右键单击  “证书”，指向  “所有任务”，然后单击  “申请新证书”。  
  
6.  单击“下一步”  两次。  
  
7.  上**申请证书**页上，选择个 1.5.2 中的说明创建的证书模板的复选框配置证书模板。 如有必要，单击  “注册此证书需要详细信息”。  
  
8.  在  “证书属性”对话框的  “使用者”选项卡上，在  “使用者名称”区域的  “类型”中选择  “公用名”。  
  
9. 在  “值”中，输入网络位置服务器网站的 FQDN，然后单击  “添加”。  
  
10. 在  “备用名称”区域的  “类型”中，选择  “DNS”。  
  
11. 在  “值”中，输入网络位置服务器网站的 FQDN，然后单击  “添加”。  
  
12. 在  “常规”选项卡的  “友好名称”中，输入一个有助于标识证书的名称。  
  
13. 依次单击  “确定”、  “注册”和  “完成”。  
  
14. 在证书管理单元的详细信息窗格中，通过“服务器身份验证的预期目的”验证是否注册了新证书。  
  
#### <a name="to-configure-the-network-location-server"></a>要配置网络位置服务器，请执行以下操作：  
  
1.  在高可用性服务器上设置网站。 该网站不需要任何内容，但是当你对它进行测试时，你可以定义客户端进行连接时提供消息的默认页面。  
  
    > [!NOTE]  
    > 如果在 DirectAccess 服务器上托管网络位置服务器网站，则不需要此步骤。  
  
2.  将 HTTPS 服务器证书绑定到该网站。 该证书的公用名应与网络位置服务器网站的名称相匹配。 请确保 DirectAccess 客户端信任发证 CA。  
  
    > [!NOTE]  
    > 如果在 DirectAccess 服务器上托管网络位置服务器网站，则不需要此步骤。  
  
3.  设置在内部网络中高度可用的 CRL 站点。  
  
    可以通过以下服务器访问 CRL 分发点：  
  
    -   通过使用基于 HTTP 的 URL，如 web 服务器： https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   文件服务器，如通过通用命名约定 (UNC) 路径访问\\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    如果仅可通过 IPv6 访问内部的 CRL 分发点，则必须配置高级安全 Windows 防火墙连接安全规则，以免除从 Intranet 的 IPv6 地址到 CRL 分发点的 IPv6 地址的 IPsec 保护。  
  
4.  请确保内部网络上的 DirectAccess 客户端可以解析网络位置服务器的名称。 并确保 Internet 上的 DirectAccess 客户端不可解析该名称。  
  
## <a name="BKMK_Links"></a>下一步  
  
-   [步骤 2：配置高级 DirectAccess 服务器](da-adv-configure-s2-servers.md)  
  


