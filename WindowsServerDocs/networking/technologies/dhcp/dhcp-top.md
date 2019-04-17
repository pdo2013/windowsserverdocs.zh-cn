---
title: 动态主机配置协议(DHCP)
description: 本主题提供动态主机配置协议 (DHCP 在 Windows Server 2016) 的简短概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 770cfd78c7b9a0e122bd9f9936623a73b56af808
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>动态主机配置协议(DHCP)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用有关在 Windows Server 2016 DHCP 的简要概述的本主题。

>[!NOTE]
>本主题中，除了以下 DHCP 文档才可用。
>
>- [什么是 DHCP 中的新增功能](What-s-New-in-DHCP.md)
>- [部署 DHCP 使用 Windows PowerShell](dhcp-deploy-wps.md)

动态主机配置协议(DHCP) 都是客户月服务器协议自动与它的 IP 地址和其他相关的配置信息，如子网掩码和默认网关提供 Internet 协议 (IP) 主机。 2131 和 2132 Rfc 定义 DHCP 作为 Internet 工程工作组 (IETF) 标准基于上引导协议 (BOOTP)，一种协议 DHCP 与其共享许多实现详细信息。 DHCP 允许主机，以获得 DHCP 服务器的所需的 TCP/IP 配置的信息。

Windows Server 2016 包括 DHCP 服务器上，这是你可以在你的网络租赁 IP 地址和 DHCP 客户端到的其他信息将部署可选网络服务器角色。 默认情况下启用 DHCP 客户端和所有基于 Windows 的客户端操作系统作为 TCP/IP 的一部分包含 DHCP 客户端。

## <a name="why-use-dhcp"></a>为什么要使用 DHCP？

基于 IP 网络上的每台设备都必须具有来访问该网络及其资源一个唯一的单址广播 IP 地址。 DHCP，不为新的计算机或移动到另一个子网从的计算机的 IP 地址必须手动配置;必须手动回收从网络中删除的计算机的 IP 地址。

使用 DHCP，这整个过程自动，并且集中管理。 DHCP 服务器维护 IP 地址的池，并在网络上的它启动时租借向任何已启用 DHCP 的客户端的地址。 因为（租赁）而不是（永久分配）的静态 IP 地址是动态，不会再在使用的地址自动返回的池分配。

网络管理员联系建立 DHCP 服务器的维护 TCP/IP 配置信息并提供以下形式出现：租赁优惠中启用 DHCP 的客户端地址配置。 DHCP 服务器将配置信息存储在数据库中，其中包括：

- 网络上的所有客户的有效 TCP/IP 配置参数。

- 有效的 IP 地址、保留在池分配给客户端，以及排除地址。

- 保留与特定 DHCP 客户端关联的 IP 地址。 这允许一致分配的单个 DHCP 客户端到一个 IP 地址。

- 租赁持续时间或需要租赁续订之前，则可以使用的 IP 地址的时间长度。

启用 DHCP 的客户端，接受租赁协议后，在收到：

- 连接到的子网有效的 IP 地址。  
  
- 请求 DHCP 选项，都 DHCP 服务器配置到的其他参数分配给客户端。 DHCP 选项的一些示例包括路由器（默认网关）、DNS 服务器和 DNS 域名称。

## <a name="benefits-of-dhcp"></a>DHCP 的好处

DHCP 会提供以下好处。

- **可靠 IP 地址配置**。 DHCP 降至最低配置错误，手动 IP 地址配置，例如打字错误所致或处理冲突所致同时到多台计算机的 IP 地址的分配。

- **减少网络管理**。 DHCP 包含以下功能，以减少网络管理：

    - 集中和自动化 TCP/IP 配置。

    - 定义 TCP/IP 配置从中心位置的功能。

    - 将其他 TCP/IP 配置值全套分配通过 DHCP 选项的能力。

    - 有效的 IP 地址处理更改必须经常，例如便携移动到其他位置无线网络的设备更新的客户端。

    - 通过使用 DHCP 传达代理，从而无需在每个子网 DHCP 服务器的初始 DHCP 邮件转发。

