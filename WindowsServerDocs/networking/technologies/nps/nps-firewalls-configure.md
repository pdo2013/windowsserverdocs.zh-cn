---
title: 配置 RADIUS 通信防火墙
description: 本主题概述了如何将防火墙配置为允许 Windows Server 2016 中的网络策略服务器的 RADIUS 流量。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94b03349f21a40f9bf42508d5a2878a5cf2946cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819448"
---
# <a name="configure-firewalls-for-radius-traffic"></a>配置 RADIUS 通信防火墙

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以将防火墙配置为允许或阻止的 IP 通信来回于计算机或设备在其运行防火墙的类型。 如果防火墙未正确配置以允许 RADIUS 客户端之间的 RADIUS 流量，RADIUS 代理和 RADIUS 服务器，网络访问身份验证可能会失败，从而阻止用户访问网络资源。 

您可能需要配置防火墙以允许 RADIUS 通信的两种类型：

- 运行网络策略服务器 (NPS) 的本地服务器上的高级安全 Windows Defender 防火墙。
- 防火墙在其他计算机或硬件设备上运行。

## <a name="windows-firewall-on-the-local-nps"></a>在本地 NPS 上的 Windows 防火墙

默认情况下，NPS 将发送和接收 RADIUS 流量使用用户数据报协议\(UDP\)端口 1812年、 1813年、 1645年和 1646年。 自动将 NPS 上的 Windows Defender 防火墙配置例外情况之外，在 NPS 中，以允许发送和接收此 RADIUS 流量的安装过程中。

因此，如果使用的默认 UDP 端口，您不必更改 Windows Defender 防火墙配置，以允许 RADIUS 通信来回于 NPSs。

在某些情况下，你可能想要更改 NPS 用于 RADIUS 流量的端口。 如果您配置 NPS 和网络访问服务器发送和接收默认值以外的端口上的 RADIUS 流量，必须执行以下操作：

- 删除允许默认端口上的 RADIUS 流量的例外。
- 创建允许新端口上的 RADIUS 流量的新异常。

有关详细信息，请参阅[配置 NPS UDP 端口信息](nps-udp-ports-configure.md)。

## <a name="other-firewalls"></a>其他防火墙

在最常见的配置中，防火墙连接到 Internet，NPS 是连接到外围网络的 intranet 资源。

若要访问 intranet 内的域控制器，可能必须在 NPS:

- 外围网络和 intranet 上的接口上的接口 （IP 路由未启用）。 
- 外围网络上的单个接口。 在此配置中，NPS 将与通过另一个连接到 intranet 外围网络的防火墙的域控制器进行通信。

## <a name="configuring-the-internet-firewall"></a>配置 Internet 防火墙

连接到 Internet 的防火墙必须配置有其 Internet 接口上的输入和输出筛选器\(和 （可选） 及其网络外围接口\)，以允许 NPS 之间的 RADIUS 消息转发和 RADIUS 客户端或 Internet 上的代理。 可以使用其他筛选器，以允许将通信传递到 Web 服务器、 VPN 服务器和其他类型的服务器在外围网络上。

单独的输入和输出数据包筛选器可以在 Internet 接口和外围网络接口配置。

### <a name="configure-input-filters-on-the-internet-interface"></a>Internet 接口上配置输入筛选器

防火墙以允许以下类型的通信的 Internet 接口上配置以下输入的数据包筛选器：

- 外围网络接口和 UDP 目标端口 1812 (0x714) 的 NPS 的目标 IP 地址。  向 NPS 情况下，此筛选器允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 身份验证流量。 在 RFC 2865 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1812年。
- 外围网络接口和 UDP 目标端口 1813 (0x715) 的 NPS 的目标 IP 地址。 此筛选器向 NPS 允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 记帐通信。 RFC 2866 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1813年。
- \(可选\)目标 IP 地址的外围网络接口和 UDP 目标端口 1645年\(0x66D\)的 NPS。 向 NPS 情况下，此筛选器允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 身份验证流量。 这是旧 RADIUS 客户端使用的 UDP 端口。
- \(可选\)目标 IP 地址的外围网络接口和 UDP 目标端口 1646年\(0x66E\)的 NPS。 此筛选器向 NPS 允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 记帐通信。 这是旧 RADIUS 客户端使用的 UDP 端口。

### <a name="configure-output-filters-on-the-internet-interface"></a>Internet 接口上配置输出筛选器

防火墙以允许以下类型的通信的 Internet 接口上配置以下输出筛选器：

- 外围网络接口和 UDP 源端口 1812 (0x714) 的 NPS 的源 IP 地址。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 身份验证流量。 在 RFC 2865 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1812年。
- 外围网络接口和 UDP 源端口 1813 (0x715) 的 NPS 的源 IP 地址。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 记帐通信。 RFC 2866 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1813年。
- \(可选\)源 IP 地址的外围网络接口和 UDP 源端口 1645年\(0x66D\)的 NPS。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 身份验证流量。 这是旧 RADIUS 客户端使用的 UDP 端口。
- \(可选\)源 IP 地址的外围网络接口和 UDP 源端口 1646年\(0x66E\)的 NPS。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 记帐通信。 这是旧 RADIUS 客户端使用的 UDP 端口。

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>外围网络接口上配置输入筛选器

防火墙以允许以下类型的通信的外围网络接口上配置以下输入筛选器：

- 外围网络接口和 UDP 源端口 1812 (0x714) 的 NPS 的源 IP 地址。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 身份验证流量。 在 RFC 2865 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1812年。
- 外围网络接口和 UDP 源端口 1813 (0x715) 的 NPS 的源 IP 地址。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 记帐通信。 RFC 2866 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1813年。
- \(可选\)源 IP 地址的外围网络接口和 UDP 源端口 1645年\(0x66D\)的 NPS。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 身份验证流量。 这是旧 RADIUS 客户端使用的 UDP 端口。
- \(可选\)源 IP 地址的外围网络接口和 UDP 源端口 1646年\(0x66E\)的 NPS。 此筛选器为基于 Internet 的 RADIUS 客户端从 NPS 允许 RADIUS 记帐通信。 这是旧 RADIUS 客户端使用的 UDP 端口。

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>外围网络接口上配置输出筛选器

防火墙以允许以下类型的通信的外围网络接口上配置以下输出数据包筛选器：

- 外围网络接口和 UDP 目标端口 1812 (0x714) 的 NPS 的目标 IP 地址。 向 NPS 情况下，此筛选器允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 身份验证流量。 在 RFC 2865 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1812年。
- 外围网络接口和 UDP 目标端口 1813 (0x715) 的 NPS 的目标 IP 地址。 此筛选器向 NPS 允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 记帐通信。 RFC 2866 中定义，这是 NPS 使用的默认 UDP 端口。 如果使用其他端口，该端口号替换为 1813年。
- \(可选\)目标 IP 地址的外围网络接口和 UDP 目标端口 1645年\(0x66D\)的 NPS。 向 NPS 情况下，此筛选器允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 身份验证流量。 这是旧 RADIUS 客户端使用的 UDP 端口。
- \(可选\)目标 IP 地址的外围网络接口和 UDP 目标端口 1646年\(0x66E\)的 NPS。 此筛选器向 NPS 允许来自基于 Internet 的 RADIUS 客户端的 RADIUS 记帐通信。 这是旧 RADIUS 客户端使用的 UDP 端口。

为了提高安全性，可以使用发送的数据包通过防火墙的客户端和 NPS 的 IP 地址之间的流量筛选器定义外围网络上每个 RADIUS 客户端的 IP 地址。

### <a name="filters-on-the-perimeter-network-interface"></a>外围网络接口上的筛选器

Intranet 防火墙以允许以下类型的通信的外围网络接口上配置以下输入的数据包筛选器：

- NPS 的外围网络接口的源 IP 地址。 此筛选器允许流量从外围网络上的 NPS。

Intranet 防火墙以允许以下类型的通信的外围网络接口上配置以下输出筛选器：

- NPS 的外围网络接口的目标 IP 地址。 此筛选器允许外围网络上向 NPS 的通信。

### <a name="filters-on-the-intranet-interface"></a>Intranet 接口上的筛选器

防火墙以允许以下类型的通信的 intranet 接口上配置以下输入筛选器：

- NPS 的外围网络接口的目标 IP 地址。 此筛选器允许外围网络上向 NPS 的通信。

防火墙以允许以下类型的通信的 intranet 接口上配置以下输出数据包筛选器：

- NPS 的外围网络接口的源 IP 地址。 此筛选器允许流量从外围网络上的 NPS。


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。




