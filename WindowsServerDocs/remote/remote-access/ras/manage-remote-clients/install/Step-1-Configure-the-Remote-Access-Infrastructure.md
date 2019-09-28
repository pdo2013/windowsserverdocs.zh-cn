---
title: 步骤1配置远程访问基础结构
description: 本主题是在 Windows Server 2016 中远程管理 DirectAccess 客户端的指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 110696d9f1ff082cfae315632c78fddc14359d52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367322"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>步骤1配置远程访问基础结构

>适用于：Windows Server（半年频道）、Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
本主题介绍如何使用混合的 IPv4 和 IPv6 环境中的单个远程访问服务器配置高级远程访问部署所需的基础结构。 在开始执行部署步骤之前，请确保已完成 @no__t 0Step 1 中所述的规划步骤：规划远程访问基础结构 @ no__t。  
  
|任务|描述|  
|----|--------|  
|配置服务器网络设置|配置远程访问服务器上的服务器网络设置。|  
|配置企业网络中的路由|配置企业网络中的路由以确保正确地路由通信。|  
|配置防火墙|根据需要配置其他防火墙。|  
|配置 CA 和证书|配置证书颁发机构（CA）（如果需要）以及部署中所需的任何其他证书模板。|  
|配置 DNS 服务器|配置远程访问服务器的 DirectAccess 设置。|  
|配置 Active Directory|将客户端计算机和远程访问服务器联接到 Active Directory 域。|  
|配置 GPO|如果需要，为部署配置组策略对象（Gpo）。|  
|配置安全组|配置将包含 DirectAccess 客户端计算机的安全组，以及部署中所需的任何其他安全组。|  
|配置网络位置服务器|配置网络位置服务器，包括安装网络位置服务器网站证书。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_ConfigNetworkSettings"></a>配置服务器网络设置  
根据你决定将远程访问服务器放置在边缘还是位于网络地址转换（NAT）设备后面，以下网络接口地址设置是使用 IPv4 和 IPv6 的环境中的单个服务器部署所必需的。 可使用“Windows 网络和共享中心”中的“更改适配器设置”配置所有 IP 地址。  
  
**边缘拓扑**：  
  
需要以下各项：  
  
-   两个面向 Internet 的连续公共静态 IPv4 或 IPv6 地址。  
  
    > [!NOTE]  
    > Teredo 需要两个连续的公用 IPv4 地址。 如果你不使用 Teredo，则可以配置单个公用静态 IPv4 地址。  
  
-   单个内部静态 IPv4 或 IPv6 地址。  
  
在**NAT 设备后面（两个网络适配器）** ：  
  
需要一个面向内部网络的静态 IPv4 或 IPv6 地址。  
  
在**NAT 设备后面（一个网络适配器）** ：  
  
需要单个静态 IPv4 或 IPv6 地址。  
  
如果远程访问服务器具有两个网络适配器（一个用于域配置文件，另一个用于公共或专用配置文件），但使用单个网络适配器拓扑，则建议如下所示：  
  
1.  确保第二个网络适配器也分类到域配置文件中。  
  
2.  如果由于任何原因无法为域配置文件配置第二个网络适配器，则必须使用以下 Windows PowerShell 命令手动将 DirectAccess IPsec 策略的作用域设置为所有配置文件：  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    要在此命令中使用的 IPsec 策略的名称是**DaServerToInfra**和**DaServerToCorp**。  
  
## <a name="BKMK_ConfigRouting"></a>在企业网络中配置路由  
在企业网络中配置路由，如下所示：  
  
-   在组织中部署本机 IPv6 时，添加一个路由，以便内部网络上的路由器通过远程访问服务器将 IPv6 通信路由回来。  
  
-   在远程访问服务器上手动配置组织 IPv4 和 IPv6 路由。 添加已发布的路由，以便将具有（/48） IPv6 前缀的所有流量转发到内部网络。 此外，对于 IPv4 通信，请添加显式路由，以便将 IPv4 通信转发到内部网络。  
  
## <a name="BKMK_ConfigFirewalls"></a>配置防火墙  
根据你选择的网络设置，当你在部署中使用其他防火墙时，请为远程访问通信应用以下防火墙例外：  
  
### <a name="remote-access-server-on-ipv4-internet"></a>IPv4 Internet 上的远程访问服务器  
当远程访问服务器位于 IPv4 Internet 上时，为远程访问通信应用以下面向 Internet 的防火墙例外：  
  
-   **Teredo 流量**  
  
    用户数据报协议（UDP）目标端口3544入站，UDP 源端口3544出站。 为远程访问服务器上的两个面向 Internet 的连续公用 IPv4 地址应用此例外。  
  
-   **6to4 流量**  
  
    IP 协议41入站和出站。 为远程访问服务器上的两个面向 Internet 的连续公用 IPv4 地址应用此例外。  
  
-   **Ip-https 流量**  
  
    传输控制协议（TCP）目标端口443和 TCP 源端口443出站。 当远程访问服务器配有单一网络适配器，且网络位置服务器位于远程访问服务器上时，还需要 TCP 端口 62000。 仅将这些例外应用于服务器的外部名称解析的地址。  
  
    > [!NOTE]  
    > 此例外是在远程访问服务器上配置的。 在边缘防火墙上配置所有其他例外。  
  
### <a name="remote-access-server-on-ipv6-internet"></a>IPv6 Internet 上的远程访问服务器  
当远程访问服务器位于 IPv6 Internet 上时，为远程访问通信应用以下面向 Internet 的防火墙例外：  
  
-   IP 协议 50  
  
-   UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
-   适用于 IPv6 的 Internet 控制消息协议（ICMPv6）仅适用于 Teredo 实现的入站和出站通信。  
  
### <a name="remote-access-traffic"></a>远程访问流量  
为远程访问通信应用以下内部网络防火墙例外：  
  
-   ISATAP协议41入站和出站  
  
-   所有 IPv4 或 IPv6 通信的 TCP/UDP  
  
-   所有 IPv4 或 IPv6 通信的 ICMP  
  
## <a name="BKMK_ConfigCAs"></a>配置 Ca 和证书  
使用 Windows Server 2012 中的远程访问，可以选择是使用证书进行计算机身份验证，还是使用使用用户名和密码的内置 Kerberos 身份验证。 还必须在远程访问服务器上配置 ip-https 证书。 本部分介绍如何配置这些证书。  
  
有关设置公钥基础结构（PKI）的信息，请参阅[Active Directory 证书服务](https://technet.microsoft.com/library/cc770357.aspx)。  
  
### <a name="BKMK_ConfigIPsec"></a>配置 IPsec 身份验证  
远程访问服务器和所有 DirectAccess 客户端上都需要一个证书，以便这些客户端可以使用 IPsec 身份验证。 证书必须由内部证书颁发机构（CA）颁发。 远程访问服务器和 DirectAccess 客户端必须信任颁发根证书和中间证书的 CA。  
  
##### <a name="to-configure-ipsec-authentication"></a>配置 IPsec 身份验证  
  
1.  在内部 CA 上，决定是使用默认计算机证书模板，还是创建新的证书模板（如[创建证书模板](https://technet.microsoft.com/library/cc731705.aspx)中所述）。  
  
    > [!NOTE]  
    > 如果创建新模板，则必须将其配置为使用客户端身份验证。  
  
2.  如果需要，请部署证书模板。 有关详细信息，请参阅[部署证书模板](https://technet.microsoft.com/library/cc770794.aspx)。  
  
3.  如果需要，为自动注册配置模板。  
  
4.  如果需要，配置证书自动注册。 有关详细信息，请参阅[配置证书自动注册](https://technet.microsoft.com/library/cc731522.aspx)。  
  
### <a name="BKMK_ConfigCertTemp"></a>配置证书模板  
当你使用内部 CA 颁发证书时，必须为 IP-HTTPS 证书和网络位置服务器网站证书配置证书模板。  
  
##### <a name="to-configure-a-certificate-template"></a>配置证书模板的步骤  
  
1.  在内部 CA 中，根据 [创建证书模板](https://technet.microsoft.com/library/cc731705.aspx)中所述创建一个证书模板。  
  
2.  根据 [部署证书模板](https://technet.microsoft.com/library/cc770794.aspx)中所述部署该证书模板。  
  
准备好模板之后，可以使用它们来配置证书。 有关详细信息，请参阅以下过程：  
  
-   [配置 ip-https 证书](#BKMK_IPHTTPS)  
  
-   [配置网络位置服务器](#BKMK_ConfigNLS)  
  
### <a name="BKMK_IPHTTPS"></a>配置 ip-https 证书  
远程访问需要使用 IP-HTTPS 证书对到远程访问服务器的 IP-HTTPS 连接进行身份验证。 有三个 IP-HTTPS 证书的证书选项：  
  
-   **Public**  
  
    由第三方提供。  
  
-   **Private**  
  
    此证书基于你在[配置证书模板](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)中创建的证书模板。 它需要可从可公开解析的 FQDN 访问的证书吊销列表（CRL）分发点。  
  
-   **自签名**  
  
    此证书需要可从可公开解析的 FQDN 访问的 CRL 分发点。  
  
    > [!NOTE]  
    > 无法在多站点部署中使用自签名证书。  
  
确保用于 IP-HTTPS 身份验证的网站证书符合以下要求：  
  
-   证书使用者名称应该是仅用于远程访问服务器 ip-https 连接的 IP-HTTPS URL （ConnectTo 地址）的可从外部解析的完全限定的域名（FQDN）。  
  
-   该证书的公用名应与 IP-HTTPS 站点的名称相匹配。  
  
-   在 "使用者" 字段中，指定远程访问服务器的面向外部的适配器的 IPv4 地址或 IP-HTTPS URL 的 FQDN。  
  
-   对于 "**增强型密钥用法**" 字段，请使用服务器身份验证对象标识符（OID）。  
  
-   对于“CRL 分发点”字段，请指定已连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   必须直接将 IP-HTTPS 证书导入到个人存储中。  
  
-   IP-HTTPS 证书可以在名称中包含通配符符。  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>安装内部 CA 颁发的 IP-HTTPS 证书  
  
1.  在远程访问服务器上：在 "**开始**" 屏幕上，键入**mmc.exe**，然后按 enter。  
  
2.  在 MMC 控制台中的“文件”菜单上，单击“添加/删除管理单元”。  
  
3.  在“添加或删除管理单元”对话框中，依次单击“证书”、“添加”、“计算机帐户”、“下一步”、“本地计算机”和“完成”，然后单击“确定”。  
  
4.  在证书管理单元的控制台树中，依次打开“证书(本地计算机)\个人\证书”。  
  
5.  右键单击 "**证书**"，指向 "**所有任务**"，单击 "**申请新证书**"，然后单击两次 "**下一步**"。  
  
6.  在 "**申请证书**" 页上，选中在配置证书模板中创建的证书模板的复选框，并根据需要单击 "**注册此证书需要详细信息**"。  
  
7.  在“证书属性”对话框的“使用者”选项卡上，在“使用者名称”区域的“类型”中选择“公用名”。  
  
8.  在 "**值**" 中，指定远程访问服务器面向外部的适配器的 IPv4 地址，或 ip-https URL 的 FQDN，然后单击 "**添加**"。  
  
9. 在“备用名称”区域的“类型”中，选择“DNS”。  
  
10. 在 "**值**" 中，指定远程访问服务器面向外部的适配器的 IPv4 地址，或 ip-https URL 的 FQDN，然后单击 "**添加**"。  
  
11. 在“常规”选项卡的“友好名称”中，输入一个有助于标识证书的名称。  
  
12. 在“扩展”选项卡上，单击“扩展密钥用法”旁边的箭头，并确保“服务器身份验证”出现在“已选选项”列表中。  
  
13. 依次单击“确定”、“注册”和“完成”。  
  
14. 在 "证书" 管理单元的详细信息窗格中，验证是否已将新证书注册到服务器身份验证的预期目的。  
  
## <a name="BKMK_ConfigDNS"></a>配置 DNS 服务器  
你必须为部署中的内部网络手动配置用于网络位置服务器网站的 DNS 条目。  
  
### <a name="NLS_DNS"></a>添加网络位置服务器和 web 探测  
  
1.  在内部网络 DNS 服务器上：在 "**开始**" 屏幕上，键入 "**dnsmgmt.msc**"，然后按 enter。  
  
2.  在“DNS 管理器”控制台的左窗格中，展开域的前向查找区域。 右键单击该域，然后单击 "**新建主机（A 或 AAAA）** "。  
  
3.  在 "**新建主机**" 对话框的 "**名称（如果为空则使用父域名称）** " 框中，输入网络位置服务器网站的 DNS 名称（这是 DirectAccess 客户端用于连接到网络位置服务器的名称）。 在 " **IP 地址**" 框中，输入网络位置服务器的 IPv4 地址，然后单击 "**添加主机**"，然后单击 **"确定"** 。  
  
4.  在 "**新建主机**" 对话框的 "**名称（如果为空则使用父域名称）** " 框中，输入 web 探测的 DNS 名称（默认 web 探测的名称为 directaccess-webprobehost）。 在“IP 地址”框中，输入 Web 探测的 IPv4 地址，然后单击“添加主机”。  
  
5.  为 directaccess corpconnectivityhost 和任何手动创建的连接性验证程序重复此过程。 在 " **DNS** " 对话框中，单击 **"确定"** 。  
  
6.  单击 **“完成”** 。  
  
@no__t 0Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
还必须为以下内容配置 DNS 条目：  
  
-   **Ip-https 服务器**  
  
    DirectAccess 客户端必须能够从 Internet 解析远程访问服务器的 DNS 名称。  
  
-   **CRL 吊销检查**  
  
    DirectAccess 使用证书吊销检查来检查 DirectAccess 客户端与远程访问服务器之间的 ip-https 连接，以及 DirectAccess 客户端和网络位置服务器之间基于 HTTPS 的连接。 在这两种情况下，DirectAccess 客户端都必须能够解析和访问 CRL 分发点位置。  
  
-   **ISATAP**  
  
    站点内自动隧道寻址协议（ISATAP）使用隧道来使 DirectAccess 客户端能够通过 IPv4 Internet 连接到远程访问服务器，并在 IPv4 标头中封装 IPv6 包。 远程访问使用它提供通过 Intranet 到 ISATAP 主机的 IPv6 连接。 在非本机 IPv6 网络环境中，远程访问服务器会自动将自身配置为 ISATAP 路由器。 要求支持对 ISATAP 名称进行解析。  
  
## <a name="BKMK_ConfigAD"></a>配置 Active Directory  
必须将远程访问服务器和所有 DirectAccess 客户端计算机都加入 Active Directory 域。 DirectAccess 客户端计算机必须是以下域类型之一的成员：  
  
-   与远程访问服务器属于同一林的域。  
  
-   属于与远程访问服务器林具有双向信任关系的林的域。  
  
-   与远程访问服务器域具有双向域信任的域。  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>将远程访问服务器加入域  
  
1.  在服务器管理器中，单击 **“本地服务器”** 。 在详细信息窗格中，单击“计算机名”旁边的链接。  
  
2.  在“系统属性” 对话框中，单击“计算机名” 选项卡，然后单击“更改”。  
  
3.  如果在将服务器加入域时还要更改计算机名，请在 "**计算机名**" 框中键入计算机的名称。 在 "**隶属**于" 下，单击 "**域**"，然后键入要将服务器加入的域的名称（例如，corp.contoso.com），然后单击 **"确定"** 。  
  
4.  当系统提示你输入用户名和密码时，请输入有权将计算机加入域的用户的用户名和密码，然后单击 **"确定"** 。  
  
5.  当你看到欢迎你进入域的对话框时，请单击“确定”。  
  
6.  当系统提示你必须重新启动计算机时，请单击“确定”。  
  
7.  在 **“系统属性”** 对话框中，单击 **“关闭”** 。  
  
8.  当系统提示你重新启动计算机时，请单击“立即重新启动”。  
  
#### <a name="to-join-client-computers-to-the-domain"></a>将客户端计算机加入域  
  
1.  在 "**开始**" 屏幕上，键入 "**资源管理器**"，然后按 enter。  
  
2.  右键单击计算机图标，然后单击“属性”。  
  
3.  在“系统”页上，单击“高级系统设置”。  
  
4.  在 **“系统属性”** 对话框中的 **“计算机名”** 选项卡上，单击 **“更改”** 。  
  
5.  如果在将服务器加入域时还要更改计算机名，请在 "**计算机名**" 框中键入计算机的名称。 在“隶属于”下面单击“域”，键入服务器要加入到的域的名称（例如 corp.contoso.com），然后单击“确定”。  
  
6.  当系统提示你输入用户名和密码时，请输入有权将计算机加入域的用户的用户名和密码，然后单击 **"确定"** 。  
  
7.  当你看到欢迎你进入域的对话框时，请单击“确定”。  
  
8.  当系统提示你必须重新启动计算机时，请单击“确定”。  
  
9. 在 "**系统属性**" 对话框中，单击 "关闭"。  
  
10. 出现提示时单击“立即重新启动”。  
  
@no__t 0Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
> [!NOTE]  
> 输入以下命令后，必须提供域凭据。  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="BKMK_ConfigGPOs"></a>配置 Gpo  
若要部署远程访问，需要至少两个组策略的对象。 一个组策略对象包含远程访问服务器的设置，另一个包含 DirectAccess 客户端计算机的设置。 配置远程访问时，向导将自动创建所需的组策略对象。 但是，如果你的组织强制使用命名约定，或者你没有创建或编辑组策略对象所需的权限，则必须在配置远程访问之前创建它们。  
  
若要创建组策略对象，请参阅[创建和编辑组策略对象](https://technet.microsoft.com/library/cc754740.aspx)。  
  
管理员可以手动将 DirectAccess 组策略对象链接到组织单位（OU）。 请考虑下列各项：  
  
1.  在配置 DirectAccess 之前，请将已创建的 Gpo 链接到各自的 Ou。  
  
2.  在你配置 DirectAccess 时，请为客户端计算机指定安全组。  
  
3.  自动配置 Gpo，无论管理员是否有权限将 Gpo 链接到域。  
  
4.  如果已将 Gpo 链接到 OU，则将不会删除这些链接，但这些链接不会链接到域。  
  
5.  对于服务器 Gpo，OU 必须包含服务器计算机对象，否则该 GPO 将链接到域的根。  
  
6.  如果之前未通过运行 DirectAccess 安装向导链接 OU，则在配置完成后，管理员可以将 DirectAccess Gpo 链接到所需的 Ou，并删除指向域的链接。  
  
    有关详细信息，请参阅[链接组策略对象](https://technet.microsoft.com/library/cc732979.aspx)。  
  
> [!NOTE]  
> 如果组策略对象手动创建，则在 DirectAccess 配置过程中可能无法使用组策略对象。 可能没有将组策略对象复制到最接近管理计算机的域控制器。 管理员可以等待复制完成，或强制进行复制。  
  
## <a name="BKMK_ConfigSGs"></a>配置安全组  
客户端计算机组策略对象中包含的 DirectAccess 设置仅应用于配置远程访问时指定的安全组成员的计算机。  
  
### <a name="Sec_Group"></a>为 DirectAccess 客户端创建安全组  
  
1.  在 "**开始**" 屏幕上，键入**dsa.msc**，然后按 enter。  
  
2.  在“Active Directory 用户和计算机”控制台的左窗格中，展开将包含安全组的域，右键单击“用户”，指向“新建”，然后单击“组”。  
  
3.  在“新建对象 – 组”对话框中的“组名”下，输入该安全组的名称。  
  
4.  在“组范围”下单击“全局”，并在“组类型”下单击“安全”，然后单击“确定”。  
  
5.  双击 "DirectAccess 客户端计算机" 安全组，然后在 "**属性**" 对话框中，单击 "**成员**" 选项卡。  
  
6.  在“成员” 选项卡上，单击“添加”。  
  
7.  在“选择用户、联系人、计算机或服务帐户”对话框中，选择你希望为 DirectAccess 启用的客户端计算机，然后单击“确定”。  
  
@no__t 0Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**Windows powershell 等效命令**  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_ConfigNLS"></a>配置网络位置服务器  
网络位置服务器应在具有高可用性的服务器上，并且它需要受 DirectAccess 客户端信任的有效安全套接字层（SSL）证书。  
  
> [!NOTE]  
> 如果网络位置服务器网站位于远程访问服务器上，则在你配置远程访问时将自动创建一个网站，并且该网站将绑定到你提供的服务器证书。  
  
网络位置服务器证书有两个证书选项：  
  
-   **Private**  
  
    > [!NOTE]  
    > 此证书基于你在[配置证书模板](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)中创建的证书模板。  
  
-   **自签名**  
  
    > [!NOTE]  
    > 无法在多站点部署中使用自签名证书。  
  
无论你使用的是专用证书还是自签名证书，它们都需要以下各项：  
  
-   用于网络位置服务器的网站证书。 证书使用者应该是网络位置服务器的 URL。  
  
-   在内部网络上具有高可用性的 CRL 分发点。  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>安装内部 CA 颁发的网络位置服务器证书  
  
1.  在将托管网络位置服务器网站的服务器上：在 "**开始**" 屏幕上，键入**mmc.exe**，然后按 enter。  
  
2.  在 MMC 控制台中的“文件”菜单上，单击“添加/删除管理单元”。  
  
3.  在“添加或删除管理单元”对话框中，依次单击“证书”、“添加”、“计算机帐户”、“下一步”、“本地计算机”和“完成”，然后单击“确定”。  
  
4.  在证书管理单元的控制台树中，依次打开“证书(本地计算机)\个人\证书”。  
  
5.  右键单击 "**证书**"，指向 "**所有任务**"，单击 "**申请新证书**"，然后单击 "**下一步**" 两次。  
  
6.  在 "**申请证书**" 页上，选中在配置证书模板中创建的证书模板的复选框，并根据需要单击 "**注册此证书需要详细信息**"。  
  
7.  在“证书属性”对话框的“使用者”选项卡上，在“使用者名称”区域的“类型”中选择“公用名”。  
  
8.  在“值”中，输入网络位置服务器网站的 FQDN，然后单击“添加”。  
  
9. 在“备用名称”区域的“类型”中，选择“DNS”。  
  
10. 在“值”中，输入网络位置服务器网站的 FQDN，然后单击“添加”。  
  
11. 在“常规”选项卡的“友好名称”中，输入一个有助于标识证书的名称。  
  
12. 依次单击“确定”、“注册”和“完成”。  
  
13. 在 "证书" 管理单元的详细信息窗格中，验证是否已为 "服务器身份验证" 的预期目的注册了新证书。  
  
#### <a name="to-configure-the-network-location-server"></a>要配置网络位置服务器，请执行以下操作：  
  
1.  在高可用性服务器上设置网站。 该网站不需要任何内容，但是当你对它进行测试时，你可以定义客户端进行连接时提供消息的默认页面。  
  
    如果在远程访问服务器上托管网络位置服务器网站，则不需要执行此步骤。  
  
2.  将 HTTPS 服务器证书绑定到该网站。 该证书的公用名应与网络位置服务器网站的名称相匹配。 请确保 DirectAccess 客户端信任发证 CA。  
  
    如果在远程访问服务器上托管网络位置服务器网站，则不需要执行此步骤。  
  
3.  设置在内部网络上 hass 高可用性的 CRL 站点。  
  
    可以通过以下服务器访问 CRL 分发点：  
  
    -   使用基于 HTTP 的 URL 的 Web 服务器，例如： https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   通过通用命名约定（UNC）路径（例如 \\ \ com\crld\corp-APP1-CA.crl）访问的文件服务器。  
  
    如果内部 CRL 分发点只能通过 IPv6 访问，则必须配置具有高级安全性的 Windows 防火墙连接安全规则。 此豁免 IPsec 保护从 intranet 的 IPv6 地址空间到 CRL 分发点的 IPv6 地址。  
  
4.  请确保内部网络上的 DirectAccess 客户端可以解析网络位置服务器的名称，并且 Internet 上的 DirectAccess 客户端无法解析该名称。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 2：配置远程访问服务器](Step-2-Configure-the-Remote-Access-Server.md)

