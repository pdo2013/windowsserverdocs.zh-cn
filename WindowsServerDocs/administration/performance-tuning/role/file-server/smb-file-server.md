---
title: SMB 文件服务器的性能优化
description: SMB 文件服务器的性能优化
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 5383d16ac4c98651aa6afe996dbad88a6d60ee7a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370233"
---
# <a name="performance-tuning-for-smb-file-servers"></a>SMB 文件服务器的性能优化

## <a name="smb-configuration-considerations"></a>SMB 配置注意事项
不要启用您的文件服务器和客户端不需要的任何服务或功能。 其中可能包括 SMB 签名、客户端缓存、文件系统小型筛选器、搜索服务、计划任务、NTFS 加密、NTFS 压缩、IPSEC、防火墙筛选器、Teredo 和 SMB 加密。

确保 BIOS 和操作系统电源管理模式按需要设置，这可能包括高性能模式或更改的 C 状态。 确保安装了最新、最灵活、最快的存储和网络设备驱动程序。

复制文件是在文件服务器上执行的常见操作。 Windows Server 具有多个可使用命令提示符运行的内置文件复制实用工具。 建议使用 Robocopy。 在 Windows Server 2008 R2 中引入， **Robocopy 选项 Robocopy 可以在复制**多个小文件时使用多个线程显著提高远程文件传输速度。 我们还建议使用 **/log**选项，通过将日志重定向到 NUL 设备或文件来减少控制台输出。 使用 Xcopy 时，建议将 **/q**和 **/k**选项添加到现有参数中。 前者通过减少控制台输出降低了 CPU 开销，后者可减少网络流量。

## <a name="smb-performance-tuning"></a>SMB 性能优化


文件服务器性能和可用的 tunings 取决于在每个客户端和服务器之间协商的 SMB 协议，以及已部署的文件服务器功能。 当前可用的最高协议版本是 Windows Server 2016 和 Windows 10 中的 SMB 3.1.1。 可以通过在客户端上使用 Windows PowerShell **SMBConnection**和 SMBSession | 来检查网络上使用的 SMB 版本。服务器上的 FL。

### <a name="smb-30-protocol-family"></a>SMB 3.0 协议系列

在 Windows Server 2012 中引入了 SMB 3.0，并在 Windows Server 2012 R2 （SMB 3.02）和 Windows Server 2016 （SMB 3.1.1）中进一步增强了这些功能。 此版本引入了可显著提高文件服务器性能和可用性的技术。 有关详细信息，请参阅[Windows Server 2012 中的 SMB 和 2012 R2 2012](https://aka.ms/smb3plus)以及[smb 3.1.1 中的新增功能](https://aka.ms/smb311)。



### <a name="smb-direct"></a>SMB 直通

SMB Direct 引入了将 RDMA 网络接口用于高吞吐量、低延迟和低 CPU 使用率的能力。

当 SMB 检测到支持 RDMA 功能的网络时，它会自动尝试使用 RDMA 功能。 但是，如果出于任何原因，SMB 客户端无法使用 RDMA 路径进行连接，它将直接使用 TCP/IP 连接。 与 SMB 直通兼容的所有 RDMA 接口都需要实现 TCP/IP 堆栈，并且 SMB 多通道可感知到这一点。

Smb Direct 在任何 SMB 配置中都不是必需的，但对于那些希望降低延迟和 CPU 使用率较低的用户，始终建议使用 SMB 直通。

有关 SMB 直通的详细信息，请参阅[使用 Smb Direct 提高文件服务器的性能](https://aka.ms/smbdirect)。

### <a name="smb-multichannel"></a>SMB 多通道

SMB 多通道允许文件服务器同时使用多个网络连接并提供更高的吞吐量。

有关 SMB 多通道的详细信息，请参阅[部署 smb 多通道](https://aka.ms/smbmulti)。

### <a name="smb-scale-out"></a>SMB 横向扩展

SMB 扩展允许群集配置中的 SMB 3.0 在群集的所有节点中显示共享。 利用此主动/主动配置，可以进一步缩放文件服务器群集，而无需使用多个卷、共享和群集资源的复杂配置。 最大共享带宽是所有文件服务器群集节点的总带宽。 总带宽不再受限于单个群集节点的带宽，而是取决于后备存储系统的容量。 你可以通过添加节点来增加总带宽。

有关 SMB 横向扩展的详细信息，请参阅[应用程序数据概述横向扩展文件服务器](https://technet.microsoft.com/library/hh831349.aspx)，以及[要横向扩展或不横向](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)扩展的博客文章。

### <a name="performance-counters-for-smb-30"></a>SMB 3.0 的性能计数器

以下 SMB 性能计数器是在 Windows Server 2012 中引入的，并且在监视 SMB 2 和更高版本的资源使用情况时，它们被视为一组基本计数器。 将性能计数器记录到本地原始（.blg）性能计数器日志。 使用通配符（\*）收集所有实例的开销较低，然后使用 dcdiag.exe 在后期处理过程中提取特定的实例。

-   **SMB 客户端共享**

    这些计数器显示有关正在使用 SMB 2.0 或更高版本的客户端访问的服务器上的文件共享的信息。

    如果你已熟悉 Windows 中的常规磁盘计数器，则可能会注意到某个相似性。 这不是意外情况。 SMB 客户端共享性能计数器的设计与磁盘计数器完全匹配。 这样一来，您就可以轻松地重复使用目前所用的应用程序磁盘性能优化指南。 有关计数器映射的详细信息，请参阅[按共享客户端性能计数器博客](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx)。

-   **SMB 服务器共享**

    这些计数器显示有关服务器上的 SMB 2.0 或更高版本的文件共享的信息。

-   **SMB 服务器会话**

    这些计数器显示有关使用 SMB 2.0 或更高版本的 SMB 服务器会话的信息。

    在服务器端（服务器共享或服务器会话）上打开计数器可能会对高 IO 工作负荷产生显著的性能影响。

-   **继续密钥筛选器**

    这些计数器显示有关简历密钥筛选器的信息。

-   **SMB 直接连接**

    这些计数器衡量连接活动的不同方面。 一台计算机可以具有多个 SMB 直接连接。 SMB 直接连接计数器将每个连接表示为一对 IP 地址和端口，其中，第一个 IP 地址和端口表示连接的本地终结点，第二个 IP 地址和端口表示连接的远程终结点。

-   **物理磁盘、SMB、CSV FS 性能计数器关系**

    有关物理磁盘、SMB 和 CSV FS （文件系统）计数器如何相关的详细信息，请参阅以下博客文章：[群集共享卷性能计数器](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx)。

## <a name="tuning-parameters-for-smb-file-servers"></a>SMB 文件服务器的优化参数


以下 REG @ no__t-0DWORD 注册表设置可能会影响 SMB 文件服务器的性能：

- **Smb2CreditsMin**和**smb2creditsmax 为**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  默认值分别为512和8192。 这些参数允许服务器在指定边界内动态地限制客户端操作并发。 某些客户端可能会提高吞吐量，增加并发限制，例如，通过高带宽、高延迟链路复制文件。
    
  > [!TIP]
  > 在 Windows 10 和 Windows Server 2016 之前，向客户端授予的信用额度将在 Smb2CreditsMin 和 Smb2creditsmax 为之间动态变化，这是基于尝试根据网络延迟确定要授予的最佳信用额度的算法。和信用使用情况。 在 Windows 10 和 Windows Server 2016 中，SMB 服务器已更改为在请求达到配置的最大信用额度时无条件地授予信用额度。 作为此更改的一部分，信用限制机制会在服务器处于内存压力下时减少每个连接的信用时段的大小。 触发限制的内核内存不足事件仅在服务器内存不足（< 几 MB）时才会发出信号。 由于服务器不再缩小信用时段，Smb2CreditsMin 设置不再是必需的，现已被忽略。
  > 
  > 你可以监视 SMB 客户端共享 @ no__t-0Credit 每隔一秒，查看信用是否有任何问题。

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    默认值为0，表示不添加其他关键内核工作线程。 此值影响文件系统缓存用于预读和后写请求的线程数。 如果引发此值，则可以在存储子系统中进行更多的排队 i/o，还可以提高 i/o 性能，尤其是在具有很多逻辑处理器和强大的存储硬件的系统上。

    >[!TIP]
    > 如果缓存管理器脏数据（性能计数器缓存 @ no__t-0Dirty 页）的数量正在增长以消耗大量的数据（超过约 25%），则可能需要增加该值如果系统正在进行大量同步读 i/o，则为。

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  默认值为20。 增加此值会引发文件服务器可用于为并发请求服务的线程数。 当需要为大量活动的连接提供服务，并且硬件资源（例如存储带宽）足够时，增加该值可以提高服务器的可伸缩性、性能和响应时间。

  >[!TIP]
  > 指示可能需要增加值的情况是，SMB2 工作队列是否增长得非常大（性能计数器 "服务器工作队列 @ no__t-0Queue Length @ no__t-1SMB2 非阻止性 \*" 持续时间超过 ~ 100）。

  >[!Note]
  >在 Windows 10 和 Windows Server 2016 中，MaxThreadsPerQueue 不可用。 线程池的线程数将为 "20 * NUMA 节点中的处理器数"。
     

- **AsynchronousCredits**

  ``` 
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  默认值为512。 此参数限制单个连接上允许的并发异步 SMB 命令数。 某些情况下（例如，当具有后端 IIS 服务器的前端服务器时）需要大量并发（特别是对于文件更改通知请求）。 此项的值可以增加以支持这些情况。

### <a name="smb-server-tuning-example"></a>SMB 服务器优化示例

在许多情况下，以下设置可以优化计算机的文件服务器性能。 所有计算机上的这些设置都不是最佳或最合适的。 应在应用各个设置之前评估其影响。

| 参数                       | ReplTest1 | 默认 |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>SMB 客户端性能监视器计数器

有关 SMB 客户端计数器的详细信息，请参阅 [Windows Server 2012 文件服务器提示：新的基于共享的 SMB 客户端性能计数器提供了 @ no__t。
