---
title: 在 DNS 客户端上禁用 DNS 客户端缓存
description: 本文介绍如何在 DNS 客户端上禁用 DNS 客户端缓存。
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 0b20400029b462798587c2291431b5a7c3d61775
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917804"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>在 DNS 客户端上禁用 DNS 客户端缓存

Windows 包含客户端 DNS 缓存。 客户端 DNS 缓存功能可能会产生错误的印象, 即 DNS "轮循机制" 负载平衡不会从 DNS 服务器发生到 Windows 客户端计算机。 使用 ping 命令搜索相同的 A 记录域名时, 客户端可能会使用相同的 IP 地址。  

## <a name="how-to-disable-client-side-caching"></a>如何禁用客户端缓存

若要停止 DNS 缓存, 请运行以下命令之一:

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


若要在 Windows 中永久禁用 DNS 缓存, 请使用 "服务控制器" 工具或 "服务" 工具将 DNS 客户端服务启动类型设置为 "**已禁用**"。 请注意, Windows DNS 客户端服务的名称也可能显示为 "Dnscache"。 

> [!NOTE]
> 如果停用 DNS 解析程序缓存, 客户端计算机的总体性能会降低, 并且 DNS 查询的网络流量将增加。 

DNS 客户端服务通过将之前解析的名称存储在内存中来优化 DNS 名称解析的性能。 如果 DNS 客户端服务处于关闭状态, 则计算机仍可以通过使用网络的 DNS 服务器解析 DNS 名称。 

当 Windows 解析程序将响应 (无论是肯定的还是否定的) 接收到查询时, 它会将该响应添加到其缓存中, 从而创建 DNS 资源记录。 解析程序在查询任何 DNS 服务器之前始终检查缓存。 如果缓存中有 DNS 资源记录, 解析程序将使用缓存中的记录, 而不是查询服务器。 此行为可加速查询并减少 DNS 查询的网络流量。 

可以使用 ipconfig 工具查看和刷新 DNS 解析程序缓存。 若要查看 DNS 解析程序缓存, 请在命令提示符下运行以下命令:

```cmd
ipconfig /displaydns 
```

此命令显示 DNS 解析程序缓存的内容, 包括从 Hosts 文件预加载的 DNS 资源记录, 以及由系统解析的任何最近查询的名称。 经过一段时间后, 解析程序将从缓存中丢弃该记录。 时间段由与 DNS 资源记录关联的生存**时间 (TTL)** 值指定。 还可以手动刷新缓存。 刷新缓存后, 计算机必须为计算机以前解析的任何 DNS 资源记录再次查询 DNS 服务器。 若要删除 DNS 解析程序缓存中的条目, `ipconfig /flushdns`请在命令提示符下运行。

## <a name="next-step"></a>下一步

有关详细信息, 请参阅[如何在 Windows 中禁用客户端 DNS 缓存](https://support.microsoft.com/kb/318803)。
