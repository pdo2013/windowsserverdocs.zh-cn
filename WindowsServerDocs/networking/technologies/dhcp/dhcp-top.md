---
title: 动态主机配置协议 (DHCP)
description: 本主题简要概述了 Windows Server 2016 中的动态主机配置协议（DHCP）。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c71517bc742cf9eda62cc7d83128f1ab9bd04547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355395"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>动态主机配置协议 (DHCP)

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来概述 Windows Server 2016 中的 DHCP。

> [!NOTE]
> 除本主题外，还提供以下 DHCP 文档。
>
> - [DHCP 中的新增功能](What-s-New-in-DHCP.md)
> - [使用 Windows PowerShell 部署 DHCP](dhcp-deploy-wps.md)

动态主机配置协议（DHCP）是一种客户端/服务器协议，该协议自动向 Internet 协议（IP）主机提供其 IP 地址和其他相关的配置信息，如子网掩码和默认网关。 Rfc 2131 和2132将 DHCP 定义为基于启动协议（BOOTP）的 Internet 工程任务组（IETF）标准，这是 DHCP 共享多个实现细节的协议。 DHCP 允许主机从 DHCP 服务器获取所需的 TCP/IP 配置信息。

Windows Server 2016 包括 DHCP 服务器，这是可在网络上部署的可选网络服务器角色，可将 IP 地址和其他信息租给 DHCP 客户端。 所有基于 Windows 的客户端操作系统都包括作为 TCP/IP 一部分的 DHCP 客户端，并在默认情况下启用 DHCP 客户端。

## <a name="why-use-dhcp"></a>为什么使用 DHCP？

基于 TCP/IP 的网络上的每个设备都必须具有唯一的单播 IP 地址，才能访问网络及其资源。 如果没有 DHCP，必须手动配置从一个子网移到另一个子网的新计算机或计算机的 IP 地址;必须手动回收从网络中删除的计算机的 IP 地址。

对于 DHCP，这整个过程是自动进行的，并集中管理。 DHCP 服务器维护 IP 地址池，并在网络上启动时为任何启用了 DHCP 的客户端租用一个地址。 由于 IP 地址是动态的（租用），而不是静态的（永久分配），因此不再使用的地址会自动返回到池以进行重新分配。

网络管理员建立 DHCP 服务器来维护 TCP/IP 配置信息，并以租约提议的形式为启用 DHCP 的客户端提供地址配置。 DHCP 服务器将配置信息存储在包含以下内容的数据库中：

- 网络上的所有客户端的有效 TCP/IP 配置参数。

- 在分配给客户端的池中维护的有效 IP 地址以及排除的地址。

- 保留 IP 与特定 DHCP 客户端相关联的地址。 这允许将单个 IP 地址的分配一致分配给单个 DHCP 客户端。

- 租约持续时间，或在需要进行租约续订之前，可以使用 IP 地址的时间长度。

在接受租约时，启用了 DHCP 的客户端将接收：

- 要连接到的子网的有效 IP 地址。  
  
- 请求的 DHCP 选项，这是 DHCP 服务器配置为分配给客户端的其他参数。 DHCP 选项的一些示例包括路由器（默认网关）、DNS 服务器和 DNS 域名。

## <a name="benefits-of-dhcp"></a>DHCP 的优点

DHCP 提供了以下优势。

- **可靠的 IP 地址配置**。 DHCP 最大程度地减少了由手动 IP 地址配置（如录入错误）导致的配置错误，或将 IP 地址分配给多台计算机时导致的地址冲突。

- **降低了网络管理**。 DHCP 包含以下功能来减少网络管理：

    - 集中式和自动 TCP/IP 配置。

    - 能够从一个中心位置定义 TCP/IP 配置。

    - 通过 DHCP 选项分配其他所有 TCP/IP 配置值的功能。

    - 有效地处理必须经常更新的客户端的 IP 地址更改，例如，移动到无线网络上不同位置的便携设备的 IP 地址更改。

    - 使用 DHCP 中继代理转发初始 DHCP 消息，这样就无需在每个子网上使用 DHCP 服务器。

