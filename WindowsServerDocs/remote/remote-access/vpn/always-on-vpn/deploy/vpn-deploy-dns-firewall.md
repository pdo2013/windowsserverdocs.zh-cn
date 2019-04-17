---
title: 配置 DNS 和防火墙设置
description: 本主题提供有关部署始终启用 VPN Windows Server 2016 中的详细的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067033"
---
# 步骤 5： 配置 DNS 和防火墙设置

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**以前：** 步骤 4。安装和配置 NPS 服务器](vpn-deploy-nps.md)<br>
& #187; [**下一步：** 步骤 6。始终对 VPN 连接配置 Windows 10 客户端](vpn-deploy-client-vpn-connections.md)

在此步骤中，你可以配置 DNS 和防火墙 VPN 连接的设置。

## 配置 DNS 名称解析

当远程 VPN 客户端连接时，他们使用相同的 DNS 服务器的内部的客户端使用，这使它们可以解析内部工作站的其余部分的相同的方式的名称。 

因此，你必须确保外部客户端用于连接到 VPN 服务器的计算机名称，与 VPN 服务器证书中定义的使用者备用名称相匹配。

若要确保远程客户端可以连接到 VPN 服务器，你可以在外部的 DNS 区域中创建 DNS A （主机） 记录。 A 记录，应使用 VPN 服务器证书使用者备用名称。


### 若要添加的主机 \ （A 或 AAAA\） 资源记录到一个区域

1. 在 DNS 服务器上，在服务器管理器中，单击**工具**，然后单击**DNS**。 打开 DNS 管理器。
2. 在 DNS 管理器控制台树中，单击你想要管理的服务器。
3. 在详细信息窗格中，在**名称**double\ 单击以展开视图**正向查找区域**中。
4. **正向查找区域**的详细信息，right\ 单击正向查找区域中为想要添加的记录，然后单击**新主机 \(A or AAAA\)**。 打开**新建主机**对话框。
5. 在**新的主机**，在**名称**中，键入 VPN 服务器证书使用者备用名称。
6. IP 地址中，键入 VPN 服务器的 IP 地址。 你可以键入 IP 版本 4 (IPv4) 地址格式，以添加主机 \(A\) 资源记录或 IP 版本 6 \(IPv6\) 格式，以添加主机 \(AAAA\) 资源记录。
7. 如果你创建的范围内的 IP 地址，包括你键入，IP 地址反向查找区域，然后选择**创建相关的指针 (PTR) 记录**复选框。  选择此选项将创建一个附加的指针 \(PTR\) 资源记录反向区域中为此主机，具体取决于你**名**和**IP 地址**中输入的信息。
8. 单击**添加主机**。

## 配置边缘防火墙

Edge 防火墙从公共 Internet 分隔外部外围网络。 这种分离的视觉表示形式，请参阅下图中的主题[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。

Edge 防火墙必须允许和转发到 VPN 服务器特定的端口。 如果你使用网络地址转换 \(NAT\) edge 防火墙上时，你可能需要允许用户数据报协议 \(UDP\) ports500 和 4500 的端口转发。 转发到已分配给你的 VPN 服务器的外部接口的 IP 地址的这些端口。

如果你正在路由流量入站和执行 NAT 或隐藏 VPN 服务器，你必须打开你的防火墙规则以允许 UDP ports500 然后 4500 入站的外部 IP 地址应用到 VPN 服务器上的公共接口。

在任一情况下，如果防火墙支持深入数据包检查，并且你有困难建立客户端连接，你应尝试放松或禁用深入数据包检查 IKE 会话。

有关如何进行以下配置更改的信息，请参阅你的防火墙文档。

## 配置内部外围网络防火墙

内部外围网络防火墙从内部外围网络分隔组织/企业网络。 这种分离的视觉表示形式，请参阅下图中的主题[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。

在此部署中，外围网络上的远程访问 VPN 服务器配置为 RADIUS 客户端。  VPN 服务器发送到公司网络上 NPS RADIUS 通信和还从 NPS 接收 RADIUS 通信。

配置防火墙以允许在两个方向流动 RADIUS 通信。


>[!NOTE]
>NPS 服务器组织/企业网络上的功能为 RADIUS 服务器 VPN 服务器，即 RADIUS 客户端。 有关 RADIUS 基础结构的详细信息，请参阅[网络策略服务器 (NPS)](../../../../../networking/technologies/nps/nps-top.md)。

### RADIUS 通信端口的 VPN 服务器和 NPS 服务器

默认情况下，NPS VPN 侦听的端口 1812年、 1813年、 1645年和 1646年上所有已安装的网络适配器上 RADIUS 通信。 如果安装 NPS 时启用高级安全 Windows 防火墙，防火墙例外，以便这些端口获取自动创建在安装过程中为 IPv4 和 IPv6 流量。

>[!IMPORTANT]
>如果你的网络访问服务器配置为通过这些默认值以外的端口发送 RADIUS 通信，删除 NPS 在安装期间，在高级安全 Windows 防火墙中创建的异常，并创建异常务必使用的端口RADIUS 通信。

### 内部外围网络防火墙配置为使用相同的半径端口

如果你使用的默认半径端口配置 VPN 服务器和 NPS 服务器上时，请确保你打开内部外围网络防火墙上的以下端口：

- 端口 UDP1812、 UDP1813、 UDP1645，以及 UDP1646

如果你不使用默认 RADIUS 端口 NPS 部署中，你必须配置防火墙以允许你使用的端口上的 RADIUS 通信。 有关详细信息，请参阅[配置 RADIUS 通信防火墙](../../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## 下一步
[步骤 6。配置 Windows 10 客户端始终在 VPN 连接](vpn-deploy-client-vpn-connections.md)： 在此步骤中，你配置 Windows 10 客户端计算机与通过 VPN 连接的基础结构进行通信。 你可以使用几种技术来配置 Windows 10 VPN 客户端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune。 所有三个需要 XML VPN 配置文件配置适当的 VPN 设置。 

---
