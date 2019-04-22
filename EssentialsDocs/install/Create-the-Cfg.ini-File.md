---
title: 创建 cfg.ini 文件
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820118"
---
# <a name="create-the-cfgini-file"></a>创建 cfg.ini 文件

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

在以下方案中，cfg.ini 文件用于自动安装操作系统。  
  
-   当在目标计算机上使用预安装映像测试最终用户的体验时，“初始配置”部分用于指导你完成有人参与模式或无人参与模式下的安装过程。 若要执行此操作，请参阅[创建“初始配置”部分](Create-the-Cfg.ini-File.md#BKMK_CreateInit2)。  
  
##  <a name="BKMK_CreateInit2"></a> 创建初始配置部分  
 使用 cfg.ini 文件中的“初始配置”部分完成有人参与模式或无人参与模式下的安装过程。  
  
#### <a name="to-define-the-initial-configuration-section"></a>定义“初始配置”部分  
  
1.  如果存在 cfg.ini 文件，则在记事本中打开该文件；否则，创建一个新文件。  
  
2.  添加以下文本以创建“初始配置”部分。  
  
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
    >  未提供在初始配置过程中选择其他语言的选项。 如果重置系统，则操作系统的语言应为最初安装的语言。  
  
    |参数名称|参数说明|  
    |--------------------|---------------------------|  
    |*AcceptEula*|指明用户接受 Microsoft 软件许可条款。 此值可以为 True 或 False，但仅当设置为 True 时，才会继续进行安装。|  
    |*AcceptOEMEula*|（可选）表示用户接受合作伙伴许可协议。 此值可以为 True 或 False。 仅当服务器是从单独提供许可协议的合作伙伴那里购买时，才需要填写此字段。|  
    |*CompanyName*|（可选）公司名称。 公司名称用于将服务器与公司相关联，并自定义你的公司报告。 长度不得超过 254 个字符。|  
    |*国家/地区*|（可选）字符串代表所需国家/地区。 例如：US 代表美国。|  
    |*ServerName*|服务器名称可在网络上唯一地标识服务器。 你的服务器名称必须满足以下条件：<br /><br /> -可以为最多 15 个字符。<br /><br /> -可以包含字母、 数字和连字符 （-）。<br /><br /> -不得以连字符开头。<br /><br /> -不得包含任何空格。<br /><br /> -必须只包含数字。<br /><br /> 例如：ContosoServer。|  
    |*DNSName*|内部域将服务器和客户端计算机组合在一起以共享通用的数据库（用户名、密码和其他通用信息）。 用户会在登录到计算机时看到此名称，但是此名称仅在内部使用，并且与 Internet 域名不同。 内部域名必须满足与为 *ServerName* 指定的相同的条件。<br /><br /> 示例：contoso.local。|  
    |*NetbiosName*|NetBIOS 名称用于标识服务器上运行的资源。 长度不得超过 15 个字符。 例如：Contoso。|  
    |*语言*|（可选）指定显示语言。 只能是已安装的语言之一。 示例：en-us 代表美国英语。|  
    |*区域设置*|（可选）使用 *LocaleID* 格式指定时间和货币格式。 示例：en-us 代表用英语显示货币和时间并采用美国标准格式。|  
    |*键盘*|键盘可以采用以下两种格式：<br /><br /> - **输入的语言： 键盘布局。** 例如 0409:00000409，其中 **:** 之前的 0409 为输入语言，而 **00000409** 为键盘布局。 可以在注册表项 **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts** 下找到键盘布局的列表。<br /><br /> - **输入语言： IME 标识符。** 下面是 IME 标识符的完整列表。<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{8F96574E-C86C-4bd6-9666-3F7327D4CBE8} 阿姆哈拉语输入法<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e}{FA550B04-5AD7-411F-A5AC-CA038EC515D7} 微软拼音-简单快速 （简体中文）<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{B2F9C502-1742-11D4-9790-0080C882687E} 中文 （繁体）-新拼音<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} 中文 （繁体）-仓颉<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{6024B45F-5C54-11D4-B921-0080C882687E} 中文 （繁体）-快速<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B}            Chinese Traditional Array<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A}            Chinese Traditional DaYi<br /><br /> - {03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76}            Microsoft IME (Japanese)<br /><br /> - {A028AE76-01B1-46C2-99C4-ACD9858AE02F}{B5FE1F02-D5F2-4445-9C03-C568F23C99A1}             Microsoft IME (Korean)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} 旧朝鲜文输入法 （朝鲜语）<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D}              Yi Input Method<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{3CAB88B7-CC3E-46A6-9765-B772AD7761FF}            Tigrinya Input Method|  
    |*设置*|设置用户的更新选择。 使用以下值之一：<br /><br /> **-所有**等于使用建议的设置。<br /><br /> **-更新**表示仅安装重要更新。 仅<br /><br /> **-None**等于不检查更新。|  
    |*UserName*|-新安装过程中创建的管理员帐户的名称。 你的管理员和标准用户帐户名必须满足以下条件：<br /><br /> -可以为超过 19 个字符。<br /><br /> - Cannot contain / \  [ ] &#124; < > + = ; , ? *<br /><br /> -必须无法启动，或以句点结尾。<br /><br /> -必须包含两个连续句点。<br /><br /> -不能与服务器名称或内部域名相同。<br /><br /> -不能与如管理员或来宾的预定义的用户名称相同。|  
    |*PlainTextPassword*|这是安装过程中创建的新管理员帐户的密码。<br /><br /> -必须至少为八个字符。<br /><br /> -必须包含至少 3 个四个以下类别：<br /><br /> 大写字符。<br /><br /> 小写字符。<br /><br /> -数字。<br /><br /> -符号。|  
    |*StdUserName*|安装过程中创建的新标准用户帐户的名称。 请参阅 *UserName* 参数以了解相关要求。|  
    |*StdUserPlainTextPassword*|安装过程中创建的标准用户帐户的密码。|  
    |WebDomainName|（可选）配置服务器的 Internet 域名。 通过此文件可以配置与域名设置向导中用于手动配置的方法类似的域名。|  
    |TrustedCertFileName|（可选）为域名配置可信证书。 这使你可以放置包含私钥的 .PFX 证书。|  
    |TrustedCertPassword|（可选）用于导入 .PFX 的密码。|  
    |EnableVPN|（可选）默认情况下打开 VPN。|  
    |VpnIPv4StartAddress|（可选）设置 VPN 起始地址。|  
    |VpnIPv4EndAddress|（可选）设置 VPN 结束地址。|  
    |VpnBaseIPv6Address|（可选）为 VPN 设置 IPV6 基址。|  
    |VpnIPv6PrefixLength|（可选）设置 VPN IPv6 地址的前缀长度。|  
    |IsHosted|（可选）如果未指定，则默认值为 false。 如果在宿主环境中设置，请设置此值。 它会禁用路由器配置。|  
    |StaticIPv4Address|（可选）如果要配置静态 IP 地址而不是动态地址，请指定静态 IP 地址。|  
    |StaticIPv4Gateway|（可选）如果要配置静态 IP 地址而不是动态地址，请指定默认网关地址。|  
    |StaticIPv4SubnetMask|（可选）如果要配置静态 IP 地址而不是动态地址，请指定子网掩码。|  
    |StaticIPv6Address|（可选）如果要配置静态 IP 地址而不是动态地址，请指定默认 IP 地址。|  
    |StaticIPv6SubnetPrefixLength|（可选）如果要配置静态 IP 地址而不是动态地址，请指定默认 IPV6 子网前缀长度。|  
    |StaticIPv6Gateway|（可选）如果要配置静态 IP 地址而不是动态地址，请指定默认网关地址。|  
    |ClientBackupOn|（可选）在新客户端加入服务器时，默认情况下关闭客户端备份。|  
    |FileHistoryOn|（可选）在运行 Windows 8 Consumer Preview 的新客户端加入服务器时，默认情况下关闭文件历史记录备份。|  
    |EnableRWA|安装 Windows Server Essentials 时，它将启用远程 Web 访问，但会跳过路由器配置。 仅在产品的干净安装中支持此功能。 默认值为 false。|  
    |IPv4DNSForwarder|设置 IPv4 DNS Forwarder。|  
    |IPv6DNSForwarder|设置 IPv6 DNS Forwarder。|  
    |LaunchPadHiddenTasks|-（可选） 可以隐藏备份条目或 / 和快速启动板上的管理仪表板条目。<br /><br /> -若要禁用仪表板：LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -若要禁用备份：LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -若要禁用备份和仪表板：LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  保存该文件。 请确保将文件保存为 cfg.ini，而不是 cfg.ini.txt。  
  
    > [!NOTE]
    >  你可以将该文件保存到可用于特定安装阶段的 U 盘中，cfg.ini 文件也可位于目标服务器上任何硬盘的根目录中。 必须确保将文件的编码设置为 ANSI 或 Unicode，不支持 UTF-8 编码。  
  
> [!IMPORTANT]
>  cfg.ini 的“初始配置”部分应仅供要对服务器进行个性化设置的最终用户使用，或供使用无人管理应答文件测试服务器用户体验的合作伙伴使用。 该文件中的此部分不应用于创建映像。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [部署准备的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

