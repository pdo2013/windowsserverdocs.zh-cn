---
title: 网络策略服务器 (NPS)
description: 本主题概述了网络策略服务器在 Windows Server 2016 和 Windows Server 2019，并包括有关 NPS 的附加指导的链接。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: c76483031bdca184e0943738a8c921776440d1fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829028"
---
# <a name="network-policy-server-nps"></a>网络策略服务器 (NPS)

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2019

有关在 Windows Server 2016 和 Windows Server 2019 网络策略服务器的概述，可以使用本主题。 在 Windows Server 2016 和 Server 2019 安装网络策略和访问服务 (NPAS) 功能时安装 NPS。

>[!NOTE]
>除了本主题提供了以下 NPS 文档。
> - [网络策略服务器最佳实践](nps-best-practices.md)
> - [开始使用网络策略服务器](nps-getstart-top.md)
> - [规划网络策略服务器](nps-plan-top.md)
> - [部署网络策略服务器](nps-deploy.md)
> - [管理网络策略服务器](nps-manage-top.md)
> - [网络策略服务器 (NPS) Cmdlet 在 Windows PowerShell 中的](https://technet.microsoft.com/library/jj872739.aspx)适用于 Windows Server 2016 和 Windows 10
> - [网络策略服务器 (NPS) Cmdlet 在 Windows PowerShell 中的](https://technet.microsoft.com/library/jj872739.aspx)适用于 Windows Server 2012 R2 和 Windows 8.1
> - [Windows PowerShell 中的 NPS Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)适用于 Windows Server 2012 和 Windows 8

通过网络策略服务器 (NPS)，你可以针对连接请求身份验证和授权创建并实施组织级网络访问策略。

此外可以将 NPS 配置为将连接请求转发到远程 NPS 或其他 RADIUS 服务器，以便可以请求进行负载平衡连接并将其转发到正确的域进行身份验证的远程身份验证拨入用户服务 (RADIUS) 代理和授权。

NPS 可集中配置和管理网络访问身份验证、 授权和记帐具有以下功能：

- **RADIUS 服务器**。 NPS 为无线身份验证交换机、 远程访问拨号和虚拟专用网络 (VPN) 连接执行集中式身份验证、 授权和记帐。 将 NPS 用作 RADIUS 服务器时，可以将无线访问点和 VPN 服务器等网络访问服务器配置为 NPS 中的 RADIUS 客户端。 也可以配置有关使用 NPS 对连接请求进行授权的网络策略，并且可以配置 RADIUS 记帐，以便 NPS 将记帐信息记录到本地硬盘上或 Microsoft SQL Server 数据库中的日志文件。 有关详细信息，请参阅[RADIUS 服务器](#bkmk_server)。
- **RADIUS 代理**。 当将 NPS 用作 RADIUS 代理时，配置连接请求策略，以告诉的 NPS 哪些连接请求转发给其他 RADIUS 服务器和你想要将连接请求转发哪些 RADIUS 服务器。 也可以配置 NPS，以转发将由远程 RADIUS 服务器组中的一台或多台计算机记录的记帐数据。 若要将 NPS 配置为 RADIUS 代理服务器，请参阅以下主题。 有关详细信息，请参阅[RADIUS 代理](#bkmk_proxy)。
    - [配置连接请求策略](nps-crp-configure.md)
- **RADIUS 记帐**。 你可以配置 NPS 事件记录到本地的日志文件或 Microsoft SQL Server 的本地或远程实例。 有关详细信息，请参阅[NPS 日志记录](#bkmk_logging)。

>[!IMPORTANT]
>网络访问保护\(NAP\)，健康注册机构\(HRA\)，以及主机凭据授权协议\(HCAP\)在 Windows Server 2012 R2 中，不推荐使用和 Windows Server 2016 中不可用。 如果使用的操作系统早于 Windows Server 2016 的 NAP 部署，您不能将您的 NAP 部署迁移到 Windows Server 2016。

您可以使用这些功能的任意组合配置 NPS。 例如，可以配置 NPS 用作 RADIUS 服务器以进行 VPN 连接，而且还作为 RADIUS 代理，以便将某些连接请求转发到身份验证和授权，另一个域中的远程 RADIUS 服务器组的成员。

## <a name="windows-server-editions-and-nps"></a>Windows Server 版本和 NPS

NPS 提供不同的功能，具体取决于你安装的 Windows Server 版本。

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 或 Windows Server 2019 Standard/数据中心版

使用 Windows Server 2016 Standard 或 Datacenter 中的 NPS，可以配置无限的数量的 RADIUS 客户端和远程 RADIUS 服务器组。 此外，你还可通过指定一个 IP 地址范围来配置 RADIUS 客户端。

>[!NOTE]
>WIndows 网络策略和访问服务功能不可用在使用服务器核心安装选项安装的系统上。

以下部分提供有关 NPS 作为 RADIUS 服务器和代理的更多详细的信息。

## <a name="radius-server-and-proxy"></a>RADIUS 服务器和代理

可以将 NPS 用作 RADIUS 服务器、 RADIUS 代理，或两者。

### <a name="bkmk_server"></a>RADIUS 服务器

NPS 是 RADIUS 标准指定由 Internet 工程任务组的 Microsoft 实现\(IETF\) Rfc 2865 和 2866年中。 NPS 用作 RADIUS 服务器，执行集中化的连接身份验证、 授权和记帐的许多类型的网络访问权限，包括无线、 身份验证切换、 拨号和虚拟专用网络\(VPN\)远程访问和路由器到路由器的连接。

>[!NOTE]
>部署 NPS 作为 RADIUS 服务器的信息，请参阅[部署网络策略服务器](nps-deploy.md)。

NPS 支持组异类无线、 交换机、 远程访问或 VPN 设备的使用。 使用远程访问服务时，可在 Windows Server 2016，可以使用 NPS。

NPS 使用 Active Directory 域服务\(AD DS\)域或本地安全帐户管理器 (SAM) 用户帐户数据库的连接尝试的用户凭据进行身份验证。 运行 NPS 的服务器是 AD DS 域的成员，NPS 将目录服务用作其用户帐户数据库，并且单一登录解决方案的一部分。 同一组凭据用于网络访问控制\(进行身份验证和授权对网络进行访问\)并登录到 AD DS 域。

>[!NOTE]
>NPS 使用用户帐户和网络策略的拨入属性的连接进行授权。

Internet 服务提供商\(Isp\)和维护网络访问的组织都从单一管理，而不考虑类型的网络访问点管理所有类型的网络访问权限的增加的挑战使用的设备。 RADIUS 标准在同类和异类环境中支持此功能。 RADIUS 是使得网络访问设备 （用作 RADIUS 客户端） 来提交身份验证和记帐请求到 RADIUS 服务器的客户端-服务器协议。

RADIUS 服务器有权访问用户帐户信息，并可以检查网络访问身份验证凭据。 如果用户凭据进行身份验证，并且连接尝试进行授权，RADIUS 服务器根据指定条件的用户访问权限可授权和记帐日志中记录的网络访问连接。 使用 RADIUS 允许网络访问用户身份验证、 授权和记帐数据，以进行收集和维护在一个中心位置，而不是每个访问服务器上。

#### <a name="using-nps-as-a-radius-server"></a>使用 NPS 作为 RADIUS 服务器

您可以将 NPS 用作 RADIUS 服务器时：

- 使用 AD DS 域或本地 SAM 用户帐户数据库作为用户帐户数据库访问客户端。 
- 将远程访问多个拨号服务器、 VPN 服务器或请求拨号路由器，并且想要集中配置网络策略和连接日志记录和记帐。 
- 外包拨号、 VPN 或无线访问服务提供程序。 访问服务器使用 RADIUS 身份验证和授权由你的组织的成员进行的连接。
- 你想要集中身份验证、 授权和记帐的一组不同种类的访问服务器。

下图显示 NPS 用作 RADIUS 服务器的访问客户端有多种。 

![NPS 用作 RADIUS 服务器](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>RADIUS 代理

为 RADIUS 代理，NPS 将身份验证和记帐消息转发到 NPS 和其他 RADIUS 服务器。 因为 RADIUS 代理，以提供路由的 RADIUS 消息之间的 RADIUS 客户端，您可以使用 NPS\(也称为网络访问服务器\)和执行用户身份验证、 授权和记帐的 RADIUS 服务器连接尝试。 

当用作 RADIUS 代理，NPS 是中央切换点或路由点通过 RADIUS 访问和记帐消息流。 NPS 记录到记帐日志中有关信息都将被转发的消息。

#### <a name="using-nps-as-a-radius-proxy"></a>使用 NPS 作为 RADIUS 代理

您可以将 NPS 用作 RADIUS 代理时：

- 是服务提供商，为多个客户提供外包的拨号、 VPN 或无线网络访问服务。 你 Nas 将连接请求发送到 NPS RADIUS 代理。 根据连接请求中的用户名的领域部分，NPS RADIUS 代理将转发连接请求发送到客户维持的 RADIUS 服务器和可以进行身份验证和授权连接尝试。
- 你想要为不是在 NPS 中的域或具有在 NPS 中的域具有双向信任关系的另一个域的成员的用户帐户提供身份验证和授权。 这包括帐户中不受信任的域、 单向受信任的域和其他林。 而不是你访问服务器配置为将其连接请求发送到 NPS RADIUS 服务器，则可以配置它们以将其连接请求发送到 NPS RADIUS 代理。 NPS RADIUS 代理使用的用户名的领域名称部分，并将请求转发到正确的域或林中的 NPS。 在一个域或林中的帐户可以进行身份验证的 nas 接受在另一个域或林中的用户的连接尝试。
- 你想要使用的数据库，不是一个 Windows 帐户数据库执行身份验证和授权。 在这种情况下，与指定的领域名称匹配的连接请求转发到 RADIUS 服务器，有权访问的用户帐户和授权数据执行不同的数据库。 其他用户数据库的示例包括 Novell 目录服务 (NDS) 和结构化查询语言 (SQL) 数据库。
- 你想要处理大量的连接请求。 在这种情况下，而不是配置 RADIUS 客户端可以尝试跨多个 RADIUS 服务器平衡其连接和记帐请求，则可以配置它们以将其连接和记帐请求发送到 NPS RADIUS 代理。 NPS RADIUS 代理动态地平衡负载的连接和记帐请求跨多个 RADIUS 服务器，并会增加大量的 RADIUS 客户端的处理和第二次身份验证。
- 你想要为外包的服务提供商提供 RADIUS 身份验证和授权和最大程度减少 intranet 防火墙配置。 在 intranet 防火墙是外围网络 （intranet 和 Internet 之间的网络） 和 intranet 之间。 通过将 NPS 放在外围网络上，外围网络和 intranet 之间的防火墙必须允许 NPS 和多个域控制器之间进行通信。 通过将 NPS 代理替换 NPS，防火墙必须允许仅 RADIUS 通信在 NPS 代理和 intranet 内的一个或多个 NPSs 之间流动。

下图显示 NPS 作为 RADIUS 客户端和 RADIUS 服务器之间的 RADIUS 代理。

![NPS 用作 RADIUS 代理](../../media/Nps-Proxy/Nps-Proxy.jpg)


使用 NPS，组织可以还将向服务提供程序的远程访问基础结构外包同时保留对用户身份验证、 授权和记帐的控制。

可以在下列情况下创建的 NPS 配置：

- 无线访问
- 组织拨号或虚拟专用网络 (VPN) 远程访问
- 外包的拨号或无线访问
- Internet 访问权限
- 经过身份验证的访问业务合作伙伴 extranet 资源

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>RADIUS 服务器和 RADIUS 代理配置示例

以下配置示例演示如何可以将 NPS 配置为 RADIUS 服务器和 RADIUS 代理。

**NPS 用作 RADIUS 服务器**。 在此示例中，NPS 配置为 RADIUS 服务器，默认连接请求策略是唯一配置的策略由本地 NPS 处理所有连接请求。 NPS 可以进行身份验证和授权的用户使用其帐户是域中的 NPS 和受信任域中。

**NPS 用作 RADIUS 代理**。 在此示例中，NPS 配置为 RADIUS 代理将连接请求转发到两个不受信任域中的远程 RADIUS 服务器组。 删除默认连接请求策略，并请求转发到的每个两个不受信任域创建两个新的连接请求策略。 在此示例中，NPS 不处理本地服务器上的任何连接请求。 

**NPS 用作 RADIUS 服务器和 RADIUS 代理**。 除了默认连接请求策略，它指定了本地处理连接请求，被创建新的连接请求策略，将连接请求转发到的 NPS 或其他 RADIUS 服务器位于不受信任域中。 此第二个策略是名为代理策略。 在此示例中，代理策略将显示策略的有序列表中的第一行。 如果连接请求与代理策略匹配，则连接请求转发到远程 RADIUS 服务器组中的 RADIUS 服务器。 如果连接请求与代理策略不匹配，但与默认连接请求策略匹配，NPS 会处理本地服务器上的连接请求。 如果连接请求与任一策略不匹配，则会放弃。

**NPS 用作 RADIUS 服务器的远程记帐服务器**。 在此示例中，本地 NPS 未配置为执行记帐和默认连接请求策略进行修改以使 RADIUS 记帐消息转发到的 NPS 或其他 RADIUS 服务器的远程 RADIUS 服务器组中。 尽管转发记帐消息，但不转发身份验证和授权消息，并且本地 NPS 执行这些函数中的本地域和所有受信任的域。

**与远程 RADIUS 到 Windows 用户映射的 NPS**。 在此示例中，NPS 充当 RADIUS 服务器和 RADIUS 代理，为每个单独的连接请求通过将身份验证请求转发到远程 RADIUS 服务器同时使用本地 Windows 用户帐户进行授权。 此配置是通过将配置为连接请求策略的条件的远程 RADIUS 到 Windows 用户映射的属性实现的。 \(此外，必须与远程 RADIUS 服务器对其执行身份验证的远程用户帐户同名的 RADIUS 服务器上本地创建的用户帐户。\)

## <a name="configuration"></a>配置

若要将 NPS 配置为 RADIUS 服务器，可以在 NPS 控制台中或服务器管理器中使用标准配置或高级的配置。 若要将 NPS 配置为 RADIUS 代理，必须使用高级的配置。

### <a name="standard-configuration"></a>标准配置

使用标准配置提供向导来帮助您配置 NPS 以用于以下方案：

- 用于拨号或 VPN 连接的 RADIUS 服务器
- 用于 802.1X 无线或有线连接的 RADIUS 服务器

若要配置 NPS 使用向导，请打开 NPS 控制台，选择上述方案之一，然后单击打开向导的链接。

### <a name="advanced-configuration"></a>高级的配置

当你使用高级的配置时，手动将 NPS 配置为 RADIUS 服务器或 RADIUS 代理。 

若要通过使用高级的配置来配置 NPS，打开 NPS 控制台中，，然后单击箭头下一步**高级配置**可展开该部分。

提供的以下高级的配置项。

#### <a name="configure-radius-server"></a>配置 RADIUS 服务器

若要将 NPS 配置为 RADIUS 服务器，必须配置 RADIUS 客户端、 网络策略和 RADIUS 记帐。

进行这些配置的说明，请参阅以下主题。

- [配置 RADIUS 客户端](nps-radius-clients-configure.md)
- [配置网络策略](nps-np-configure.md)
- [配置网络策略服务器记帐](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>配置 RADIUS 代理

若要将 NPS 配置为 RADIUS 代理，必须配置 RADIUS 客户端、 远程 RADIUS 服务器组和连接请求策略。

进行这些配置的说明，请参阅以下主题。

- [配置 RADIUS 客户端](nps-radius-clients-configure.md)
- [配置远程 RADIUS 服务器组](nps-crp-rrsg-configure.md)
- [配置连接请求策略](nps-crp-configure.md)

## <a name="bkmk_logging"></a>NPS 日志记录

NPS 日志记录也称为 RADIUS 记帐。 配置 NPS 日志记录与您的需求，是否将 NPS 用作 RADIUS 服务器、 代理或这些配置的任意组合。

若要配置 NPS 日志记录，必须配置要记录哪些事件想记录和查看使用事件查看器，然后再来决定哪些其他信息。 此外，您必须决定是否想要用户身份验证和记帐信息记录到文本日志文件存储在本地计算机上或在本地计算机或远程计算机上的 SQL Server 数据库。

有关详细信息，请参阅[配置网络策略服务器记帐](nps-accounting-configure.md)。
