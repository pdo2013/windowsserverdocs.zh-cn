---
title: 网络策略服务器 (NPS)
description: 本主题提供的 Windows Server 2016，网络策略服务器概述，并包含指向有关 NPS 其他指南。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f48cbdaec82e9e60f734a4a8949082187b13166
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-nps"></a>网络策略服务器 (NPS)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用有关在 Windows Server 2016 的网络策略服务器的概述的本主题。

>[!NOTE]
>本主题中，除了以下 NPS 文档才可用。
> - [网络策略 Server 最佳做法](nps-best-practices.md)
> - [网络策略服务器入门](nps-getstart-top.md)
> - [计划网络策略服务器](nps-plan-top.md)
> - [部署策略服务器的网络](nps-deploy.md)
> - [管理网络策略服务器](nps-manage-top.md)
> - [网络策略 (NPS) 服务器 Cmdlet](https://technet.microsoft.com/library/jj872739.aspx) Windows Server 2016 和 Windows 10
> - [网络策略 (NPS) 服务器 Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) Windows Server 2012 R2 和 Windows 8.1
> - [在 Windows PowerShell NPS Cmdlet](https://technet.microsoft.com/library/jj872739.aspx) Windows Server 2012 和 Windows 8

网络策略 Server (NPS) 允许您创建和履行组织范围网络连接请求身份验证和授权的访问权限策略。

你还可以将连接请求转发到远程 NPS 或其他 RADIUS 服务器，以便你可以请求平衡连接，并将它们转发给身份验证和授权的正确的域的远程身份验证拨入用户服务 (RADIUS) 代理配置 NPS。

NPS 允许你集中配置和管理网络的访问权限身份验证、授权和与以下功能：

- **RADIUS 服务器**。 NPS 执行集中的身份验证、授权和记帐无线，身份验证开关、远程访问拨号和虚拟专用网络 (VPN) 连接。 当你 NPS RADIUS 服务器用作，你将网络的访问权限服务器，如无线接入点和 VPN 服务器配置为在 NPS RADIUS 客户端。 你还配置了 NPS 使用授权连接请求的网络策略和配置 RADIUS 记帐 NPS 日志记录当地的硬盘或 Microsoft SQL Server 数据库中的文件的帐户信息。 有关详细信息，请参阅[RADIUS 服务器](#bkmk_server)。
- **RADIUS 代理**。 当你 NPS RADIUS 代理用作，您将配置连接请求策略，告知 NPS 服务器哪个连接将转发给其他 RADIUS 服务器和你想要向前连接请求哪些 RADIUS 服务器的请求。 你还可以对前进记帐数据无法记录远程 RADIUS 服务器组中的一个或多个计算机配置 NPS。 若要配置为 RADIUS 代理服务器 NPS，请参阅以下主题。 有关详细信息，请参阅[RADIUS 代理](#bkmk_proxy)。
    - [配置连接请求策略](nps-crp-configure.md)
- **RADIUS 记帐**。 可以配置为与本地日志文件或 Microsoft SQL server 实例本地或远程记录事件的 NPS。 有关详细信息，请参阅[NPS 日志记录](#bkmk_logging)。

>[!IMPORTANT]
>网络的访问权限保护 \(NAP\)、健康注册机构 \(HRA\) 主机凭据授权协议 \(HCAP\) 已在 Windows Server 2012 R2 弃用和 Windows Server 2016 中不可用。 如果你有 NAP 部署 Windows Server 2016 比之前使用的操作系统，不能迁移到 Windows Server 2016 的 NAP 部署。

你可以使用这些功能的任意组合来配置 NPS。 例如，您可以配置一个 NPS 服务器用于 VPN 连接 RADIUS 服务器，而且还为 RADIUS 代理转发给远程 RADIUS 服务器身份验证和授权另一个域中的组成员的某些连接请求。

## <a name="windows-server-editions-and-nps"></a>Windows Server 版本和 NPS

NPS 提供不同的功能，具体取决于你所安装的 Windows Server 的版本。

### <a name="windows-server-2016-datacenter-edition"></a>Windows Server 2016Datacenter Edition

与 Windows Server 2016 数据中心中 NPS 中，您可以配置无数 RADIUS 客户端和远程 RADIUS 服务器组。 此外，你可以通过指定 IP 地址范围配置 RADIUS 客户端。

### <a name="windows-server-2016-standard-edition"></a>Windows Server 2016Standard Edition

与 Windows Server 2016 标准中 NPS 中，您可以配置最多个 50 RADIUS 客户端，最多 2 远程 RADIUS 服务器组。 你可以通过使用合法的域名或 IP 地址，定义 RADIUS 客户端，但你无法通过指定 IP 地址范围定义 RADIUS 客户端的组。 如果完整的域名 RADIUS 客户端解析为多个 IP 地址，NPS 服务器使用域名系统 (DNS) 查询中返回的第一个 IP 地址。

以下部分提供有关 NPS RADIUS 服务器代理以及的详细的信息。

## <a name="radius-server-and-proxy"></a>RADIUS 服务器和代理服务器

你可以用作 NPS RADIUS 服务器、RADIUS 代理，或两者都。

### <a name="bkmk_server"></a>RADIUS 服务器

NPS 是由，2866 年 Internet 工程任务强制 \(IETF\) 标准 RADIUS Microsoft 实现。 作为 RADIUS 服务器、NPS 多种类型的网络访问权限，包括无线、进行切换，拨号身份验证和虚拟专用网络 \(VPN\) 远程访问，以及连接到路由器为执行集中的连接身份验证、授权和记帐。

>[!NOTE]
>有关部署 NPS RADIUS 服务器以信息，请参阅[部署网络策略服务器](nps-deploy.md)。

NPS 启用组不同种类的无线、切换远程访问，或 VPN 设备。 你可以使用 NPS 远程访问服务，这适用于 Windows Server 2016。

NPS 使用 Active Directory 域服务 \(AD DS\) 域或当地的安全帐户管理器（三千）用户帐户数据库进行身份验证用户连接有关的凭据。 运行 NPS 服务器是广告 DS 域中的成员，NPS 目录服务用作其用户帐户的数据库，并且单一登录解决方案的一部分。 凭据的同一组用于网络访问控制 \（身份验证和授权的访问权限 network\）并登录到广告 DS 域。

>[!NOTE]
>NPS 用于用户帐户和网络策略的拨属性授权连接。

Internet 服务提供商 \(ISPs\) 和维护网络访问权限的组织有管理所有类型的网络访问权限管理，无论的网络类型的单一从增加的挑战访问使用的设备。 RADIUS 标准同类和不同环境中支持此功能。 RADIUS 是使网络的访问权限设备（用于为 RADIUS 客户端）以验证和到 RADIUS 服务器记帐请求提交客户端服务器协议。

RADIUS 服务器有权访问用户帐户信息，以及可以查看身份验证的网络访问权限的凭据。 如果用户凭据进行验证，尝试连接已授权 RADIUS 服务器授权基于指定的条件，用户访问，然后中记帐日志记录网络的访问权限连接。 允许使用 RADIUS 的网络访问权限的用户身份验证、授权和记帐数据收集和保留在中心的位置，而不是每个访问服务器上。

#### <a name="using-nps-as-a-radius-server"></a>使用 NPS RADIUS 服务器作为

你可以使用 NPS RADIUS 服务器作为时：

- 你所访问客户端与你的用户帐户数据库使用广告 DS 域或当地三千用户帐户的数据库。 
- 你在多个拨号服务器，VPN 服务器上使用远程访问或拨号路由器，并且你想要集中配置网络策略和连接日志记录并记帐。 
- 外包你拨号、VPN 或无线访问服务提供商。 访问服务器使用 RADIUS 以验证和授权由你的组织中的成员的连接。
- 你想要集中身份验证、授权和的一组不同种类访问服务器记帐。

下图显示的 NPS RADIUS 服务器作为各种访问客户端。 

![NPS RADIUS 服务器用作](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>RADIUS 代理

为 RADIUS 代理、NPS 转发给 NPS 和其他 RADIUS 服务器身份验证和记帐消息。 你可以使用 NPS RADIUS 代理来提供 RADIUS 路由 RADIUS 客户端之间消息为 \（也称为网络的访问权限 servers\）和有关连接尝试执行用户身份验证、授权和记帐 RADIUS 服务器。 

当用作 RADIUS 代理，NPS 是中央切换或通过哪些 RADIUS 访问和记帐邮件流路由点。 NPS 记录中记帐日志信息转发的消息。

#### <a name="using-nps-as-a-radius-proxy"></a>使用 NPS RADIUS 代理作为

你可以使用 NPS RADIUS 代理作为时：

- 是的服务提供商提供多个客户外包的拨号、VPN 或无线网络的访问权限的服务。 你的 Nas 发送到 NPS RADIUS 代理连接请求。 根据连接请求中的用户名的领域部分，NPS RADIUS 代理转发给客户的维护 RADIUS 服务器连接请求可以进行身份验证和授权连接尝试。
- 你想要提供身份验证和授权的用户帐户不是的域的 NPS 服务器或另一台已与的域的 NPS 服务器双向信任的域的成员。 这包括不受信任的域单向受信任的域，森林其他帐户。 而不是配置你访问服务器来发送到 NPS RADIUS 服务器他们连接请求时，您可以配置它们将其连接请求发送给 NPS RADIUS 代理。 NPS RADIUS 代理使用的用户名的领域名称部分，并且正确的域或树林中 NPS 服务器向转发请求。 为用户一个域或森林中的帐户可以进行身份验证的另一个域或森林 Nas 尝试连接。
- 你想要使用的不是 Windows 的帐户数据库数据库执行身份验证和授权。 在本例中，连接请求指定的领域名称匹配转发到 RADIUS 服务器，其中有权访问不同的用户帐户和数据授权的数据库。 其他用户的数据库的示例包括 Novell 目录服务 (NDS) 和结构化查询语言 (SQL) 数据库。
- 你想要处理大量连接请求。 在此情况下，而不是配置 RADIUS 客户端尝试在多个 RADIUS 服务器平衡他们连接和记帐请求，您可以配置它们将其连接和记帐请求发送给 NPS RADIUS 代理。 NPS RADIUS 代理动态余额加载的连接和记帐请求跨多个 RADIUS 服务器，并提高每秒的身份验证和处理大量 RADIUS 客户端。
- 你想要提供 RADIUS 身份验证和资源外的服务提供商的授权最小化 intranet 防火墙配置。 周边网络（网络上的 intranet 和 Internet 之间）和 intranet 之间是 intranet 防火墙。 通过周边网络上放置 NPS 服务器，intranet 和网络外围之间防火墙必须允许 NPS 服务器和多个域控制器之间流通信。 通过将替换 NPS 代理 NPS 服务器，防火墙必须允许仅 RADIUS 之间 NPS 代理和在 intranet 的一个或多个 NPS 服务器流通信。

下图显示为之间 RADIUS 客户端和 RADIUS 服务器 RADIUS 代理 NPS。

![NPS RADIUS 代理用作](../../media/Nps-Proxy/Nps-Proxy.jpg)


与 NPS，组织可以还外部远程访问与服务提供商的基础结构资源同时保留控制的用户身份验证、授权和记帐。

以下情况下，可以创建 NPS 配置：

- 无线访问
- 组织拨号或虚拟专用网络 (VPN) 远程访问
- 外包的拨号或无线访问
- Internet 访问权限
- 业务伙伴外部网络资源的身份验证的访问

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>RADIUS 服务器和 RADIUS 的代理配置示例

下面的示例配置展示如何配置 NPS RADIUS 服务器以及 RADIUS 代理。

**NPS RADIUS 服务器用作**。 在此示例中，配置 NPS RADIUS 服务器、默认连接请求策略是唯一配置的策略，并且所有连接进行处理通过本地 NPS 服务器。 NPS 服务器可以身份验证和授权其帐户是 NPS 服务器的域中和受信任的域中的用户。

**NPS RADIUS 代理用作**。 在此示例中，NPS 服务器配置为在不受信任的两个域远程 RADIUS 服务器组转发连接请求 RADIUS 代理。 默认连接请求策略被删除，并两个新的连接要求策略创建向转发两个不受信任的域的每个请求。 在此示例中，NPS 不处理本地服务器上的任何连接请求。 

**NPS RADIUS 服务器和 RADIUS 代理用作**。 除了将指定连接请求本地处理，默认连接请求策略被创建新的连接要求策略转发给 NPS 或不受信任的域中的其他 RADIUS 服务器连接请求。 该第二个策略名为代理策略。 在此示例中，首先有序策略的列表中出现的代理策略。 连接请求匹配代理策略，如果连接请求转发给 RADIUS 服务器远程 RADIUS 服务器组中。 连接请求不匹配代理策略，但与匹配默认连接请求策略，如果 NPS 处理本地服务器上的连接请求。 连接请求不匹配任一策略，如果它已被丢弃。

**NPS RADIUS 服务器远程记帐服务器用作**。 在此示例中，本地 NPS 服务器未配置为执行记帐，并且默认连接请求策略进行修改以使转发给 NPS 服务器或远程 RADIUS 服务器组中的其他 RADIUS 服务器 RADIUS 记帐消息。 虽然转发记帐消息、未转发身份验证和授权的消息，本地 NPS 服务器为地域执行这些功能和所有受信任的域。

**远程 RADIUS Windows 用户映射到与 NPS**。 在此示例中，NPS 的作用作为 RADIUS 服务器，并为每个单独的连接请求 RADIUS 代理通过转发授权使用 Windows 本地用户帐户时对远程 RADIUS 服务器的身份验证请求。 通过连接请求策略的条件配置 Windows 用户映射属性远程 RADIUS 实现此配置。 \ (另外，用户帐户必须创建本地有与同名对其执行远程 RADIUS 服务器身份验证的远程用户帐户 RADIUS 服务器上。\)

## <a name="configuration"></a>配置

配置 NPS RADIUS 服务器作为中，你可以使用标准配置或高级的配置 NPS 控制台或中服务器管理器。 若要为 RADIUS 代理配置 NPS，必须使用高级的配置。

### <a name="standard-configuration"></a>标准配置

使用标准配置，提供了向导以帮助你在以下情况下配置 NPS:

- 拨号或 VPN 连接 RADIUS 服务器
- 用于 802.1 X 有线或无线连接 RADIUS 服务器

配置使用向导 NPS，打开 NPS 主机、选择其中一个上述情形，，然后单击打开向导中的链接。

### <a name="advanced-configuration"></a>高级的配置

当你使用的高级的配置时，手动作为 RADIUS 服务器或 RADIUS 代理配置 NPS。 

若要使用的高级的配置配置 NPS，打开 NPS 主机，然后依次单击旁边的箭头**高级配置**以展开此部分中。

提供以下各项高级的配置。

#### <a name="configure-radius-server"></a>配置 RADIUS 服务器

配置 NPS RADIUS 服务器作为中，您必须配置 RADIUS 客户端、网络策略和 RADIUS 记帐。

使这些配置上的说明进行操作，请参阅下面的主题。

- [配置 RADIUS 客户端](nps-radius-clients-configure.md)
- [配置网络策略](nps-np-configure.md)
- [配置网络策略服务器记帐](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>配置 RADIUS 代理服务器

若要为 RADIUS 代理配置 NPS，必须配置 RADIUS 客户端远程 RADIUS 服务器组，，连接要求策略。

使这些配置上的说明进行操作，请参阅下面的主题。

- [配置 RADIUS 客户端](nps-radius-clients-configure.md)
- [配置远程 RADIUS 服务器组](nps-crp-rrsg-configure.md)
- [配置连接请求策略](nps-crp-configure.md)

## <a name="bkmk_logging"></a>NPS 日志记录

NPS 日志记录也称为 RADIUS 记帐。 将配置 NPS 是否用作 NPS RADIUS 服务器、代理，或者这些配置的任意组合身份登录到你的要求。

若要配置 NPS 日志记录，你必须配置你希望记录的事件，你需要登录并查看事件查看器，然后决定的其他信息。 此外，你必须确定是否想要登录用户身份验证和帐户信息存储在本地计算机上的文本日志文件或 SQL Server 数据库本地计算机或远程计算机上。

有关详细信息，请参阅[配置网络策略服务器记帐](nps-accounting-configure.md)。
