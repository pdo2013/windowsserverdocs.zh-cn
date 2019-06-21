---
title: 步骤 1 配置远程访问基础结构
description: 本主题是指南的在 Windows Server 2016 中远程管理 DirectAccess 客户端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 712ef2fb5ada3f1890d0cdeac33bed1c0fb82e41
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282845"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>步骤 1 配置远程访问基础结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
本主题介绍如何配置基础结构所需的高级远程访问部署在混合的 IPv4 和 IPv6 环境中使用单个远程访问服务器。 开始部署步骤之前，确保已完成中所述的规划步骤[步骤 1:规划远程访问基础结构](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md)。  
  
|任务|描述|  
|----|--------|  
|配置服务器网络设置|配置远程访问服务器上的服务器网络设置。|  
|配置企业网络中的路由|配置企业网络中的路由以确保正确地路由通信。|  
|配置防火墙|根据需要配置其他防火墙。|  
|配置 CA 和证书|配置证书颁发机构 (CA)，如有必要和任何其他部署中所需的证书模板。|  
|配置 DNS 服务器|配置远程访问服务器的 DirectAccess 设置。|  
|配置 Active Directory|将客户端计算机和远程访问服务器加入到 Active Directory 域。|  
|配置 GPO|如果所需的部署配置组策略对象 (Gpo)。|  
|配置安全组|配置将包含 DirectAccess 客户端计算机的安全组，以及部署中所需的任何其他安全组。|  
|配置网络位置服务器|配置网络位置服务器，包括安装网络位置服务器网站证书。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_ConfigNetworkSettings"></a>配置服务器网络设置  
具体取决于你决定将远程访问服务器放置在边缘或网络地址转换 (NAT) 设备，后面的以下网络接口地址设置需要针对具有 IPv4 和 IPv6 的环境中的单个服务器部署。 可使用  “Windows 网络和共享中心”中的  “更改适配器设置”配置所有 IP 地址。  
  
**边缘拓扑**:  
  
需要以下项：  
  
-   两个面向 Internet 的连续公用静态 IPv4 或 IPv6 地址。  
  
    > [!NOTE]  
    > Teredo 需要两个连续的公用 IPv4 地址。 如果你不使用 Teredo，则可以配置单个公用静态 IPv4 地址。  
  
-   单个内部静态 IPv4 或 IPv6 地址。  
  
**在 NAT 设备 （两个网络适配器） 后面**:  
  
需要单个内部面向网络的静态 IPv4 或 IPv6 地址。  
  
**在 NAT 设备 （一个网络适配器） 后面**:  
  
需要单个静态 IPv4 或 IPv6 地址。  
  
如果远程访问服务器具有两个网络适配器 （一个是用于域配置文件，另一个用于公共或专用配置文件中），但在使用单个网络适配器拓扑，建议如下所示：  
  
1.  请确保第二个网络适配器也归类于域配置文件。  
  
2.  如果出于任何原因，不能为域配置文件配置第二个网络适配器，将 DirectAccess IPsec 策略必须的范围手动扩展至所有配置文件使用以下 Windows PowerShell 命令：  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    若要在此命令中使用的 IPsec 策略的名称是**Directaccess-daservertoinfra**并**Directaccess-daservertocorp**。  
  
## <a name="BKMK_ConfigRouting"></a>配置企业网络中的路由  
在企业网络中配置路由，如下所示：  
  
-   在组织中部署本机 IPv6 时，添加一个路由，以便内部网络上的路由器通过远程访问服务器将 IPv6 通信路由回来。  
  
-   在远程访问服务器上手动配置组织 IPv4 和 IPv6 路由。 添加已发布的路由，以便所有流量都使用 （/ 48） IPv6 前缀转发到内部网络。 此外，对于 IPv4 通信，请添加显式路由，以便将 IPv4 通信转发到内部网络。  
  
## <a name="BKMK_ConfigFirewalls"></a>配置防火墙  
选择具体取决于网络设置，在部署中使用其他防火墙时，应用远程访问通信的以下防火墙例外情况：  
  
### <a name="remote-access-server-on-ipv4-internet"></a>IPv4 Internet 上的远程访问服务器  
当远程访问服务器位于 IPv4 Internet 上时，将应用远程访问通信的以下面向 Internet 的防火墙例外情况：  
  
-   **Teredo 通信**  
  
    用户数据报协议 (UDP) 目标端口 3544 入站，以及 UDP 源端口 3544 出站。 远程访问服务器上应用此例外，这两个面向 Internet 的连续公用 IPv4 地址。  
  
-   **6to4 通信**  
  
    IP 协议 41 入站和出站。 远程访问服务器上应用此例外，这两个面向 Internet 的连续公用 IPv4 地址。  
  
-   **IP HTTPS 流量**  
  
    传输控制协议 (TCP) 目标端口 443，以及 TCP 源端口 443 出站。 当远程访问服务器配有单一网络适配器，且网络位置服务器位于远程访问服务器上时，还需要 TCP 端口 62000。 将应用这些例外仅对服务器的外部名称解析的地址。  
  
    > [!NOTE]  
    > 远程访问服务器上配置此例外。 在边缘防火墙上配置所有其他例外。  
  
### <a name="remote-access-server-on-ipv6-internet"></a>IPv6 Internet 上的远程访问服务器  
当远程访问服务器位于 IPv6 Internet 上时，将应用远程访问通信的以下面向 Internet 的防火墙例外情况：  
  
-   IP 协议 50  
  
-   UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
-   IPv6 的 Internet 控制消息协议 (ICMPv6) 流量入站和出站-适用于仅 Teredo 实施。  
  
### <a name="remote-access-traffic"></a>远程访问通信  
将应用远程访问通信的以下内部网络防火墙例外情况：  
  
-   ISATAP:协议 41 入站和出站  
  
-   所有的 IPv4 或 IPv6 通信的 TCP/UDP  
  
-   所有的 IPv4 或 IPv6 通信的 ICMP  
  
## <a name="BKMK_ConfigCAs"></a>配置 Ca 和证书  
在 Windows Server 2012，您可以选择是使用证书进行计算机身份验证还是使用使用用户名和密码的内置 Kerberos 身份验证的远程访问。 此外必须在远程访问服务器上配置 IP-HTTPS 证书。 本部分介绍如何配置这些证书。  
  
有关设置公钥基础结构 (PKI) 的信息，请参阅[Active Directory 证书服务](https://technet.microsoft.com/library/cc770357.aspx)。  
  
### <a name="BKMK_ConfigIPsec"></a>配置 IPsec 身份验证  
远程访问服务器和所有 DirectAccess 客户端上需要使用证书，以便他们可以使用 IPsec 身份验证。 必须由内部证书颁发机构 (CA) 颁发证书。 远程访问服务器和 DirectAccess 客户端必须信任颁发根和中间证书的 CA。  
  
##### <a name="to-configure-ipsec-authentication"></a>配置 IPsec 身份验证  
  
1.  在内部 CA 中，确定将使用默认的计算机证书模板，还是将创建新的证书模板，如中所述[创建证书模板](https://technet.microsoft.com/library/cc731705.aspx)。  
  
    > [!NOTE]  
    > 如果您创建一个新的模板，它必须配置客户端身份验证。  
  
2.  如果需要，部署该证书模板。 有关详细信息，请参阅[部署证书模板](https://technet.microsoft.com/library/cc770794.aspx)。  
  
3.  如果需要，配置为自动注册的模板。  
  
4.  如果需要，配置证书自动注册。 有关详细信息，请参阅[配置证书自动注册](https://technet.microsoft.com/library/cc731522.aspx)。  
  
### <a name="BKMK_ConfigCertTemp"></a>配置证书模板  
当你使用内部 CA 颁发证书时，必须配置用于 IP-HTTPS 证书和网络位置服务器网站证书的证书模板。  
  
##### <a name="to-configure-a-certificate-template"></a>配置证书模板的步骤  
  
1.  在内部 CA 中，根据 [创建证书模板](https://technet.microsoft.com/library/cc731705.aspx)中所述创建一个证书模板。  
  
2.  根据 [部署证书模板](https://technet.microsoft.com/library/cc770794.aspx)中所述部署该证书模板。  
  
准备你的模板后，可以使用它们来配置证书。 请参阅以下详细信息的过程：  
  
-   [配置 IP-HTTPS 证书](#BKMK_IPHTTPS)  
  
-   [配置网络位置服务器](#BKMK_ConfigNLS)  
  
### <a name="BKMK_IPHTTPS"></a>配置 IP-HTTPS 证书  
远程访问需要使用 IP-HTTPS 证书对到远程访问服务器的 IP-HTTPS 连接进行身份验证。 有三个 IP-HTTPS 证书的证书选项：  
  
-   **Public**  
  
    第三方提供。  
  
-   **专用**  
  
    该证书基于你在中创建的证书模板[配置证书模板](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)。 它需要，从可公开解析的 FQDN 访问的证书吊销列表 (CRL) 分发点。  
  
-   **自签名**  
  
    此证书需要从可公开解析的 FQDN 访问的 CRL 分发点。  
  
    > [!NOTE]  
    > 无法在多站点部署中使用自签名证书。  
  
确保用于 IP-HTTPS 身份验证的网站证书符合以下要求：  
  
-   证书使用者名称应仅用于远程访问服务器的 IP-HTTPS 连接的 IP-HTTPS URL （ConnectTo 地址） 的外部解析完全限定的域名称 (FQDN)。  
  
-   该证书的公用名应与 IP-HTTPS 站点的名称相匹配。  
  
-   在主题字段中，指定远程访问服务器面向外部的适配器或 IP-HTTPS URL 的 FQDN 的 IPv4 地址。  
  
-   有关**增强型密钥用法**字段中，使用服务器身份验证对象标识符 (OID)。  
  
-   对于“CRL 分发点”  字段，请指定已连接到 Internet 的 DirectAccess 客户端可访问的 CRL 分发点。  
  
-   IP-HTTPS 证书必须包含私钥。  
  
-   必须直接将 IP-HTTPS 证书导入到个人存储中。  
  
-   IP-HTTPS 证书可以在名称中包含通配符符。  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>安装内部 CA 颁发的 IP-HTTPS 证书  
  
1.  在远程访问服务器上：上**启动**屏幕上，键入**mmc.exe**，然后按 ENTER。  
  
2.  在 MMC 控制台中的“文件”  菜单上，单击“添加/删除管理单元”  。  
  
3.  在  “添加或删除管理单元”对话框中，依次单击  “证书”、  “添加”、  “计算机帐户”、  “下一步”、  “本地计算机”和  “完成”，然后单击  “确定”。  
  
4.  在证书管理单元的控制台树中，依次打开  “证书(本地计算机)\个人\证书”。  
  
5.  右键单击**证书**，依次指向**的所有任务**，单击**申请新证书**，然后单击**下一步**两次...  
  
6.  上**申请证书**页上，选中配置证书模板中创建的证书模板的复选框，如有必要，单击**注册为此需要的详细信息证书**。  
  
7.  在  “证书属性”对话框的  “使用者”选项卡上，在  “使用者名称”区域的  “类型”中选择  “公用名”。  
  
8.  在中**值**，指定的远程访问服务器面向外部的适配器或 IP-HTTPS URL 的 FQDN 的 IPv4 地址，然后单击**添加**。  
  
9. 在  “备用名称”区域的  “类型”中，选择  “DNS”。  
  
10. 在中**值**，指定的远程访问服务器面向外部的适配器或 IP-HTTPS URL 的 FQDN 的 IPv4 地址，然后单击**添加**。  
  
11. 在  “常规”选项卡的  “友好名称”中，输入一个有助于标识证书的名称。  
  
12. 在“扩展”  选项卡上，单击“扩展密钥用法”  旁边的箭头，并确保“服务器身份验证”出现在“已选选项”  列表中。  
  
13. 依次单击  “确定”、  “注册”和  “完成”。  
  
14. 在证书管理单元的详细信息窗格中，验证具有服务器身份验证的预期用途，注册新的证书。  
  
## <a name="BKMK_ConfigDNS"></a>配置 DNS 服务器  
你必须为部署中的内部网络手动配置用于网络位置服务器网站的 DNS 条目。  
  
### <a name="NLS_DNS"></a>若要添加网络位置服务器和 web 探测  
  
1.  在内部网络 DNS 服务器上：上**启动**屏幕上，键入**dnsmgmt.msc**，然后按 ENTER。  
  
2.  在  “DNS 管理器”控制台的左窗格中，展开域的前向查找区域。 右键单击域，然后单击**新建主机 （A 或 AAAA）** 。  
  
3.  在中**新的主机**对话框中**名称 （使用父域名称如果为空）** 框中，输入网络位置服务器网站的 DNS 名称 (这是 DirectAccess 客户端用于连接到的名称网络位置服务器）。 在中**IP 地址**中，输入网络位置服务器的 IPv4 地址，单击**添加主机**，然后单击**确定**。  
  
4.  在中**新的主机**对话框中**名称 （使用父域名称如果为空）** 框中，输入 （用于默认 web 探测的名称为 directaccess-webprobehost） 的 web 探测的 DNS 名称。 在  “IP 地址”框中，输入 Web 探测的 IPv4 地址，然后单击“添加主机”  。  
  
5.  为 directaccess corpconnectivityhost 和任何手动创建的连接性验证程序重复此过程。 在中**DNS**对话框中，单击**确定**。  
  
6.  单击 **“完成”** 。  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
还必须为以下内容配置 DNS 条目：  
  
-   **IP-HTTPS 服务器**  
  
    DirectAccess 客户端必须能够解析来自 Internet 的远程访问服务器的 DNS 名称。  
  
-   **CRL 吊销检查**  
  
    DirectAccess 将使用证书吊销检查的 DirectAccess 客户端和远程访问服务器之间的 IP-HTTPS 连接以及为 DirectAccess 客户端和网络位置服务器之间基于 HTTPS 的连接。 在这两种情况下，DirectAccess 客户端都必须能够解析和访问 CRL 分发点位置。  
  
-   **ISATAP**  
  
    站点内自动隧道寻址协议 (ISATAP) 使用隧道以使 DirectAccess 客户端可以通过在 IPv4 标头的 IPv6 数据包封装在 IPv4 Internet 连接到远程访问服务器。 远程访问使用它提供通过 Intranet 到 ISATAP 主机的 IPv6 连接。 在非本机 IPv6 网络环境中，远程访问服务器本身会自动配置为 ISATAP 路由器。 要求支持对 ISATAP 名称进行解析。  
  
## <a name="BKMK_ConfigAD"></a>配置 Active Directory  
必须将远程访问服务器和所有 DirectAccess 客户端计算机都加入 Active Directory 域。 DirectAccess 客户端计算机必须是以下域类型之一的成员：  
  
-   与远程访问服务器属于同一林的域。  
  
-   属于与远程访问服务器林具有双向信任关系的林的域。  
  
-   与远程访问服务器域具有双向域信任的域。  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>将远程访问服务器加入域  
  
1.  在服务器管理器中，单击 **“本地服务器”** 。 在详细信息窗格中，单击“计算机名”旁边的链接  。  
  
2.  在“系统属性”  对话框中，单击“计算机名”  选项卡，然后单击“更改”  。  
  
3.  在中**计算机名**框中，键入计算机的名称，如果在将服务器加入到域时还要更改计算机名称。 下**的成员**，单击**域**，然后键入你想要加入的服务器 (例如，corp.contoso.com)，然后单击的域的名称**确定**。  
  
4.  当系统提示输入用户名和密码的值时，输入用户名和密码具有权限的用户，以将计算机加入到域，然后单击**确定**。  
  
5.  当你看到欢迎你进入域的对话框时，请单击“确定”  。  
  
6.  当系统提示你必须重新启动计算机时，请单击“确定”  。  
  
7.  在 **“系统属性”** 对话框中，单击 **“关闭”** 。  
  
8.  当系统提示你重新启动计算机时，请单击“立即重新启动”  。  
  
#### <a name="to-join-client-computers-to-the-domain"></a>将客户端计算机加入域  
  
1.  上**启动**屏幕上，键入**explorer.exe**，然后按 ENTER。  
  
2.  右键单击计算机图标，然后单击“属性”  。  
  
3.  在  “系统”页上，单击  “高级系统设置”。  
  
4.  在 **“系统属性”** 对话框中的 **“计算机名”** 选项卡上，单击 **“更改”** 。  
  
5.  在中**计算机名**框中，键入计算机的名称，如果在将服务器加入到域时还要更改计算机名称。 在“隶属于”  下面单击“域”  ，键入服务器要加入到的域的名称（例如 corp.contoso.com），然后单击“确定”  。  
  
6.  当系统提示输入用户名和密码的值时，输入用户名和密码具有权限的用户，以将计算机加入到域，然后单击**确定**。  
  
7.  当你看到欢迎你进入域的对话框时，请单击“确定”  。  
  
8.  当系统提示你必须重新启动计算机时，请单击“确定”  。  
  
9. 在中**系统属性**对话框中，单击关闭。  
  
10. 出现提示时单击“立即重新启动”  。  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
> [!NOTE]  
> 输入以下命令后，你必须提供域凭据。  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="BKMK_ConfigGPOs"></a>配置 Gpo  
若要部署远程访问，你至少需要两个组策略对象。 一个组策略对象包含用于远程访问服务器的设置，另一个包含 DirectAccess 客户端计算机的设置。 在配置远程访问时，该向导将自动创建所需的组策略对象。 但是，如果你的组织强制使用命名约定，或者您没有所需的权限来创建或编辑组策略对象，则必须在之前创建配置远程访问。  
  
若要创建组策略对象，请参阅[创建和编辑组策略对象](https://technet.microsoft.com/library/cc754740.aspx)。  
  
管理员可以手动将 DirectAccess 组策略对象链接到组织单位 (OU)。 请考虑下列各项：  
  
1.  在配置 DirectAccess 之前，创建的 Gpo 链接到各自的 Ou。  
  
2.  在你配置 DirectAccess 时，请为客户端计算机指定安全组。  
  
3.  Gpo 是自动配置，而不考虑如果管理员有权将 Gpo 链接到域。  
  
4.  如果已将 Gpo 链接到 OU，链接将不会删除，但它们并未链接到域。  
  
5.  对于服务器 Gpo，OU 必须包含服务器的计算机对象-否则，该 GPO 将链接到域的根。  
  
6.  如果 OU 未通过配置完成后运行 DirectAccess 安装向导中，链接之前，管理员可以将 DirectAccess Gpo 链接到所需的 Ou，并删除链接到域。  
  
    有关详细信息，请参阅[链接组策略对象](https://technet.microsoft.com/library/cc732979.aspx)。  
  
> [!NOTE]  
> 如果手动创建组策略对象，则可能的组策略对象将不在可用在 DirectAccess 配置过程。 组策略对象可能尚未复制到最接近管理计算机的域控制器。 管理员可以等待复制完成或强制复制。  
  
## <a name="BKMK_ConfigSGs"></a>配置安全组  
客户端计算机的组策略对象中包含的 DirectAccess 设置仅应用于配置远程访问时指定的安全组的成员计算机。  
  
### <a name="Sec_Group"></a>若要为 DirectAccess 客户端创建安全组  
  
1.  上**启动**屏幕上，键入**dsa.msc**，然后按 ENTER。  
  
2.  在  “Active Directory 用户和计算机”控制台的左窗格中，展开将包含安全组的域，右键单击  “用户”，指向  “新建”，然后单击  “组”。  
  
3.  在  “新建对象 – 组”对话框中的  “组名”下，输入该安全组的名称。  
  
4.  在  “组范围”下单击  “全局”，并在  “组类型”下单击“安全”  ，然后单击“确定”  。  
  
5.  双击 DirectAccess 客户端计算机安全组，然后在**属性**对话框中，单击**成员**选项卡。  
  
6.  在“成员”  选项卡上，单击“添加”  。  
  
7.  在  “选择用户、联系人、计算机或服务帐户”对话框中，选择你希望为 DirectAccess 启用的客户端计算机，然后单击  “确定”。  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**Windows PowerShell 等效命令**  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_ConfigNLS"></a>配置网络位置服务器  
网络位置服务器应该具有高可用性的服务器上，它需要有效的安全套接字层 (SSL) 证书受信任的 DirectAccess 客户端。  
  
> [!NOTE]  
> 如果网络位置服务器网站位于远程访问服务器上，则当你配置远程访问，它绑定到你提供的服务器证书时，将自动创建网站。  
  
有两个网络位置服务器证书的证书选项：  
  
-   **专用**  
  
    > [!NOTE]  
    > 该证书基于你在中创建的证书模板[配置证书模板](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)。  
  
-   **自签名**  
  
    > [!NOTE]  
    > 无法在多站点部署中使用自签名证书。  
  
无论是使用私有证书还是自签名的证书时，他们需要以下项：  
  
-   用于网络位置服务器的网站证书。 证书使用者应该是网络位置服务器的 URL。  
  
-   CRL 分发点上内部网络中具有高可用性。  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>安装内部 CA 颁发的网络位置服务器证书  
  
1.  在将托管网络位置服务器网站的服务器上：上**启动**屏幕上，键入**mmc.exe**，然后按 ENTER。  
  
2.  在 MMC 控制台中的“文件”  菜单上，单击“添加/删除管理单元”  。  
  
3.  在  “添加或删除管理单元”对话框中，依次单击  “证书”、  “添加”、  “计算机帐户”、  “下一步”、  “本地计算机”和  “完成”，然后单击  “确定”。  
  
4.  在证书管理单元的控制台树中，依次打开  “证书(本地计算机)\个人\证书”。  
  
5.  右键单击**证书**，依次指向**的所有任务**，单击**申请新证书**，然后单击**下一步**两次。  
  
6.  上**申请证书**页上，选中配置证书模板中创建的证书模板的复选框，如有必要，单击**注册为此需要的详细信息证书**。  
  
7.  在  “证书属性”对话框的  “使用者”选项卡上，在  “使用者名称”区域的  “类型”中选择  “公用名”。  
  
8.  在  “值”中，输入网络位置服务器网站的 FQDN，然后单击  “添加”。  
  
9. 在  “备用名称”区域的  “类型”中，选择  “DNS”。  
  
10. 在  “值”中，输入网络位置服务器网站的 FQDN，然后单击  “添加”。  
  
11. 在  “常规”选项卡的  “友好名称”中，输入一个有助于标识证书的名称。  
  
12. 依次单击  “确定”、  “注册”和  “完成”。  
  
13. 在证书管理单元的详细信息窗格中，验证具有服务器身份验证的预期用途注册该新证书。  
  
#### <a name="to-configure-the-network-location-server"></a>要配置网络位置服务器，请执行以下操作：  
  
1.  在高可用性服务器上设置网站。 该网站不需要任何内容，但是当你对它进行测试时，你可以定义客户端进行连接时提供消息的默认页面。  
  
    如果远程访问服务器上托管网络位置服务器网站，则不需要此步骤。  
  
2.  将 HTTPS 服务器证书绑定到该网站。 该证书的公用名应与网络位置服务器网站的名称相匹配。 请确保 DirectAccess 客户端信任发证 CA。  
  
    如果远程访问服务器上托管网络位置服务器网站，则不需要此步骤。  
  
3.  设置的 CRL 站点的已获高可用性在内部网络上。  
  
    可以通过以下服务器访问 CRL 分发点：  
  
    -   使用基于 HTTP 的 URL，如下所示的 web 服务器： https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   文件服务器，如通过通用命名约定 (UNC) 路径访问\\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    如果内部的 CRL 分发点是否可仅通过 IPv6，则必须使用高级安全连接安全规则来配置 Windows 防火墙。 这免除对 IPsec 保护的 intranet 的 IPv6 地址空间中的 IPv6 地址到 CRL 分发点。  
  
4.  请确保内部网络上的 DirectAccess 客户端可以解析网络位置服务器的名称和在 Internet 上的 DirectAccess 客户端无法解析的名称。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 2：配置远程访问服务器](Step-2-Configure-the-Remote-Access-Server.md)

