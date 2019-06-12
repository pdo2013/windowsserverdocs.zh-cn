---
title: 将本地 Exchange Server 与 Windows Server Essentials 集成
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4759d33dc89c0ce458b2143cff94f78ea2d9a5cc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433396"
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>将本地 Exchange Server 与 Windows Server Essentials 集成

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南提供了一些信息和基本说明，帮助你设置运行 Exchange Server 的本地服务器并将该服务器与运行 Windows Server Essentials 的服务器进行集成。  

 尝试在 Windows Server Essentials 网络上部署运行 Exchange Server 的本地服务器之前，应当先阅读此指南。  

> [!NOTE]
>  Exchange Server 2010 不支持在运行 Windows Server 2012 的计算机上进行安装。  

## <a name="prerequisites"></a>先决条件  
 在 Windows Server Essentials 网络上安装 Exchange Server 之前，请务必完成本节所述的任务。  

-   [将运行 Windows Server Essentials 的服务器设置](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  

-   [准备在其上安装 Exchange Server 的第二个服务器](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  

-   [配置 Internet 域名](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  

###  <a name="BKMK_SetUpSBS8"></a> 将运行 Windows Server Essentials 的服务器设置  
 必须已设置好一个运行 Windows Server Essentials 的服务器。 该服务器将作为运行 Exchange Server 的服务器的域控制器。 有关如何设置 Windows Server Essentials 的信息，请参阅[安装 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)。  

###  <a name="BKMK_SecondServer"></a> 准备在其上安装 Exchange Server 的第二个服务器  
 必须在运行 Windows Server 操作系统，且该版本的操作系统支持 Exchange Server 2010 或 Exchange Server 2013 的第二个服务器上安装 Exchange Server。 必须将第二个服务器加入 Windows Server Essentials 域。  

 有关如何将第二个服务器加入到 Windows Server Essentials 域的信息，请参阅将第二个服务器加入到中的网络[获取连接](../use/Get-Connected-in-Windows-Server-Essentials.md)。  

> [!NOTE]
>  Microsoft 不支持在运行 Windows Server Essentials 的服务器上安装 Exchange Server。  

###  <a name="BKMK_DomainNames"></a> 配置 Internet 域名  
 若要将运行 Exchange Server 的本地服务器与 Windows Server Essentials 集成，你必须为你的企业注册一个有效的 Internet 域名（如 *contoso.com*）。 必须与你的域名提供商合作创建 Exchange Server 所需的 DNS 资源记录。  

 例如，如果你的公司 Internet 域名为 contoso.com，而且你希望使用 *mail.contoso.com* 的完全限定域名 (FQDN) 来引用运行 Exchange Server 的本地服务器，请与你的域名提供商合作创建下表中的 DNS 资源记录。  


| 资源记录名称 |     记录类型     |                                                                         记录设置                                                                          |                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                              |
|----------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         邮件         |      主机 (A)       |                                                        地址=*你的 ISP 分配的公用 IP 地址*                                                         |                                                                                                                                                                                                   Exchange Server 将接收的邮件地址为 mail.contoso.com。<br /><br /> 你可以自行选择其他名称。                                                                                                                                                                                                    |
|          MX          | 邮件交换器 (MX) |                                            主机名=@<br /><br /> 地址=mail.contoso.com<br /><br /> 首选项=0                                             |                                                                                                                                                                                                      提供有关的电子邮件路由email@contoso.com到达运行 Exchange Server 的本地服务器。                                                                                                                                                                                                       |
|         SPF          |     文本 (TXT)      |                                                                        v=spf1 a mx ~all                                                                         |                                                                                                                                                                                                                      避免从你的服务器发送的电子邮件被识别为垃圾邮件的资源记录。                                                                                                                                                                                                                      |
|  autodiscover._tcp   |    服务 (SRV)    | 服务：_autodiscover<br /><br /> 协议：_tcp<br /><br /> 优先级：0<br /><br /> 权重：0<br /><br /> 端口：443<br /><br /> 目标主机：mail.contoso.com | 支持 Microsoft Office Outlook 和移动设备自动发现运行 Exchange Server 的本地服务器。<br /><br /> **注意：** 此外可以配置自动发现主机 (A) 资源记录和记录指向运行 Exchange Server 的本地服务器的公共 IP 地址。 但是，如果执行此选项，则还必须提供使用者可选名称 (SAN) SSL 证书，该证书既支持 mail.contoso.com 域名，也支持 autodiscover.contoso.com 域名。 |

> [!NOTE]
>  -   将此示例中 *contoso.com* 的实例替换为已注册的 Internet 域名。  

 必须为运行 Exchange Server 的本地服务器选择一个 FQDN，该 FQDN 不得和用于运行 Windows Server Essentials 的服务器的 FQDN 相同。 例如，你可以选择使用 *remote.contoso.com* 作为此 FQDN，计算机使用该 FQDN 从 Internet 访问运行 Windows Server Essentials 的服务器。 你可以使用 *mail.contoso.com* 作为具备此用途的 FQDN：将电子邮件路由到运行 Exchange Server 的本地服务器。  

## <a name="install-exchange-server"></a>安装 Exchange Server  
 Windows Server Essentials 上的 Exchange Server 集成功能支持下面版本的 Exchange Server：  

- Exchange Server 2013  

- 带 Service Pack 1 (SP1) 的 Exchange Server 2010  

  在第二个服务器上安装 Exchange Server 之前，必须首先向 **Enterprise Admins** 组添加当前的管理员帐户。  

#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>向 Enterprise Admins 组添加当前管理员帐户的步骤  

1.  以管理员身份登录到 Windows Server Essentials。  

2.  以管理员身份运行 Windows PowerShell。  

3.  在 Windows PowerShell 命令提示符下键入**Add-adgroupmember"企业管理员"$env： 用户名**，然后按 Enter。  

#### <a name="to-install-exchange-server"></a>安装 Exchange Server 的步骤  

1.  以管理员身份登录到第二个服务器。  

2.  打开 Internet 浏览器，然后导航到 [Exchange Server 部署助手](https://go.microsoft.com/fwlink/p/?LinkID=249163) 网站。  

3.  单击“On-Premises Only”  。  

4.  单击要安装的 Exchange Server 版本的新安装选项。  

    > [!NOTE]
    >  如果你从 Windows Small Business Server 安装进行迁移，应当选择包含迁移步骤的相应升级选项。  

5.  在下一页上，接受默认设置，然后单击“下一步”  。  

    > [!NOTE]
    >  如果你计划在新安装 Exchange Server 时使用公用文件夹，请将此设置更改为“是”  。  

6.  按照清单中的分步说明部署 Exchange Server。  

     通过 Exchange Server 部署助手也可以：  

    -   打印清单副本。  

    -   将清单副本发送给电子邮件收件人。  

    -   下载此清单为 PDF 文件。  

> [!NOTE]
> - 在运行 Exchange Server 的服务器上必须始终选择安装管理工具。 在 Windows Server Essentials 上，管理工具是 Exchange Server 集成功能所必需的。  
>   -   如果要配置虚拟目录，建议你将每个虚拟目录的 **InternalUrl** 属性设置为与 **ExternalUrl** 属性一样，都使用同样的 URL。 有关更多信息，请参阅 Exchange Server 2010 在线帮助网站上的 [管理客户端访问服务器虚拟目录](https://go.microsoft.com/fwlink/p/?LinkId=251058) 。  
>   -   如果你希望从 Windows Server Essentials 上的远程 Web 访问站点内访问 Outlook Web Access (OWA)，则必须设置 OWA 的外部 URL 属性。  

 如果以全新方式安装 Exchange Server 2010，也可以使用下面的脚本设置 Exchange Server。  

#### <a name="to-use-scripts-to-set-up-exchange-server"></a>使用脚本设置 Exchange Server 的步骤  

1.  打开记事本，并将以下脚本粘贴到新文件：  

```powershell
Import-Module ServerManager

Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  Restart
```

2. 将该文件另存为 **InstallDependencies.ps1**。  

3. 将 Exchange SSL 证书复制到服务器上的某个位置。  

4. 打开一个新的文本文件，将下列文本复制到此文件：  

```powershell
param (
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]
    $CertPath = "c:\certificates\ExchangeCertificate.pfx",
    [Security.SecureString]
    [Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]
    $CertPassword = $null,
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]
    $DomainName = "contoso.com",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]
    $ServerIpAddress = "192.168.0.1",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]
    $InternalIpRange = "192.168.0.0-192.168.0.255"
)

#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.

Import-ExchangeCertificate  -FileData ([Byte[]]$(Get-content -Path $CertPath  -Encoding byte  -ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force

#New AcceptedDomain and set it to default

New-AcceptedDomain  -Name "official name"  -DomainName $domainname

Set-AcceptedDomain  -Identity "official name"  -MakeDefault $true

#New EmailAddress Policy

$address = "%m@" + $DomainName

New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address

#Set owa and ecp VirtualDirectory ExternalUrl

$hostname = "mail." + $DomainName

$owa = "https://" + $hostname + "/owa"

$ecp = "https://" + $hostname + "/ecp"

$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"

$oab = "https://" + $hostname + "/OAB"

$ews = "https://" + $hostname + "/EWS/Exchange.asmx"

Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  -ExternalUrl $owa  -InternalUrl $owa

Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  -ExternalUrl $ecp  -InternalUrl $ecp

Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  -InternalUrl $activesync

Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true

Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force

#Enable outlook Anywhere

Enable-OutlookAnywhere  -ClientAuthenticationMethod:Basic  -ExternalHostname:$hostname  -SSLOffloading:$false

#new receive/send connector

$machinename = get-content env:computername

$bindingIpaddress = $ServerIpAddress + ":25"

$ReceiveConnectorName = $machinename + "\Default " + $machinename

Set-ReceiveConnector $ReceiveConnectorName -RemoteIPRanges $InternalIpRange

New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated

New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename
```

5. 设置脚本开头的参数以反映你的网络环境。  

6. 将文件另存为 **ConfigureExchange.ps1**。  

7. 以管理员身份运行 Windows PowerShell。  

8. 在 Windows PowerShell 命令提示符下，键入 **Set-ExecutionPolicy RemoteSigned**，然后按 Enter 键。  

9. 运行脚本 **InstallDependencies.ps1**。  

10. 重新启动服务器，然后以管理员身份运行 Windows PowerShell。  

11. 在 Windows PowerShell 命令提示符下，运行下面的脚本：  

    `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  

   > [!NOTE]
   >  请务必键入 Exchange Server 安装程序的正确路径。  

12. 完成 Exchange Server 设置后，请以管理员身份打开 Exchange 命令行管理程序。  

13. 在 Exchange 命令行管理程序的命令提示符下，键入 **Set-ExecutionPolicy RemoteSigned**，然后按 Enter 键。  

14. 运行脚本 **ConfigureExchange.ps1**。  

15. 重新启动服务器。  

> [!NOTE]
>  如果您决定使用公开受信任的 SSL 证书，而不是自行颁发的证书，可以按照安装指南创建的证书请求并将其发送到所选的证书颁发机构中的说明进行操作。 你也可以使用 Exchange PowerShell cmdlet 创建一个证书请求。 下面是相关示例。  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  自定义脚本参数以反映你的网络环境。  

## <a name="post-installation-tasks"></a>安装后任务  
 本节描述了在安装后阶段你可能需要完成的服务器配置任务，该阶段包含了在 Windows Server Essentials 网络上设置运行 Exchange Server 的本地服务器的特定信息。  

### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>添加公用电子邮件域并配置电子邮件地址策略  

> [!NOTE]
>  执行全新安装时，此步骤是必需的。 如果你是从 Windows Small Business Server 进行迁移，请跳过此步骤。  

 你必须将你的电子邮件域指定为默认接受的域，然后配置电子邮件地址策略。  

##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>将你的电子邮件域添加为默认接受的域的步骤  

1.  按照 Exchange Server 文章 [创建接受的域](https://go.microsoft.com/fwlink/p/?LinkId=249174) 中的说明来添加一个接受的域。  

2.  以管理员身份登录到第二个服务器，打开 Exchange 管理控制台，然后导航到“组织配置”  的“集线器传输”  选项卡。  

3.  在“Exchange 管理控制台”工作窗格中，右键单击新接受的域名，然后单击“设置为默认”  。  

4.  按照 Exchange Server 文章 [创建电子邮件地址策略](https://go.microsoft.com/fwlink/p/?LinkId=249179) 中的说明来创建一个新的电子邮件地址策略。 你可以接受除了该电子邮件地址之外的所有默认值。 为电子邮件地址指定你的公用电子邮件域。  

### <a name="create-smtp-send-and-receive-connectors"></a>创建 SMTP 发送和接收连接器  

> [!NOTE]
>  这是一个必须完成的任务。  

 必须配置一个 SMTP 发送连接器和一个 SMTP 接收连接器来对电子邮件进行出站/入站传输。  

 若要创建一个 SMTP 发送连接器，请按照 Exchange Server 文章 [创建 SMTP 发送连接器](https://technet.microsoft.com/library/aa997285.aspx)中的说明进行操作。  

 若要创建一个 SMTP 接收连接器，请按照 Exchange Server 文章 [创建 SMTP 接收连接器](https://technet.microsoft.com/library/bb125159.aspx)中的说明进行操作。  

 你可以选择参考此文档中之前所述的内容来使用 Exchange Powershell cmdlet 创建发送和接收连接器。  

### <a name="configure-the-network-router"></a>配置网络路由器  

> [!NOTE]
>  执行全新安装时，此步骤是必需的。 如果是从 Windows Small Business Server 进行迁移，请参阅 [Migrate Server Data to Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) 来获取有关如何配置网络的说明。  

 至少必须在路由器上配置如下端口设置：  

|路由器端口|目标 IP|目标端口|注意|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|运行 Exchange Server 的本地服务器的内部 IP。|25||  
|80 (HTTP)|运行 Windows Server Essentials 的服务器的内部 IP|80||  
|443 (HTTPS)|运行 Windows Server Essentials 的服务器的内部 IP|443||  

 如果你的网络支持 POP3 或 IMAP 邮件传输协议，则还必须为这些协议配置端口转发。 有关相关信息，请参阅 Exchange Server TechNet 库中 **Exchange 网络端口引用** 主题的 [客户端访问服务器](https://go.microsoft.com/fwlink/p/?LinkId=250773) 一节。  

> [!NOTE]
> - 我们建议你为运行 Windows Server Essentials 的服务器和运行 Exchange Server 的第二个服务器配置静态 IP 地址。 有关如何在运行 Windows Server 2003 或 Windows Server 2008 R2 的计算机上配置静态 IP 地址的说明，请参阅 Windows Server TechNet 库中的 [配置静态 IP 地址](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx)  
> 
>   **注意：** DNS 服务器设置应当始终指向运行 Windows Server Essentials 的服务器的 IP 地址。  
>   -   在路由器上，确保运行 Windows Server Essentials 的服务器和运行 Exchange Server 的服务器的 IP 地址得到保留，或超出 DHCP IP 地址的范围。  
>   -   此部分中的路由器配置假设你只有一个由 Internet 服务提供商 (ISP) 分配的公用 IP 地址。 如果有多个公用 IP 地址，则可以为运行 Windows Server Essentials 的服务器和运行 Exchange Server 的服务器分配其他 IP 地址，然后根据这些公用 IP 地址创建端口转发。  

### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>在 Windows Server Essentials 上启用本地 Exchange Server 集成  

> [!NOTE]
>  如果是从 Windows Small Business Server 安装进行迁移，建议你暂时跳过此步骤，等卸载源服务器上之前安装的 Exchange Server 后再执行此步骤。  

 安装并配置好运行 Exchange Server 的服务器之后，必须在运行 Windows Server Essentials 的服务器上启用本地 Exchange Server 集成。  

##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>通过仪表板启用本地 Exchange Server 集成  

1.  以管理员身份登录到运行 Windows Server Essentials 的服务器，然后打开仪表板。  

2.  在“主页”  页面上，单击“连接到我的电子邮件服务”  ，然后单击“集成你的 Exchange Server”  。  

3.  在信息窗格中，单击“设置 Exchange Server 集成”  。  

4.  按照向导中的说明进行操作。  

### <a name="configure-a-reverse-proxy"></a>配置反向代理  

> [!NOTE]
>  如果你的 Internet 服务提供商只提供了一个 Internet 连接，那么此任务是必需的。  

 Windows Server Essentials 和 Exchange Server 均支持某些网络用户远程访问方案。 例如，如果你在运行 Windows Server Essentials 的服务器上启用随处访问，则可以远程访问远程 Web 访问站点，或使用虚拟专用网络 (VPN) 来远程连接到 Windows Server Essentials 网络。 要远程访问电子邮件，必须使用 Outlook Anywhere、Outlook Web Access (OWA) 或 ActiveSync。  

 如果 Windows Server Essentials 和运行 Exchange Server 的服务器均连接到同一个路由器，而且 Internet 服务提供商只提供了一个与路由器相连的入站 Internet 连接，则必须使用反向代理解决方案来根据目标主机名从 Internet 路由不同类型的远程访问请求。 建议你使用支持 Microsoft 的 IIS 应用程序请求路由 (ARR) 扩展作为你的反向代理解决方案。 有关 IIS 应用程序请求路由的详细信息，请访问 [应用程序请求路由网站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。  

##### <a name="to-install-and-configure-application-request-routing"></a>安装和配置应用程序请求路由的步骤  

1. 以管理员身份登录到 Windows Server Essentials。  

2. 打开 Internet 浏览器，然后导航到 [应用程序请求路由网站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。  

3. 在 ARR 网站上单击“安装”  ，然后按照相关说明安装 ARR。  

   > [!NOTE]
   >  安装 ARR 的过程中必须选择 URL 重写模块。  
   >   
   >  在 ARR 安装结束时，你可能会收到一个错误，告知 ARR 2.5 的 KB 2589179 未成功安装。 你可以安全地忽略此错误。  

4. ARR 安装完成后，如果“远程桌面网关”  服务未运行，请重新启动该服务。  

   > [!NOTE]
   >  完成 ARR 的安装后，“远程桌面网关”  服务可能会停止。 若要手动重新启动该服务，请打开“服务”  管理工具，然后重新启动“远程桌面网关”  服务。  

5. [下载 ARR 2.5 的 KB2732764](https://go.microsoft.com/fwlink/?LinkID=258302)，然后在运行 Windows Server Essentials 的服务器上安装此更新。  

6. 将 Exchange Server 的 SSL 证书文件复制到运行 Windows Server Essentials 的服务器。 该证书文件必须包含私钥，且必须为 PFX 文件格式。  

   > [!NOTE]
   >  如果使用自颁发证书，请按照 Exchange Server 文章 [导出 Exchange 证书](https://technet.microsoft.com/library/dd351274.aspx) 中的说明导出该证书。  

7. 根据你所运行的 Windows Server Essentials 版本，执行以下任一操作：  

   -   在 Windows Server Essentials:以管理员身份打开命令窗口，然后打开 %ProgramFiles%\Windows Server\Bin 目录  

   -   在 Windows Server Essentials:以管理员身份打开命令窗口，然后打开 %Windir%\System32\Essentials 目录。  

8. 根据你的安装方案，按照以下步骤之一配置 ARR：  

   - 如果执行全新安装，请运行以下命令：  

      **ARRConfig config-cert** *证书文件路径* **-主机名***托管 Exchange 服务器的名称* ****  

     > [!NOTE]
     >  例如，* * ARRConfig config-cert ***c:\temp\certificate.pfx*** -主机名 ***mail.contoso.com***  
     > 
     >  将 *mail.contoso.com* 替换为受此证书保护的域的名称。  

   - 如果是从 Windows Small Business Server 进行迁移，请运行以下命令：  

      **ARRConfig config-cert** *证书文件路径* **-主机名***托管 Exchange 服务器的名称* **-targetserver** *Exchange Server 的服务器名称* ****  

      For example; **ARRConfig config  -cert ***c:\temp\certificate.pfx***  -hostnames ***mail.contoso.com*** -targetserver ***ExchangeSvr*****  

      将 *mail.contoso.com* 替换为你的域名。 将 *ExchangeSvr* 替换为运行 Exchange Server 的服务器的名称。  

9. 出现提示时，请键入该证书的密码。  

> [!NOTE]
> - 提供的主机名必须包含在为 Exchange Server 购买的 SSL 证书中。  
>   -   如果你有多个主机名，请使用逗号 (,) 将它们分开。  

 若要验证配置是否正常工作，请尝试访问运行 Exchange Server 的服务器的 OWA 网站 (https://mail。 *yourdomainname*.com/owa)。 若要对连接问题进行故障排除也可以使用在线 [Microsoft 远程连接分析器](https://go.microsoft.com/fwlink/p/?LinkId=249455) 工具。  

### <a name="configure-split-dns-for-exchange-server"></a>为 Exchange Server 配置拆分式 DNS  

> [!NOTE]
>  此为建议任务。  

 通过拆分式 DNS 可以为同一主机名配置不同的 DNS IP 地址，这取决于 DNS 的请求源。 如果客户端计算机位于 Intranet 中，那么 DNS 请求会解析到某个 Intranet IP 地址。 如果客户端计算机位于 Internet 中，那么 DNS 请求会解析到某个 Internet IP 地址。 这对用户是透明的。  

 我们建议你配置拆分式 DNS 的方式能够让用户无论在什么位置都可以始终使用同一主机名访问 Exchange Server 服务。  

##### <a name="to-configure-split-dns-for-exchange-server"></a>为 Exchange Server 配置拆分式 DNS 的步骤  

1.  以管理员身份登录到 Windows Server Essentials，然后打开 DNS 管理器。  

2.  在 DNS 管理器控制台树中，右键单击你的服务器，然后单击“新建区域”  。 此时将打开“新建区域向导”  。  

3.  在该向导的“区域类型”  页面上，接受默认选项，然后单击“下一步”  。  

4.  在“Active Directory 区域传送作用域”  页面上，接受默认选项，然后单击“下一步”  。  

5.  在“正向或反向查找区域”  页面上，接受或选择“正向查找区域”  ，然后单击“下一步”  。  

6.  上**区域名称**页上，键入运行 Exchange Server （例如，在服务器的 FQDN*mail.contoso.com*)，然后单击**下一步**。  

7.  在“动态更新”  页上，接受默认选项，单击“下一步”  ，然后单击“完成”  。  

8.  在 DNS 管理器控制台树中，右键单击“新建向前查找区域”，然后单击 **“新建主机 (A 或 AAAA)”** 。  

9. 在“新建主机”  页面上，将“名称”  字段留空，键入运行 Exchange Server 的服务器的 Intranet IP 地址，然后单击“添加主机”  。  

    > [!NOTE]
    >  将“名称”  字段留空后，服务器默认将使用父域名。  

10. 在“新建主机”  页面上，单击“完成”  。  

> [!NOTE]
>  如果使用 ActiveSync 但无法同步某些邮箱帐户的电子邮件，请确定这些帐户是否为一个或多个受保护组（如 Domain Administrators）的成员。 有关可帮助你解决此问题的相关信息，请参阅 [Exchange ActiveSync 返回 HTTP 500 错误](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx)。  

## <a name="related-topics"></a>相关主题  
 有关集成本地 Exchange Server 的详细信息，请参阅以下部分。  

### <a name="what-happens-if-i-disable-exchange-integration"></a>如果禁用 Exchange 集成将会出现什么情况？  
 如果禁用与本地 Exchange Server 的集成，你将不再能够使用 Windows Server Essentials 仪表板来查看、创建或管理 Exchange Server 邮箱。  

### <a name="what-do-i-need-to-know-about-email-accounts"></a>关于电子邮件帐户，我需要知道哪些信息？  
 在服务器上配置托管电子邮件解决方案。 从托管的电子邮件提供程序，如 Microsoft Office 365 解决方案可以为网络用户提供个人电子邮件帐户。 当你在 Windows Server Essentials 中运行“添加用户帐户”向导来创建用户帐户时，该向导会尝试将用户帐户添加到可用的托管电子邮件解决方案。 同时，该向导将向用户分配电子邮件名称（别名），并设置邮箱的最大大小（配额）。 邮箱的最大大小根据使用的电子邮件提供商的不同而有所不同。 添加用户帐户之后，可以继续从用户的属性页管理邮箱别名和配额信息。 若要完全管理用户帐户和托管电子邮件提供商，请使用托管提供商的管理控制台。 可以从基于 Web 的门户，或从服务器仪表板中的选项卡访问提供商的管理控制台，具体取决于你的提供商。  

 在运行“添加用户帐户”向导时所提供的别名，将作为用户别名的建议名称发送给托管电子邮件提供商。 例如，如果用户别名为*FrankM*，可能是用户的电子邮件地址<em>FrankM@Contoso.com</em>。  

 此外，在“添加用户帐户”向导中为用户设置的密码将是托管电子邮件解决方案中的用户初始密码。  

 最后，如果在服务器上通过使用“删除用户帐户”向导删除用户，则该向导还会向托管电子邮件提供商发送一个有关还要从其系统中删除该用户的请求。 提供程序可能会删除用户的帐户以及与帐户相关联的电子邮件。  

 有关如何设置所需的电子邮件客户端软件或如何访问电子邮件帐户的用户信息，请参阅托管电子邮件提供商提供的帮助文档。  

### <a name="what-is-a-mailbox-quota"></a>什么是邮箱配额？  
 分配并供网络用户的 Exchange 邮箱数据的存储空间量称为邮箱配额。  

 当你在仪表板上运行“设置 Exchange Server 集成”  任务时，该向导将向“添加用户帐户”向导添加一个页面，以允许你选择是否开启邮箱配额限制以及是否指定配额大小。 默认情况下，“开启邮箱配额限制”  选项处于选中状态（打开），并且向用户邮箱分配了 2 GB 的存储空间。 Exchange 管理员可以自定义邮箱配额设置以满足其业务的需求。  

## <a name="see-also"></a>请参阅  

-   [Windows Server Essentials 的系统要求](../get-started/system-requirements.md)  

-   [管理电子邮件服务集成](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
