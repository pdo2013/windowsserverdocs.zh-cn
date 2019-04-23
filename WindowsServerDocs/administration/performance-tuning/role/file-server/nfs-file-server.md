---
title: NFS 文件服务器的性能优化
description: NFS 文件服务器的性能优化
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 06a2a7206d3673046bd5a926a657bac91b02bf5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879008"
---
# <a name="performance-tuning-nfs-file-servers"></a>性能优化 NFS 文件服务器

## <a href="" id="servicesnfs"></a>有关 NFS 模型服务


以下部分提供的 Microsoft 服务的客户端-服务器通信的网络文件系统 (NFS) 模型的信息。 因为 NFS v2 和 NFS v3 是仍最广泛部署的协议的版本，所有的 MaxConcurrentConnectionsPerIp 除外的注册表项适用于 NFS v2 和 NFS v3 仅。

无注册表优化是必需的 NFS 4.1 版协议。

### <a name="service-for-nfs-model-overview"></a>Service for NFS 模型概述

Microsoft nfs 服务拥有 Windows 和 UNIX 混合的环境的企业提供文件共享解决方案。 此通信模型由客户端计算机和一台服务器组成。 在客户端上的应用程序请求的重定向程序 (Rdbss.sys) 和 NFS 最小化重定向程序 (Nfsrdr.sys) 通过在服务器存在的文件。 最小-重定向程序使用 NFS 协议发送 TCP/IP 通过其请求。 服务器接收来自客户端通过 TCP/IP 的多个请求，并将请求路由到本地文件系统 (Ntfs.sys) 访问的存储堆栈。

下图显示 nfs 通信模型。

![nfs 通信模型](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>优化参数为 NFS 文件服务器

以下 REG\_DWORD 注册表设置可能会影响 NFS 文件服务器的性能：

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    默认值为 0。 此参数确定是否为文件的打开文件\_随机\_访问权限或文件\_SEQUENTIAL\_仅，具体取决于工作负荷 I/O 特征。 将此值设置为 1 以强制要打开的文件的文件\_随机\_访问。 文件\_随机\_访问阻止的文件系统和缓存管理器预提取。

    >[!NOTE]
    > 必须慎重考虑此设置，因为它可能会有增长对系统文件缓存的潜在影响。


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    默认值为 5。 此参数控制的文件句柄缓存中的 NFS 缓存项的生存期。 参数是指具有关联的打开 NTFS 文件句柄的缓存项。 实际的生存期为约等于 RdWrHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值为 1，最大值为 60。

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    默认值为 5。 此参数控制的文件句柄缓存中的 NFS 缓存项的生存期。 参数是指没有关联的打开 NTFS 文件句柄的缓存项。 Nfs 服务使用这些缓存条目来存储文件的文件属性而不保留与文件系统的打开句柄。 实际的生存期为约等于 RdWrNfsHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值为 1，最大值为 60。

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    默认值为 5。 此参数控制 NFS 读取文件句柄缓存中的缓存项的生存期。 实际的生存期为约等于 RdWrNfsReadHandlesLifeTime 乘以 RdWrThreadSleepTime。 最小值为 1，最大值为 60。

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    默认值为 5。 此参数控制运行清除线程上的文件句柄缓存之前的等待时间间隔。 值是以刻度为单位，和不确定性。 一个对勾相当于大约 100 纳秒为单位。 最小值为 1，最大值为 60。

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    默认值为 4。 此参数指定的最大的内存可供文件句柄缓存条目。 最小值为 1，最大值为 1\*1024年\*1024年\*1024 (1073741824)。

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    默认值为 0。 此参数指定为 FileHandleCacheSizeInMB 指定的缓存大小分配的物理页被锁定在内存中。 此值设置为 1 将使此活动。 页面会被锁定在内存中 （不分页到磁盘），这可以提高解析的文件句柄的性能，但减少应用程序的内存。

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    默认值为 64。 此参数指定每个卷读取的数据缓存的句柄的最大数量。 仅在具有多个 1 GB 的内存的系统上创建读取缓存项。 最小值为 0，最大值为 0xFFFFFFFF。

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    默认值为 1。 此参数控制是否可以 NFS 文件服务器的句柄密码学角度上签名。 将其设置为 0 会禁用签名句柄。

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    默认值为 60。 此参数是控制 NFS V3 不稳定编写数据缓存的持续时间的软超时。 最小值为 1，且最大值为 600。 实际的生存期为约等于 RdWrNfsDeferredWritesFlushDelay 乘以 RdWrThreadSleepTime。

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    默认值为 1 （启用）。 此参数控制处理程序将保留在文件中的 NFS V2 和 V3 创建和 MKDIR RPC 过程期间打开的句柄是否处理缓存。 将此值设置为 0 可禁用条目添加到缓存中创建和 MKDIR 代码路径。

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    创建指定的工作队列的延迟的辅助线程的数量增加。 延迟辅助线程过程工作项不被视为关键时间且，可能会有等待工作项时换出它们的内存堆栈。 在哪些工作项提供服务; 速率减少到的线程数目不足过高的值会不必要地消耗系统资源。

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    Windows Server 2012 和 Windows Server 2012 R2 中的默认值为 2。 在 Windows Server 2012 之前的版本中，默认值为 0。 此参数确定是否 NTFS 中 8dot3 生成短名称 (MSDOS) 命名约定为长文件名和文件包含扩展的字符集中的字符的名称。 如果此项的值为 0，文件可以具有两个名称： 用户指定的名称和 NTFS 生成的短名称。 如果用户指定的名称后跟 8dot3 命名约定，NTFS 不会生成一个短名称。 此参数，可以配置每个卷的 2 表示一个值。

    >[!NOTE]
    > 系统卷上有 8dot3 默认情况下启用。 Windows Server 2012 和 Windows Server 2012 R2 中的所有其他卷具有默认情况下禁用 8.3 文件名。 更改此值不会更改文件的内容，但它避免了该文件，这样也会更改 NTFS 如何显示和管理文件的短名称属性创建。 对于大多数文件服务器，建议的设置为 1 （禁用）。


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    默认值为 1。 此系统全局开关通过禁用的最后一个文件或目录访问权限的日期和时间戳更新减少了磁盘 I/O 负载和延迟。

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    MaxConcurrentConnectionsPerIp 参数的默认值为 16。 您可以增大此值最多 8192 以增加每个 IP 地址的连接数。
