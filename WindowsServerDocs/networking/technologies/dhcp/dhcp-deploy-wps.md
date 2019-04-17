---
title: 部署 DHCP 使用 Windows PowerShell
description: 你可以使用本主题以部署 Windows Server 2016 Internet 协议 (IP) 版本 4 DHCP 服务器的给 IPv4 DHCP 客户连接到网络上的一个或多个子网提供自动 IP 地址和 DHCP 选项。
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2be3c02f32229c9b9ee411f5e97305776f9825d8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-dhcp-using-windows-powershell"></a>部署 DHCP 使用 Windows PowerShell

>适用于：Windows Server（半年通道），Windows Server 2016

本指南提供有关如何使用 Windows PowerShell 部署会自动将 IP 地址和 DHCP 选项分配给 IPv4 DHCP 客户连接到网络上的一个或多个子网 Internet 协议 (IP) 版本 4 动态主机配置协议 \(DHCP\) 服务器的说明进行操作。

>[!NOTE]
>若要从 TechNet 库下载 Word 格式本文档，请参阅[在 Windows Server 2016 的部署 DHCP 使用 Windows PowerShell](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293)。

使用 DHCP 服务器若要指定 IP 地址节省中管理开销，因为不需要你手动配置 TCP/IP v4 设置为每个网络适配器，在你的网络上的每台计算机。 DHCP，当计算机自动执行 TCP/IP v4 配置或其他 DHCP 客户已连接到你的网络。

作为独立服务器上，或域的 Active Directory 的一部分，你可以将部署 DHCP 服务器工作组中。

本指南包含以下部分。

- [DHCP 部署概述](#bkmk_overview)
- [技术概述](#bkmk_technologies)
- [套餐 DHCP 部署](#bkmk_plan)
- [在测试实验使用本指南](#bkmk_lab)
- [部署 DHCP](#bkmk_deploy)
- [验证 Server 功能](#bkmk_verify)
- [Windows PowerShellDHCP 的命令](#bkmk_dhcpwps)
- [本指南中的 Windows PowerShell 命令列表](#bkmk_list)

## <a name="bkmk_overview"></a>DHCP 部署概述

下图显示了，你可以使用本指南部署的方案。 此情形的 Active Directory 域中包括一个 DHCP 服务器。 服务器配置为在两种不同的子网 DHCP 客户提供 IP 地址。 个子网路由器具有 DHCP 转发启用的分隔。

![DHCP 网络拓扑概述](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>技术概述

以下部分中提供 DHCP 和 TCP/IP 的简短的概述。

### <a name="dhcp-overview"></a>DHCP 概述

DHCP 是一种 IP 标准简化主机 IP 配置管理。 DHCP 标准提供适用于使用 DHCP 服务器的方法，你的网络上已启用 DHCP 的客户端管理动态分配的 IP 地址和其他相关的配置的详细信息。

DHCP 允许你使用 DHCP 服务器动态分配到计算机或其他设备，如打印机，请在你的本地网络，而无需手动配置的静态 IP 地址的每台设备的 IP 地址。

TCP/IP 网络上的每台计算机必须具有一个唯一的 IP 地址，因为在 IP 地址与之相关子网掩码识别主计算机和子网连接到计算机。 通过使用 DHCP，你可以确保所有计算机配置为 DHCP 客户端都接收适合他们的网络位置和子网，IP 地址，并使用 DHCP 选项，如默认网关和 DNS 服务器，你可以自动提供 DHCP 客户端与他们的需求才能正常工作，你的网络上的信息。

对于基于 IP 网络的复杂性和管理的工作涉及配置计算机中的金额，减少了 DHCP。

### <a name="tcpip-overview"></a>TCP/IP 概述

默认情况下，Windows Server 和 Windows 客户端操作系统的所有版本都具有 IP 版本 4 网络连接配置为自动获取 IP 地址和其他信息，请从 DHCP 服务器称为 DHCP 选项 TCP/IP 设置。 出于此原因，你不需要手动配置 TCP/IP 设置，除非计算机处于服务器计算机或需要手动配置的静态 IP 地址的其他设备。 

例如，建议你手动配置 DHCP 服务器的 IP 地址和 DNS 服务器和正在运行 Active Directory 域服务 \(AD DS\) 的域控制器的 IP 地址。

在 Windows Server 2016 TCP/IP 是以下方法：

-   网络基于符合行业标准网络协议的软件。

-   可路由的企业网络协议支持基于 Windows 的本地网络 (LAN) 和宽的区域广域网环境计算机的连接。

-   核心技术和实用程序可用于连接和共享的信息对于不同系统基于 Windows 的计算机。

-   访问全球的 Internet 服务，如 Web 和文件传输协议 (FTP) 服务器的基础。

-   强大、 可缩放跨平台的客户端月服务器框架。

TCP/IP 提供启用基于 Windows 的计算机连接和其他 Microsoft 和非 Microsoft 系统，包括共享信息的基本 TCP/IP 实用程序：

- Windows Server 2016

- Windows 10

- Windows Server 2012R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Internet 主机

- Apple Macintosh 系统

- IBM 大型机

- UNIX 和 Linux 系统

- 开放 VM 系统

- 准备好销售网络打印机

- 平板电脑和手机网络电话有线的以太网或 802.11 项无线技术，启用

## <a name="bkmk_plan"></a>套餐 DHCP 部署

DHCP 服务器角色在安装之前，下面是关键规划的步骤。

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>计划 DHCP 服务器和 DHCP 转移

DHCP 消息是广播，因为它们不转发之间路由器个子网。 如果你有多个子网并且想要提供的每个子网 DHCP 服务时，你必须执行以下任一操作：

-   安装每个子网 DHCP 服务器

-   将配置路由器子网间向前 DHCP 广播的消息并配置 DHCP 服务器、 网每一个范围上的多个范围。

在大多数情况下，是网络的成本较低比部署 DHCP 服务器的每个物理段上配置路由器转发 DHCP 广播的消息。

### <a name="planning-ip-address-ranges"></a>计划 ip 地址

每个子网必须自己独特的 IP 地址范围。 这些范围表示范围具有 DHCP 服务器上。

范围是，使用 DHCP 服务子网上计算机的 IP 地址管理分组。 管理员首先创建为每个物理子网范围，然后使用范围定义客户使用的参数。

范围具有以下属性：

-   范围内的 IP 地址在其中包括或地址，用于 DHCP 租赁服务排除在外。

-   子网掩码，确定子网前缀给定的 IP 地址。

-   它创建时分配范围名称。

-   租赁持续时间值，归入 DHCP 客户端接收动态分配的 IP 地址。

-   配置分配给 DHCP 客户端，如 DNS 服务器 IP 地址和路由器 / 默认网关 IP 地址任何 DHCP 范围选项。

-   （可选） 使用保留确保 DHCP 客户端始终接收同一 IP 地址。

部署你服务器之前, 列出你子网和你想要为每个子网使用的 IP 地址范围。

### <a name="planning-subnet-masks"></a>计划子网掩码

通过使用子网掩码区分的网络 Id 和 IP 地址内的主机 Id。 每个子网掩码是 32 位使用的都是连续位组大量 (1) 来标识该网络 ID 和所有零 (0) 来识别主机 ID 部分的 IP 地址。

例如，通常使用具有 IP 地址 131.107.16.200 子网掩码是以下 32 位二进制数：

```
11111111 11111111 00000000 00000000
```

此子网掩码号是 16 一位跟 16 零位，指示网络 ID 和主机 ID 部分此 IP 地址的两种 16 位的长度。 通常，这子网掩码显示的小数点分隔 255.255.0.0。

下表显示子网掩码 Internet 的地址类别。

|地址类别|位子网掩码|子网掩码|
|-----------------|------------------------|---------------|
|类 A|11111111 00000000 00000000 00000000|255.0.0.0|
|类 B|11111111 11111111 00000000 00000000|255.255.0.0|
|类 C|11111111 11111111 11111111 00000000|255.255.255.0|

当在 DHCP 创建范围并范围输入的 IP 地址范围时，DHCP 提供了这些默认子网掩码值。 通常，默认子网掩码值是适用于大多数网络没有特殊的要求，其中每个 IP 网络段对应于单个物理网络。

在某些情况下，你可以使用自定义子网掩码实现 IP 子网。 可以与 IP 子网细分默认主机 ID 部分的 IP 地址，若要指定个子网，它是原始类基于网络 id。

通过自定义子网掩码长度，你可以减少位数用于实际主机 id。

若要防止寻址和路由出现问题，你应确保所有 TCP/IP 计算机在网络段上的都使用的相同子网掩码并且每台计算机或设备具有一个唯一的 IP 地址。

### <a name="planning-exclusion-ranges"></a>计划排除范围

当您创建 DHCP 服务器上的范围内时，你指定 IP 地址范围包含所有允许 DHCP 服务器租赁 DHCP 的客户端，如计算机和其他设备的 IP 地址。 如果再转，然后手动配置某些服务器，并且静态与其他设备从同一 IP 地址范围 DHCP 服务器正在使用的 IP 地址，你会无意中创建冲突 IP 地址，你和 DHCP 服务器了具有都分配同一 IP 地址到不同的设备。

若要解决此问题，可以创建 DHCP 范围排除范围。 排除项范围是不允许使用 DHCP 服务器的范围内的 IP 地址范围内连续的 IP 地址。 如果创建排除范围，DHCP 服务器不分配在此范围内，这允许你手动分配这些地址，而无需创建冲突 IP 地址的地址。

你可以排除 IP 地址 distribution DHCP 服务器的通过创建排除范围，为每个范围。 你应该使用的所有设备上的静态 IP 地址与配置排除项。 排除的地址应该包含所有分配给其他服务器、 非 DHCP 客户端，无盘工作站或路由远程访问和 PPP 客户端的手动的 IP 地址。

建议你与额外地址以适应将来网络增长配置排除范围。 下表的 10.0.0.1-的 IP 地址范围范围内提供一个示例排除范围 10.0.0.254 和子网掩码 255.255.255.0。

|配置项目|示例值|
|-----------------------|------------------|
|排除范围启动 IP 地址|10.0.0.1|
|排除范围结束 IP 地址|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>计划 TCP/IP 静态配置

某些设备，如路由器、 DHCP 服务器和 DNS 服务器，必须配置的静态 IP 地址。 此外，你可能已其他设备，如打印机，你想要始终确保具有相同的 IP 地址。 要静态配置每个网的设备列表中，然后计划排除范围你想要使用 DHCP 服务器上以确保 DHCP 服务器不租赁静态配置设备的 IP 地址。 排除范围是从 DHCP 服务中排除的范围内的 IP 地址的有限的序列。 排除范围确保这些区域中的任何地址不向你的网络上的 DHCP 客户提供服务器。

例如，如果你拥有想要使用的静态 IP 地址配置十设备 IP 地址范围子网是通过 192.168.0.254 192.168.0.1，可以创建 192.168.0 排除范围。*x*包括十或多个 IP 地址的范围： 通过 192.168.0.15 192.168.0.1。

在此示例中，使用十排除的 IP 地址配置服务器和其他设备的静态 IP 地址，五个额外的 IP 地址左轻扫可供你可能想要在以后添加的新设备的静态配置。 使用此排除项范围 DHCP 服务器都不会通过 192.168.0.254 192.168.0.16 地址池。

下表中提供了有关广告 DS 和 DNS 的其他示例配置项目。

|配置项目|示例值|
|-----------------------|------------------|
|网络连接绑定|以太网|
|DNS 服务器设置|DC1.corp.contoso.com|
|首选的 DNS 服务器 IP 地址|10.0.0.2|
|范围内值<br /><br />1.范围名称<br />2.起始 IP 地址<br />3.结局 IP 地址<br />4.子网掩码<br />5.默认网关 （可选）<br />6。 租赁持续时间|1.主要子网<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6。 8 天|
|IPv6 DHCP 服务器操作模式|未启用|

## <a name="bkmk_lab"></a>在测试实验使用本指南

你可以使用本指南部署 DHCP 测试实验中之前生产环境中部署。 

>[!NOTE]
>如果你不希望在的测试实验部署 DHCP，则可跳到节[部署 DHCP](#bkmk_deploy)。

实验的要求均不相同，具体取决于您使用的实体服务器或虚拟机 \(VMs\) 和您是使用 Active Directory 域还是部署独立 DHCP 服务器。 

以下信息可用于确定你需要使用本指南 DHCP 部署进行测试的最低资源。

### <a name="test-lab-requirements-with-vms"></a>对虚拟机的功能的测试实验要求

若要在虚拟机的功能的测试实验部署 DHCP，你将需要以下资源。

对于域部署或独立部署，你需要一台配置为 Hyper\ V 主机的服务器。

**域部署**

此部署需要一台物理服务器、一个虚拟交换机用来、两个的虚拟服务器和一个虚拟客户端：

在你的物理服务器，Hyper-V 管理器中，在创建以下各项。

1. 一个**内部**虚拟交换机用来。 不要创建**外部**虚拟切换，因为如果 Hyper\ V 主机位于包括 DHCP 服务器的子网时，您的测试虚拟机的功能将收到来自 DHCP 服务器的 IP 地址。 此外，你部署测试 DHCP 服务器可能分配到子网装有 Hyper\ V 主机上的其他计算机的 IP 地址。
1. 运行 Windows Server 2106 VM 配置为与已连接到虚拟内部 Active Directory 域服务域控制器一个切换你创建。 若要匹配本指南，此服务器必须 10.0.0.2 的静态配置的 IP 地址。 有关部署广告 DS 的信息，请参阅部分**部署 DC1** Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 运行 Windows Server 2106，您将使用本指南，并配置为 DHCP 服务器的 VM 连接到虚拟内部一个切换你创建。 
1. 运行 Windows 客户端操作系统的连接到虚拟内部一个 VM 切换创建您和您将使用验证，DHCP 服务器程序动态分配 IP 地址和 DHCP 选项到 DHCP 客户端。

**独立 DHCP 服务器部署**

此部署需要一台物理服务器、一个虚拟交换机用来、一台的虚拟服务器和一个虚拟客户端：

在你的物理服务器，Hyper-V 管理器中，在创建以下各项。

1. 一个**内部**虚拟交换机用来。 不要创建**外部**虚拟切换，因为如果 Hyper\ V 主机位于包括 DHCP 服务器的子网时，您的测试虚拟机的功能将收到来自 DHCP 服务器的 IP 地址。 此外，你部署测试 DHCP 服务器可能分配到子网装有 Hyper\ V 主机上的其他计算机的 IP 地址。
1. 运行 Windows Server 2106，您将使用本指南，并配置为 DHCP 服务器的 VM 连接到虚拟内部一个切换你创建。
1. 运行 Windows 客户端操作系统的连接到虚拟内部一个 VM 切换创建您和您将使用验证，DHCP 服务器程序动态分配 IP 地址和 DHCP 选项到 DHCP 客户端。

### <a name="test-lab-requirements-with-physical-servers"></a>具有物理服务器的测试实验要求

若要在物理服务器的测试实验部署 DHCP，你将需要以下资源。

**域部署**

此部署需要一个中心或切换，两个物理服务器一个物理客户端：

1. 一个以太网中心或切换你可以连接到的物理计算机使用以太网电缆
1. 一台运行 Windows Server 2106 配置为使用 Active Directory 域服务域控制器的物理计算机。 若要匹配本指南，此服务器必须 10.0.0.2 的静态配置的 IP 地址。 有关部署广告 DS 的信息，请参阅部分**部署 DC1** Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 一台运行 Windows Server 2106，您将使用本指南配置为 DHCP 服务器的物理计算机。 
1. 一台运行 Windows 客户端操作系统，您将使用它来验证你 DHCP 服务器的物理计算机正在动态分配的 IP 地址和 DHCP 选项到 DHCP 客户端。

>[!NOTE]
>如果你没有足够测试机此部署，可用于一个测试机广告 DS 和 DHCP-但是，此配置建议不要生产环境。

**独立 DHCP 服务器部署**

此部署需要一个中心或切换、一台的物理服务器和一个物理客户端：

1. 一个以太网中心或切换你可以连接到的物理计算机使用以太网电缆
2. 一台运行 Windows Server 2106，您将使用本指南配置为 DHCP 服务器的物理计算机。 
3. 一台运行 Windows 客户端操作系统，您将使用它来验证你 DHCP 服务器的物理计算机正在动态分配的 IP 地址和 DHCP 选项到 DHCP 客户端。


## <a name="bkmk_deploy"></a>部署 DHCP

此部分中提供可用于在服务器上一个部署 DHCP 的示例 Windows PowerShell 命令。 在服务器上运行这些示例命令之前，则必须修改匹配你的网络和环境的命令。 

例如，运行命令之前，应替换示例值中的命令以下各项：

- 计算机名称
- 你想要配置（1 范围每子网）的每个范围的 IP 地址范围
- 你想要配置每个 IP 地址范围子网掩码
- 每个范围范围名
- 对于每个范围排除范围
- DHCP 选项值，如默认网关、域和 DNS 或 WINS 服务器
- 接口名称

>[!IMPORTANT]
>检查并运行此命令之前修改您的环境的每个命令。

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>物理计算机或虚拟机上安装 DHCP-到在哪里？

你可以在物理计算机上或在虚拟机上安装 DHCP 服务器角色 \(VM\) Hyper\ V 主机上安装。 如果在虚拟机上安装 DHCP，并且你希望 DHCP 服务器物理 Hyper-V 主机连接到网络上提供指定计算机的 IP 地址，你必须将 VM 虚拟网络适配器连接到是 Hyper-V 虚拟交换机**外部**。

有关详细信息，请参阅部分**Hyper-V 管理器中创建一个虚拟交换机用来**主题中[创建虚拟网络](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network)。

### <a name="run-windows-powershell-as-an-administrator"></a>以管理员身份运行的 Windows PowerShell

你可以使用下面的过程中运行具有管理员权限的 Windows PowerShell。

1. 在运行 Windows Server 2016 的计算机中，单击**开始**，然后右键单击 Windows PowerShell 图标。 将显示一个菜单。 

2. 在菜单中，单击**更多**，然后单击**以管理员身份运行**。 如果出现提示，请键入具有管理员权限在计算机的帐户有关的凭据。 如果与你登录到计算机的用户帐户管理员级别的帐户，你将不会收到凭据提示。

3. 使用管理员权限打开 Windows PowerShell。

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>重命名 DHCP 服务器和配置的静态 IP 地址

如果已未执行此操作，你可以使用下面的 Windows PowerShell 命令重命名 DHCP 服务器和配置为服务器静态 IP 地址。

**配置的静态 IP 地址**

你可以使用以下命令指定的静态 IP 地址 DHCP 服务器，并配置 DHCP 服务器 TCP/IP 属性正确 DNS 服务器 IP 地址。 您还必须你想要使用来配置你的计算机的值更换接口名称和在此示例中的 IP 地址。

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

有关这些命令的详细信息，请参阅下面的主题。

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**将计算机重命名**

你可以使用以下命令以重命名，然后重启计算机。

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

有关这些命令的详细信息，请参阅下面的主题。

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>将计算机连接到域 \(Optional\)

如果你安装 DHCP 服务器的 Active Directory 域环境中，你必须计算机加入域。 打开具有管理员权限的 Windows PowerShell，然后替换域名 NetBios 后运行以下命令**CORP**适用于您的环境的值。

    Add-Computer CORP

出现提示时，键入的域的用户帐户有权计算机加入域的凭据。 

    Restart-Computer

有关 Add-Computer 命令的详细信息，请参阅下面的主题。

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>安装 DHCP

计算机重新启动后，打开具有管理员权限的 Windows PowerShell 和 DHCP 安装通过运行以下命令。

    Install-WindowsFeature DHCP -IncludeManagementTools

有关此命令的详细信息，请参阅下面的主题。

- [安装 WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>创建 DHCP 安全组

若要创建安全组，必须在 Windows PowerShell、运行网络 Shell \(netsh\) 命令，然后重新启动 DHCP 服务，以便新组会处于活动状态。

在 DHCP 服务器上，运行以下 netsh 命令时**DHCP 管理员**和**DHCP 用户**中创建安全组**本地用户和组**DHCP 服务器上。

    netsh dhcp add securitygroups

以下命令重启 DHCP 服务本地计算机上。

    Restart-service dhcpserver

有关这些命令的详细信息，请参阅下面的主题。

- [网络 Shell (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>授权中 Active Directory \(Optional\) DHCP 服务器

如果你在域环境安装 DHCP，你必须执行以下步骤授权 DHCP 服务器在域中运行。

>[!NOTE]
>安装的域的 Active Directory 的未授权的 DHCP 服务器无法正常工作，并且不租赁 DHCP 客户端的 IP 地址。 未经授权便 DHCP 服务器自动禁用是将不正确的 IP 地址分配给网络上的客户端防止未经授权便的 DHCP 服务器的安全功能。

你可以使用以下命令添加到列表中的 Active Directory 的授权 DHCP 服务器 DHCP 服务器。 

>[!NOTE]
>如果你没有域环境，不要运行此命令。

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

若要验证 DHCP 服务器被授权 Active Directory 中，你可以使用以下命令。

    Get-DhcpServerInDC

以下是在 Windows PowerShell 中显示的示例结果。

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

有关这些命令的详细信息，请参阅下面的主题。

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>通知服务器管理器该 post\ 安装 DHCP 配置已完成 \(Optional\)

你已完成 post\ 安装任务，如创建安全组和 DHCP 服务器的 Active Directory 中，在授权后服务器管理器可能仍显示在用户界面指出后，必须使用 DHCP 文章安装配置向导完成 post\ 安装步骤警报。

你可以显示配置使用此 Windows PowerShell 命令以下注册表项中服务器管理器中阻止此 now\ 不必要并不准确的消息。

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

有关此命令的详细信息，请参阅下面的主题。

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>设置服务器级别 DNS 动态更新配置设置 \(Optional\)

如果你想 DHCP 服务器 DHCP 客户端计算机执行 DNS 的动态更新，你可以运行以下命令以配置此设置。 这是设置，不范围级别设置，以便它会影响你配置服务器的所有范围服务器级别。 此示例命令还配置 DHCP 服务器要删除的资源 DNS 记录的客户端，客户端至少到期时。

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

你可以使用以下命令配置 DHCP 服务器使用，以注册或注销 DNS 服务器上的客户端记录的凭据。 此示例中节省 DHCP 服务器的凭据。 第一个命令使用**获取凭据**创建**PSCredential**对象时，并将存储在对象**$Credential**变量。 此命令会提示你的用户名和密码，因此请确保你提供的帐户有权更新资源记录 DNS 服务器上的凭据。

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

有关这些命令的详细信息，请参阅下面的主题。

- [设置 DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>配置的企业网络的范围

DHCP 安装完成后，可用于以下命令配置和激活的企业网络范围、创建的范围内，排除范围配置 DHCP 选项默认网关、DNS 服务器地址，以及 DNS 域名称。

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

有关这些命令的详细信息，请参阅下面的主题。

- [添加 DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [添加 DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [设置 DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>配置 Corpnet2 范围 \(Optional\)

如果已连接到第一个子网借助路由器中启用 DHCP 转移后的第二个子网时，你可以使用以下命令添加一个第二个的范围内，命名 Corpnet2 本例中。 此示例中，也配置排除范围和默认网关的 IP 地址 \（路由器 IP 地址上 subnet\）Corpnet2 子网。

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

如果你拥有其他由此 DHCP 服务器提供服务的子网时，你可以重复这些命令，使用不同的值为所有的命令参数，若要添加的每个子网范围。

>[!IMPORTANT]
>确保 DHCP 邮件转发配置之间 DHCP 客户和 DHCP 服务器的所有路由器。 有关如何配置 DHCP 转移，请参阅路由器相关文档信息。

## <a name="bkmk_verify"></a>验证 Server 功能

若要验证 DHCP 服务器已提供给 DHCP 客户的 IP 地址的动态分配，你可以连接到服务的子网另一台计算机。 将以太网电缆连接到的网络适配器和计算机上的电源后，它将从你 DHCP 服务器请求 IP 地址。 你可以通过使用验证是否成功配置**ipconfig /all**命令和查看结果，或通过执行连接性测试，如尝试访问 Web 与 Windows 资源管理器或其他应用使用你的浏览器或文件共享文件夹的资源。

如果客户端没有接收从 DHCP 服务器的 IP 地址，请执行以下疑难解答步骤。

1. 确保以太网电缆已插入同时计算机和以太网交换机、中心或路由器。
1. 如果客户端计算机插入路由器的分隔从 DHCP 服务器的网络段后，请确保路由器配置 DHCP 邮件转发到。
1. 确保 DHCP 服务器的 Active Directory 中授权通过运行以下命令以从 Active Directory 检索授权 DHCP 服务器的列表。 [获取 DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)。
1. 确保你范围激活方法是打开 DHCP 控制台 \ (服务器管理器**工具**，**DHCP**\)，扩展服务器树，查看范围，然后 right\ 单击每个范围。 如果出现的菜单包括所选内容旁**激活**，单击**激活**。 \ (如果已激活范围，将显示为菜单选择**停用**。\) 

## <a name="bkmk_dhcpwps"></a>Windows PowerShellDHCP 的命令

以下参考提供命令说明和语法所有 DHCP 服务器的 Windows PowerShell 命令 Windows Server 2016。 主题列表按字母顺序排列的动词开头的命令，如基于命令**获取**或**设置**。

>[!NOTE]
>你无法在 Windows Server 2012 R2 使用 Windows Server 2016 命令。

- [进行模块](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

以下参考提供命令说明和语法所有 DHCP 服务器的 Windows PowerShell 命令 Windows Server 2012 R2。 主题列表按字母顺序排列的动词开头的命令，如基于命令**获取**或**设置**。

>[!NOTE]
>在 Windows Server 2016 中，你可以使用 Windows Server 2012 R2 的命令。

- [在 Windows PowerShell DHCP 服务器 Cmdlet](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>本指南中的 Windows PowerShell 命令列表

下面是一个简单的命令和使用本指南中的示例值列表。

    
    New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
    Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
    Rename-Computer -Name DHCP1
    Restart-Computer
    
    Add-Computer CORP
    Restart-Computer
    
    Install-WindowsFeature DHCP -IncludeManagementTools
    netsh dhcp add securitygroups
    Restart-service dhcpserver
    
    Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
    Get-DhcpServerInDC
    
    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
    
    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    
    rem At prompt, supply credential in form DOMAIN\user, password
    
    
    rem Configure scope Corpnet
    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    
    rem Configure scope Corpnet2
    
    Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
    


