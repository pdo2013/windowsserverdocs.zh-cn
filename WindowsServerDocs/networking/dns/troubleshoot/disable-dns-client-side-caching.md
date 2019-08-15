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
ms.openlocfilehash: 3aeb7cb06f82b6f2220e42866682ce918389bf1d
ms.sourcegitcommit: b17ccf7f81e58e8f4dd844be8acf784debbb20ae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2019
ms.locfileid: "69023899"
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

## <a name="using-the-registry-to-control-the-caching-time"></a>使用注册表控制缓存时间

> [!IMPORTANT]  
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，以便在出现问题时可以还原。

正或负响应缓存的时间长度取决于以下注册表项中的条目值:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSCache\Parameters**

肯定响应的 TTL 是以下值中的较小者: 

- 解析程序收到的查询响应中指定的秒数

- **MaxCacheTtl**注册表设置的值。

>[!Note]
>- 肯定响应的默认 TTL 为86400秒 (1 天)。
>- 负响应的 TTL 是在 MaxNegativeCacheTtl 注册表设置中指定的秒数。
>- 负响应的默认 TTL 为900秒 (15 分钟)。
如果不想缓存否定响应, 请将 MaxNegativeCacheTtl 注册表设置设置为0。

若要设置客户端计算机上的缓存时间:

1. 启动注册表编辑器 (Regedit.exe)。

2. 在注册表中找到并单击以下项:

   **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters**

3. 在 "编辑" 菜单上, 指向 "新建", 单击 "DWORD 值", 然后添加以下注册表值:

   - 值名称:MaxCacheTtl

     数据类型：REG_DWORD

     值数据:默认值为86400秒。 
     
     如果将客户端 DNS 缓存中的最大 TTL 值降低到1秒, 这会使客户端 DNS 缓存已禁用。    

   - 值名称:MaxNegativeCacheTtl

     数据类型：REG_DWORD

     值数据:默认值为900秒。 
     
     如果不想缓存否定响应, 请将值设置为0。

4. 键入要使用的值, 然后单击 "确定"。

5. 退出注册表编辑器。