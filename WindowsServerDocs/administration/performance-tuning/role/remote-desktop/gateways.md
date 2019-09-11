---
title: 性能优化远程桌面网关
description: 远程桌面网关的性能优化建议
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ad314fbf6701da3f96ddc68a598bf3024eaafe16
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866467"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>性能优化远程桌面网关

> [!NOTE]
> 在 Windows 8 + 和 Windows Server 2012 R2 + 中，远程桌面网关（RD 网关）支持 TCP、UDP 和旧 RPC 传输。 以下大部分数据都是关于旧 RPC 传输的。 如果未使用旧 RPC 传输，则此部分不适用。

本主题介绍与性能相关的参数，这些参数有助于提高客户部署的性能和依赖于客户网络使用模式的 tunings。

RD 网关在其核心中执行许多在远程桌面连接实例与客户网络中 RD 会话主机服务器实例之间执行的数据包转发操作。

> [!NOTE]
> 以下参数仅适用于 RPC 传输。

Internet Information Services （IIS）和 RD 网关导出以下注册表参数，以帮助提高 RD 网关中的系统性能。

**线程 tunings**

-   **Maxiothreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    此应用特定的线程池指定 RD 网关创建的用于处理传入请求的线程数。 如果存在此注册表设置，则它会生效。 线程数等于逻辑进程数。 如果逻辑处理器数小于5，则默认值为5个线程。

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    此参数指定要为每个逻辑处理器创建的 IIS 池线程数。 IIS 池线程监视网络中的请求并处理所有传入的请求。 **MaxPoolThreads**计数不包括 RD 网关使用的线程。 默认值为4。

**RD 网关的远程过程调用 tunings**

以下参数可帮助优化远程桌面连接和 RD 网关计算机接收的远程过程调用（RPC）。 更改 windows 有助于限制在每个连接上流动的数据量，并可提高 RPC over HTTP v2 方案的性能。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    默认值为 64 KB。 此值指定服务器用于从 RPC 代理接收的数据的窗口。 最小值设置为 8 KB，最大值设置为 1 GB。 如果值不存在，则使用默认值。 对此值进行更改时，必须重新启动 IIS 才能使更改生效。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    默认值为 64 KB。 此值指定客户端用于从 RPC 代理接收的数据的窗口。 最小值为 8 KB，最大值为 1 GB。 如果值不存在，则使用默认值。

## <a name="monitoring-and-data-collection"></a>监视和数据收集

监视 RD 网关上的资源使用情况时，下面的性能计数器列表被视为一组基本计数器：

-   \\终端服务网关\\\*

-   \\RPC/HTTP 代理\\\*

-   \\每台服务器的 RPC/HTTP 代理\\\*

-   \\Web 服务\\\*

-   \\W3SVC\_W3WP.EXE\\\*

-   \\IPv4\\\*

-   \\记忆\\\*

-   \\网络接口（\*）\\\*

-   \\Process （\*）\\\*

-   \\处理器信息（\*）\\\*

-   \\同步（\*）\\\*

-   \\主板\\\*

-   \\TCPv4\\\*

以下性能计数器仅适用于旧 RPC 传输：

-   \\Rpc/HTTP 代理\\rpc \*

-   \\Rpc/HTTP Proxy Per Server\\rpc \*

-   \\Web 服务\\ \* RPC

-   \\W3SVC\_W3WP.EXE\\ RPC\*

> [!NOTE]
> 如果适用，则添加\\IPv6\\ \*和\\TCPv6\\ 对象\* 。ReplaceThisText

