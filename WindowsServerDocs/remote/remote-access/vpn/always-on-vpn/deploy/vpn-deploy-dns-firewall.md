---
title: 配置 DNS 和防火墙设置
description: 本主题提供有关在 Windows Server 2016 中部署 Always On VPN 的详细说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: b1efa918971208bb9819de189de7298b4de144a5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871316"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>步骤 5： 配置 DNS 和防火墙设置

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**步骤 4：安装和配置 NPS 服务器](vpn-deploy-nps.md)
- [**一个**步骤 6：配置 Windows 10 客户端始终启用 VPN 连接](vpn-deploy-client-vpn-connections.md)

在此步骤中，你将为 VPN 连接配置 DNS 和防火墙设置。

## <a name="configure-dns-name-resolution"></a>配置 DNS 名称解析

当远程 VPN 客户端连接时，它们将使用您的内部客户端使用的相同 DNS 服务器，这允许它们以与内部工作站的其余部分相同的方式解析名称。

因此，你必须确保外部客户端用于连接到 VPN 服务器的计算机名称与颁发给 VPN 服务器的证书中定义的使用者备用名称匹配。

为了确保远程客户端可以连接到 VPN 服务器，你可以在外部 DNS 区域中创建 DNS A （主机）记录。 A 记录应使用 VPN 服务器的证书使用者备用名称。

### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>将主机（A 或 AAAA）资源记录添加到区域

1. 在 DNS 服务器上的服务器管理器中，选择 "**工具**"，然后选择 " **DNS**"。 此时将打开 DNS 管理器。
2. 在 DNS 管理器控制台树中，选择要管理的服务器。
3. 在详细信息窗格的 "**名称**" 中，双击 "**正向查找区域**" 展开该视图。
4. 在 "**正向查找区域**" 详细信息中，右键单击要向其添加记录的正向查找区域，然后选择 "**新建主机（a 或 AAAA）** "。 此时将打开 "**新建主机**" 对话框。
5. 在 "**新建主机**" 的 "**名称**" 中，输入 VPN 服务器的证书使用者备用名称。
6. 在 "IP 地址" 中，输入 VPN 服务器的 IP 地址。 可以输入 IP 版本4（IPv4）格式的地址，添加主机（A）资源记录，或使用 IP 版本6（IPv6）格式添加主机（AAAA）资源记录。
7. 如果为某个 IP 地址范围（包括所输入的 IP 地址）创建了反向查找区域，则选中 "**创建关联的指针（PTR）记录**" 复选框。  如果选择此选项，将根据你在 "**名称**和**IP 地址**" 中输入的信息，在该主机的反向区域中创建其他指针（PTR）资源记录。
8. 选择 "**添加主机**"。

## <a name="configure-the-edge-firewall"></a>配置边缘防火墙

边缘防火墙将外部外围网络与公共 Internet 隔离开来。 有关此分离的可视化表示形式，请参阅主题[ALWAYS ON VPN 技术概述](../always-on-vpn-technology-overview.md)"。

边缘防火墙必须允许并将特定端口转发到 VPN 服务器。 如果使用边缘防火墙上的网络地址转换（NAT），则可能需要启用用户数据报协议（UDP）端口500和4500的端口转发。 将这些端口转发到分配给 VPN 服务器的外部接口的 IP 地址。

如果要路由流量入站并在 VPN 服务器的后面执行 NAT，则必须打开防火墙规则，以允许 UDP 端口500和4500入站发送到 VPN 服务器上的公共接口的外部 IP 地址。

在任一情况下，如果你的防火墙支持深层数据包检查，并且你在建立客户端连接时遇到困难，你应尝试放宽或禁用 IKE 会话的深度数据包检查。

有关如何进行这些配置更改的信息，请参阅防火墙文档。

## <a name="configure-the-internal-perimeter-network-firewall"></a>配置内部外围网络防火墙

内部外围网络防火墙将组织/企业网络与内部外围网络隔离开来。 有关此分离的可视化表示形式，请参阅主题[ALWAYS ON VPN 技术概述](../always-on-vpn-technology-overview.md)"。

在此部署中，外围网络上的远程访问 VPN 服务器配置为 RADIUS 客户端。  VPN 服务器将 RADIUS 流量发送到公司网络上的 NPS，同时接收来自 NPS 的 RADIUS 流量。

配置防火墙以允许 RADIUS 流量双向流动。

>[!NOTE]
>组织/企业网络上的 NPS 服务器充当 VPN 服务器（RADIUS 客户端）的 RADIUS 服务器。 有关 RADIUS 基础结构的详细信息，请参阅[网络策略服务器（NPS）](../../../../../networking/technologies/nps/nps-top.md)。

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>VPN 服务器和 NPS 服务器上的 RADIUS 流量端口

默认情况下，NPS 和 VPN 侦听所有已安装网络适配器上端口1812、1813、1645和1646上的 RADIUS 流量。 如果在安装 NPS 时启用具有高级安全性的 Windows 防火墙，则这些端口的防火墙例外会在安装过程中自动创建 IPv6 和 IPv4 通信。

>[!IMPORTANT]
>如果网络访问服务器被配置为通过这些默认端口之外的端口发送 RADIUS 流量，请在安装 NPS 期间删除在 "高级安全 Windows 防火墙" 中创建的例外，并为用于的端口创建例外。RADIUS 流量。

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>为内部外围网络防火墙配置使用相同的 RADIUS 端口

如果在 VPN 服务器和 NPS 服务器上使用默认 RADIUS 端口配置，请确保在内部外围网络防火墙上打开以下端口：

- 端口 UDP1812、UDP1813、UDP1645 和 UDP1646

如果在 NPS 部署中未使用默认 RADIUS 端口，则必须将防火墙配置为允许使用的端口上的 RADIUS 流量。 有关详细信息，请参阅为[RADIUS 流量配置防火墙](../../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="next-steps"></a>后续步骤

[步骤 6.配置 Windows 10 客户端 Always On VPN](vpn-deploy-client-vpn-connections.md)连接：在此步骤中，你将 Windows 10 客户端计算机配置为使用 VPN 连接与该基础结构进行通信。 你可以使用多种技术来配置 Windows 10 VPN 客户端，包括 Windows PowerShell、System Center Configuration Manager 和 Intune。 所有三个都需要一个 XML VPN 配置文件来配置相应的 VPN 设置。