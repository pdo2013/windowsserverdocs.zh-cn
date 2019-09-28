---
title: 在多宿主计算机上配置 NPS
description: 本主题提供有关在 Windows Server 2016 中使用运行网络策略服务器的多个网络适配器配置服务器的说明。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df3c157ea4f453d965cd8754ef4ef9f7a71532a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396070"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>在多宿主计算机上配置 NPS

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来配置具有多个网络适配器的 NPS。

在运行网络策略服务器（NPS）的服务器中使用多个网络适配器时，可以配置以下内容：

- 不发送和接收远程身份验证拨入用户服务 \(RADIUS @ no__t 通信的网络适配器。
- 在每个网络适配器基础上，NPS 是否监视 Internet 协议版本4上的 RADIUS 流量 \(IPv4 @ no__t、IPv6，或同时使用 IPv4 和 IPv6。
- UDP 端口号，在其上发送和接收基于每个协议的 RADIUS 流量 @no__t 0IPv4 或 IPv6 @ no__t，每个网络适配器。

默认情况下，对于所有已安装的网络适配器，NPS 都侦听端口1812、1813、1645和1646上的 RADIUS 流量。 由于 NPS 自动将所有网络适配器用于 RADIUS 流量，因此当你想要阻止 NPS 使用特定网络适配器时，只需指定你希望 NPS 用于 RADIUS 流量的网络适配器。

>[!NOTE]
>如果在网络适配器上卸载 IPv4 或 IPv6，则 NPS 不会监视已卸载协议的 RADIUS 流量。

在安装了多个网络适配器的 NPS 上，你可能想要将 NPS 配置为仅在指定的适配器上发送和接收 RADIUS 流量。

例如，安装在 NPS 中的一个网络适配器可能会导致不包含 RADIUS 客户端的网络段，而另一个网络适配器为 NPS 提供到其配置的 RADIUS 客户端的网络路径。 在这种情况下，务必要使 NPS 对所有 RADIUS 流量使用第二个网络适配器。

在另一个示例中，如果 NPS 安装了三个网络适配器，但仅希望 NPS 将两个适配器用于 RADIUS 流量，则只能为这两个适配器配置端口信息。 通过排除第三个适配器的端口配置，可以防止 NPS 将适配器用于 RADIUS 流量。

## <a name="using-a-network-adapter"></a>使用网络适配器

若要将 NPS 配置为在网络适配器上侦听和发送 RADIUS 流量，请在 NPS 控制台中的网络策略服务器的 "属性" 对话框中使用以下语法：

- IPv4 流量语法：IPAddress： UDPport，其中，IPAddress 是在要发送 RADIUS 流量的网络适配器上配置的 IPv4 地址，UDPport 是要用于 RADIUS 身份验证或记帐流量的 RADIUS 端口号。
- IPv6 流量语法： [IPv6Address]：UDPport，其中，IPv6Address 是要在其上发送 RADIUS 流量的网络适配器上配置的 IPv6 地址，UDPport 是要用于 RADIUS 身份验证的 RADIUS 端口号，或记帐流量。

以下字符可用作配置 IP 地址和 UDP 端口信息的分隔符：

- 地址/端口分隔符：冒号（:)
- 端口分隔符：逗号（，）
- 接口分隔符：分号（;)

## <a name="configuring-network-access-servers"></a>配置网络访问服务器

请确保使用在 NPSs 上配置的相同 RADIUS UDP 端口号配置网络访问服务器。 Rfc 2865 和2866中定义的 RADIUS 标准 UDP 端口是用于身份验证的1812和用于记帐的 1813;但是，某些访问服务器默认配置为使用 UDP 端口1645进行身份验证请求，并将 UDP 端口1646用于记帐请求。

>[!IMPORTANT]
>如果不使用 RADIUS 默认端口号，则必须在防火墙上为本地计算机配置例外，以允许新端口上的 RADIUS 流量。 有关详细信息，请参阅为[RADIUS 流量配置防火墙](nps-firewalls-configure.md)。

## <a name="configure-the-multihomed-nps"></a>配置多宿主 NPS

你可以使用以下过程来配置多宿主 NPS。

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>指定 NPS 用于 RADIUS 流量的网络适配器和 UDP 端口

1. 在服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**" 以打开 NPS 控制台。

2. 右键单击 "**网络策略服务器**"，然后单击 "**属性**"。

3. 单击 "**端口**" 选项卡，并预先为要用于 RADIUS 流量的网络适配器的 IP 地址添加到现有端口号。 例如，如果要将 IP 地址192.168.1.2 和 RADIUS 端口1812和1645用于身份验证请求，请将端口设置从**1812、1645**改为**192.168.1.2：1812、1645**。 如果 RADIUS 身份验证和 RADIUS 记帐 UDP 端口不同于默认值，请相应地更改端口设置。

4. 若要将多个端口设置用于身份验证或记帐请求，请用逗号分隔端口号。

有关 NPS UDP 端口的详细信息，请参阅[配置 NPS Udp 端口信息](nps-udp-ports-configure.md)


有关 NPS 的详细信息，请参阅[网络策略服务器](nps-top.md)

