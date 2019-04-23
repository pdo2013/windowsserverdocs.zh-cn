---
title: DHCP 子网选择选项
description: 本主题提供有关 DHCP 子网选择选项的动态主机配置协议 (DHCP) 在 Windows Server 2016 中的信息。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 034ca48ef13a6bdac63ca99ac753fc9826460922
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870808"
---
# <a name="dhcp-subnet-selection-options"></a>DHCP 子网选择选项

>适用于：Windows 服务器 （半年频道），Windows Server 2016

有关新的 DHCP 子网选择选项的信息，可以使用本主题。

DHCP 现在支持选项 82\(子选项 5\)。 这些选项可用于允许 DHCP 代理客户端和中继代理请求的 IP 地址的特定子网，以及从特定 IP 地址范围和作用域。  有关更多详细信息，请参阅**选项 82 子选项 5**:[RFC 3527 链接选择中继代理信息选项的 DHCPv4 的子选项](https://tools.ietf.org/html/rfc3527)。

如果使用 DHCP 选项 82 时配置的 DHCP 中继代理，子选项 5，中继代理可以从特定的 IP 地址范围的 DHCP 客户端的请求的 IP 地址租约。


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>选项 82 子选项 5:链接选择子选项

中继代理链接选择子选项允许 DHCP 中继代理指定 IP 子网的 DHCP 服务器应从其分配 IP 地址和选项。

通常情况下，DHCP 中继代理依赖于网关 IP 地址\(GIADDR\)字段与 DHCP 服务器进行通信。 但是，GIADDR 限制由两个操作函数：

1. 若要告知正在请求 IP 地址租用的 DHCP 客户端在其所在的子网的 DHCP 服务器。
2. 若要将通知使用与中继代理进行通信的 IP 地址的 DHCP 服务器。

在某些情况下，中继代理使用与 DHCP 服务器进行通信的 IP 地址可能不同于需要以分配为 DHCP 客户端 IP 地址的 IP 地址范围。 

选项 82 链接选择子选项可在此情况下，允许中继代理可以显式声明 82 子选项 5 中 DHCP v4 选项的形式分配它想要从其 IP 地址的子网。

> [!NOTE]
>
> 所有中继代理 IP 地址 (GIADDR) 必须都是活动的 DHCP 作用域 IP 地址范围的一部分。 DHCP 作用域 IP 地址范围之外的任何 GIADDR 被视为无管理中继并且 Windows DHCP 服务器将收到来自这些中继代理的 DHCP 客户端请求。
>
> 可以创建特殊的作用域为"授权"中继代理。 用 GIADDR （或如果 GIADDR 的是连续的 IP 地址的多个） 创建一个范围，GIADDR 地址从分发中排除，然后激活作用域。 这将同时阻止 GIADDR 地址被分配授权中继代理。


### <a name="use-case-scenario"></a>用例场景

在此方案中，组织网络包括 DHCP 服务器和无线访问点\(亚太\)来宾用户。 从组织的 DHCP 服务器-但是，由于防火墙策略限制来宾客户端 IP 地址分配，DHCP 服务器无法访问来宾无线网络或无线客户端使用 broadcase 消息。

若要解决此限制，配置 AP 用来指定它想要从其分配中还指定会导致在内部接口的 IP 地址 GIADDR 来宾客户端的 IP 地址的子网链接选择子选项 5公司网络。
