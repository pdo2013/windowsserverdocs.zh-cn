---
title: DHCP 中的新功能
description: 本主题概述了 Windows Server 2016 中的动态主机配置协议（DHCP）的新增功能。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8032b7c8e78170d57b0367775672577d9fd900e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355448"
---
# <a name="whats-new-in-dhcp"></a>DHCP 中的新功能

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍 Windows Server 2016 中新增或更改的动态主机配置协议（DHCP）功能。
  
DHCP 是一项 Internet 工程任务组（IETF）标准，旨在减轻在 TCP/IP @ no__t-0based 网络（如专用 intranet）上配置主机的管理负担和复杂性。 使用 DHCP 服务器服务，在 DHCP 客户端上配置 TCP/IP 的过程是自动进行的。

以下各部分提供了有关新功能的信息和 DHCP 的功能更改。

## <a name="dhcp-subnet-selection-options"></a>DHCP 子网选择选项

DHCP 现在支持选项118和 82 \(sub 5 @ no__t-1。 你可以使用这些选项，允许 DHCP 代理客户端和中继代理请求特定子网的 IP 地址，并从特定的 IP 地址范围和范围请求 IP 地址。


如果你使用的是配置了 DHCP 选项82，sub @ no__t-0option 5 的 DHCP 中继代理，则中继代理可以从特定 IP 地址范围请求 DHCP 客户端的 IP 地址租约。

有关详细信息，请参阅[DHCP 子网选择选项](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>DHCP 服务器 DNS 注册失败的新日志记录事件

DHCP 现在包括在 DNS 服务器上的 DHCP 服务器 DNS 记录注册失败的情况下的日志记录事件。

有关详细信息，请参阅[DNS 记录注册的 DHCP 日志记录事件](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Windows Server 2016 不支持 DHCP NAP

网络访问保护 \(NAP @ no__t 在 Windows Server 2012 R2 中已弃用，在 Windows Server 2016 中，DHCP 服务器角色不再支持 NAP。 有关详细信息，请参阅[Windows Server 2012 R2 中删除或弃用的功能](https://technet.microsoft.com/library/dn303411.aspx)。  
  
Windows Server 2008 向 DHCP 服务器角色引入了 NAP 支持，windows 10 和 Windows Server 2016 之前的 Windows 客户端和服务器操作系统支持 NAP 支持。 下表汇总了对 Windows Server 中的 NAP 的支持。  
  
|操作系统|NAP 支持|  
|--------------------|---------------|  
| Windows Server 2008 |支持|  
| Windows Server 2008 R2 |支持|  
| Windows Server 2012 |支持|  
| Windows Server 2012 R2 |支持|  
| Windows Server 2016|不支持|  
  
在 NAP 部署中，运行支持 NAP 的操作系统的 DHCP 服务器可充当 NAP DHCP 强制方法的 NAP 强制点。 有关 NAP 中的 DHCP 的详细信息，请参阅 [Checklist：实现 DHCP 强制设计 @ no__t-0。  
  
在 Windows Server 2016 中，DHCP 服务器不强制实施 NAP 策略，并且 DHCP 作用域不能是 NAP @ no__t-0enabled。 同时为 NAP 客户端的 DHCP 客户端计算机使用 DHCP 请求发送健康声明 \(SoH @ no__t-1。 如果 DHCP 服务器正在运行 Windows Server 2016，则将处理这些请求，就好像没有 SoH 存在一样。 DHCP 服务器向客户端授予普通的 DHCP 租约。 

如果运行 Windows Server 2016 的服务器是将身份验证请求转发给网络策略服务器的 RADIUS 代理 \(NPS @ no__t-1，它支持 NAP，则 NPS 会将这些 NAP 客户端评估为非 NAP @ no__t-2capable，而 NAP 处理会失败。
  
## <a name="see-also"></a>请参阅  
  
-   [动态主机配置协议 (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

