---
title: SMB 文件服务器的性能优化
description: SMB 文件服务器的性能优化
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 87ad8058f7353c938087b1211e0f17820f0bd2ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435656"
---
# <a name="performance-tuning-for-smb-file-servers"></a>SMB 文件服务器的性能优化

## <a name="smb-configuration-considerations"></a>SMB 配置注意事项
不要启用任何服务或文件服务器和客户端不需要的功能。 其中可能包括 SMB 签名，客户端缓存、 文件系统微筛选器，搜索服务、 计划的任务、 NTFS 加密、 NTFS 压缩、 IPSEC、 防火墙筛选器、 Teredo 和 SMB 加密。

这可能包括高性能模式或更改 C 状态，请确保根据需要设置了 BIOS 和操作系统电源管理模式。 请确保安装最新、 最灵活且最快的存储和网络设备驱动程序。

将文件复制是文件服务器上执行的常见操作。 Windows Server 具有几个内置的文件复制实用程序，可以使用命令提示符下运行。 建议使用 Robocopy。 在 Windows Server 2008 R2 中引入 **/mt**选项的 Robocopy 可以显著提高复制多个小型文件时使用多个线程的远程文件传输速度。 我们还建议使用 **/log**选项以将日志重定向到 NUL 设备或文件，从而减小控制台输出。 当使用 Xcopy 时，我们建议添加 **/q**并 **/k**到你的现有参数的选项。 前一个选项可减少 CPU 开销通过减少控制台输出，后者可减少网络流量。

## <a name="smb-performance-tuning"></a>SMB 性能优化


文件服务器的性能和可用 tunings 取决于每个客户端和服务器之间协商的 SMB 协议以及已部署的文件服务器功能。 当前可用的最高协议版本是 Windows Server 2016 和 Windows 10 中的 SMB 3.1.1。 您可以检查的 SMB 版本正在使用网络上使用 Windows PowerShell **Get SMBConnection**客户端上和**Get SMBSession |FL**服务器上。

### <a name="smb-30-protocol-family"></a>SMB 3.0 协议族

SMB 3.0 是 Windows Server 2012 中引入，进一步增强在 Windows Server 2012 R2 (SMB 3.02) 和 Windows Server 2016 (SMB 3.1.1) 中。 此版本引入了可能会显著提高性能和可用性文件服务器的技术。 有关详细信息，请参阅[SMB 在 Windows Server 2012 和 2012 R2 2012](https://aka.ms/smb3plus)和[中 SMB 3.1.1 新增](https://aka.ms/smb311)。



### <a name="smb-direct"></a>SMB 直通

SMB 直通引入了使用 RDMA 网络接口用于实现高吞吐量、 低延迟和 CPU 利用率较低的功能。

每当 SMB 检测到支持 RDMA 的网络，则会自动尝试使用 RDMA 功能。 但是，如果出于任何原因 SMB 客户端无法使用 RDMA 路径进行连接，则它将只需继续改为使用 TCP/IP 连接。 此外实现 TCP/IP 堆栈所需的所有与 SMB 直通兼容的 RDMA 接口和 SMB 多通道是注意这一点。

SMB 直通中不需要的任何 SMB 配置，但它 ' s 始终建议对于那些想要更低的延迟和较低的 CPU 利用率。

有关 SMB 直通的详细信息，请参阅[提高使用 SMB 直通的文件服务器的性能](https://aka.ms/smbdirect)。

### <a name="smb-multichannel"></a>SMB 多通道

SMB 多通道允许文件服务器同时使用多个网络连接，并提供更高的吞吐量。

有关 SMB 多通道的详细信息，请参阅[部署 SMB 多通道](https://aka.ms/smbmulti)。

### <a name="smb-scale-out"></a>SMB 横向扩展

SMB 横向扩展允许在群集配置要在群集的所有节点中显示一个共享的 SMB 3.0。 此活动/活动配置使得横向文件服务器群集进一步，而无需使用多个卷、 共享和群集资源的复杂配置。 最大共享带宽是全部文件服务器群集节点的带宽总和。 总带宽不再受到单个群集节点的带宽，但而是取决于后备存储系统的功能。 你可以通过添加节点来增加总带宽。

有关 SMB 横向扩展的详细信息，请参阅[横向扩展文件服务器应用程序数据概述](https://technet.microsoft.com/library/hh831349.aspx)和博客文章[进行横向扩展的或不向外扩展，这是问题](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)。

### <a name="performance-counters-for-smb-30"></a>SMB 3.0 的性能计数器

Windows Server 2012 中引入了以下的 SMB 性能计数器和监视的 SMB 2 和更高版本中的资源使用情况时，它们将被视为一组基本的计数器。 登录到本地，性能计数器原始 (.blg) 性能计数器日志。 若要使用通配符收集所有实例的成本较低 (\*)，并通过使用 Relog.exe 后期处理期间提取特定实例。

-   **SMB 客户端共享**

    这些计数器显示正在使用 SMB 2.0 或更高版本的客户端访问服务器上的文件共享的信息。

    如果您熟悉 Windows 中的常规磁盘计数器，您可能注意到某些相似。 S 不是由意外。 SMB 客户端共享性能计数器旨在与磁盘计数器完全匹配。 这种方式可以轻松地重复使用任何应用程序磁盘性能调整指南你当前没有。 有关计数器映射的详细信息，请参阅[每个共享客户端性能计数器博客](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx)。

-   **SMB 服务器共享**

    这些计数器显示在服务器上的 SMB 2.0 或更高版本的文件共享信息。

-   **SMB 服务器会话**

    这些计数器显示有关正在使用 SMB 2.0 或更高版本的 SMB 服务器会话的信息。

    启用服务器端 （服务器共享或服务器会话） 的计数器可能会产生显著的性能影响的高 IO 工作负荷。

-   **恢复密钥筛选器**

    这些计数器显示有关恢复密钥筛选器信息。

-   **SMB 直接连接**

    这些计数器来测量连接活动的不同方面。 一台计算机可以有多个 SMB 直通的连接。 SMB 直接连接计数器表示为一对 IP 地址和端口，其中的第一个 IP 地址和端口表示连接的本地终结点，而第二个 IP 地址和端口表示连接的远程终结点的每个连接。

-   **物理磁盘、 SMB、 CSV FS 性能计数器的关系**

    有关如何关联物理磁盘、 SMB 和 CSV FS （文件系统） 计数器的详细信息，请参阅以下博客文章：[群集共享卷的性能计数器](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx)。

## <a name="tuning-parameters-for-smb-file-servers"></a>SMB 文件服务器的优化参数


以下 REG\_DWORD 注册表设置可能会影响 SMB 文件服务器的性能：

- **Smb2CreditsMin**和**Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  默认值分别为 512 和 8192。 这些参数允许服务器限制动态中的指定边界的客户端操作并发性。 某些客户端可能会实现更高的吞吐量，具有更高并发限制，例如，通过高带宽、 高延迟链接复制的文件。
    
  > [!TIP]
  > 在 Windows 10 和 Windows Server 2016 之前, 授予客户端的可修读人数随其而变化动态 Smb2CreditsMin 和 Smb2CreditsMax 基于一种算法，尝试确定最优数目的信用额度以授予基于网络延迟之间和信用额度的使用情况。 在 Windows 10 和 Windows Server 2016 中，在 SMB 服务器已更改，以无条件地授予根据请求到已配置的最大数目的信用额度信用额度。 作为此更改的一部分，已删除阻止机制，当服务器处于内存压力下时，减少了每个连接的信用额度窗口的大小，信用额度。 触发的阻止的内核的低内存事件仅发出信号时服务器因此内存不足 (< 几 MB)，就可以使用无意义。 因为服务器无法再收缩信用额度 windows Smb2CreditsMin 设置将不再需要，并且现在被忽略。
  > 
  > 你可以监视 SMB 客户端共享\\信用额度停留数/秒以查看是否有使用信用额度的任何问题。

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    默认值为 0，这意味着会添加任何其他关键核心工作线程。 此值会影响文件系统缓存使用的预读读取和写入请求的线程数。 有关详细信息中的存储子系统，排队 I/O 和它可以提高 I/O 性能，特别是在具有多个逻辑处理器和功能强大的存储硬件的系统上，增加此值可以实现。

    >[!TIP]
    > 如果缓存管理器的脏数据增加可能需要的值 (性能计数器缓存\\脏页) 的增长，使用了很大一部分 （超过大约 25%)内存或系统正在执行大量同步读取 I/o。

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  默认值为 20。 增加此值将引发文件服务器可以使用为并发请求提供服务的线程的数。 如果大量的活动连接需要提供服务，并且硬件资源，例如存储带宽已足够，增加的值可以提高服务器的可伸缩性、 性能和响应时间。

  >[!TIP]
  > 可能需要增加值指示是如果 SMB2 工作队列会增长得非常大 (性能计数器服务器工作队列\\队列长度\\SMB2 非阻止性\*持续高于 ~ 100)。

  >[!Note]
  >在 Windows 10 和 Windows Server 2016，MaxThreadsPerQueue 不可用。 线程池的线程数将为"20 * 的 NUMA 节点中的处理器数"。
     

- **AsynchronousCredits**

  ``` 
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  默认值为 512。 此参数限制在单个连接允许的并发异步 SMB 命令数。 某些情况下 (例如，当没有与后端 IIS 服务器的前端服务器) （有关文件更改通知请求，特别是） 需要大量的并发。 若要支持这种情况下，可以增加此项的值。

### <a name="smb-server-tuning-example"></a>SMB 服务器优化示例

以下设置，可以优化在许多情况下的文件服务器性能的计算机。 所有计算机上的这些设置都不是最佳或最合适的。 应在应用各个设置之前评估其影响。

| 参数                       | ReplTest1 | 默认 |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>SMB 客户端性能监视器计数器

有关 SMB 客户端计数器的详细信息，请参阅[Windows Server 2012 文件服务器提示：新的每个共享 SMB 客户端性能计数器提供很好的见解](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx)。
