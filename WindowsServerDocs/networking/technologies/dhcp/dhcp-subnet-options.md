---
title: DHCP 子网选择选项
description: 本主题提供动态主机配置协议 (DHCP) 在 Windows Server 2016 DHCP 子网所选内容选项的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43bc3d165f895767ded921b41118ecaccf9734e8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-subnet-selection-options"></a>DHCP 子网选择选项

>适用于：Windows Server（半年通道），Windows Server 2016

有关新 DHCP 子网选择选项的信息，你可以使用本主题。

DHCP 现在支持选项 118 和 82 \(sub-option 5\)。 你可以使用这些选项，以允许 DHCP 代理客户端和传达代理请求特定的网的和特定的 IP 地址范围并范围 IP 地址。

如果你使用 DHCP 代理客户端与 DHCP 选项 118 配置，如虚拟专用网络 (VPN) 服务器运行 Windows Server 2016 和远程访问服务器角色、VPN 服务器可以请求 IP 地址租赁的 VPN 客户端中的特定的 IP 地址范围。

如果你使用 DHCP 传达代理配置了 DHCP 选项 82，子选项 5 传达代理可以 IP 地址租赁征求 DHCP 特定的 IP 地址范围内的客户端。

以下是有关这些选项的评论主题请求的链接。

- **选项 118**: [dhcp RFC 3011 IPv4 子网选择选项](http://www.rfc-base.org/rfc-3011.html)
- **选项 82 子选项 5**: [RFC 3527 链接选择子选项传达代理信息选项 DHCPv4](https://tools.ietf.org/html/rfc3527)


## <a name="dhcp-option-118-client-subnet-and-relay-agent-link-selection"></a>DHCP 选项 118：客户网和传达代理链接选择

DHCP 子网选择选项提供机制 DHCP 代理服务器，若要指定 IP 子网从中 DHCP 服务器应指定 IP 地址和选项。

### <a name="use-case-scenario"></a>使用情况

在此情况下，虚拟专用网络 \(VPN\) 服务器分配到 VPN 客户端的 IP 地址。 

在此情况下，这两个装有 DHCP 服务器的 intranet 和到 Internet 时，连接 VPN 服务器，以便 VPN 客户端从遥远可以连接到 VPN 服务器。

VPN 服务器可以提供给 VPN 客户端的 IP 地址租赁之前，请联系 DHCP 服务器上的内部子网服务器，并保留的块 IP 地址。 然后，VPN 服务器管理来自 DHCP 服务器的 IP 地址。 如果 VPN 服务器提供的所有租赁中保留的 IP 地址到 VPN 客户端，VPN 服务器然后 DHCP 服务器从获得其他 IP 地址。

通过使用 DHCP 选项 118 配置 VPN 服务器，你可以指定 IP 地址范围和你想要用于 VPN 客户端的范围。 配置选项 118 后，VPN 服务器将请求特定的 IP 地址范围并从 DHCP 服务器的范围内的 IP 地址。

### <a name="the-dhcp-subnet-selection-option-field"></a>DHCP 子网选择选项字段

DHCP 子网选择选项字段包含一个 IPv4 地址将用于表示 DHCP 租赁请求的原始子网地址。  配置响应此选项 DHCP 服务器从分配地址：

1. 在子网选择选项中指定的子网。
2. 位于同一为子网选择选项中指定的子网网络段子网。

## <a name="option-82-sub-option-5-link-selection-sub-option"></a>选项 82 子选项 5：链接选择子选项

传达代理链接选择子选项使 DHCP 传达代理若要指定 IP 子网从中 DHCP 服务器应指定 IP 地址和选项。

通常，DHCP 传达代理依赖于要通信具有 DHCP 服务器的网关 IP 地址 \(GIADDR\) 字段。 但是，GIADDR 受其两种操作的功能：

1. 有关子网 DHCP 客户端请求 IP 地址租赁驻留在其通知 DHCP 服务器。
2. 若要告知 DHCP 服务器的 IP 地址，使用与传达专员进行通信。

在某些情况下，可能不同于需要分配 DHCP 客户 IP 地址的 IP 地址范围传达代理用于通信具有 DHCP 服务器的 IP 地址。 

DHCP 传达代理不能使用的选项 118，因为它们的功能是有限和可以仅写入 GIADDR 字段或传达代理信息选项 \(option 82\)。 

选项 82 的链接选择子选项是这种情况，允许明确说明子的网它想要从中 IP 地址以以下形式出现：DHCP v4 选项 82 子选项 5 分配传达人员很有用。

### <a name="use-case-scenario"></a>使用情况

在此情况下，组织网络包含 DHCP 服务器和无线接入点的访客用户 \(AP\)。 从组织 DHCP 服务器，但是，由于防火墙策略限制分配来宾客户端 IP 地址、来宾无线网络或具有 broadcase 消息无线的客户端将无法访问 DHCP 服务器。

若要解决这一限制，接入点配置链接选择子选项第 5 步指定子的网它想要从中来宾中的客户端，同时还指定 IP 地址的内部会转到公司的网络接口 GIADDR 分配的 IP 地址与。
