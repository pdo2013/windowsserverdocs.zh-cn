---
title: 托管 Windows Server Essentials
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: dded002df4ed0bbd70c549a8841b769a77f2fd6a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433546"
---
# <a name="hosted-windows-server-essentials"></a>托管 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本文档包括特定于想要在其实验室中部署 Windows Server Essentials 和 Windows Server Essentials 作为服务提供给其客户提供的信息。  
  
## <a name="what-is-windows-server-essentials"></a>什么是 Windows Server Essentials？  
 Windows Server Essentials 是一个跨界小型企业解决方案，其中融合了最佳的、 64 位产品技术提供非常适合小型企业的绝大多数服务器环境。 Windows Server Essentials 中包括以下技术。  
  
 **服务器操作系统：** Windows Server 2012 产品技术提供了 Windows Server Essentials 的内核。 有关详细信息，请访问 [Windows Server 2012 网站](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh)。  
  
 **数据保护：** Windows Server Essentials 利用多个新功能适用于 Windows Server 2012 提供经过显著提高的数据保护性能。 [最新“存储空间”功能](https://technet.microsoft.com/library/hh831739.aspx) 允许你将分散的硬盘的物理存储容量集合起来，以动态地增加硬盘，并根据指定的修复能力级别创建数据卷。 Windows Server Essentials 可以执行完整系统备份和裸机还原的服务器本身以及连接到网络的客户端计算机？ 与目前支持大于 2 TB 的卷。 作为 Windows Server 2012 的新功能， [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) 可用于保护由微软管理的云存储服务中的文件和文件夹。 Windows Server Essentials 还能集中管理和配置 Windows 8.1 客户端，帮助用户恢复意外删除或覆盖文件中，而无需管理员帮助的文件历史记录功能。  
  
 **随处访问：** “远程 Web 访问”旨在提供精简的触摸式浏览器体验：几乎可从任何具有 Internet 连接的位置、使用几乎任何设备访问应用程序和数据。 Windows Server Essentials 还提供了更新的 Windows Phone 应用程序和新的应用程序的 Windows 8.1 客户端计算机，允许用户直观地连接，在进行搜索，并访问文件和服务器上的文件夹。 当与服务器的连接变得可用时，文件还可自动高速缓存，以便脱机访问和同步。 Windows Server Essentials 将设置到只需几下鼠标，轻松的向导驱动的进程的虚拟专用网络 (VPN)，并简化用户的 VPN 访问权限的管理。 客户端计算机可以通过 VPN 连接来远程连接 Windows SBS 的环境，且无需在办公室中就能操作。  
  
 **工作负载灵活性：** Windows Server Essentials 的设计可使用户能够灵活地选择哪些应用程序和服务运行在本地和云中运行。 在以前的版本中，Windows Small Business Server Standard 将 Exchange Server 作为自己的一个组建产品，客户想要使用云端消息服务和协作服务时就必须多花费用，而且操作更加复杂。 与 Windows Server Essentials，客户可以利用的相同类型的集成的管理体验无论决定运行 Exchange Server 的本地副本、 订阅托管的 Exchange 服务，或 Microsoft Office 365 订阅。  
  
 **运行状况监视：** Windows Server Essentials 监视其自己的运行状况状态和运行 Windows 8.1、 Windows 7 和 Mac OS X 版本 10.5 及更高版本的客户端计算机的状态。 运行状况会显示与计算机备份、服务器存储、磁盘空间不足等相关的结果或问题。  
  
 **可扩展性：** Windows Server Essentials 的 Windows SBS 2011 Essentials，它允许其他软件供应商为核心产品中，添加功能和特性并添加一组新的 web 服务 Api 的可扩展模型上构建。 而且，它还兼容现有的 [软件开发工具包](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) 和为 Windows SBS 2011 Essentials 而创建的 [加载项](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) 。  
  
## <a name="how-can-i-customize-an-image"></a>我怎样自定义映像?  
 请参阅[Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124)，这是一个标准的 Windows Server sysprep 过程，与其他 Windows Server Essentials 的自定义步骤。 如需结束自定义，可按照 [创建简单的自定义映像](https://technet.microsoft.com/library/jj200117) 和 [自定义该映像](https://technet.microsoft.com/library/jj200161)中的说明，然后按照 [准备要部署的映像](https://technet.microsoft.com/library/jj200142) 中的说明来捕捉最终映像。  
  
 请注意以下几点：  
  
1. 通过向任一驱动器的根目录添加 SkipIC.txt 文件来跳过初始配置 (IC)。 在安装服务器之后和进行初始配置之前，按组合键 Shift+F10 打开命令窗口，并在 C:/驱动器下创建 SkipIC.txt 文件。 自定义之后，切记删除该 SkipIC.txt 文件。  
  
2. 如果你需要在小于 90GB 的磁盘上部署 Windows Server Essentials，你应添加系统准备之前注册表项：  
  
   ```  
   %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
   ```  
  
   系统准备完成后，便可使用系统准备的磁盘映像，或重新封装回 Install.wim 以进行新的部署。  
  
   如果你使用的是“虚拟机管理器”，则可使用正在运行的实例来创建模板。 创建模板时会对实例进行系统准备，并关闭该服务器。 将其存储在程序库中，可根据情况提出实例。  
  
##  <a name="BKMK_automatedeployment"></a> 如何自动部署？  
 获取自定义映像后，你可以用自己的映像来部署。 如需进行半自动安装，你必须为 WinPE 设置提供或部署 unattend.xml。 若要执行完全无人参与的安装，还需要为 Windows Server Essentials 初始配置提供 cfg.ini 文件。  
  
1. 仅执行无人值守式 WinPE 设置。 这操作只能自动设置 WinPE，并使安装在“初始配置”之前停止，以便最终用户在 RDP 至服务器会话之后自己提供公司、域名及管理员的信息。 要实现此目的，请执行以下操作：  
  
   1.  提供 Windows unattend.xml 文件。 请按照[Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694)生成该文件，并提供所有必要的信息包括服务器名称、 产品密钥和管理员密码。 在 unattend.xml 文件的 Microsoft Windows 安装程序部分中，提供按如下所示的信息。  
  
       ```  
       <InstallFrom>  
                <MetaData>  
                    <Key>IMAGE/WINDOWS/EDITIONID</Key>  
                    <Value>ServerSolution</Value>  
                </MetaData>  
                <MetaData>  
                    <Key>IMAGE/WINDOWS/INSTALLATIONTYPE</Key>  
                    <Value>Server</Value>  
                </MetaData>  
          </InstallFrom>  
       ```  
  
   2.  因此，客户可以使用管理员，并通过 rdp 连接到该服务器在 unattend.xml 文件中指定的密码以完成初始配置，必须在公共 IP 上打开 RDP 端口 3389。  
  
   > [!NOTE]
   >  如果不修改默认密码，则服务器安装将会停在要求输入密码的屏幕上。**注意** 最终用户必须使用默认的管理员账号登录服务器并执行“初始配置”。  
  
   如果你使用的是“虚拟机管理器”，则当从模板创建新的实例时，可在控制台指定系统管理员密码。  
  
2. 执行完全无人值守式设置，包括无人值守式“初始配置”。 要实现此目的，请执行以下操作：  
  
   1.  如果部署是从 WinPE 设置开始，请按上述操作提供 unattend.xml 文件。  
  
   2.  Windows Server Essentials ADK 部分标题，请参阅[创建 Cfg.ini 文件](https://technet.microsoft.com/library/jj200150)来生成 cfg.ini。  
  
   3.  在 [InitialConfiguration] 中提供信息。  
  
       ```  
       WebDomainName=yourdomainname  
       TrustedCertFileName=c:\cert\a.pfx  
       TrustedCertPassword=Enteryourpassword  
       EnableVPN=true  
       EnableRWA=true  
       ; Provide all information so that after setup is complete, your customer can use your domain name to visit the server directly with the admin/user information you provide in the [InitialConfiguration] section.  
  
       VpnIPv4StartAddress=<IPV4Address>  
       VpnIPv4EndAddress=<IPV4Address>  
       VpnBaseIPv6Address=<IPV6Address>  
       VpnIPv6PrefixLength=<number>  
       ; Provide this information. IPv4StartAddress and IPv4Endaddress are required so that your VPN client can acquire valid IP through this range.  
  
       IPv4DNSForwarder=<IPV4Address,IPV4Address,Â¦>  
       IPv6DNSForwarder=<IPV6Address,IPV6Address,Â¦>  
       ; Provide this information as needed according to your network environment settings.  
       ```  
  
   4.  在 [PostOSInstall] 中提供信息。  
  
       ```  
       IsHosted=true   
       ; Must have, this will prevent Initial Configure webpage available for other computers under same subnet.  
  
       StaticIPv4Address=<IPV4Address>  
       StaticIPv4Gateway=<IPV4Address>  
       StaticIPv6Address=<IPV6Address>  
       StaticIPv6SubnetPrefixLength=<number>  
       StaticIPv6Gateway=<IPV6Address>  
       ; All these are optional if you have DHCP Server Service on the subnet, otherwise provide static IP here.  
       ```  
  
   5.  如果你提供 WebDomainName 参数，请确保已更新的 DNS 记录以指向服务器的公共 IP。  
  
   6.  如果没有提供上述 WebDomainName 信息，请确保打开端口 3389 以便客户能够使用 RDP 连接到服务器并结束 VPN 配置。  
  
> [!NOTE]
>  请确保 VM 主机服务器与 Windows Server Essentials VM 的时区设置相同。 否则，可能会出现几种不同的错误（“初始配置”可能在证书相关的任务上会失效；证书在安装之后几小时仍未生效；设备信息更新不正确；等等）。  
  
 部署后，请检查 HKLM\software\windows server\setup 路径下的下列注册表项，以验证“初始配置”是否成功。 如果 SetupStage == ICDone && ICStatus == 1，则表示“初始配置”成功完成。  
  
## <a name="what-is-the-supported-network-topology"></a>支持何种网络拓扑?  
 若要从漫游客户端使用 Windows Server Essentials，应启用 VPN。 我们建议启用 443 端口进行 VPN SSTP 连接。 如果“媒体服务器”需要用于“远程网站访问”或“网站服务应用”，则还应启用 80 端口。  
  
 如果启用 VPN ，可在无人值守的部署中通过 Windows PowerShell 脚本完成，或者可以在初始配置后用我们的向导来配置。  
  

- 若要在无人参与部署期间启用 VPN，请参阅本文档中的 [How do I automate the deployment?](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) 。  

- 若要在无人参与部署期间启用 VPN，请参阅本文档中的 [How do I automate the deployment?](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) 。  

  
- 如需通过 Windows PowerShell 启用 VPN，请根据管理权限运行以下 cmdlet， 并提供所有的必要信息。  
  
  ```  
  ##  
  ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
  ##  
  
  $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
  $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
  $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate  
  $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
  Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';  
  [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);  
  ##  
  ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
  ##  
  
  Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
  ```  
  
  如果在为客户提供服务器之前不能提供 VPN 连接，请确保服务器端口 3389 可以通过 Internet 访问，以便最终用户能够使用 RDP 连接到服务器并自己完成配置。  
  
  以下是两个典型的服务器端网络拓扑布局，同时介绍了 VPN/远程网站访问 (RWA) 如何配置：  
  
- 拓扑 1（优先选用）  
  
  -   NAT 设备下的服务器采用独立虚拟网络。  
  
  -   虚拟网络中已启用 DHCP 服务，或者该服务器已分配静态 IP 地址。  
  
  -   服务器端口 443 可从公共 IP 端口 443 获得。  
  
  -   端口 443 允许使用 VPN 通道。  
  
  -   VPN IPv4 地址池的范围应与该服务器地址的子网相同。  
  
  -   任何第二台服务器均应在同一子网内分配一个静态 IP，但需在 VPN 地址池之外。  
  
- 拓扑 2：  
  
  -   该服务器有一个专用 IP 地址。  
  
  -   从公共 IP 地址的端口 443 是可访问服务器上的端口 443。  
  
  -   端口 443 允许使用 VPN 通道。  
  
  -   VPN IPv4 地址池处于与该服务器地址不同的范围。  
  
  拓扑 2 不支持第二台服务器的方案。  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>我如何通过 Windows PowerShell 执行常见任务?  
 **启用远程 Web 访问**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 例如：  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 此命令将启用“远程 Web 访问”并自动配置路由器，并改变全部现有用户的默认访问权限。  
  
 **添加用户**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 例如：  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 此命令将添加一个管理员密码 Passw0rd 创建名为 User2Test ！。  
  
 **启用/禁用用户**  
  
 例如：  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 此命令将禁用名为 user2test 的用户。 如果将 UserStatus 设定为 1，将会启用该用户。  
  
 **添加服务器文件夹**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 例如：  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 此命令将添加一个名为 MyTestFolder 中指定位置的服务器文件夹。  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>如何将第二个服务器添加到 Windows Server Essentials 域？  
 因为 Windows Server Essentials 是域控制器，可以将第二个服务器加入域以标准方式。  
  
## <a name="which-email-solutions-can-be-integrated"></a>可以集成哪一种电子邮件解决方案?  
 Windows Server Essentials 支持与在初始状态下的两个电子邮件解决方案集成：Office 365 和本地 Exchange。 如果运行的托管电子邮件解决方案，您将需要开发外接程序与托管的电子邮件解决方案集成，Windows Server Essentials。  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>如何将本地 Windows SBS (2011年/2008年/2003) 迁移到托管的 Windows Server Essentials？  
 了可用于在本地 Windows Small Business Server (Windows SBS) 与 Windows Server Essentials 迁移的迁移指南。 有些步骤可能不完全适用于你的托管环境。 然而，普通的迁移任务和工作负载应该是同样的。 建议你参考 [迁移指南](https://go.microsoft.com/fwlink/p/?LinkID=254292) 并根据自己的托管环境进行必要的自定义。  
  
 建议将源服务器和目标服务器放在同一个子网内。 如果无法放在同一子网内，请确保：  
  
-   源服务器和目标服务器的内部 DNS 名称可相互访问。  
  
-   所有必要的端口都已打开。  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>如何将 Windows Server Essentials 升级到 Windows Server Standard？  
 可以将 Windows Server Essentials 升级到 Windows Server Standard。 解除锁定和限制，添加 Windows Server Standard 缺少的程序包。 若要获取详细信息，请 [下载文档](https://go.microsoft.com/fwlink/p/?LinkID=253181)。  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>监测和管理所用的本机工具有哪些？  
  
### <a name="group-policy-management"></a>“组策略”管理  
 Windows Server Essentials 利用 Windows Server 2012 中的本机组策略支持，并提供用户界面来配置文件夹重定向和安全设置。  
  
> [!NOTE]
>  在托管环境中，用户配置文件的文件夹重定向一旦启用，当数据量很大时就有可能增加最终用户的登录时间。  
  
### <a name="management-pack"></a>管理包  
 Windows Server Essentials 管理包提供监视功能通过 Windows Server Essentials，以帮助机主管理大量的 Windows Server Essentials 服务器专用于不同小公司中的运行状况警报系统。 此版本的监测功能仅包括系统内的严重警报。  
  
#### <a name="management-pack-scope"></a>管理包范围  
 此管理包可帮助你监视特定于 Windows Server Essentials 的功能。 它无法监测 Windows Server 2012 Standard 操作系统中的通用功能。 为了监视 Windows Server Essentials，应使用 Windows Server Essentials 管理包和管理包为 Windows Server 2012 Standard。  
  
#### <a name="mandatory-configuration"></a>必须要进行的配置  
 需执行以下步骤方可使用管理包：  
  
1.  安装“代理”并利用证书信任配置信任关系。 Windows Server Essentials 已预配置为域控制器，并且不能具有与其他域或林信任关系，因为 System Center Operation Manager 代理应安装在 Windows Server Essentials 上，并使用管理配置信任使用证书的服务器。  
  
2.  下载管理包。 若要使用 Operations Manager 2007 监测 Windows Server Essentials，必须先下载[Windows Server 操作系统管理包](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010)从管理包目录。  
  
3.  下载管理包文件。 如果你正在使用管理包的本地化版本，则需同时导入主要管理包文件和语言包。  
  
#### <a name="files-in-this-monitoring-pack"></a>在此“监测包”中的文件  
 监视 Windows Server Essentials 的包包括以下文件：  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft.Windows.Server.2012.Essentials.<locale\>.mp  
  
### <a name="back-up-and-restore"></a>备份和还原  
 Windows Server Essentials 可以备份服务器和客户端。  
  
#### <a name="back-up-the-server"></a>备份服务器  
 Windows Server Essentials 支持两种方法可以备份服务器： 在本地备份和异地备份。  
  
 **本地备份**允许你定期执行块级的增量备份，并备份到独立的硬盘上。 作为机主，无法将虚拟磁盘附加到 Windows Server Essentials VM 并配置服务器备份到此虚拟磁盘。 虚拟磁盘应位于非 Windows Server Essentials VM 所在的物理磁盘上。  
  
- 如果你还有去其他机制备份 Windows Server Essentials VM，并且不希望让用户看到 Windows Server Essentials 本机服务器备份功能，可以将其关闭并从 Windows Server Essentials 中删除所有相关的用户界面仪表板。 有关详细信息，请参阅的自定义服务器备份部分[ADK 文档](https://go.microsoft.com/fwlink/p/?LinkID=249124)。  
  
  **非本地备份**允许你定期地将服务器数据备份到云计算服务。 您可以下载并安装 Microsoft Azure 备份集成模块的 Windows Server Essentials 来利用由 Microsoft 提供的 Azure 备份。  
  
  如果你或你的用户喜欢别的云服务，可以：  
  
1.  更新 Windows Server Essentials 仪表板的用户界面，以便其提供指向你的首选的云服务，而不是默认 Azure 备份。 有关详细信息，请参考 [ADK 文档](https://go.microsoft.com/fwlink/p/?LinkID=249124)的“自定义映像”一节。  
  
2.  （可选）开发外接程序的 Windows Server Essentials 仪表板，若要配置和管理云备份服务。  
  
#### <a name="back-up-the-client"></a>备份客户端  
 Windows Server Essentials 支持两种类型的客户端数据备份： 完全客户端备份和文件历史记录。  
  
> [!NOTE]
>  备份客户端可能会影响到性能，因为数据需要通过 VPN 从客户端传送到服务器。  
  
 **完全客户端备份**连接到 Windows Server Essentials 网络的所有客户端设备上是默认情况下。 它以递增的方式备份完全客户端（系统及数据），并支持重复数据删除。 备份数据将运行 Windows Server Essentials 的服务器上。 客户端一旦发生故障，可以使其数据返回到上一个备份点。 您可以关闭此功能的步骤中创建 Cfg.ini 文件部分[ADK 文档](https://technet.microsoft.com/library/jj200150)。  
  
 完全客户端备份需要考虑以下几点：  
  
- 性能：客户端初始备份可能很耗时，因为要上载的数据量相当大。  
  
- 稳定性：有时客户端侧的 Internet 连接不稳定。 客户端备份是可恢复的，默认检查点是 40GB（每完成 40GB 的数据备份，客户端备份数据库就会创建一个检查点）。 如果你预计 Internet 连接不可靠，则可将此容量值改小。  
  
  -   如需启用检查点，可在服务器上将注册表项 HKLM\Software\Microsoft\Windows\server\Backup\GetCheckPointJobs 设为 1。  
  
  -   如需改变检查点的阈值，可在客户端改变 HKLM\Software\Microsoft\Windows\server\Backup\CheckPointThreshold 的默认值 (40GB)。  
  
- 客户端裸机恢复：因为 Windows Preinstall Environment（预安装环境）不支持 VPN 连接，所以不支持客户端裸机备份。  
  
  **文件历史记录**是 Windows 8.1 的特性的配置文件数据 （库、 桌面、 联系人、 收藏夹） 备份到网络共享。 在 Windows Server Essentials 中，我们允许集中管理已加入到 Windows Server Essentials 的所有 Windows 8.1 客户端的文件历史记录设置。 备份数据存储在运行 Windows Server Essentials 的服务器上。 您可以关闭此功能的步骤中创建 Cfg.ini 文件部分[ADK 文档](https://technet.microsoft.com/library/jj200150)。  
  
### <a name="storage-management"></a>存储管理  
 [最新“存储空间”功能](https://technet.microsoft.com/library/hh831739.aspx) 允许你将分散的硬盘的物理存储容量集合起来，以动态地增加硬盘，并根据指定的修复能力级别创建数据卷。 你还可以将 iSCSI 磁盘附加到 Windows Server Essentials 来扩展其存储量。  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>我应该测试什么样的主要场景？  
 从托管的视角来看，建议你测试以下场景：  
  
 **服务器部署**  
  
- 部署 Windows Server Essentials 服务器实验室环境中。  
  
- 根据需要自定义 Windows Server Essentials 映像。  
  
- 自动使用无人参与的文件和 cfg.ini 的 Windows Server Essentials 部署。  
  
- 将本地 Windows SBS 迁移到托管的 Windows Server Essentials。  
  
- 从 Windows Server Essentials 升级到 Windows Server 2012。  
  
  **服务器配置**  
  
- 配置随处访问（VPN、远程 Web 访问、DirectAccess）。  
  
- 配置存储及服务器文件夹。  
  
- （如需要）配置服务器备份、在线备份、客户端备份和 文件历史记录。  
  
- （如需要）配置并管理存储空间。  
  
- （如需要）配置电子邮件解决方案集成（Office 365、托管 Exchange，等等）。  
  
- （如需要）配置 Media Server（媒体服务器）。  
  
  **服务器管理**  
  
- 管理用户。  
  
- 配置并接收电邮通知警报。  
  
- 在发生错误或发出警告的情况下，运行 BPA。  
  
- 配置系统中心监控包。  
  
- 在数据损坏的情况下配置服务器恢复。  
  
  **客户端体验**  
  
- 通过 Internet（个人计算机或 Mac 操作系统）进行客户端部署。  
  
- 使用客户端上的启动面板访问“共享文件夹”。  
  
- 从不同的设备（个人计算机、手机、平板电脑）通过“远程 Web 访问”访问服务器资产。  
  
- Windows Phone 的“我的服务器“。  
  
- （如适用）文件历史记录、客户端备份与恢复（无 BMR）、文件夹重定向。  
  
- （如适用）电邮集成体验。  
  
## <a name="where-can-i-get-more-support"></a>我在哪里可以获得更多支持？  
 你可以从下面的链接获取软件开发工具包 (SDK) 和 应用系统开发包 (ADK) 文件：  
  
- [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
- [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
  你可以通过 Connect 向功能团队报告缺陷。 若要生成日志，请在服务器和连接该服务器的客户端两处压缩文件夹：C:\ProgramData\Microsoft\Windows Server\Logs。
