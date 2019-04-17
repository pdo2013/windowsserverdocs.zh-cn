---
title: "创建 Cfg.ini 文件"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 967db5f36ea27fb04eab9a6682a106ba0072d45d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-cfgini-file"></a>创建 Cfg.ini 文件

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

自动以下情形中的操作系统安装用于 cfg.ini 文件：  
  
-   当测试的最终用户提供预安装图像目标计算机上的体验，初始配置部分用于指导参与或无人照看模式中安装。 若要执行此操作，请参阅[创建初始配置部分](Create-the-Cfg.ini-File.md#BKMK_CreateInit2)。  
  
##  <a name="BKMK_CreateInit2"></a>创建的初始配置部分  
 使用 cfg.ini 文件在初始配置部分指导参与或无人照看模式中安装。  
  
#### <a name="to-define-the-initial-configuration-section"></a>若要定义初始配置部分  
  
1.  如果存在，则在记事本中打开 cfg.ini 文件否则，创建一个新文件。  
  
2.  添加以下文本创建 InitialConfiguration 部分。  
  
    ```  
  
    [InitialConfiguration]  
    ;Optional, display language can only be one of the installed language  
    Language=en-us  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
    ;Optional  
    Locale=en-us  
    ;Optional  
    Country=US  
    ;Optional  
    Keyboard=0409:00000409  
    AcceptEula=true  
    ;This is only required on a server where an OEM EULA has been specified   
    ;by using the OOBE.xml file  
    AcceptOEMEula=true  
    ;Optional. Example: My Company Name  
    CompanyName=EnterCompanyName  
    ServerName=EnterServerName  
    ; Example: CONTOSO  
    NetbiosName=EnterNetbiosDomainName  
    ; Example: contoso.local  
    DNSName=EnterDNSDomain   
    ; Used to set the user name for the domain admin  
    UserName=EnterDomainAdminUserName  
    ;The password has to be strong and at least 8 characters  
    PlainTextPassword=EnterAdminPassword  
    ;. Used to set the user name for the domain standard user account. Ignored in migration mode.  
    StdUserName=EnterDomainStandardUserName  
    ;. The password for the domain standard user account has to be strong and at least 8 characters  
    StdUserPlainTextPassword=EnterStandardUserPassword  
    ;Controls the Watson and automatic update settings  
    Settings=All or Updates or None  
    WebDomainName=www.abc.com  
    TrustedCertFileName=c:\cert\a.pfx  
    TrustedCertPassword=Enteryourpassword  
    EnableVPN=true  
    EnableRWA=true  
    IPv4DNSForwarder=<IPV4Address,IPV4Address,¦>  
    IPv6DNSForwarder=<IPV6Address,IPV6Address,¦>  
    VpnIPv4StartAddress=<IPV4Address>  
    VpnIPv4EndAddress=<IPV4Address>  
    VpnBaseIPv6Address=<IPV6Address>  
    VpnIPv6PrefixLength=<number>  
    ;All these section are optional.  
     [PostOSInstall]  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
  
    IsHosted=true  
    StaticIPv4Address=<IPV4Address>  
    StaticIPv4Gateway=<IPV4Address>  
    StaticIPv4SubnetMask=<IPV4SubnetMask>   
    StaticIPv6Address=<IPV6Address>  
    StaticIPv6SubnetPrefixLength=<number>  
    StaticIPv6Gateway=<IPV6Address>  
    ClientBackupOn=true  
    FileHistoryOn=true  
    LaunchPadHiddenTasks=<Microsoft.LaunchPad.AdminDashboard,Microsoft.LaunchPad.Backup>  
  
    ```  
  
    > [!NOTE]
    >  在初始配置过程中选择另一种语言选项未提供。 如果重置系统操作系统的语言将最初安装一个。  
  
    |参数名称|参数说明|  
    |--------------------|---------------------------|  
    |*AcceptEula*|指示用户接受 Microsoft 软件许可条款。 值可以等于真假，但只有在该设置已设置为 True 的进行安装。|  
    |*AcceptOEMEula*|（可选）指示用户接受合作伙伴许可协议。 值可以等于真假。 仅当从提供单独的许可协议合作伙伴所购服务器，此字段是必需的。|  
    |*公司名称*|（可选）公司名称。 公司名称用于你的公司相关联服务器并自定义你的公司报告。 可以长达 254 字符。|  
    |*国家/地区*|（可选）表示所需的国家/地区的字符串。 示例： 我们来说美国。|  
    |*服务器名称*|服务器名称唯一标识网络上的服务器。 服务器名称必须满足以下条件：<br /><br /> -可以长达 15 个字符。<br /><br /> -可能包含字母、 数字和连字符 （-）。<br /><br /> -不得开头用连字符。<br /><br /> -必须不包含任何空间。<br /><br /> -不得包含仅数字。<br /><br /> 示例： ContosoServer。|  
    |*DNSName*|内部域分组服务器和客户端计算机连接在一起共享用户名、 密码和其他的常见信息的一个常见数据库。 登录到他们的计算机，但使用内部仅，并不是 Internet 域名相同时，用户将看到此名称。 内部域名必须满足同一个条件为指定*服务器名称*。<br /><br /> 示例： contoso.local。|  
    |*NetbiosName*|NetBIOS 名称用于识别服务器运行的资源。 它可以是最多 15 个字符。 示例： Contoso。|  
    |*语言*|（可选）指定显示语言。 仅可以是已安装语言之一。 示例： 短-作为使用美国英语我们。|  
    |*区域设置*|（可选）通过使用指定时间和货币格式*LocaleID*格式。 示例： 短-我们货币和时间以英语显示，并且在美国使用标准根据格式化。|  
    |*键盘*|以下两种方式可以键盘：<br /><br /> - **输入的语言： 键盘布局。** 例如 0409:00000409，其中 0409年之前**:**是输入的语言，并**00000409**是键盘布局。 你可以找到列表中的注册表项下的键盘布局**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard 布局**。<br /><br /> - **输入语言： IME 标识符。** 以下是 IME 标识符的完整列表。<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{8F96574E-C86C-4bd6-9666-3F7327D4CBE8} 阿姆哈拉语输入法<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e}{FA550B04-5AD7-411F-A5AC-CA038EC515D7} 微软拼音-简单 Fast （简体中文）<br /><br /> {531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{B2F9C502-1742-11D4-9790-0080C882687E} 中文 （繁体） 的-新语语音<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} 中文 （繁体）-仓颉输入法<br /><br /> {531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{6024B45F-5C54-11D4-B921-0080C882687E} 中文 （繁体） 的-快速<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B} 中文传统深刻<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A} 繁体中文大易<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76} Microsoft 输入法 （日本）<br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F}{B5FE1F02-D5F2-4445-9C03-C568F23C99A1} Microsoft 输入法 （朝鲜语）<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} 旧朝鲜文输入法 （朝鲜语）<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D} Yi 输入法<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{3CAB88B7-CC3E-46A6-9765-B772AD7761FF} Tigrinya 输入法|  
    |*设置*|设置的用户选择更新。 使用以下值之一：<br /><br /> **-所有**等于使用推荐的设置。<br /><br /> **-更新**等安装重要更新。 仅<br /><br /> **-无**等不会检查更新。|  
    |*用户名*|的在安装期间创建新管理员帐户名称。 你的管理员和标准用户帐户名，必须满足以下条件：<br /><br /> -可以长达 19 字符。<br /><br /> -无法包含 / \ [] 和 #124;< > + =;, ? *<br /><br /> -必须无法启动或结尾。<br /><br /> -不得包含两个连续时段。<br /><br /> -不得服务器名称或内部域名相同。<br /><br /> -不得预定义的用户名如管理员或来宾相同。|  
    |*PlainTextPassword*|这是在安装期间创建新管理员帐户的密码。<br /><br /> -必须至少为 8 个字符。<br /><br /> -必须至少包含三个退出下面的四个类别：<br /><br /> -大写形式字符。<br /><br /> -小写字符。<br /><br /> -数字。<br /><br /> -符号。|  
    |*StdUserName*|在安装期间创建新标准用户帐户的名称。 请参阅*用户名*参数要求。|  
    |*StdUserPlainTextPassword*|在设置过程中创建的标准用户帐户的密码。|  
    |WebDomainName|（可选）将配置为服务器 Internet 域名。 此文件，可配置域名类似于用于手动配置域名称安装向导中的方法。|  
    |TrustedCertFileName|（可选）配置域名受信任的证书。 这使你只需设置。PFX 证书，包含专用键。|  
    |TrustedCertPassword|（可选）导入的密码。PFX。|  
    |EnableVPN|（可选）默认情况下打开 VPN。|  
    |VpnIPv4StartAddress|（可选）VPN 开始地址设置。|  
    |VpnIPv4EndAddress|（可选）VPN 结束地址设置。|  
    |VpnBaseIPv6Address|（可选）VPN 设置基本 IPV6 地址。|  
    |VpnIPv6PrefixLength|（可选）设置 VPN IPv6 地址前缀长度。|  
    |IsHosted|（可选）默认值为 false 如果未指定。 如果在主机的环境中设置此，设置此值。 这将禁用路由器配置。|  
    |StaticIPv4Address|（可选）如果你想要配置而不是动态地址的静态 IP 地址，请指定的静态 IP 地址。|  
    |StaticIPv4Gateway|（可选）如果你想要配置而不是动态地址的静态 IP 地址，请指定默认网关的地址。|  
    |StaticIPv4SubnetMask|（可选）如果你想要配置而不是动态地址的静态 IP 地址，请指定子网掩码。|  
    |StaticIPv6Address|（可选）如果你想要配置而不是动态地址的静态 IP 地址，请指定默认 IP 地址。|  
    |StaticIPv6SubnetPrefixLength|（可选）如果你想要配置而不是动态地址的静态 IP 地址，请指定默认 IPV6 子网前缀长度。|  
    |StaticIPv6Gateway|（可选）如果你想要配置而不是一个动态的静态 IP 地址，请指定默认网关的地址。|  
    |ClientBackupOn|（可选）关闭客户端备份默认情况下在新的客户端加入服务器。|  
    |FileHistoryOn|（可选）关闭文件历史记录备份默认情况下时运行的 Windows 8 Consumer Preview 的新客户端加入服务器。|  
    |EnableRWA|安装 Windows Server Essentials 时，它将使远程网站的访问权限，但将跳路由器配置。 这仅在干净安装时，该产品的支持。 默认值为 false。|  
    |IPv4DNSForwarder|设置 IPv4 DNS 转发器。|  
    |IPv6DNSForwarder|设置 IPv6 DNS 转发器。|  
    |LaunchPadHiddenTasks|-（可选） 可以隐藏备份条目或 / 和启动栏上的管理员仪表板条目。<br /><br /> -为禁用仪表板： LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -为禁用备份： LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -为禁用备份和仪表板： LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  保存文件。 请确保你为 cfg.ini，不 cfg.ini.txt 保存该文件。  
  
    > [!NOTE]
    >  你可以将文件保存到 U 盘，这可用于安装的特定阶段，或 cfg.ini 文件可能位于目标服务器上的任何硬盘驱动器根。 你必须确保该文件的编码已设置为 ANSI 或 Unicode，请编码不受支持。  
  
> [!IMPORTANT]
>  Cfg.ini 的初始配置部分应仅用于最终用户个性化服务器或合作伙伴能够使用无人照看的答案文件测试服务器的用户体验。 此部分中的不是文件的用于创建该映像。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

