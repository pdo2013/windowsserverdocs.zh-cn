---
title: 配置 DNS 和防火墙设置
description: 本主题提供有关部署 Always On VPN Windows Server 2016 中的详细的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886248"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>步骤 5： 配置 DNS 和防火墙设置

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

&#171;  [**上一：** 步骤 4：安装和配置 NPS 服务器](vpn-deploy-nps.md)<br>
&#187;  [**下一步：** 步骤 6：配置 Windows 10 客户端 Always On VPN 连接](vpn-deploy-client-vpn-connections.md)

在此步骤中，配置 DNS 和防火墙设置 VPN 连接。

## <a name="configure-dns-name-resolution"></a>配置 DNS 名称解析

当远程 VPN 客户端连接时，它们使用相同的 DNS 服务器的内部客户端使用，这使它们在内部工作站的其余部分相同的方式解析名称可以。 

因此，必须确保外部客户端用于连接到 VPN 服务器的计算机名称匹配到 VPN 服务器颁发的证书中定义的使用者备用名称。

若要确保远程客户端可以连接到你的 VPN 服务器，可以在外部 DNS 区域中创建一条 DNS A （主机） 记录。 A 记录应使用的 VPN 服务器的证书使用者备用名称。


### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>若要将主机添加\(A 或 AAAA\)资源记录到区域

1. 在 DNS 服务器上，在服务器管理器中，单击**工具**，然后单击**DNS**。 此时将打开 DNS 管理器。
2. 在 DNS 管理器控制台树中，单击你想要管理的服务器。
3. 在详细信息窗格中，在**名称**，双精度型\-单击**正向查找区域**可展开的视图。
4. 在中**正向查找区域**详细介绍，右侧\-单击你想要添加一条记录，并单击正向查找区域**新主机\(A 或 AAAA\)**。 **新的主机**对话框随即打开。
5. 在中**新的主机**，在**名称**，键入 VPN 服务器的证书使用者备用名称。
6. 在 IP 地址，键入 VPN 服务器的 IP 地址。 你可以在 IP 版本 4 (IPv4) 中键入的地址格式，将添加主机\(A\)资源记录或 IP 版本 6 \(IPv6\)格式，将添加主机\(AAAA\)资源记录。
7. 如果创建反向查找区域范围的 IP 地址，包括 IP 地址的键入，然后选择**创建相关的指针 (PTR) 记录**复选框。  选择此选项创建的附加指针\(PTR\)资源记录中反向区域的信息，根据此主机中输入**名称**并**IP 地址**。
8. 单击“添加主机” 。

## <a name="configure-the-edge-firewall"></a>配置边缘防火墙

在边缘防火墙与公共 Internet 隔开外部外围网络。 这种分离的可视表示形式，请参阅本主题中的图示[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。

边缘防火墙必须允许并转发到 VPN 服务器的特定端口。 如果使用网络地址转换\(NAT\)上边缘防火墙中，您可能需要启用用户数据报协议的端口转发\(UDP\)端口 500 和 4500。 将转发到分配给你的 VPN 服务器的外部接口的 IP 地址的这些端口。

如果要路由流量入站和执行 NAT 或后面的 VPN 服务器，则必须打开防火墙规则以允许 UDP 端口 500 和 4500 入站到外部应用到 VPN 服务器上的公共接口的 IP 地址。

在任一情况下，如果你的防火墙支持深度数据包检查，并且您很难建立客户端连接，您应该尝试放松或禁用 IKE 会话的深度数据包检查。

有关如何进行这些配置更改的信息，请参阅防火墙文档。

## <a name="configure-the-internal-perimeter-network-firewall"></a>内部外围网络防火墙配置

内部的外围网络防火墙与内部的外围网络隔开组织/企业网络。 这种分离的可视表示形式，请参阅本主题中的图示[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。

在此部署中，外围网络上的远程访问 VPN 服务器配置为 RADIUS 客户端。  VPN 服务器将 RADIUS 流量发送到企业网络上的 NPS 和还从 NPS 接收 RADIUS 流量。

配置防火墙以允许 RADIUS 通信在两个方向流动。


>[!NOTE]
>组织/企业网络上的 NPS 服务器充当 RADIUS 服务器的 VPN 服务器，这是 RADIUS 客户端。 有关 RADIUS 基础结构的详细信息，请参阅[网络策略服务器 (NPS)](../../../../../networking/technologies/nps/nps-top.md)。

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>VPN 服务器和 NPS 服务器上的 RADIUS 流量端口

默认情况下，NPS 和 VPN 侦听的端口 1812年、 1813年、 1645年和 1646年上所有已安装的网络适配器上的 RADIUS 流量。 如果安装 NPS 时启用高级安全 Windows 防火墙，为这些端口的防火墙例外获取 IPv6 和 IPv4 流量的安装过程中自动创建。

>[!IMPORTANT]
>如果您的网络访问服务器配置为这些默认值以外的端口上发送 RADIUS 流量，删除 NPS 安装期间在具有高级安全性的 Windows 防火墙中创建的例外和您使用的端口创建例外RADIUS 流量。

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>内部外围网络防火墙配置为使用相同的 RADIUS 端口

如果 VPN 服务器和 NPS 服务器上使用默认的 RADIUS 端口配置，请确保打开内部外围网络防火墙上的以下端口：

- 端口 UDP1812、 UDP1813、 UDP1645 和 UDP1646

如果不将 NPS 部署中的默认 RADIUS 端口，则必须配置防火墙以允许你使用的端口上的 RADIUS 流量。 有关详细信息，请参阅[配置对 RADIUS 流量的防火墙](../../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="next-step"></a>下一步
[步骤 6。配置 Windows 10 客户端 Always On VPN 连接](vpn-deploy-client-vpn-connections.md):在此步骤中，您配置 Windows 10 客户端计算机与该基础结构与 VPN 连接进行通信。 可以使用多种技术来配置 Windows 10 VPN 客户端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune。 所有这三个需要 XML VPN 配置文件来配置适当的 VPN 设置。 

---
