---
title: 步骤1配置基本 DirectAccess 基础结构
description: 本主题是使用 Windows Server 2016 的入门向导部署单个 DirectAccess 服务器指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba4de2a4-f237-4b14-a8a7-0b06bfcd89ad
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2cd84949dddf75730aca6302f1244f784b5933d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388572"
---
# <a name="step-1-configure-the-basic-directaccess-infrastructure"></a>步骤1配置基本 DirectAccess 基础结构

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍了如何在混合的 IPv4 和 IPv6 环境中配置基本 DirectAccess 部署所需的基础结构，该部署使用单台 DirectAccess 服务器。 在开始执行部署步骤之前，请确保已完成[规划基本 DirectAccess 部署](../../../remote-access/directaccess/single-server-wizard/Plan-a-Basic-DirectAccess-Deployment.md)中所述的规划步骤。  
  
|任务|描述|  
|----|--------|  
|配置服务器网络设置|配置 DirectAccess 服务器上的服务器网络设置。|  
|配置企业网络中的路由|配置企业网络中的路由以确保正确地路由通信。|  
|配置防火墙|根据需要配置其他防火墙。|  
|配置 DNS 服务器|为 DirectAccess 服务器配置 DNS 设置。|  
|配置 Active Directory|将客户端计算机和 DirectAccess 服务器加入到 Active Directory 域。|  
|配置 GPO|如果必要，为部署配置 GPO。|  
|配置安全组|配置将包含 DirectAccess 客户端计算机的安全组，以及部署中所需的任何其他安全组。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="ConfigNetworkSettings"></a>配置服务器网络设置  
在使用 IPv4 和 IPv6 的环境中部署单一服务器需要下面的网络接口设置。 可使用“Windows 网络和共享中心”中的“更改适配器设置”配置所有 IP 地址。  
  
-   边缘拓扑  
  
    -   一个面向 Internet 的公用静态 IPv4 或 IPv6 地址。  
  
        > [!NOTE]  
        > Teredo 需要两个连续的公用 IPv4 地址。 如果你不使用 Teredo，则可以配置单个公用静态 IPv4 地址。  
  
    -   单个内部静态 IPv4 或 IPv6 地址。  
  
-   在 NAT 设备后面（两个网络适配器）  
  
    -   单个面向内部网络的静态 IPv4 或 IPv6 地址。  
  
    -   单个面向外围网络的静态 IPv4 或 IPv6 地址。  
  
-   在 NAT 设备后面（一个网络适配器）  
  
    -   单个静态 IPv4 或 IPv6 地址。  
  
> [!NOTE]  
> 如果 DirectAccess 服务器具有两个或更多网络适配器（一个归类于域配置文件，另一个归类于公用/专用配置文件），但要使用单个 NIC 拓扑，则推荐是：  
>   
> 1.  确保第二个 NIC 和任何其他 NIC 也归类于域配置文件。  
> 2.  如果出于任何原因，不能为域配置文件配置第二个 NIC，则必须使用以下 Windows PowerShell 命令手动将 DirectAccess IPsec 策略的作用域覆盖到所有配置文件：  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
>   
>     IPsec 策略的名称是 DirectAccess-DaServerToInfra 和 DirectAccess-DaServerToCorp。  
  
## <a name="ConfigRouting"></a>在企业网络中配置路由  
在企业网络中配置路由，如下所示：  
  
-   在组织中部署本机 IPv6 时，添加一个路由，以便内部网络上的路由器通过远程访问服务器将 IPv6 通信路由回来。  
  
-   在远程访问服务器上手动配置组织 IPv4 和 IPv6 路由。 添加已发布的路由，以便将所有具有组织 (/48) IPv6 前缀的通信都转发到内部网络。 此外，对于 IPv4 通信，请添加显式路由，以便将 IPv4 通信转发到内部网络。  
  
## <a name="ConfigFirewalls"></a>配置防火墙  
在部署中使用其它防火墙的情况下，当远程访问服务器位于 IPv4 Internet 上时，应用远程访问通信的以下面向 Internet 的防火墙例外情况：  
  
-   6to4 流量-IP 协议41入站和出站。  
  
-   Ip-https-传输控制协议（TCP）目标端口443和 TCP 源端口443出站。 当远程访问服务器配有单一网络适配器，且网络位置服务器位于远程访问服务器上时，还需要 TCP 端口 62000。  
  
    > [!NOTE]  
    > 必须在远程访问服务器上配置免除。 必须在边缘防火墙上配置所有其他免除。  
  
> [!NOTE]  
> 对于 Teredo 和 6to4 通信，这些例外应适用于远程访问服务器上两个面向 Internet 的连续公用 IPv4 地址。 对于 IP-HTTPS，仅需将例外情况应用于外部服务器名称解析为的地址。  
  
在使用其它防火墙的情况下，当远程访问服务器位于 IPv6 Internet 上时，应用远程访问通信的以下面向 Internet 的防火墙例外：  
  
-   IP 协议 50  
  
-   UDP 目标端口 500 入站，以及 UDP 源端口 500 出站。  
  
当使用其它防火墙时，应用远程访问通信的以下内部网络防火墙例外情况：  
  
-   ISATAP-协议41入站和出站  
  
-   所有 IPv4/IPv6 通信的 TCP/UDP  
  
## <a name="ConfigDNS"></a>配置 DNS 服务器  
你必须为部署中的内部网络手动配置用于网络位置服务器网站的 DNS 条目。  
  
### <a name="NLS_DNS"></a>创建网络位置服务器和 NCSI 探测 DNS 记录  
  
1.  在内部网络 DNS 服务器上，运行 " **dnsmgmt.msc** "，然后按 enter。  
  
2.  在“DNS 管理器”控制台的左窗格中，展开域的前向查找区域。 右键单击该域，然后单击“新建主机(A 或 AAAA)”。  
  
3.  在“新主机”对话框的“名称(如果为空则使用父域名)”框中，输入网络位置服务器网站的 DNS 名称（这是 DirectAccess 客户端用于连接到网络位置服务器的名称）。 在“IP 地址”框中，输入网络位置服务器的 IPv4 地址，然后单击“添加主机”。 在“DNS”对话框中，单击“确定”。  
  
4.  在“新主机”对话框的“名称(如果为空则使用父域名)”框中，输入 Web 探测的 DNS 名称（默认 Web 探测的名称为 directaccess-webprobehost）。 在“IP 地址”框中，输入 Web 探测的 IPv4 地址，然后单击“添加主机”。 为 directaccess corpconnectivityhost 和任何手动创建的连接性验证程序重复此过程。 在“DNS”对话框中，单击“确定”。  
  
5.  单击 **“完成”** 。  
  
@no__t 0Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
还必须为以下内容配置 DNS 条目：  
  
-   Ip-https**服务器**-DirectAccess 客户端必须能够从 Internet 解析远程访问服务器的 DNS 名称。  
  
-   **CRL 吊销检查**-directaccess 使用证书吊销检查来检查 directaccess 客户端与远程访问服务器之间的 ip-https 连接，以及 directaccess 客户端和网络之间基于 HTTPS 的连接的证书吊销检查位置服务器。 在这两种情况下，DirectAccess 客户端都必须能够解析和访问 CRL 分发点位置。  
  
## <a name="ConfigAD"></a>配置 Active Directory  
必须将远程访问服务器和所有 DirectAccess 客户端计算机都加入 Active Directory 域。 DirectAccess 客户端计算机必须是以下域类型之一的成员：  
  
-   与远程访问服务器属于同一林的域。  
  
-   属于与远程访问服务器林具有双向信任关系的林的域。  
  
-   与远程访问服务器域具有双向域信任的域。  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>将远程访问服务器加入域  
  
1.  在服务器管理器中，单击 **“本地服务器”** 。 在详细信息窗格中，单击“计算机名”旁边的链接。  
  
2.  在“系统属性” 对话框中，单击“计算机名” 选项卡。在“计算机名” 选项卡上，单击“更改”。  
  
3.  如果在将服务器加入域时还要更改计算机名，请在“计算机名”中键入计算机的名称。 在“隶属于”下面单击“域”，键入服务器要加入到的域的名称（例如 corp.contoso.com），然后单击“确定”。  
  
4.  当系统提示你输入用户名和密码时，请输入有权将计算机加入域的用户的用户名和密码，然后单击“确定”。  
  
5.  当你看到欢迎你进入域的对话框时，请单击“确定”。  
  
6.  当系统提示你必须重新启动计算机时，请单击“确定”。  
  
7.  在“系统属性” 对话框中单击“关闭”。  
  
8.  当系统提示你重新启动计算机时，请单击“立即重新启动”。  
  
#### <a name="to-join-client-computers-to-the-domain"></a>将客户端计算机加入域  
  
1.  运行**资源管理器。**  
  
2.  右键单击计算机图标，然后单击“属性”。  
  
3.  在“系统”页上，单击“高级系统设置”。  
  
4.  在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
5.  如果在将服务器加入域时还要更改计算机名，请在“计算机名”中键入计算机的名称。 在“隶属于”下面单击“域”，键入服务器要加入到的域的名称（例如 corp.contoso.com），然后单击“确定”。  
  
6.  当系统提示你输入用户名和密码时，请输入有权将计算机加入域的用户的用户名和密码，然后单击“确定”。  
  
7.  当你看到欢迎你进入域的对话框时，请单击“确定”。  
  
8.  当系统提示你必须重新启动计算机时，请单击“确定”。  
  
9. 在“系统属性”对话框中单击“关闭”。 出现提示时单击“立即重新启动”。  
  
@no__t 0Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
请注意，输入下面的 Add-Computer 命令后，必须提供域凭据。  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>配置 Gpo  
若要部署远程访问，需要至少两个组策略对象：一个组策略对象包含远程访问服务器的设置，另一个包含 DirectAccess 客户端计算机的设置。 配置远程访问时，向导将自动创建所需的组策略对象。 但是，如果你的组织强制使用命名约定，或者你没有创建或编辑组策略对象所需的权限，则必须在配置远程访问之前创建它们。  
  
若要创建组策略对象，请参阅[创建和编辑组策略对象](https://technet.microsoft.com/library/cc754740.aspx)。  
  
> [!IMPORTANT]  
> 管理员可以使用以下步骤手动将 DirectAccess 组策略对象链接到组织单位：  
>   
> 1.  在配置 DirectAccess 之前，请将已创建的 GPO 链接到各自的组织单位。  
> 2.  要配置 DirectAccess，请为客户端计算机指定安全组。  
> 3.  管理员不一定具有将组策略对象链接到域的权限。 在任一情况下，都将自动配置组策略对象。 如果已将 GPO 链接到 OU，则将不会删除这些链接。 也不会将 GPO 链接到域。 对于服务器 GPO，OU 必须包含该服务器计算机对象，否则该 GPO 将链接到域的根。  
> 4.  如果在运行 DirectAccess 向导之前尚未链接到 OU，则配置完成后，管理员可以将 DirectAccess 组策略对象链接到所需的组织单位。 可以删除指向域的链接。 可在[此处](https://technet.microsoft.com/library/cc732979.aspx)找到将组策略对象链接到组织单位的步骤  
  
> [!NOTE]  
> 如果手动创建了组策略对象，则在 DirectAccess 配置过程中可能无法使用组策略对象。 可能未将组策略对象复制到最接近管理计算机的域控制器。 在这种情况下，管理员可以等待复制完成，或者强制进行复制。  
  
## <a name="ConfigSGs"></a>配置安全组  
客户端计算机组策略对象中包含的 DirectAccess 设置仅应用于配置远程访问时指定的安全组成员的计算机。  
  
### <a name="Sec_Group"></a>为 DirectAccess 客户端创建安全组  
  
1.  运行**dsa.msc**。 在“Active Directory 用户和计算机”控制台的左窗格中，展开将包含安全组的域，右键单击“用户”，指向“新建”，然后单击“组”。  
  
2.  在“新建对象 – 组”对话框中的“组名”下，输入该安全组的名称。  
  
3.  在“组范围”下单击“全局”，在“组类型”下单击“安全”，然后单击“确定”。  
  
4.  双击 DirectAccess 客户端计算机安全组，然后在属性对话框中，单击“成员”选项卡。  
  
5.  在“成员” 选项卡上，单击“添加”。  
  
6.  在“选择用户、联系人、计算机或服务帐户”对话框中，选择你希望为 DirectAccess 启用的客户端计算机，然后单击“确定”。  
  
@no__t 0Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**Windows powershell 等效命令**  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_Links"></a>下一步  
  
-   [步骤 2：配置基础 DirectAccess 服务器](da-basic-configure-s2-server.md)  
  


