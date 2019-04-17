---
title: "托管的 Windows Server Essentials"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 23e586c22ca14af7b02550e2fa1c9522e379170c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="hosted-windows-server-essentials"></a>托管的 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本文档包括特定于想要在其实验部署 Windows Server Essentials 和即服务的 Windows Server Essentials 提供给客户的宿主的信息。  
  
## <a name="what-is-windows-server-essentials"></a>什么是 Windows Server Essentials？  
 Windows Server Essentials 是跨本地小型企业解决方案，其中包含最佳同类、 64 位产品的技术来提供非常适合用于绝大多数的小型企业服务器环境。 在 Windows Server Essentials 包含以下技术。  
  
 **服务器操作系统：** Windows Server 2012 产品的技术提供的 Windows Server Essentials 的核心。 有关详细信息，请访问[Windows Server 2012 网站](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh)。  
  
 **数据保护：** Windows Server Essentials 利用适用于提供改进了很大的数据保护功能的 Windows Server 2012 的几个新功能。 [新存储空间功能](https://technet.microsoft.com/library/hh831739.aspx)允许你聚合物理存储容量的不同硬盘驱动器动态添加硬盘驱动器，并创建指定级别的复原的数据量。 Windows Server Essentials 可以执行全面的系统备份和裸露 metal 还原服务器本身以及连接到该网络的客户端计算机？ 现在具有卷大于 TB 2 的支持。 Windows Server 2012 的新功能[Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx)可用于保护的文件和文件夹由 Microsoft 托管的基于云存储服务。 Windows Server Essentials 还集中管理并配置 Windows 8.1 的客户端，帮助恢复从意外删除或覆盖的文件，而无需管理员获取帮助的用户的文件历史记录功能。  
  
 **随时随地访问：**远程访问 Web 提供更流畅、 触摸体验更加友好的浏览器体验随时随地访问的应用程序和数据你已连接到 Internet 并使用几乎所有设备。 Windows Server Essentials 还提供了更新的 Windows Phone 应用和新的应用的 Windows 8.1 的客户端计算机，，允许用户直观地连接，可跨来搜索，并访问文件和文件夹的服务器上。 此外可以自动缓存供离线访问文件并同步到服务器的连接可用时。 Windows Server Essentials 会为虚拟专用网络 (VPN) 轻松、 向导的过程，只需几次单击，为孩子设置，并简化了的用户的 VPN 访问管理。 客户端计算机可以利用远程加入 Windows SBS 环境，而无需到 office 的长途跋涉 VPN 连接。  
  
 **工作负载的灵活性：** Windows Server Essentials 具有设计，以使客户可以选择哪些应用程序和服务运行本地和这在云中运行。 在上一版本中，Windows 小型企业服务器标准包含 Exchange Server 为组件的产品，其中添加支出和复杂性想要充分利用基于云的消息传递和协作服务的客户。 与 Windows Server Essentials，客户可以利用类型都相同的集成的管理体验是否他们所选择要运行的 Exchange Server 本地副本、 托管 Exchange 服务、 订阅或 Microsoft Office 365 订阅。  
  
 **健康监视：** Windows Server Essentials 会监视其自己的健康状态并运行 Windows 8.1、 Windows 7 和 Mac OS X 版本 10.5 及更高版本的客户端计算机的状态。 运行状况会通知你的问题或计算机备份、 服务器存储、 磁盘空间不足，等相关的问题。  
  
 **扩展性：** Windows Server Essentials 版本上的 Windows 要它允许其他软件供应商，若要添加的功能和特性 core 产品，并添加了一组新的 web 服务 Api 扩展性模型。 它还将保留兼容性现有[软件开发工具包](https://msdn.microsoft.com/library/gg513958.aspx)(SDK) 和[外接程序](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials)为 Windows 要创建。  
  
## <a name="how-can-i-customize-an-image"></a>如何自定义映像？  
 请参阅[Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124)，这是 Windows Server Essentials 自定义的其他步骤使用一个标准 Windows Server sysprep 过程。 完成自定义项，请按[创建一个简单的自定义映像](https://technet.microsoft.com/en-us/library/jj200117)和[自定义映像](https://technet.microsoft.com/en-us/library/jj200161)，然后按照说明进行操作，并在[准备部署该映像](https://technet.microsoft.com/en-us/library/jj200142)捕获你最终的图像。  
  
 应注意以下几点：  
  
1.  你应该通过添加任何驱动器根 SkipIC.txt 文件跳过初始配置 （集成电路）。 服务器，表示之前, 安装后，按 Shift + F10 以启动 cmd 窗口中，并创建下 c: SkipIC.txt 文件 / 驱动器。 自定义后必须请记住，若要删除 SkipIC.txt 文件。  
  
2.  如果你需要在小于 90 GB 磁盘部署 Windows Server Essentials 时，你应该添加注册表项 sysprep 之前：  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 在 sysprep 之后, 你可以使用 sysprepped 磁盘图像或插入 Install.wim 重新用于部署新重新封装。  
  
 如果你使用虚拟机管理器，则可以创建使用运行实例模板。 创建模板将 sysprep 实例和关机服务器。 将其存储在您的库后，你可以打开上通过情况的实例。  
  
##  <a name="BKMK_automatedeployment"></a>如何自动进行部署  
 获取自定义的映像后，你可以使用自己的图像执行部署。 为了更 semiunattended 安装，你需要提供/部署 unattend.xml WinPE 安装。 为完全无人照看的安装，你还需要为 Windows Server Essentials 初始配置提供 cfg.ini 文件。  
  
1.  执行无人照看的 WinPE 设置。 这将自动仅 WinPE 安装和通知，以便最终用户提供 Corp、 域和管理员信息本身后 RDP 到服务器会话停止在初始配置之前安装。 若要执行此操作：  
  
    1.  提供 Windows unattend.xml 文件。 按照[Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694)生成该文件，并提供所有必需的信息，包括服务器的名称，产品密钥和系统管理员密码。 在 unattend.xml 文件 Microsoft Windows 安装部分中，提供如下所示的信息。  
  
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
  
    2.  在公共 IP 必须打开 RDP 端口 3389，以便客户可以使用管理员并服务器插入到 RDP unattend.xml 文件中指定的密码才能完成初始配置。  
  
    > [!NOTE]
    >  如果你不会更改默认的密码，服务器安装将停止要求输入密码的屏幕上。**注意**最终用户必须使用默认的管理员帐户登录到服务器，并执行初始配置。  
  
 如果你使用虚拟机管理器，你可以在主机指定管理员密码，当您从模板创建新实例。  
  
1.  执行完整无人照看的设置，包括无人照看初始配置。 若要执行此操作：  
  
    1.  如果部署启动从 WinPE 设置，正如你所做上方，提供 unattend.xml 文件。  
  
    2.  Windows Server Essentials ADK 节，是指[创建 Cfg.ini 文件](https://technet.microsoft.com/en-us/library/jj200150)，以便生成 cfg.ini。  
  
    3.  提供 [InitialConfiguration] 以信息。  
  
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
  
    4.  提供 [PostOSInstall] 以信息。  
  
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
  
    5.  如果您提供 WebDomainName 参数，请确保 DNS 记录还正在更新指向服务器 s 公共 IP。  
  
    6.  如果你未提供上述 WebDomainName 信息，请确保打开端口 3389，以便客户可以使用 RDP 连接到服务器，并完成 VPN 配置。  
  
> [!NOTE]
>  请确保 VM 主服务器和 Windows Server Essentials VM 时区的区域设置，都相同。 否则，你可能会遇到几种不同的错误 (上可能会失败初始配置证书相关任务; 证书可能无法工作的几个小时在安装后设备的信息不会更新正确; 等)。  
  
 部署后，查看以下注册表项在 HKLM\software\microsoft\windows server\setup 验证初始配置是否成功。 如果 SetupStage = = ICDone & & ICStatus = = 1，则意味着在初始配置成功完成。  
  
## <a name="what-is-the-supported-network-topology"></a>什么是受支持的网络拓扑？  
 若要从漫游客户使用 Windows Server Essentials，应启用 VPN。 我们建议启用 VPN SSTP 连接 443 端口。 如果远程 Web 访问或 Web 服务的应用需要使用媒体服务器功能时，应该还启用端口 80。  
  
 可以通过我们的 Windows PowerShell 脚本无人照看部署过程完成 VPN 启用或它可以设置使用我们向导后初始配置。  
  

-   若要启用无人照看部署期间 VPN，请参阅[如何自动部署？](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment)本文档中。  

-   若要启用无人照看部署期间 VPN，请参阅[如何自动部署？](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment)本文档中。  

  
-   若要启用 Windows PowerShell 通过 VPN，运行以下 cmdlet 具备管理权限，并提供所有必需的信息。  
  
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
  
 如果你不能提供前向客户提供服务器 VPN 连接，确保服务器端口 3389 是可以在 Internet 上的访问，以便最终用户使用 RDP 连接到服务器，并通过本身执行配置。  
  
 下面是两种典型服务器端网络拓扑，以及如何可能配置 VPN/远程网站的访问权限 (RWA):  
  
-   （首选） 拓扑 1  
  
    -   下 NAT 设备单独虚拟网络中的服务器。  
  
    -   在虚拟网络启用 DHCP 服务或服务器分配的静态 IP 地址。  
  
    -   从公共 IP 端口 443 服务器端口 443 是可到达。  
  
    -   VPN 直通的端口 443 允许。  
  
    -   在相同子网服务器地址，应范围 VPN IPv4 地址池。  
  
    -   应静态 IP 内相同子网，但退出 VPN 地址池分配的任何第二个服务器。  
  
-   拓扑 2:  
  
    -   服务器有专用的 IP 地址。  
  
    -   从公共 IP 地址的端口 443 是可到达端口 443 服务器上。  
  
    -   VPN 直通的端口 443 允许。  
  
    -   不同区域中的服务器是地址的 VPN IPv4 地址池。  
  
 拓扑 2 服务器的第二个场景均不支持。  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>如何执行常见任务通过 Windows PowerShell？  
 **允许远程访问**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 示例：  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 此命令将与路由器配置自动启用远程 Web 访问和更改现有的所有用户的默认的访问权限。  
  
 **添加用户**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 示例：  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 此命令将使用密码 Passw0rd 命名 User2Test 管理员 ！。  
  
 **启用/禁用用户**  
  
 示例：  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 此命令将禁用命名 user2test 用户。 设置为 1 UserStatus 可使用户。  
  
 **添加服务器文件夹**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 示例：  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 此命令将添加服务器文件夹名为 MyTestFolder 指定位置。  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>如何将第二个服务器添加到 Windows Server Essentials 域？  
 由于 Windows Server Essentials 域控制器，你可以加入第二个服务器域标准的方式。  
  
## <a name="which-email-solutions-can-be-integrated"></a>可以集成的电子邮件解决方案？  
 Windows Server Essentials 支持与两个邮件解决方案开箱集成： Office 365 和本地更换费用。 如果你运行的托管的电子邮件解决方案，你将需要外接程序集成托管的电子邮件解决方案与 Windows Server Essentials 开发。  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>如何将本地 Windows SBS （2003 年 2011年月 2008年） 迁移到托管 Windows Server Essentials？  
 迁移指南是适用于本地 Windows 小型企业服务器 (Windows SBS) Windows Server Essentials 迁移到。 某些步骤可能不适用于完全相同托管环境。 但是，常规任务和迁移工作负载应相同。 我们建议你参考[迁移指南](https://go.microsoft.com/fwlink/p/?LinkID=254292)并进行必要的自定义基于你宿主环境。  
  
 建议在相同子网使源服务器和目的地的服务器。 如果不能，你应确保：  
  
-   源服务器和目的地服务器的内部 DNS 名称是可由彼此访问。  
  
-   打开了所有所需的端口。  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>如何升级到 Windows Server 标准 Windows Server Essentials？  
 你可以将 Windows Server Essentials 升级到 Windows Server 标准。 删除锁和限制，并添加了从 Windows Server 标准缺少的程序包。 有关详细信息，[下载文档](https://go.microsoft.com/fwlink/p/?LinkID=253181)。  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>本机监视和管理工具是什么？  
  
### <a name="group-policy-management"></a>组策略管理  
 Windows Server Essentials 利用 Windows Server 2012 本地组策略支持，并提供配置文件夹重定向和安全设置的用户界面。  
  
> [!NOTE]
>  在托管的环境中，如果用户配置文件的文件夹重定向处于启用状态，它可能会增加为最终用户登录的数据大小大时的时间。  
  
### <a name="management-pack"></a>管理包  
 Windows Server Essentials 管理包能够在 Windows Server Essentials 来帮助管理大量 Windows Server Essentials 服务器专注于不同的小型企业公司宿主健康警报系统提供监视功能。 在此版本中监视包含以及系统中仅重要的提醒。  
  
#### <a name="management-pack-scope"></a>管理 pack 范围  
 此管理包可帮助你监控特定于 Windows Server Essentials 功能。 它不会监视通用 Windows Server 2012 标准操作系统中的功能。 为了监视 Windows Server Essentials，你应使用 Windows Server Essentials 管理包和管理 Pack 的 Windows Server 2012 标准。  
  
#### <a name="mandatory-configuration"></a>强制性配置  
 需要之前，你可以使用管理包采取以下步骤：  
  
1.  安装代理和配置信任使用证书信任。 由于 Windows Server Essentials 为域控制器预先配置，并且不能与其他域或森林信任，系统中心操作管理器应在 Windows Server Essentials 上安装和配置代理信任由使用证书管理服务器。  
  
2.  下载管理包。 若要使用 Manager 2007 监视 Windows Server Essentials，必须首先下载[Windows Server 操作系统管理包](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010)管理 Pack 目录中。  
  
3.  管理 pack 文件导入。 如果你使用的管理包的本地化的版本，你将需要导入主管理 pack 文件和语言包。  
  
#### <a name="files-in-this-monitoring-pack"></a>此监视包中的文件  
 监视 Pack 的 Windows Server Essentials 包括以下文件：  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft.Windows.Server.2012.Essentials。 < locale\ >.mp  
  
### <a name="back-up-and-restore"></a>备份和还原  
 Windows Server Essentials 允许你备份的服务器和客户端。  
  
#### <a name="back-up-the-server"></a>备份服务器  
 Windows Server Essentials 支持的备份服务器两种方法： 本地备份并关闭本地备份。  
  
 **本地备份**允许你为单独磁盘定期执行阻止级别增量备份。 作为宿主，可能针对 Windows Server Essentials VM 连接虚拟磁盘和配置服务器备份到该虚拟磁盘。 应在 Windows Server Essentials VM 比不同的物理磁盘位于虚拟的磁盘。  
  
-   如果你拥有其他机制备份 Windows Server Essentials VM 中，并且你不希望你的用户，若要查看 Windows Server Essentials 本机服务器备份功能，无法将其关闭，并从 Windows Server Essentials 仪表板中删除所有相关的用户界面。 有关详细信息，请参阅的自定义服务器备份部分[ADK 文档](https://go.microsoft.com/fwlink/p/?LinkID=249124)。  
  
 **本地关闭备份**允许你定期服务器数据备份到云服务。 你可以下载并安装 Microsoft Azure 备份集成模块为 Windows Server Essentials 充分利用由 Microsoft 提供的在 Azure 备份。  
  
 如果你或你的用户喜欢另一个云服务，你应该：  
  
1.  更新 Windows Server Essentials 仪表板的用户界面，以便提供与您的首选的云服务，而不是默认 Azure 备份的链接。 有关详细信息，请参阅自定义映像部分[ADK 文档](https://go.microsoft.com/fwlink/p/?LinkID=249124)。  
  
2.  （可选）Windows Server Essentials 仪表板配置和管理云备份服务外接开发。  
  
#### <a name="back-up-the-client"></a>备份客户端  
 Windows Server Essentials 支持两种类型的客户端数据备份： 完整客户端备份和文件历史记录。  
  
> [!NOTE]
>  备份客户可能会影响性能，因为数据所需的客户端从到服务器通过传输 VPN。  
  
 **完整客户端备份**默认情况下处于打开状态的与 Windows Server Essentials 网络连接的所有客户端设备。 它会逐渐备份 （系统和数据） 完整的客户端，并支持数据消除。 备份数据将运行 Windows Server Essentials 的服务器上。 出现故障的客户可以回退到以前的备份点及其数据。 您可以关闭此功能通过中创建的步骤的 Cfg.ini 文件部分[ADK 文档](https://technet.microsoft.com/en-us/library/jj200150)。  
  
 一些注意事项完整的客户端备份是：  
  
-   性能： 初始客户端备份可能耗时由于要上载的数据量。  
  
-   稳定性： 有时 Internet 连接不稳定在客户端。 客户端备份旨在供可恢复，并且默认检查点 40 GB （客户端备份数据库将检查点每次创建备份 40 GB 数据）。 如果希望不可靠的 Internet 连接，你可以更改此值为较小的数字。  
  
    -   若要启用检查点工作、 在服务器上，请注册表项 HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs 设置为 1。  
  
    -   若要更改检查点 threshold，的客户端上时，可更改 HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold 从其默认值 (40 GB)。  
  
-   客户端空白的基本金属还原： 因为 Windows 预安装环境 VPN 连接不支持，客户端空白的基本金属还原不支持。  
  
 **文件历史记录**到网络共享是 Windows 8.1 的功能，用于备份 （库、 桌面、 联系人、 收藏夹） 的个人资料数据。 在 Windows Server Essentials，我们允许加入 Windows Server Essentials 向所有 Windows 8.1 的客户端的文件历史记录设置的集中管理。 在运行 Windows Server Essentials 服务器上存储的备份数据。 你可以关闭此功能通过中创建的步骤的 Cfg.ini 文件部分[ADK 文档](https://technet.microsoft.com/en-us/library/jj200150)。  
  
### <a name="storage-management"></a>存储管理  
 [新存储空间功能](https://technet.microsoft.com/library/hh831739.aspx)允许你聚合物理存储容量的不同硬盘驱动器动态添加硬盘驱动器，并创建指定级别的复原的数据量。 你还可以对 Windows Server Essentials 以展开及其存储附加 iSCSI 磁盘。  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>我应该测试主情况是什么？  
 从托管的角度来讲，建议你测试以下情形：  
  
 **服务器部署**  
  
-   部署 Windows Server Essentials 服务器实验环境中。  
  
-   根据需要自定义 Windows Server Essentials 图像。  
  
-   自动 cfg.ini 无人照看的文件与 Windows Server Essentials 部署。  
  
-   将本地 Windows SBS 迁移到托管 Windows Server Essentials。  
  
-   从 Windows Server Essentials 升级到 Windows Server 2012。  
  
 **服务器配置**  
  
-   配置任意位置 (VPN、 Web 远程访问 DirectAccess) 的访问权限。  
  
-   配置存储和 Server 文件夹。  
  
-   （如果适用）将配置为服务器备份、 Online Backup、 客户端备份，文件历史记录。  
  
-   （如果适用）将配置并管理存储空间。  
  
-   （如果适用）配置 （Office 365、 托管更换费用，等等） 的电子邮件解决方案集成。  
  
-   （如果适用）配置媒体服务器。  
  
 **服务器管理**  
  
-   管理用户。  
  
-   配置，并且收到的通知的电子邮件通知。  
  
-   出现错误/警告运行 BPA。  
  
-   配置 System Center 监视包。  
  
-   将配置为服务器恢复，如果损坏。  
  
 **客户端体验**  
  
-   通过 internet （个人计算机或 Mac OS） 的客户部署。  
  
-   在客户端无法访问共享文件夹使用启动栏。  
  
-   访问服务器远程网站访问、 转移资产从其他设备 （个人计算机、 手机、 平板电脑）。  
  
-   Windows Phone 我服务器应用中。  
  
-   （如果适用）文件历史记录、 客户端备份和还原 (无 BMR) 文件夹重定向。  
  
-   （如果适用）集成的邮件体验。  
  
## <a name="where-can-i-get-more-support"></a>在哪里可以获取更多支持？  
 你可以从下面的链接获取 SDK 和 ADK 文档：  
  
-   [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 你可以将一个 bug 报告给通过连接的功能团队。 若要生成日志，压缩服务器和客户端，服务器加入上的文件夹： C:\ProgramData\Microsoft\Windows Server\Logs。
