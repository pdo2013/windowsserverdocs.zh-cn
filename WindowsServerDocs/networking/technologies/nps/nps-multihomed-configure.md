---
title: 在多宿主计算机上配置 NPS
description: 本主题说明了使用 Windows Server 2016 中运行网络策略服务器的多个网络适配器配置服务器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55eccf3afc649e84c5b6f5ce7932ed97617ddca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856798"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>在多宿主计算机上配置 NPS

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于使用多个网络适配器配置 NPS。

当运行网络策略服务器 (NPS) 的服务器中使用多个网络适配器时，您可以进行下列配置：

- 执行操作，并执行不发送和接收远程身份验证拨入用户服务的网络适配器\(RADIUS\)流量。
- 是否针对每个网络适配器，NPS 监控 Internet 协议版本 4 上的 RADIUS 流量\(IPv4\)，IPv6 或 IPv4 和 IPv6。
- 对哪些 RADIUS 流量是发送和接收每个协议的 UDP 端口号\(IPv4 或 IPv6\)，每个网络适配器基础。

默认情况下，NPS 侦听端口 1812年、 1813年、 1645年和 1646年上的 RADIUS 流量的 IPv6 和 IPv4 所有已安装的网络适配器。 由于 NPS 自动对 RADIUS 流量使用所有网络适配器，只需指定你想要阻止 NPS 使用特定网络适配器的网络适配器，要让 NPS 用于 RADIUS 流量。

>[!NOTE]
>如果您卸载网络适配器上的 IPv4 或 IPv6，NPS 不会监视已卸载协议的 RADIUS 流量。

在已安装的多个网络适配器的 NPS，你可能想要将 NPS 配置为发送和接收 RADIUS 流量仅在你指定的适配器上。

例如，安装在 NPS 中的一个网络适配器可能会导致不包含 RADIUS 客户端，而第二个网络适配器提供 NPS 使用网络路径与其配置的 RADIUS 客户端的网络段。 在此方案中，务必引导 NPS 对所有 RADIUS 流量使用第二个网络适配器。

在另一个示例中，如果在 NPS 上有三个网络适配器安装，但是您只希望 NPS 对 RADIUS 流量使用两个适配器可以配置两个适配器的端口信息。 通过排除第三个适配器的端口配置，可以防止 NPS 对 RADIUS 流量使用适配器。

## <a name="using-a-network-adapter"></a>使用网络适配器

若要将 NPS 配置为侦听和网络适配器上发送 RADIUS 流量，请在属性对话框上的网络策略服务器在 NPS 控制台中使用以下语法：

- IPv4 流量语法：Ipaddress: Udpport 的其中 IPAddress 是您要发送 RADIUS 流量的网络适配器配置的 IPv4 地址，UDPport 是你想要用于 RADIUS 身份验证或记帐通讯的 RADIUS 端口号。
- IPv6 流量语法: [IPv6Address]:UDPport，其中 IPv6Address 两边的括号是必需、 IPv6Address 是您要发送 RADIUS 流量的网络适配器配置的 IPv6 地址，UDPport 是你想要用于 RADIUS 身份验证的 RADIUS 端口号或记帐通信。

配置 IP 地址和 UDP 端口信息，可以作为分隔符使用以下字符：

- 地址/端口分隔符： 冒号 （:）
- 端口分隔符： 逗号 （，）
- 接口分隔符： 分号 （;）

## <a name="configuring-network-access-servers"></a>配置网络访问服务器

请确保您的网络访问服务器使用配置了你 NPSs 配置的相同 RADIUS UDP 端口号。 在 Rfc 2865 和 2866年中定义的 RADIUS 标准 UDP 端口是进行身份验证的 1812年和用于记帐; 的 1813但是，某些访问服务器是默认 UDP 端口 1645年用于身份验证请求和 UDP 端口 1646年用于记帐请求配置。

>[!IMPORTANT]
>如果不使用 RADIUS 默认端口号，你必须在本地计算机以允许新端口上的 RADIUS 流量的防火墙上配置例外。 有关详细信息，请参阅[配置对 RADIUS 流量的防火墙](nps-firewalls-configure.md)。

## <a name="configure-the-multihomed-nps"></a>配置多宿主 NPS

可以使用以下过程来配置 NPS 在多宿主。

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>若要指定的网络适配器和 NPS 对 RADIUS 流量使用的 UDP 端口

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**打开 NPS 控制台。

2. 右键单击**网络策略服务器**，然后单击**属性**。

3. 单击**端口**选项卡，并预先考虑你想要使用 RADIUS 流量用于现有的端口号的网络适配器的 IP 地址。 例如，如果你想要使用的 IP 地址 192.168.1.2 和 RADIUS 端口 1812年和 1645年用于身份验证请求，更改端口设置从**1812，1645**到**192.168.1.2:1812,1645**。 如果您的 RADIUS 身份验证和 RADIUS 记帐 UDP 端口不同于默认值，请更改端口设置相应地。

4. 若要进行身份验证或记帐请求使用多个端口设置，请用逗号分隔的端口号。

有关 NPS UDP 端口的详细信息，请参阅[配置 NPS UDP 端口信息](nps-udp-ports-configure.md)


有关 NPS 的详细信息，请参阅[网络策略服务器](nps-top.md)

