---
title: DHCP 中的新功能
description: 本主题提供的动态主机配置协议 (DHCP) 在 Windows Server 2016 中的新功能的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73cc5134f7af5063c912ad578fa7d660b3194aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840228"
---
# <a name="whats-new-in-dhcp"></a>DHCP 中的新功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍 Windows Server 2016 中新增或更改的动态主机配置协议 (DHCP) 功能。
  
DHCP 是一种 Internet 工程任务组 (IETF) 标准，旨在减轻管理负担并降低复杂度 TCP/IP 上配置主机\-基于网络，如私人内部。 使用 DHCP 服务器服务，在 DHCP 客户端上配置 TCP/IP 的过程是自动进行的。

DHCP 的情况下，以下部分提供的功能的新功能和更改的信息。

## <a name="dhcp-subnet-selection-options"></a>DHCP 子网选择选项

DHCP 现在支持选项 118 和 82\(子选项 5\)。 这些选项可用于允许 DHCP 代理客户端和中继代理请求的 IP 地址的特定子网，以及从特定 IP 地址范围和作用域。


如果使用 DHCP 选项 82 时配置的 DHCP 中继代理，sub\-选项 5，中继代理可以从特定的 IP 地址范围的 DHCP 客户端的请求的 IP 地址租约。

有关详细信息，请参阅[DHCP 子网选择选项](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>新的日志记录事件，用于由 DHCP 服务器的 DNS 注册错误

DHCP 现在包括服务器 DNS 记录注册 DNS 服务器的 DHCP 中的失败的情况下的日志记录事件。

有关详细信息，请参阅[DNS 记录注册的 DHCP 日志记录事件](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>在 Windows Server 2016 中不支持 DHCP NAP

网络访问保护\(NAP\)在 Windows Server 2012 R2 中，已弃用，并且在 Windows Server 2016 中的 DHCP 服务器角色不再支持 NAP。 有关详细信息，请参阅[Features Removed or Deprecated in Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx)。  
  
NAP 支持引入到 Windows Server 2008 的 DHCP 服务器角色，支持在 Windows 10 和 Windows Server 2016 之前的 Windows 客户端和服务器操作系统的系统中。 下表总结了对 Windows Server 中的 NAP 的支持。  
  
|操作系统|NAP 支持|  
|--------------------|---------------|  
| Windows Server 2008 |支持|  
| Windows Server 2008 R2 |支持|  
| Windows Server 2012 |支持|  
| Windows Server 2012 R2 |支持|  
| Windows Server 2016|不支持|  
  
在 NAP 部署中，运行操作系统的支持 NAP 的 DHCP 服务器可以充当 NAP DHCP 强制方法的 NAP 强制点。 有关采用 NAP 的 DHCP 的详细信息，请参阅[核对清单：实现 DHCP 强制设计](https://technet.microsoft.com/library/dd314186.aspx)。  
  
在 Windows Server 2016 中，DHCP 服务器不会强制使用 NAP 策略，并且 DHCP 作用域不能为 NAP\-启用。 同样是 NAP 客户端的 DHCP 客户端计算机发送的健康声明\(SoH\)与 DHCP 请求。 如果 DHCP 服务器正在运行 Windows Server 2016，如同没有 SoH 是存在处理这些请求。 DHCP 服务器向客户端授予正常的 DHCP 租约。 

如果正在运行 Windows Server 2016 的服务器身份验证请求转发到网络策略服务器的 RADIUS 代理\(NPS\)的支持 NAP，作为非 NAP 的 nps 评估这些 NAP 客户端\-支持，并处理失败的 NAP。
  
## <a name="see-also"></a>请参阅  
  
-   [动态主机配置协议 (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

