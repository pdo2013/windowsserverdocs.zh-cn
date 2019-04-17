---
title: 什么是 DHCP 中的新增功能
description: 本主题提供新功能的概述的动态主机配置协议 (DHCP) 在 Windows Server 2016。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f67e5dfd8aefcf408a41f6fc919665419f0ff1c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dhcp"></a>什么是 DHCP 中的新增功能

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍了新的或更改在 Windows Server 2016 的动态主机配置协议 (DHCP) 功能。
  
DHCP 是旨在减少管理负担和基于 IP\ 的网络，如专用 intranet 配置主机复杂性工程任务组 Internet (IETF) 标准。 通过使用 DHCP 服务器的服务的客户端 DHCP 上配置 TCP/IP 的过程会自动。

以下部分 dhcp 功能中提供的新功能和更改的信息。

## <a name="dhcp-subnet-selection-options"></a>DHCP 子网选择选项

DHCP 现在支持选项 118 和 82 \(sub-option 5\)。 你可以使用这些选项，以允许 DHCP 代理客户端和传达代理请求特定的网的和特定的 IP 地址范围并范围 IP 地址。

如果你使用 DHCP 代理客户端配置了 DHCP 选项 118，如运行的 Windows Server 2016 和远程访问服务器角色，虚拟专用网络 \(VPN\) 服务器 VPN 服务器可以征求 IP 地址租赁 VPN 客户端中的特定的 IP 地址范围。

如果你使用 DHCP 传达代理配置了 DHCP 选项 82，sub\ 选项 5 传达代理可以征求 IP 地址租赁 DHCP 特定的 IP 地址范围内的客户端。

有关详细信息，请参阅[DHCP 子网选择选项](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>新的事件日志记录 DNS 注册 DHCP 服务器的失败情况

DHCP 现在包含了哪些 DHCP 服务器 DNS 记录注册导致失败 DNS 服务器的情况下事件日志记录。

有关详细信息，请参阅[DHCP DNS 记录注册的日志记录事件](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Windows Server 2016 中不支持 DHCP NAP

网络的访问权限保护 \(NAP\) 不再支持在 Windows Server 2012 R2，并且在 Windows Server 2016 DHCP 服务器角色不再支持 NAP。 有关详细信息，请参阅[功能已删除或在 Windows Server 2012 R2 的建议不再使用](https://technet.microsoft.com/library/dn303411.aspx)。  
  
NAP 支持引入到 DHCP 服务器角色与 Windows Server 2008 和 Windows 客户端和服务器操作系统之前，Windows 10 和 Windows Server 2016 中受支持。 下表总结 NAP 在 Windows Server 的支持。  
  
|操作系统|NAP 支持|  
|--------------------|---------------|  
| Windows Server 2008 |受支持|  
| Windows Server 2008 R2 |受支持|  
| Windows Server 2012 |受支持|  
| Windows Server 2012R2 |受支持|  
| Windows Server 2016|不受支持|  
  
NAP 部署，DHCP 服务器支持 NAP 操作系统运行的可以作为 NAP 强制点 NAP DHCP 执法方法。 有关在 NAP DHCP 的详细信息，请参阅[清单： 实现 DHCP 强制设计](https://technet.microsoft.com/library/dd314186.aspx)。  
  
在 Windows Server 2016，DHCP 服务器执行强制 NAP 策略，且无法 NAP\ 启用 DHCP 范围。 还 NAP 客户端的 DHCP 客户端计算机发送一份与 DHCP 请求健康 \(SoH\)。 如果 DHCP 服务器运行的 Windows Server 2016，因为如果没有 SoH 存在处理这些请求。 DHCP 服务器授予正常 DHCP 租赁客户端。 

如果运行的 Windows Server 2016 的服务器 RADIUS 转发给支持 NAP 网络策略服务器 \(NPS\) 的请求身份验证的代理服务器，为非 NAP\ 支持，并处理失败 NAP nps 评估这些 NAP 客户端。
  
## <a name="see-also"></a>请参阅  
  
-   [动态主机配置协议(DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

