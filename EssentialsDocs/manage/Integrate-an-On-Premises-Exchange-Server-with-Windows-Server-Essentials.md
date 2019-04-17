---
title: "与 Windows Server Essentials 集成本地 Exchange Server"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: c2020e08b94800a9750f095a2f772afb14ba5f0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>与 Windows Server Essentials 集成本地 Exchange Server

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南提供信息和基本的说明进行操作，以帮助你设置和集成运行的服务器上运行的 Windows Server Essentials Exchange Server 本地服务器。  
  
 您应该尝试部署运行 Windows Server Essentials 网络的 Exchange Server 本地服务器之前阅读本指南。  
  
> [!NOTE]
>  Exchange Server 2010 不支持在运行 Windows Server 2012 的计算机上安装。  
  
## <a name="prerequisites"></a>先决条件  
 在安装之前 Exchange 服务器在 Windows Server Essentials 网络上，确保你完成的任务，在此部分中所述。  
  
-   [运行的 Windows Server Essentials 服务器设置](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  
  
-   [准备在其上安装 Exchange 服务器的第二个服务器](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  
  
-   [将配置你的 Internet 域名](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  
  
###  <a name="BKMK_SetUpSBS8"></a>运行的 Windows Server Essentials 服务器设置  
 你必须已经安装了 Windows Server Essentials 运行的服务器。 这将是域控制器 Exchange Server 运行的服务器。 有关如何设置 Windows Server Essentials 的信息，请参阅[安装 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_SecondServer"></a>准备在其上安装 Exchange 服务器的第二个服务器  
 在运行的版本正式支持 Exchange Server 2010 中运行的 Windows Server 操作系统的第二个服务器或 Exchange Server 2013 上必须安装 Exchange Server。 然后，你必须到 Windows Server Essentials 域加入第二个服务器。  
  
 有关如何加入 Windows Server Essentials 域第二个服务器信息，请参阅加入到网络中的第二个服务器[连接](../use/Get-Connected-in-Windows-Server-Essentials.md)。  
  
> [!NOTE]
>  Microsoft 不支持安装在运行 Windows Server Essentials 服务器上的 Exchange Server。  
  
###  <a name="BKMK_DomainNames"></a>将配置你的 Internet 域名  
 集成与 Windows Server Essentials 运行 Exchange Server 本地服务器，你必须已注册有效 Internet 域名适用于你的企业 (如*contoso.com*)。 你必须处理域名称提供程序创建 Exchange Server 需要 DNS 资源记录。  
  
 例如，如果你的公司 Internet 域名 contoso.com，并且你想要使用的完整的域名 (FQDN) 的*mail.contoso.com*引用运行 Exchange Server 你本地服务器，适用于你的域名称提供商，在下表中创建 DNS 资源记录。  
  
|资源记录名称|记录类型|录制设置|描述|  
|--------------------------|-----------------|--------------------|-----------------|  
|邮件|主机 (A)|地址 =*公共分配 isp 的 IP 地址*|Exchange Server 将收到邮件发送给 mail.contoso.com。<br /><br /> 你可以在自己的选定使用不同的名称。|  
|MX|邮件器 (MX)|主机 = @<br /><br /> 地址 = mail.contoso.com<br /><br /> 偏好随意选择 = 0|为提供了电子邮件路由email@contoso.com到达本地服务器 Exchange Server 运行。|  
|SPF|文本 （文本文件）|v = spf1 mx ~ 所有|资源记录有助于防止从作为被标识为垃圾邮件服务器发送的电子邮件。|  
|autodiscover._tcp|服务 (SRV)|服务： _autodiscover<br /><br /> 协议： _tcp<br /><br /> 优先级： 0<br /><br /> 体重： 0<br /><br /> 端口： 443<br /><br /> 目标主机： mail.contoso.com|启用自动发现本地服务器 Exchange Server 运行的 Microsoft Office Outlook 和移动设备。<br /><br /> **注意：**还可以将自动发现 (A) 主机资源记录配置，然后记录指向运行 Exchange Server 你本地服务器的公用 IP 地址。 但是，如果实现此选项时，您还必须提供支持 mail.contoso.com 和 autodiscover.contoso.com 域名主题替代名称 (SAN) SSL 证书。|  
  
> [!NOTE]
>  -   更换的实例*contoso.com*在此示例中与你注册 Internet 域名。  
  
 你必须为你本地运行的服务器，您使用适用于运行的 Windows Server Essentials 服务器 FQDN 比 Exchange Server 选择不同 FQDN。 例如，你可以选择使用*remote.contoso.com*作为 FQDN 计算机用于访问 Internet 从运行 Windows Server Essentials 的服务器。 你可以使用*mail.contoso.com*用于将电子邮件发送给你本地运行的服务器，Exchange Server FQDN 作为。  
  
## <a name="install-exchange-server"></a>安装 Exchange Server  
 Windows Server Essentials 上的 Exchange Server 集成功能支持 Exchange Server 以下版本：  
  
-   Exchange Server 2013  
  
-   Exchange 服务器 2010 Service pack 1 (SP1)  
  
 在安装在第二个服务器上的 Exchange Server 之前，必须首先将添加到当前的管理员帐户**企业管理员**组。  
  
#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>若要添加对企业管理员组当前的管理员帐户  
  
1.  以管理员身份登录到 Windows Server Essentials。  
  
2.  以管理员身份运行的 Windows PowerShell。  
  
3.  在 Windows PowerShell 命令提示符下，键入**添加 ADGroupMember ˜Enterprise 管理员 $env： 用户名**，然后按 Enter。  
  
#### <a name="to-install-exchange-server"></a>若要安装 Exchange Server  
  
1.  以管理员身份登录到第二个服务器上。  
  
2.  打开你的 Internet 浏览器，然后导航到[Exchange Server 助手部署](https://go.microsoft.com/fwlink/p/?LinkID=249163)网站。  
  
3.  单击**本地仅**。  
  
4.  单击新版本中将安装的 Exchange Server 安装选项。  
  
    > [!NOTE]
    >  如果您要安装的 Windows 小型企业服务器从迁移，你应该选择涵盖迁移步骤相应升级选项。  
  
5.  在下一页上，接受默认设置，并单击**下一步**。  
  
    > [!NOTE]
    >  如果你打算使用新安装的 Exchange 服务器在公共文件夹，该将设置更改为**是**。  
  
6.  按照检查表中的分步说明部署 Exchange Server。  
  
     Exchange Server 部署助理还允许你为：  
  
    -   打印清单的副本。  
  
    -   电子邮件收件人到发送一份清单。  
  
    -   以 PDF 文件下载清单。  
  
> [!NOTE]
>  -   你始终必须选择安装在运行 Exchange Server 服务器管理工具。 管理工具所需的 Windows Server Essentials Exchange Server 集成功能。  
> -   如果你需要配置虚拟目录，我们建议你设置**InternalUrl**属性设置为相同的 URL 作为**ExternalUrl**属性每个虚拟目录。 有关详细信息，请参阅[管理客户端访问服务器虚拟目录](https://go.microsoft.com/fwlink/p/?LinkId=251058)Exchange Server 2010 联机帮助网站。  
> -   如果你想要远程 Web 访问站点上的 Windows Server Essentials 内访问从 Outlook Web Access (OWA)，你必须为 OWA 设置外部 URL 属性。  
  
 如果你安装 Exchange Server 2010 全新安装中，你还可以使用下面的脚本 Exchange 服务器设置。  
  
#### <a name="to-use-scripts-to-set-up-exchange-server"></a>若要使用脚本 Exchange 服务器设置  
  
1.  打开记事本，并将以下脚本粘贴到新的文件：  
  
     `Import-Module ServerManager`  
  
     `Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  �Restart`  
  
2.  保存文件作为**InstallDependencies.ps1**。  
  
3.  将 Exchange SSL 证书复制到服务器上的位置。  
  
4.  打开新记事本文件，并复制到文件以下文本：  
  
     `param (`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]`  
  
     `$CertPath = "c:\certificates\ExchangeCertificate.pfx",`  
  
     `[Security.SecureString]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]`  
  
     `$CertPassword = $null,`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]`  
  
     `$DomainName = "contoso.com",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]`  
  
     `$ServerIpAddress = "192.168.0.1",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]`  
  
     `$InternalIpRange = "192.168.0.0-192.168.0.255"`  
  
     `)`  
  
     `#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.`  
  
     `Import-ExchangeCertificate  �FileData ([Byte[]]$(Get-content -Path $CertPath  �Encoding byte  �ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force`  
  
     `#New AcceptedDomain and set it to default`  
  
     `New-AcceptedDomain  �Name "official name"  �DomainName $domainname`  
  
     `Set-AcceptedDomain  �Identity "official name"  �MakeDefault $true`  
  
     `#New EmailAddress Policy`  
  
     `$address = "%m@" + $DomainName`  
  
     `New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address`  
  
     `#Set owa and ecp VirtualDirectory ExternalUrl`  
  
     `$hostname = "mail." + $DomainName`  
  
     `$owa = "https://" + $hostname + "/owa"`  
  
     `$ecp = "https://" + $hostname + "/ecp"`  
  
     `$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"`  
  
     `$oab = "https://" + $hostname + "/OAB"`  
  
     `$ews = "https://" + $hostname + "/EWS/Exchange.asmx"`  
  
     `Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  �ExternalUrl $owa  �InternalUrl $owa`  
  
     `Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  �ExternalUrl $ecp  �InternalUrl $ecp`  
  
     `Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  �InternalUrl $activesync`  
  
     `Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true`  
  
     `Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force`  
  
     `#Enable outlook Anywhere`  
  
     `Enable-OutlookAnywhere  �ClientAuthenticationMethod:Basic  �ExternalHostname:$hostname  �SSLOffloading:$false`  
  
     `#new receive/send connector`  
  
     `$machinename = get-content env:computername`  
  
     `$bindingIpaddress = $ServerIpAddress + ":25"`  
  
     `$RecevieConnectorName = $machinename + "\Default " + $machinename`  
  
     `Set-ReceiveConnector $RecevieConnectorName -RemoteIPRanges $InternalIpRange`  
  
     `New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated`  
  
     `New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename`  
  
5.  设置此脚本以反映你的网络环境开头参数。  
  
6.  保存文件作为**ConfigureExchange.ps1**。  
  
7.  以管理员身份运行的 Windows PowerShell。  
  
8.  在 Windows PowerShell 命令提示符下，键入**组 ExecutionPolicy RemoteSigned**，然后按 Enter。  
  
9. 运行脚本**InstallDependencies.ps1**。  
  
10. 请服务器，并运行 Windows PowerShell 重新启动以管理员身份登录。  
  
11. 在 Windows PowerShell 命令提示符下运行下面的脚本：  
  
     `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  
  
    > [!NOTE]
    >  请务必键入 Exchange Server 安装程序到正确的路径。  
  
12. Exchange Server 安装完成后，则以 administrator 身份打开 Exchange 管理 Shell。  
  
13. 在 Exchange 管理 Shell 命令提示符下，键入**组 ExecutionPolicy RemoteSigned**，然后按 Enter。  
  
14. 运行脚本**ConfigureExchange.ps1**。  
  
15. 重新启动的服务器。  
  
> [!NOTE]
>  如果你决定使用公开受信任的 SSL 证书，而不是自发行证书，你可以按照创建证书请求并将其发送到你所选的证书颁发机构安装指南中的说明进行操作。 你还可以使用 Exchange PowerShell cmdlet 创建证书请求。 以下是一个示例。  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  自定义的脚本参数，以反映你的网络环境。  
  
## <a name="post-installation-tasks"></a>后 Post-installation 任务  
 此部分中介绍了服务器配置任务，你可能需要完成安装后阶段包含特定于运行 Windows Server Essentials 网络的 Exchange Server 本地服务器设置的信息。  
  
### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>添加电子邮件的公用域和配置的电子邮件地址策略  
  
> [!NOTE]
>  如果你正在执行干净安装，这是必需的任务。 如果要从 Windows 小型企业服务器迁移跳过此步骤。  
  
 你必须指定你的电子邮件域成为默认接受域中，然后配置的电子邮件地址的策略。  
  
##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>添加你的电子邮件域作为默认接受域  
  
1.  按照说明进行操作 Exchange Server 文章中的[创建接受域](https://go.microsoft.com/fwlink/p/?LinkId=249174)添加接受的域。  
  
2.  登录到第二个服务器以 administrator 身份打开 Exchange 管理控制台中，，然后导航到**中心传输**的选项卡**组织配置**。  
  
3.  Exchange 管理控制台工作窗格中，新接受的域中，右键单击，然后单击**设为默认值**。  
  
4.  按照说明进行操作 Exchange Server 文章中的[创建电子邮件地址策略](https://go.microsoft.com/fwlink/p/?LinkId=249179)创建新的电子邮件地址策略。 你可以接受所有除的电子邮件地址的默认值。 电子邮件地址，指定公开邮件域。  
  
### <a name="create-smtp-send-and-receive-connectors"></a>创建 SMTP 发送和接收接头  
  
> [!NOTE]
>  这是必需的任务。  
  
 你必须配置 SMTP 发送接头和站/归巢的电子邮件传输 SMTP 接收接头。  
  
 若要创建 SMTP 发送接头，请按照 Exchange Server 文章中的说明[创建 SMTP 发送连接器](https://technet.microsoft.com/library/aa997285.aspx)。  
  
 若要创建 SMTP 接收接头，请按照 Exchange Server 文章中的说明[创建 SMTP 收到连接器](https://technet.microsoft.com/library/bb125159.aspx)。  
  
 作为一个选项，可以参阅上文用于创建发送脚本并使用 Exchange PowerShell cmdlet 接收接头。  
  
### <a name="configure-the-network-router"></a>将网络路由器配置  
  
> [!NOTE]
>  如果你正在执行干净安装，这是必需的任务。 如果你要从 Windows 小型企业服务器迁移，请参阅[Windows Server essentials 迁移服务器数据](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)如何配置网络有关的说明。  
  
 至少，必须在路由器配置以下端口设置：  
  
|路由器端口|目标 IP|目标端口|注意|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|运行 Exchange Server 的本地服务器内部 IP。|25||  
|80 (HTTP)|运行的 Windows Server Essentials 服务器的内部 IP|80||  
|443 (HTTPS)|运行的 Windows Server Essentials 服务器的内部 IP|443||  
  
 如果你支持 POP3 或消息协议网络上的 IMAP，还必须配置端口 forwardings 对这些协议。 相关信息，请参阅部分**客户端访问服务器**主题中[Exchange 网络端口参考](https://go.microsoft.com/fwlink/p/?LinkId=250773)Exchange Server TechNet 库中。  
  
> [!NOTE]
>  -   我们建议您将配置适用于运行的 Windows Server Essentials server 的静态 IP 地址，并为第二个服务器运行 Exchange Server。 有关如何配置运行的 Windows Server 2003 或 Windows Server 2008 R2 的计算机上的静态 IP 地址的说明，请参阅[配置的静态 IP 地址](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx)Windows Server TechNet 库中  
>   
>      **注意：** DNS 服务器设置始终应指向运行的 Windows Server Essentials 服务器的 IP 地址。  
> -   在路由器上，确保的 IP 地址的运行的 Windows Server Essentials 服务器，并运行 Exchange Server 服务器可能会保留，或不退出 DHCP IP 地址范围。  
> -   在此部分中路由器配置假定公共分配你的 Internet 服务提供商 (ISP) 的只有一个 IP 地址。 如果你有多个公共 IP 地址，你可以将不同的 IP 地址分配给运行的 Windows Server Essentials server 和 Exchange Server 运行的服务器，，然后创建端口 forwardings 基于公共的 IP 地址。  
  
### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>启用 Windows Server Essentials 上的本地 Exchange Server 集成  
  
> [!NOTE]
>  如果你从 Windows 小型企业服务器安装迁移的我们建议你目前跳过此步骤并卸载源服务器上的 Exchange Server 以前安装后运行它。  
  
 安装和配置运行 Exchange Server 服务器后，你必须启用运行的 Windows Server Essentials 服务器上的本地 Exchange Server 集成。  
  
##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>若要启用本地 Exchange Server 集成从仪表板  
  
1.  登录到运行的 Windows Server Essentials 以 administrator 身份，服务器上，然后打开仪表板。  
  
2.  在**家庭**页上，单击**连接到我的电子邮件服务**，然后单击**集成 Exchange Server**。  
  
3.  在信息窗格中，单击**设置 Exchange Server 集成**。  
  
4.  按照该向导中的说明进行操作。  
  
### <a name="configure-a-reverse-proxy"></a>配置反向的代理服务器  
  
> [!NOTE]
>  如果您拥有来自你的 Internet 服务提供商的只有一个 Internet 连接，这是必需的任务。  
  
 Windows Server Essentials 和 Exchange Server 支持网络用户某些远程访问方案。 例如，如果在运行 Windows Server Essentials 服务器打开任何地方访问权限，你可以远程访问该远程网站访问的站点或使用虚拟专用网络 (VPN) 以与 Windows Server Essentials 网络连接。 若要远程访问电子邮件，必须使用 Outlook Anywhere、 Outlook Web Access (OWA) 或 ActiveSync。  
  
 如果 Windows Server Essentials 和运行 Exchange Server 服务器同时连接到同一路由器，并且只有一站的 Internet 连接 Internet 服务提供商到路由器，你必须使用反向代理解决方案路由不同类型的远程访问请求从 Internet 基于目的地主机名称。 我们建议你使用的 Microsoft 支持 IIS 应用会请求路由 (ARR) 扩展作为反向代理解决方案。 应用会请求路由 IIS 的详细信息，请访问[申请请求路径网站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。  
  
##### <a name="to-install-and-configure-application-request-routing"></a>若要安装和配置申请请求路径  
  
1.  以管理员身份登录到 Windows Server Essentials。  
  
2.  打开 Internet 浏览器，然后导航到[申请请求路径网站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。  
  
3.  在 ARR 网站上，单击**安装**，然后按照说明安装箭头  
  
    > [!NOTE]
    >  ARR 安装期间，你必须选择 URL 重写模块。  
    >   
    >  你可能会收到有关 ARR 2.5 KB 2589179 未成功安装 ARR 安装末尾错误。 你可以忽略此错误。  
  
4.  ARR 安装完成后，重新启动**远程桌面网关**服务如果它未运行。  
  
    > [!NOTE]
    >  安装 ARR 之后,**远程桌面网关**可能停止服务。 若要手动重启该服务，请打开**服务**管理工具，，然后重启**远程桌面网关**服务。  
  
5.  [下载 KB2732764 ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302)，然后在服务器上运行 Windows Server Essentials 安装更新。  
  
6.  有关 Exchange Server SSL 证书文件复制到运行 Windows Server Essentials 服务器上。 证书文件必须包含专用键，并且必须 PFX 文件格式。  
  
    > [!NOTE]
    >  如果你使用的自发行的证书，请按照 Exchange Server 文章中的指令[导出 Exchange 证书](https://technet.microsoft.com/library/dd351274.aspx)导出证书。  
  
7.  具体取决于运行哪个版本的 Windows Server Essentials，请执行以下任一操作：  
  
    -   在 Windows Server Essentials： 打开命令窗口以 administrator 身份，，然后打开 %ProgramFiles%\ Windows Server \Bin 目录  
  
    -   在 Windows Server Essentials： 打开命令窗口以 administrator 身份，，然后打开 %Windir%\System32\Essentials 目录。  
  
8.  根据安装方案，请按照以下步骤来配置 ARR 之一：  
  
    -   如果执行干净安装时，运行以下命令：  
  
         ** ARRConfig 配置证书 ***证书文件路径*** 主机名 ***主机 Exchange 服务器的名称* ****  
  
        > [!NOTE]
        >  例如;** ARRConfig 配置证书***c:\temp\certificate.pfx***主机 *** mail.contoso.com ***  
        >   
        >  更换*mail.contoso.com*您受证书的域的名称。  
  
    -   在正在迁移从 Windows 小型企业服务器，运行以下命令：  
  
         ** ARRConfig 配置证书 ***证书文件路径*** 主机名 ***主机 Exchange 服务器的名称*** 目标服务器 ** *Exchange Server 服务器名称* ****  
  
         例如;** ARRConfig 配置证书***c:\temp\certificate.pfx***主机***mail.contoso.com***目标服务器 *** ExchangeSvr ***  
  
         更换*mail.contoso.com*与你的域名。 更换*ExchangeSvr*运行 Exchange Server 你服务器的名称。  
  
9. 出现提示时，键入的密码证书。  
  
> [!NOTE]
>  -   提供您的主机名称必须包含在你购买的适用于 Exchange Server SSL 证书。  
> -   如果你有多个主机名称，使用逗号 （，） 它们的分隔。  
  
 若要验证配置适用，请尝试访问运行 Exchange Server (https://mail 你服务器 OWA 网站。 *您的域名*.com/owa) 的计算机不是在域中的成员。 若要解决连接问题，你还可以使用联机[Microsoft 远程连接分析器](https://go.microsoft.com/fwlink/p/?LinkId=249455)工具。  
  
### <a name="configure-split-dns-for-exchange-server"></a>配置为 Exchange 服务器拆分 DNS  
  
> [!NOTE]
>  这是推荐的任务。  
  
 拆分 DNS 允许你为主机同名，具体取决于发出 DNS 请求的位置中 DNS 配置不同的 IP 地址。 如果客户端计算机上的 intranet，DNS 请求解析为 intranet IP 地址。 如果客户端计算机上 Internet，DNS 请求解决了 Internet 的 IP 地址。 这是透明的用户。  
  
 我们建议您将配置拆分服务的方式，使用户可以始终使用相同的名称主机访问 Exchange Server DNS，无论他们的位置。  
  
##### <a name="to-configure-split-dns-for-exchange-server"></a>有关 Exchange Server 配置拆分 DNS  
  
1.  以 administrator 身份，登录到 Windows Server Essentials，然后打开 DNS 管理器。  
  
2.  在 DNS 管理控制台树中，右键单击你的服务器，，然后单击**新建区域**。 **新区域向导**显示。  
  
3.  在**区域类型**页面向导中，接受默认的选项，然后单击**下一步**。  
  
4.  在**Active Directory 区域复制范围**页、 接受默认的选项，然后单击**下一步**。  
  
5.  上**前进或反向查找区域**页面上，接受或选择**前进查找区域**，然后单击**下一步**。  
  
6.  在**区域名称**页上，键入你运行的服务器，Exchange Server （例如; FQDN*mail.contoso.com*)，然后单击**下一步**。  
  
7.  在**动态更新**页面接受默认的选项，请单击**下一步**，然后单击**完成**。  
  
8.  在 DNS 管理控制台树中，右键单击新转发查找区域，，然后单击**（A 或 AAAA） 的新主机**。  
  
9. 在**新主机**页上，将保留**名称**字段留空，键入你运行的服务器，Exchange Server 的 intranet IP 地址，然后单击**添加主机**。  
  
    > [!NOTE]
    >  当你离开**名称**字段留空，服务器默认情况下使用家长的域名。  
  
10. 在**新主机**页上，单击**完成**。  
  
> [!NOTE]
>  如果你使用 ActiveSync，但无法同步某些邮箱帐户的电子邮件，确定这些帐户是否域管理员如一个或多个受保护的组成员。 可以帮助你解决此问题的相关信息，请参阅[Exchange ActiveSync 返回 HTTP 500 错误](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx)。  
  
## <a name="related-topics"></a>相关的主题  
 集成本地 Exchange Server 的详细信息，请参阅以下各部分。  
  
### <a name="what-happens-if-i-disable-exchange-integration"></a>如果我禁用 Exchange 集成，会发生什么情况？  
 如果你禁用与本地 Exchange Server 集成，将不再能够使用 Windows Server Essentials 仪表板查看、 创建或管理 Exchange Server 邮箱。  
  
### <a name="what-do-i-need-to-know-about-email-accounts"></a>了解电子邮件帐户时需要哪些操作？  
 在服务器上配置托管的电子邮件解决方案。 来自托管的电子邮件提供商，如 Microsoft Office 365、 解决方案可以网络用户提供个别电子邮件帐户。 当你添加用户帐户向导在运行 Windows Server Essentials 创建用户帐户时，该向导将尝试的用户帐户添加到托管的电子邮件可用的解决方案。 同时，该向导将电子邮件名称 （别名） 分配给用户，并设置邮箱 （配额） 的最大大小。 邮箱最大大小具体取决于你所使用的电子邮件提供商而异。 后添加的用户帐户，你可以继续从用户的属性页中管理邮箱别名和定额信息。 完整管理你的用户帐户和托管的电子邮件提供商，使用你托管的提供商的管理控制台。 具体取决于你的提供商，你可以访问他们管理控制台基于 web 的入口，从或服务器仪表板中的选项卡。  
  
 运行添加用户帐户向导时提供的别名发给托管的电子邮件提供商，为用户别名建议的名称。 例如，如果用户别名是*FrankM*，用户 s 电子邮件地址可能* FrankM@Contoso.com *。  
  
 此外，在添加用户帐户向导中的用户设置的密码将初始密码解决方案托管的电子邮件中的用户。  
  
 最后，如果你删除 Delete 用户帐户向导通过在服务器上的用户，该向导还向发送请求用户从其系统删除以及托管的电子邮件提供商。 提供商可能会删除 s 用户帐户和帐户相关联的电子邮件。  
  
 有关如何设置所需的电子邮件客户端软件或访问电子邮件帐户的用户信息，请参考托管的电子邮件提供商提供的帮助文档。  
  
### <a name="what-is-a-mailbox-quota"></a>什么是邮箱配额？  
 邮箱配额就是分配给网络用户的 Exchange 邮箱数据的存储空间量。  
  
 运行时**设置 Exchange Server 集成**仪表板向导上的任务将页面添加到添加用户帐户向导，以便你可以选择是否要加强邮箱配额，并指定配额大小。 默认情况下，**强制邮箱配额**选项 （上），并且用户邮箱分配 2 GB 的存储空间。 Exchange 管理员可以自定义邮箱配额设置，可根据他们的业务的需求。  
  
## <a name="see-also"></a>请参阅  
  
-   [Windows Server Essentials 系统要求](../get-started/system-requirements.md)  
  
-   [管理电子邮件服务集成](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
