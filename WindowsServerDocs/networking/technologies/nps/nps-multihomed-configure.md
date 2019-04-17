---
title: 配置 NPS 多主计算机上
description: 本主题提供配置服务器的多个运行 Windows Server 2016 中的网络策略服务器的网络适配器上的说明进行操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f80e83a4d79036729b6b442e6362d52fbda12edd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-on-a-multihomed-computer"></a>配置 NPS 多主计算机上

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题配置 NPS 服务器具有多个网络适配器。

当你在运行网络策略 Server (NPS) 服务器中使用多个网络适配器时，您可以配置以下各项：

- 执行，并执行不发送和接收远程身份验证拨入用户服务 \(RADIUS\) 交通网络适配器。
- 针对每个网络适配器，是否 NPS 会监视 Internet 协议第四版 \(IPv4\)、 IPv6，或 IPv4 和上 IPv6 RADIUS 流量。
- 通过哪些 RADIUS 发送和接收上每协议通讯 UDP 端口号 \ （IPv4 或 IPv6\），每个网络适配器基础。

默认情况下，NPS 侦听上端口 1812年 1813年、 1645年，1646 RADIUS 交通 IPv6 和 IPv4 为所有已安装的网络适配器。 因为 NPS RADIUS 交通将自动使用的所有网络适配器，你只需指定希望 NPS RADIUS 用于网络适配器交通的方法，当你想要阻止 NPS 使用特定的网络适配器。

>[!NOTE]
>如果你卸载网络适配器 IPv4 或 IPv6，NPS 如此监视器已卸载协议的 RADIUS 通信。

NPS 在服务器上已安装了多个网络适配器，你可能需要配置 NPS 发送和接收 RADIUS 仅在你所指定的适配器上的交通。

例如，一个 NPS 服务器中安装的网络适配器可能会导致网络段不包含 RADIUS 客户端，在第二个时网络适配器提供 NPS 与网络路径为其配置 RADIUS 客户端。 这种情况下，在很重要直接 NPS RADIUS 的所有流量使用该第二个的网络适配器。

在另一个示例中，如果 NPS 服务器有三个安装网络适配器，但你只想 NPS RADIUS 流量的使用两个适配器可以配置端口这两种适配器的信息。 通过排除的第三个适配器端口配置，你防止 NPS RADIUS 流量的使用适配器。

## <a name="using-a-network-adapter"></a>使用网络适配器

若要配置 NPS 聆听并发送 RADIUS 流量网络适配器，请在 NPS 控制台在属性对话框网络策略服务器上使用以下语法：

- IPv4 交通语法： IPAddress:UDPport，其中 ip 地址是配置你希望发送 RADIUS 通信的网络适配器的 IPv4 地址，UDPport 是你想要使用 RADIUS 身份验证或记帐通信 RADIUS 端口号。
- IPv6 交通语法: [IPv6Address]: UDPport，其中 IPv6Address 方括号都需要、 IPv6Address 配置你希望发送 RADIUS 通信的网络适配器的 IPv6 地址并且 UDPport 你想要使用 RADIUS 身份验证或记帐通信 RADIUS 端口号。

以下字符可作为分隔符配置 IP 地址和 UDP 端口的信息：

- 地址月端口分隔符： 分号 （:）
- 端口分隔符： 逗号 （，）
- 接口分隔符： 分号 （;）

## <a name="configuring-network-access-servers"></a>配置网络的访问权限服务器

请确保你的网络访问权限服务器配置您将配置你 NPS 服务器的同一 RADIUS UDP 端口号。 RADIUS 标准 UDP 端口中 2865年，可 Rfc 定义是 1812 进行身份验证和 1813 记帐;但是，某些访问服务器配置默认情况下用于用于身份验证请求 1645年端口 UDP 和 UDP 端口 1646年记帐请求。

>[!IMPORTANT]
>如果你不使用 RADIUS 默认端口号，你必须在本地计算机允许 RADIUS 流量新端口上的防火墙配置例外。 有关详细信息，请参阅[RADIUS 交通配置防火墙](nps-firewalls-configure.md)。

## <a name="configure-the-multihomed-nps-server"></a>配置多主 NPS 服务器

你可以使用下面的过程配置 NPS 多主服务器。

在会员**域管理员**，或等效的最低要求完成此过程。

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>若要指定的网络适配器和 NPS RADIUS 流量的使用的 UDP 端口

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**以打开 NPS 主机。

2. 右键单击**网络策略服务器**，然后单击**属性**。

3. 单击**端口**选项卡，然后在你想要使用现有端口号到 RADIUS 交通该网络适配器的 IP 地址。 例如，如果你想要使用的 IP 地址 192.168.1.2 和 1812年和 1645 RADIUS 端口身份验证请求、 更改端口设置从**1812,1645**到**192.168.1.2:1812,1645**。 如果你 RADIUS 身份验证和 RADIUS 会计 UDP 端口不同的默认值，请更改端口设置相应地。

4. 若要进行身份验证或记帐请求使用多个端口设置，请用逗号分隔端口号。

有关 NPS UDP 端口的详细信息，请参阅[配置 NPS UDP 端口信息](nps-udp-ports-configure.md)


有关 NPS 的详细信息，请参阅[网络策略服务器](nps-top.md)

