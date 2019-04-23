---
title: 使用 Windows PowerShell 部署 DHCP
description: 本主题可用于部署 Windows Server 2016 Internet 协议 (IP) 版本 4 DHCP 服务器向连接到网络上的一个或多个子网的 IPv4 DHCP 客户端提供自动 IP 地址和 DHCP 选项。
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a53f6293067fa0f7014e7794696cf75f7179545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849088"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>使用 Windows PowerShell 部署 DHCP

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本指南说明了如何使用 Windows PowerShell 来部署 Internet 协议 (IP) 版本 4 动态主机配置协议\(DHCP\)到 IPv4 DHCP 自动分配 IP 地址和 DHCP 服务器选项连接到网络上的一个或多个子网的客户端。

>[!NOTE]
>若要从 TechNet 库下载本文档中的 Word 格式，请参阅[部署 DHCP 使用 Windows PowerShell 在 Windows Server 2016 中](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293)。

将 DHCP 服务器分配的 IP 地址将保存在管理开销，因为不需要在网络上的每台计算机中手动配置每个网络适配器的 TCP/IP v4 设置。 使用 DHCP 的计算机时自动执行 TCP/IP v4 配置或其他 DHCP 客户端连接到网络。

可以将 DHCP 服务器在工作组中的作为独立的服务器，或 Active Directory 域的一部分进行部署。

本指南包含下列各节。

- [DHCP 部署概述](#bkmk_overview)
- [技术概述](#bkmk_technologies)
- [规划 DHCP 部署](#bkmk_plan)
- [在测试实验室中使用本指南](#bkmk_lab)
- [部署 DHCP](#bkmk_deploy)
- [验证服务器功能](#bkmk_verify)
- [用于 DHCP 的 Windows PowerShell 命令](#bkmk_dhcpwps)
- [本指南中的 Windows PowerShell 命令列表](#bkmk_list)

## <a name="bkmk_overview"></a>DHCP 部署概述

下图描绘了可以使用本指南部署方案。 此方案包括在 Active Directory 域中的一台 DHCP 服务器。 服务器配置为向两个不同子网上的 DHCP 客户端提供 IP 地址。 子网被分隔已启用 DHCP 转发的路由器。

![DHCP 网络拓扑概述](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>技术概述

以下部分提供 DHCP 和 TCP/IP 的简要概述。

### <a name="dhcp-overview"></a>DHCP 概述

DHCP 是用于简化主机 IP 配置管理的 IP 标准。 DHCP 标准将 DHCP 服务器用于管理网络上启用 DHCP 客户端的 IP 地址动态分配以及其他相关的配置详细信息。

DHCP 可以使用 DHCP 服务器将 IP 地址动态分配到计算机或其他设备，如打印机，请在本地网络，而不是手动配置每台设备具有静态 IP 地址。

TCP/IP 网络上的每台计算机都必须有一个独特的 IP 地址，因为 IP 地址和其相关的子网掩码同时标识主计算机和该计算机连接至的子网。 通过使用 DHCP，你可以确保所有配置为 DHCP 客户端的计算机都能收到适合于他们网络位置和子网的 IP 地址，并且通过使用 DHCP 选项，如默认网关和 DNS 服务器，你可以自动向 DHCP 客户端提供它们在网络上正常运作所需的信息。

对于基于 IP 的网络，DHCP 降低了复杂性和管理工作涉及配置计算机的量。

### <a name="tcpip-overview"></a>TCP/IP 概述

默认情况下，所有版本的 Windows Server 和 Windows 客户端操作系统都具有配置为自动获取 IP 地址和其他信息，称为 DHCP 选项，从 DHCP 服务器的 IP 版本 4 的网络连接的 TCP/IP 设置。 因此，不需要手动配置 TCP/IP 设置，除非计算机是服务器计算机或其他设备，需要手动配置静态 IP 地址。 

例如，建议您手动配置 DHCP 服务器的 IP 地址和 DNS 服务器和运行 Active Directory 域服务的域控制器的 IP 地址\(AD DS\)。

以下是 Windows Server 2016 中的 TCP/IP:

-   基于行业标准网络协议的网络软件。

-   支持基于 Windows 的计算机连接到局域网 (LAN) 和广域网 (WAN) 环境的可路由企业网络协议。

-   用于连接基于 Windows 的计算机与不同的系统以便共享信息的核心技术和实用程序。

-   获取对全局 Internet 服务，如 Web 和文件传输协议 (FTP) 服务器的访问权限的基础。

-   强大、可扩展的跨平台客户端/服务器框架。

TCP/IP 提供了基本的 TCP/IP 实用程序，它使基于 Windows 的计算机可以与其他 Microsoft 和非 Microsoft 系统连接并共享信息，包括：

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

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

- 开放 VMS 系统

- 网络打印机

- 平板电脑和移动电话网络使用电话与有线以太网或 802.11 的无线技术，已启用

## <a name="bkmk_plan"></a>规划 DHCP 部署

下面是安装 DHCP 服务器角色之前的主要规划步骤。

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>规划 DHCP 服务器和 DHCP 转发

由于 DHCP 消息是广播消息，因此路由器不会在子网之间转发这些消息。 如果你有多个子网并且想为每个子网提供 DHCP 服务，则必须执行以下操作之一：

-   在每个子网上安装 DHCP 服务器

-   将路由器配置为跨子网转发 DHCP 广播消息，并且在 DHCP 服务器上配置多个作用域，每个子网一个作用域。

大多数情况下，将路由器配置为转发 DHCP 广播消息比在网络的每个物理网段上部署 DHCP 服务器更经济有效。

### <a name="planning-ip-address-ranges"></a>规划 IP 地址范围

每个子网必须具有其自己的独特 IP 地址范围。 在 DHCP 服务器上用作用域表示这些范围。

作用域是为了便于管理而对子网上使用 DHCP 服务的计算机 IP 地址进行的分组。 管理员首先为每个物理子网创建一个作用域，然后使用此作用域定义客户端所用的参数。

作用域具有下列属性：

-   IP 地址的范围，可在其中包含或排除用于提供 DHCP 服务租用的地址。

-   子网掩码，它确定给定 IP 地址的子网前缀。

-   作用域名称，在创建作用域时指定该名称。

-   租用期限值，这些值被分配到接收动态分配的 IP 地址的 DHCP 客户端。

-   为向 DHCP 客户端进行分配而配置的所有 DHCP 作用域选项，例如 DNS 服务器 IP 地址和路由器/默认网关 IP 地址。

-   保留，可以选择用于确保 DHCP 客户端始终接收相同的 IP 地址。

部署服务器之前，请列出子网以及要用于每个子网的 IP 地址范围。

### <a name="planning-subnet-masks"></a>规划子网掩码

使用子网掩码来区分 IP 地址中的网络 ID 和主机 ID。 每个子网掩码都是一个 32 位的数字，它使用全部为 1 的连续位组标识网络 ID，使用全部为 0 的连续位组标识 IP 地址的主机 ID 部分。

例如，用于 IP 地址 131.107.16.200 的子网掩码通常是以下 32 位二进制数字：

```
11111111 11111111 00000000 00000000
```

此子网掩码数字是 16 1 的位后, 跟 16 个零的位，表示此 IP 地址的网络 ID 和主机 ID 部分长度在这两个 16 位。 通常，该子网掩码采用点分隔的十进制表示法显示为 255.255.0.0。

下表显示了 Internet 地址类别的子网掩码。

|地址类别|子网掩码的位|子网掩码|
|-----------------|------------------------|---------------|
|A 类|11111111 00000000 00000000 00000000|255.0.0.0|
|B 类|11111111 11111111 00000000 00000000|255.255.0.0|
|C 类|11111111 11111111 11111111 00000000|255.255.255.0|

在 DHCP 中创建作用域并且为该作用域输入 IP 地址范围时，DHCP 提供这些默认的子网掩码值。 通常，默认的子网掩码值对于没有特殊要求的大多数网络来说都是可以接受的，其中每个 IP 网段对应于一个物理网络。

某些情况下，可以使用自定义子网掩码实现 IP 子网。 借助 IP 子网，可以细分 IP 地址的默认主机 ID 部分以指定子网，这些子网是基于原始类别的网络 ID 的细分。

通过自定义子网掩码长度，可以减少用于实际主机 ID 的位数。

为了防止出现寻址和路由问题，应该确保网段上的所有 TCP/IP 计算机使用同一子网掩码并且每台计算机或设备具有唯一的 IP 地址。

### <a name="planning-exclusion-ranges"></a>规划排除范围

当你在 DHCP 服务器上创建一个作用域的时候，你指定一个 IP 地址范围，这个范围包括允许 DHCP 服务器租用给 DHCP 客户端（如计算机和其他设备）的全部 IP 地址。 如果你随后进行尝试，并使用来自 DHCP 服务器所用的 IP 地址范围的静态 IP 地址，手动配置一些服务器和其他设备，你可能碰巧会造成一个 IP 地址冲突，在冲突中，你和 DHCP 服务器向不同的设备分配了同样的 IP 地址。

若要解决这个问题，你可以为 DHCP 作用域创建一个排除范围。 排除范围是连续的 IP 地址范围在 DHCP 服务器不能使用的作用域的 IP 地址范围内。 如果你创建了排除范围，DHCP 服务器将不会分配那个范围内的地址，这允许你手动分配这些地址，而不会造成 IP 地址冲突。

可以通过为每个作用域创建一个排除范围，从 DHCP 服务器的分发中排除 IP 地址。 应该对配置有静态 IP 地址的所有设备使用排除。 所排除的地址应包括所有手动分配给其他服务器、非 DHCP 客户端、无盘工作站或路由和远程访问及 PPP 客户端的 IP 地址。

建议你将排除范围配置为具有额外地址，以适应未来网络的扩容。 下表提供示例排除范围的作用域 IP 地址范围为 10.0.0.1-10.0.0.254 且子网掩码 255.255.255.0。

|配置项|示例值|
|-----------------------|------------------|
|排除范围起始 IP 地址|10.0.0.1|
|排除范围结束 IP 地址|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>规划 TCP/IP 静态配置

某些设备（如路由器、DHCP 服务器和 DNS 服务器）必须配置有静态 IP 地址。 此外，你可能还拥有其他设备（如打印机），你希望确保这些设备始终具有相同的 IP 地址。 针对每个子网，列出要静态配置的设备，然后规划要在 DHCP 服务器上使用的排除范围，以确保 DHCP 服务器不会租用静态配置的设备的 IP 地址。 排除范围是作用域中有限的 IP 地址序列，这些地址不提供 DHCP 服务。 排除范围确保服务器不会将这些范围中的任何地址提供给网络上的 DHCP 客户端。

例如，如果子网的 IP 地址范围为 192.168.0.1 到 192.168.0.254 并且你有十个你想要使用静态 IP 地址配置的设备，可以创建一个排除范围的 192.168.0。*x*包括 10 个或多个 IP 地址作用域：192.168.0.1 到 192.168.0.15。

在本例中，使用 10 个排除的 IP 地址将服务器和其他设备配置为静态 IP 地址，剩下的其他 5 个 IP 地址用于将来可能希望添加的新设备的静态配置。 对于此排除范围，DHCP 服务器将保留 192.168.0.16 到 192.168.0.254 的地址池。

下表中提供的 AD DS 和 DNS 的其他示例配置项。

|配置项|示例值|
|-----------------------|------------------|
|网络连接绑定|Ethernet|
|DNS 服务器设置|DC1.corp.contoso.com|
|首选 DNS 服务器 IP 地址|10.0.0.2|
|作用域值<br /><br />1.作用域名称<br />2.起始 IP 地址<br />3.结束 IP 地址<br />4.子网掩码<br />5.默认网关（可选）<br />6.租用期限|1.主要子网<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6.8 天|
|IPv6 DHCP 服务器操作模式|未启用|

## <a name="bkmk_lab"></a>在测试实验室中使用本指南

本指南可用于在生产环境中部署之前在测试实验室中部署 DHCP。 

>[!NOTE]
>如果您不想要在测试实验室中部署 DHCP，则可以跳到节[部署 DHCP](#bkmk_deploy)。

在实验室的要求使用的物理服务器或虚拟机而异\(Vm\)，以及你是要使用 Active Directory 域，还是要部署独立的 DHCP 服务器。 

可以使用以下信息来确定测试使用本指南的 DHCP 部署所需的最少资源。

### <a name="test-lab-requirements-with-vms"></a>与虚拟机的测试实验室要求

若要在测试实验室 Vm 中部署 DHCP，需要以下资源。

对于域部署或独立部署，需要一台服务器配置为 Hyper\-V 主机。

**域部署**

此部署需要一台物理服务器、 一个虚拟交换机、 两个虚拟服务器和一台虚拟客户端：

在物理服务器上的 HYPER-V 管理器中创建以下各项。

1. 一个**内部**虚拟交换机。 不会创建**外部**虚拟交换机，因为如果 Hyper\-V 主机上的 DHCP 服务器所在的子网，你的测试虚拟机将从 DHCP 服务器接收 IP 地址。 此外，部署在测试 DHCP 服务器可能会将 IP 地址分配给子网上的其他计算机的 Hyper\-安装主机。
1. 运行 Windows Server 2016 配置为具有连接到所创建的内部虚拟交换机的 Active Directory 域服务的域控制器的一个 VM。 若要匹配此指南，此服务器必须具有静态配置的 IP 地址 10.0.0.2。 有关部署 AD DS 的信息，请参阅部分**部署 DC1**在 Windows Server 2016[核心网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 一个运行通过使用本指南，并将配置为 DHCP 服务器的 Windows Server 2016 的虚拟机连接到内部虚拟交换机所创建。 
1. 一个 VM 运行 Windows 客户端操作系统，连接到内部虚拟交换机创建和将用于验证，DHCP 服务器动态分配 IP 地址和 DHCP 选项的 DHCP 客户端。

**独立 DHCP 服务器部署**

此部署需要一台物理服务器、 一个虚拟交换机、 一台虚拟服务器和一台虚拟客户端：

在物理服务器上的 HYPER-V 管理器中创建以下各项。

1. 一个**内部**虚拟交换机。 不会创建**外部**虚拟交换机，因为如果 Hyper\-V 主机上的 DHCP 服务器所在的子网，你的测试虚拟机将从 DHCP 服务器接收 IP 地址。 此外，部署在测试 DHCP 服务器可能会将 IP 地址分配给子网上的其他计算机的 Hyper\-安装主机。
1. 一个运行通过使用本指南，并将配置为 DHCP 服务器的 Windows Server 2016 的虚拟机连接到内部虚拟交换机所创建。
1. 一个 VM 运行 Windows 客户端操作系统，连接到内部虚拟交换机创建和将用于验证，DHCP 服务器动态分配 IP 地址和 DHCP 选项的 DHCP 客户端。

### <a name="test-lab-requirements-with-physical-servers"></a>与物理服务器的测试实验室要求

若要在测试实验室使用物理服务器中部署 DHCP，需要以下资源。

**域部署**

此部署需要一个集线器或交换机、 两台物理服务器和一个物理客户端：

1. 一个以太网集线器或交换机可以连接到物理计算机通过以太网电缆
1. 运行 Windows Server 2016 配置为与 Active Directory 域服务域控制器的一台物理计算机。 若要匹配此指南，此服务器必须具有静态配置的 IP 地址 10.0.0.2。 有关部署 AD DS 的信息，请参阅部分**部署 DC1**在 Windows Server 2016[核心网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 运行将使用本指南的 DHCP 服务器配置的 Windows Server 2016 的一台物理计算机。 
1. 一台运行 DHCP 服务器将用于验证在 Windows 客户端操作系统的物理计算机动态分配 IP 地址和 DHCP 选项的 DHCP 客户端。

>[!NOTE]
>如果没有为此部署的计算机数目足以测试，可用于一台测试计算机 AD DS 和 DHCP-但是，对于生产环境不建议此配置。

**独立 DHCP 服务器部署**

此部署需要一个集线器或交换机、 一台物理服务器和一个物理客户端：

1. 一个以太网集线器或交换机可以连接到物理计算机通过以太网电缆
2. 运行将使用本指南的 DHCP 服务器配置的 Windows Server 2016 的一台物理计算机。 
3. 一台运行 DHCP 服务器将用于验证在 Windows 客户端操作系统的物理计算机动态分配 IP 地址和 DHCP 选项的 DHCP 客户端。


## <a name="bkmk_deploy"></a>部署 DHCP

本部分提供可以用于在一台服务器上部署 DHCP 的示例 Windows PowerShell 命令。 在服务器上运行这些示例命令之前，必须修改以满足您的网络和环境的命令。 

例如，运行命令之前，应替换为以下各项的命令中的示例值：

- 计算机名称
- 每个你想要配置 （1 范围内每个子网） 的作用域的 IP 地址范围
- 你想要配置每个 IP 地址范围的子网掩码
- 每个作用域的作用域名称
- 每个作用域排除范围
- DHCP 选项值，例如默认网关、 域名称和 DNS 或 WINS 服务器
- 接口名称

>[!IMPORTANT]
>检查并修改为您的环境的每个命令，然后运行命令。

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>物理计算机或 VM 上安装 DHCP-到何处？

您可以在物理计算机上或虚拟机上安装 DHCP 服务器角色\(VM\) Hyper 上安装的\-V 主机。 如果要在 VM 上安装 DHCP，并且希望 DHCP 服务器的 HYPER-V 主机连接到物理网络上提供给计算机的 IP 地址分配，必须将 VM 虚拟网络适配器连接到 HYPER-V 虚拟交换机是**外部**。

有关详细信息，请参阅明**使用 Hyper-v 管理器创建虚拟交换机**主题中的[创建虚拟网络](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network)。

### <a name="run-windows-powershell-as-an-administrator"></a>以管理员身份运行 Windows PowerShell

可以使用以下过程使用管理员特权运行 Windows PowerShell。

1. 在运行 Windows Server 2016 的计算机，单击**启动**，然后右键单击 Windows PowerShell 图标。 出现一个菜单。 

2. 在菜单中，单击**更多**，然后单击**以管理员身份运行**。 如果系统提示，请键入在计算机具有管理员权限的帐户的凭据。 如果与你登录到计算机的用户帐户是管理员级帐户，不会收到的凭据提示。

3. 使用管理员特权打开 Windows PowerShell。

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>重命名的 DHCP 服务器和配置静态 IP 地址

如果尚未这样做，可以使用以下 Windows PowerShell 命令来重命名的 DHCP 服务器和配置服务器的静态 IP 地址。

**配置静态 IP 地址**

若要将静态 IP 地址分配到 DHCP 服务器，并使用正确的 DNS 服务器 IP 地址配置 DHCP 服务器 TCP/IP 属性，可以使用以下命令。 也必须用你想要用来配置计算机的值，替换这个例子里的接口名称和 IP 地址。

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

有关这些命令的详细信息，请参阅以下主题。

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**重命名计算机**

可以使用以下命令重命名，然后重新启动计算机。

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

有关这些命令的详细信息，请参阅以下主题。

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>将计算机加入到域\(可选\)

如果在 Active Directory 域环境中安装 DHCP 服务器，你必须将计算机加入到域中。 使用管理员权限打开 Windows PowerShell，然后替换域 NetBios 名称后运行以下命令**CORP**适合于您的环境的值。

    Add-Computer CORP

出现提示时，键入有权将计算机加入到域的域用户帐户的凭据。 

    Restart-Computer

有关 Add-computer 命令的详细信息，请参阅以下主题。

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>安装 DHCP

在计算机重新启动后，使用管理员权限打开 Windows PowerShell，然后通过运行以下命令安装 DHCP。

    Install-WindowsFeature DHCP -IncludeManagementTools

有关此命令的详细信息，请参阅以下主题。

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>创建 DHCP 安全组

若要创建安全组，必须运行 Network Shell \(netsh\)在 Windows PowerShell 命令，然后重新启动 DHCP 服务，以使新组将变为活动状态。

在 DHCP 服务器上，运行以下 netsh 命令时**DHCP 管理员**并**DHCP Users**中创建安全组**本地用户和组**上 DHCP服务器。

    netsh dhcp add securitygroups

以下命令重新启动本地计算机上的 DHCP 服务。

    Restart-service dhcpserver

有关这些命令的详细信息，请参阅以下主题。

- [Network Shell (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>授权 DHCP 服务器在 Active Directory 中的\(可选\)

如果在域环境中安装 DHCP，必须执行以下步骤来授权 DHCP 服务器在域中运行。

>[!NOTE]
>安装 Active Directory 域中的未授权的 DHCP 服务器不能正常工作，并不租用给 DHCP 客户端的 IP 地址。 自动禁用未经授权的 DHCP 服务器是一项安全功能，以防止未经授权的 DHCP 服务器将错误的 IP 地址分配给你的网络上的客户端。

可以使用以下命令以将 DHCP 服务器添加到 Active Directory 中的已授权 DHCP 服务器的列表。 

>[!NOTE]
>如果没有域环境，则不会运行此命令。

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

若要验证，在 Active Directory 中授权 DHCP 服务器，可以使用以下命令。

    Get-DhcpServerInDC

以下是 Windows PowerShell 中显示的示例结果。

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

有关这些命令的详细信息，请参阅以下主题。

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>通知发布的服务器管理器\-安装 DHCP 配置已完成\(可选\)

完成文章后\-安装任务，如创建安全组和授权 DHCP 服务器在 Active Directory 中，服务器管理器仍可能会指出该文章的用户界面中显示警报\-必须使用 DHCP 安装后配置向导完成安装步骤。

可以禁止此行为现在\-不必要和不准确的消息，使其在服务器管理器不显示通过配置以下注册表项使用此 Windows PowerShell 命令。

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

有关此命令的详细信息，请参阅以下主题。

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>设置配置设置的服务器级别 DNS 动态更新\(可选\)

如果你想要为 DHCP 客户端计算机执行 DNS 动态更新的 DHCP 服务器，可以运行以下命令以配置此设置。 这是服务器级别设置，不的作用域级别设置，因此它将影响所有在服务器配置的作用域。 此示例命令还会配置 DHCP 服务器以删除客户端的 DNS 资源记录时在客户端至少到期。

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

可以使用以下命令来配置 DHCP 服务器使用注册或取消注册 DNS 服务器上的客户端记录的凭据。 此示例将凭据保存在 DHCP 服务器上。 第一个命令使用**Get-credential**来创建**PSCredential**对象，并将存储在对象 **$Credential**变量。 此命令将提示输入用户名和密码，因此请确保有权更新 DNS 服务器上的资源记录的帐户提供凭据。

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

有关这些命令的详细信息，请参阅以下主题。

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>配置公司网络作用域

DHCP 安装完成后，可以使用以下命令来配置并激活 Corpnet 作用域、 创建作用域，一个排除范围配置 DHCP 选项默认网关、 DNS 服务器的 IP 地址和 DNS 域名。

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

有关这些命令的详细信息，请参阅以下主题。

- [Add-DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [Add-DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>配置 Corpnet2 作用域\(可选\)

如果必须连接到路由器的第一个子网中启用 DHCP 转发，第二个子网，可以使用以下命令以添加第二个作用域，此示例中名为 Corpnet2。 此示例还将配置一个排除范围和默认网关的 IP 地址\(的子网上的路由器 IP 地址\)Corpnet2 子网。

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

如果必须提供此 DHCP 服务器服务的更多子网，您可以重复这些命令，对所有命令参数，都使用不同的值添加为每个子网的作用域。

>[!IMPORTANT]
>请确保 DHCP 客户端和 DHCP 服务器之间的所有路由器都配置为 DHCP 消息转发。 有关如何配置 DHCP 转发，请参阅路由器文档的信息。

## <a name="bkmk_verify"></a>验证服务器功能

若要验证 DHCP 服务器提供给 DHCP 客户端的 IP 地址动态分配，你可以连接到服务的子网的另一台计算机。 以太网电缆连接到网络适配器和的计算机上的电源后，它将从 DHCP 服务器请求的 IP 地址。 你可以通过使用验证配置成功**ipconfig /all**命令并查看结果，或通过执行连接性测试，例如尝试访问 Web 资源具有与 Windows 在浏览器或文件共享资源管理器或其他应用程序。

如果客户端不会收到来自 DHCP 服务器的 IP 地址，请执行以下故障排除步骤。

1. 请确保将以太网电缆插入都在计算机和以太网交换机、 集线器或路由器。
1. 如果客户端计算机插入路由器分隔从 DHCP 服务器的网络段，请确保将路由器配置成转发 DHCP 消息。
1. 请确保 DHCP 服务器在 Active Directory 中授权通过运行以下命令以从 Active Directory 中检索的已授权 DHCP 服务器列表。 [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. 确保你作用域通过打开 DHCP 控制台中激活\(服务器管理器**工具**， **DHCP**\)，展开服务器树来检查作用域，再向右\-单击每个作用域。 如果生成菜单上包括所选内容**激活**，单击**激活**。 \(如果已激活作用域，则将显示为菜单选择**停用**。\) 

## <a name="bkmk_dhcpwps"></a>用于 DHCP 的 Windows PowerShell 命令

以下引用的所有 DHCP 服务器的 Windows PowerShell 命令提供命令说明和语法的 Windows Server 2016。 本主题列出了基于谓词开头的命令，例如按字母顺序的命令**获取**或**设置**。

>[!NOTE]
>不能在 Windows Server 2012 R2 中使用 Windows Server 2016 命令。

- [DhcpServer Module](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

以下引用的所有 DHCP 服务器的 Windows PowerShell 命令提供命令说明和语法适用于 Windows Server 2012 R2。 本主题列出了基于谓词开头的命令，例如按字母顺序的命令**获取**或**设置**。

>[!NOTE]
>在 Windows Server 2016 中，可以使用 Windows Server 2012 R2 命令。

- [在 Windows PowerShell 中的 DHCP 服务器 Cmdlet](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>本指南中的 Windows PowerShell 命令列表

下面是一个简单的命令和本指南中使用的示例值列表。

    
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
    


