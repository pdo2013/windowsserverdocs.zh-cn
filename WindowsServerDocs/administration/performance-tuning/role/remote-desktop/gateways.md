---
title: 性能优化远程桌面网关
description: 性能优化建议，以远程桌面网关
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 70b27d45acbfb046d52271a50ca7deffb226b8d0
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266731"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>性能优化远程桌面网关

> [!Note]
> 在 Windows 8 + 和 Windows Server 2012 R2 + 中，远程桌面网关 （RD 网关） 支持 TCP、 UDP 和旧的 RPC 传输。 以下数据的大多数有关旧版 RPC 传输。 如果未使用旧的 RPC 传输，本部分不适用。

本主题介绍与性能相关的参数，可帮助提高客户部署的性能和 tunings 依赖于客户的网络使用情况模式。

就其核心而言，RD 网关执行很多包转发远程桌面连接实例和客户的网络中的 RD 会话主机服务器实例之间的操作。

> [!Note]
> 以下参数适用于仅 RPC 传输。

Internet Information Services (IIS) 和 RD 网关导出以下的注册表参数，以帮助提高系统性能在 RD 网关。

**线程 tunings**

-   **Maxiothreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    此特定于应用的线程池指定 RD 网关创建用于处理传入的请求的线程的数。 如果存在此注册表设置，则设置将生效。 线程数等于逻辑进程数。 如果逻辑处理器数小于 5，则默认值为 5 个线程。

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    此参数指定 IIS 池线程来创建每个逻辑处理器的数。 IIS 池线程监视网络中的请求并处理所有传入请求。 **MaxPoolThreads**计数不包括 RD 网关使用的线程。 默认值为 4。

**远程过程调用 tunings RD 网关**

以下参数可以帮助优化通过远程桌面连接和 RD 网关的计算机接收远程过程调用 (RPC)。 更改窗口的可帮助限制数据量流过每个连接，并可以提高 RPC over HTTP v2 方案的性能。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    默认值为 64 KB。 此值指定服务器使用的 RPC 代理从收到的数据的窗口。 最小值设置为 8 KB，最大值设置为 1 GB。 如果值不存在，则使用默认值。 当为此值进行更改时，必须更改才能生效的重新启动 IIS。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    默认值为 64 KB。 此值指定客户端使用的 RPC 代理从收到的数据的窗口。 最小值为 8 KB，并最大值为 1 GB。 如果值不存在，则使用默认值。

## <a name="monitoring-and-data-collection"></a>监视和数据收集


监视对 RD 网关的资源使用情况时，以下性能计数器的列表被视为一组基本的计数器：

-   \\终端服务网关\\\*

-   \\RPC/HTTP 代理\\\*

-   \\每个服务器的 RPC/HTTP 代理\\\*

-   \\Web 服务\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4\\\*

-   \\内存\\\*

-   \\Network Interface(\*)\\\*

-   \\Process(\*)\\\*

-   \\处理器信息 (\*)\\\*

-   \\Synchronization(\*)\\\*

-   \\系统\\\*

-   \\TCPv4\\\*

以下性能计数器是仅适用于旧 RPC 传输：

-   \\RPC/HTTP 代理\\\* RPC

-   \\每个服务器的 RPC/HTTP 代理\\\* RPC

-   \\Web 服务\\\* RPC

-   \\W3SVC\_W3WP\\\* RPC

**请注意**  如果适用，将添加\\IPv6\\ \*并\\TCPv6\\ \*对象。ReplaceThisText

 
