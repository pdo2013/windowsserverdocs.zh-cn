---
title: 配置 RADIUS 交通防火墙
description: 本主题提供如何配置适用于 Windows Server 2016 中的网络策略 Server 允许 RADIUS 流量的防火墙的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 140111e10eabbece098ae9b7c36746cc663c9cce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewalls-for-radius-traffic"></a>配置 RADIUS 交通防火墙

>适用于：Windows Server（半年通道），Windows Server 2016

防火墙可以配置允许或阻止类型的计算机或设备运行防火墙进出 IP 流量。 如果防火墙内容不正确配置允许之间 RADIUS 客户端 RADIUS 流量，可以失败 RADIUS 代理服务器和 RADIUS 服务器的网络访问权限身份验证阻止用户访问网络资源。 

你可能需要配置两种类型的防火墙允许 RADIUS 流量：

- Windows Defender 本地运行的网络策略 Server (NPS) 服务器上的防火墙高级安全。
- 防火墙可以在其他计算机或硬件设备上运行。

## <a name="windows-firewall-on-the-local-nps-server"></a>在本地 NPS 服务器上的 Windows 防火墙

默认情况下，NPS 发送和接收通过使用 1812 年 1813 年、1645 年，1646 年用户数据报协议 \(UDP\) 端口 RADIUS 通信。 Windows Defender 自动将 NPS 服务器上的防火墙配置例外，期间 NPS 允许此 RADIUS 流量发送和接收安装。

因此，如果你使用的默认 UDP 端口，你不必更改 Windows Defender 防火墙配置，以允许来回服务器 NPS RADIUS 通信。

在某些情况下，你可能想要更改 NPS RADIUS 流量的使用的端口。 如果配置 NPS 和您的网络访问权限服务器来发送和接收以外默认值端口上的 RADIUS 流量，你必须执行以下操作：

- 删除允许默认端口上的 RADIUS 流量异常。
- 创建新允许 RADIUS 流量新端口上的例外。

有关详细信息，请参阅[配置 NPS UDP 端口信息](nps-udp-ports-configure.md)。

## <a name="other-firewalls"></a>其他防火墙

最常见的配置中防火墙连接到 Internet 并且 NPS 服务器连接到外围网络 intranet 资源。

若要访问内 intranet 域控制器，可能必须 NPS 服务器：

- 周边网络和网上接口接口（IP 路由未启用）。 
- 周边网络上的单个接口。 在此配置，NPS 与该域控制器通过连接到 intranet 外围网络的其他防火墙进行通信。

## <a name="configuring-the-internet-firewall"></a>配置 Internet 的防火墙

连接到 Internet 的防火墙必须使用其 Internet 界面上输入和输出 filters 配置 \（，也可以其网络外围 interface\），在 Internet 上允许 RADIUS 之间的消息 NPS 服务器和 RADIUS 客户端或代理服务器转移。 其他筛选器可用于允许传递交通 Web 服务器、VPN 服务器和其他类型的服务器外围网络上。

单独的输入和输出数据包可以 Internet 界面和外围网络接口配置了筛选器。

### <a name="configure-input-filters-on-the-internet-interface"></a>配置 Internet 界面上输入筛选器

配置 Internet 的防火墙允许以下类型的交通接口上以下输入的包筛选器：

- 周边网络接口和 UDP 目标 1812 (0x714) NPS 服务器端口目标 IP 地址。  此筛选器允许 RADIUS 身份验证流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这将是 NPS，使用默认 UDP 端口 RFC 2865 中定义。 如果你使用的其他端口，可为 1812 年替换端口号。
- 周边网络接口和 UDP 目标 1813 (0x715) NPS 服务器端口目标 IP 地址。 此筛选器允许 RADIUS 会计流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这是 RFC 2866 在定义 NPS，使用默认 UDP 端口。 如果你使用的其他端口，可用于 1813 年替换端口号。
- \(Optional\) 外围网络接口目标 IP 地址和 UDP 目标 1645 年 \(0x66D\) NPS 服务器端口。 此筛选器允许 RADIUS 身份验证流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这是使用的较旧 RADIUS 客户端 UDP 端口。
- \(Optional\) 外围网络接口目标 IP 地址和 UDP 目标 1646 年 \(0x66E\) NPS 服务器端口。 此筛选器允许 RADIUS 会计流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这是使用的较旧 RADIUS 客户端 UDP 端口。

### <a name="configure-output-filters-on-the-internet-interface"></a>在 Internet 界面配置输出筛选器

Internet 的防火墙允许以下类型的交通接口上配置了以下输出筛选器：

- 周边网络接口和 UDP 源端口 NPS 服务器的 1812 (0x714) 的源代码 IP 地址。 此筛选器允许从服务器 NPS RADIUS 基于 Internet 的客户端到 RADIUS 身份验证流量。 这将是 NPS，使用默认 UDP 端口 RFC 2865 中定义。 如果你使用的其他端口，可为 1812 年替换端口号。
- 周边网络接口和 UDP 源端口 NPS 服务器的 1813 (0x715) 的源代码 IP 地址。 此筛选器允许从 NPS 服务器 RADIUS 基于 Internet 的客户端到 RADIUS 会计流量。 这是 RFC 2866 在定义 NPS，使用默认 UDP 端口。 如果你使用的其他端口，可用于 1813 年替换端口号。
- \(Optional\) 外围网络接口源 IP 地址和 UDP 源 1645 年 \(0x66D\) NPS 服务器端口。 此筛选器允许从服务器 NPS RADIUS 基于 Internet 的客户端到 RADIUS 身份验证流量。 这是使用的较旧 RADIUS 客户端 UDP 端口。
- \(Optional\) 外围网络接口源 IP 地址和 UDP 源 1646 年 \(0x66E\) NPS 服务器端口。 此筛选器允许从 NPS 服务器 RADIUS 基于 Internet 的客户端到 RADIUS 会计流量。 这是使用的较旧 RADIUS 客户端 UDP 端口。

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>配置周边网络接口上输入筛选器

配置上防火墙允许交通以下类型的外围网络接口以下输入筛选器：

- 周边网络接口和 UDP 源端口 NPS 服务器的 1812 (0x714) 的源代码 IP 地址。 此筛选器允许从服务器 NPS RADIUS 基于 Internet 的客户端到 RADIUS 身份验证流量。 这将是 NPS，使用默认 UDP 端口 RFC 2865 中定义。 如果你使用的其他端口，可为 1812 年替换端口号。
- 周边网络接口和 UDP 源端口 NPS 服务器的 1813 (0x715) 的源代码 IP 地址。 此筛选器允许从 NPS 服务器 RADIUS 基于 Internet 的客户端到 RADIUS 会计流量。 这是 RFC 2866 在定义 NPS，使用默认 UDP 端口。 如果你使用的其他端口，可用于 1813 年替换端口号。
- \(Optional\) 外围网络接口源 IP 地址和 UDP 源 1645 年 \(0x66D\) NPS 服务器端口。 此筛选器允许从服务器 NPS RADIUS 基于 Internet 的客户端到 RADIUS 身份验证流量。 这是使用的较旧 RADIUS 客户端 UDP 端口。
- \(Optional\) 外围网络接口源 IP 地址和 UDP 源 1646 年 \(0x66E\) NPS 服务器端口。 此筛选器允许从 NPS 服务器 RADIUS 基于 Internet 的客户端到 RADIUS 会计流量。 这是使用的较旧 RADIUS 客户端 UDP 端口。

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>周边网络接口上配置输出筛选器

防火墙允许交通以下类型的外围网络接口上配置以下输出数据包筛选器：

- 周边网络接口和 UDP 目标 1812 (0x714) NPS 服务器端口目标 IP 地址。 此筛选器允许 RADIUS 身份验证流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这将是 NPS，使用默认 UDP 端口 RFC 2865 中定义。 如果你使用的其他端口，可为 1812 年替换端口号。
- 周边网络接口和 UDP 目标 1813 (0x715) NPS 服务器端口目标 IP 地址。 此筛选器允许 RADIUS 会计流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这是 RFC 2866 在定义 NPS，使用默认 UDP 端口。 如果你使用的其他端口，可用于 1813 年替换端口号。
- \(Optional\) 外围网络接口目标 IP 地址和 UDP 目标 1645 年 \(0x66D\) NPS 服务器端口。 此筛选器允许 RADIUS 身份验证流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这是使用的较旧 RADIUS 客户端 UDP 端口。
- \(Optional\) 外围网络接口目标 IP 地址和 UDP 目标 1646 年 \(0x66E\) NPS 服务器端口。 此筛选器允许 RADIUS 会计流量 RADIUS 基于 Internet 的客户端从 NPS 服务器。 这是使用的较旧 RADIUS 客户端 UDP 端口。

为了增加安全性，你可以使用的 IP 地址的每个 RADIUS 客户端发送通过防火墙定义外围网络上的通信之间客户端和服务器 NPS 的 IP 地址筛选器数据包。

### <a name="filters-on-the-perimeter-network-interface"></a>周边网络接口上筛选器

配置上的 intranet 防火墙允许交通以下类型的外围网络接口以下输入的包筛选器：

- 源 NPS 服务器外围网络接口 IP 地址。 此筛选器外围网络上允许从 NPS 服务器的交通。

Intranet 防火墙允许交通以下类型的外围网络接口上配置了以下输出筛选器：

- 目标 NPS 服务器外围网络接口 IP 地址。 此筛选器外围网络上允许 NPS 服务器的交通。

### <a name="filters-on-the-intranet-interface"></a>上的 intranet 接口筛选器

配置上的 intranet 接口防火墙允许以下类型的交通以下输入筛选器：

- 目标 NPS 服务器外围网络接口 IP 地址。 此筛选器外围网络上允许 NPS 服务器的交通。

配置上的 intranet 接口防火墙允许以下类型的交通以下输出数据包筛选器：

- 源 NPS 服务器外围网络接口 IP 地址。 此筛选器外围网络上允许从 NPS 服务器的交通。


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。




