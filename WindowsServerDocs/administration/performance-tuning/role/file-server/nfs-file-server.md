---
title: NFS 文件服务器性能优化
description: NFS 文件服务器性能优化
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 07e5005c1bc38e791e847c8965cbc9a6c0ac96f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355179"
---
# <a name="performance-tuning-nfs-file-servers"></a>性能优化 NFS 文件服务器

## <a href="" id="servicesnfs"></a>NFS 服务模型


以下各节提供有关用于客户端-服务器通信的 Microsoft 服务网络文件系统（NFS）模型的信息。 由于 NFS v2 和 NFS v3 仍是最广泛部署的协议版本，因此除 MaxConcurrentConnectionsPerIp 之外的所有注册表项都仅适用于 NFS v2 和 NFS v3。

对于 NFS 4.1 协议，不需要进行注册表优化。

### <a name="service-for-nfs-model-overview"></a>NFS 服务模型概述

Microsoft NFS 服务为具有混合 Windows 和 UNIX 环境的企业提供文件共享解决方案。 此通信模型由客户端计算机和服务器组成。 通过重定向程序（Rdbss）和 NFS 微型重定向程序（Nfsrdr）位于服务器上的客户端请求文件上的应用程序。 小型重定向程序使用 NFS 协议通过 TCP/IP 发送请求。 服务器通过 TCP/IP 接收来自客户端的多个请求，并将请求路由到本地文件系统（.sys），该文件将访问存储堆栈。

下图显示了 NFS 的通信模型。

![nfs 通信模型](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>NFS 文件服务器的优化参数

以下 REG @ no__t-0DWORD 注册表设置可能会影响 NFS 文件服务器的性能：

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    默认值为 0。 此参数确定是否为 FILE @ no__t-0RANDOM @ no__t-1ACCESS 或 FILE @ no__t-2SEQUENTIAL @ no__t-3ONLY 打开文件，具体取决于工作负荷 i/o 特性。 将此值设置为1可强制打开文件 @ no__t-0RANDOM @ no__t-1ACCESS 的文件。 FILE @ no__t-0RANDOM @ no__t-1ACCESS 禁止文件系统和缓存管理器进行预提取。

    >[!NOTE]
    > 必须仔细评估此设置，因为它可能会对系统文件缓存增长造成潜在影响。


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    默认值为 5。 此参数控制文件句柄缓存中 NFS 缓存条目的生存期。 参数是指具有关联的开放 NTFS 文件句柄的缓存条目。 实际生存期约等于 RdWrHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值为1，最大值为60。

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    默认值为 5。 此参数控制文件句柄缓存中 NFS 缓存条目的生存期。 参数是指没有关联的开放 NTFS 文件句柄的缓存条目。 NFS 服务使用这些缓存项来存储文件的文件属性，而无需在文件系统中保留打开的句柄。 实际生存期约等于 RdWrNfsHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值为1，最大值为60。

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    默认值为 5。 此参数控制文件句柄缓存中 NFS 读取缓存条目的生存期。 实际生存期约等于 RdWrNfsReadHandlesLifeTime 乘以 RdWrThreadSleepTime。 最小值为1，最大值为60。

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    默认值为 5。 此参数控制在文件句柄缓存上运行清理线程前等待的时间间隔。 值以计时周期为单位，且不具有确定性。 勾选标记等效于大约100毫微秒。 最小值为1，最大值为60。

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    默认值为 4。 此参数指定文件句柄缓存条目使用的最大内存。 最小值为1，最大值为 1 @ no__t-01024 @ no__t-11024 @ no__t-21024 （1073741824）。

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    默认值为 0。 此参数指定是否在内存中锁定为 FileHandleCacheSizeInMB 指定的缓存大小分配的物理页。 将此值设置为1可启用此活动。 页面锁定在内存中（不分页到磁盘），这会提高解析文件句柄的性能，但会减少应用程序可用的内存。

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    默认值为64。 此参数指定读取数据缓存每个卷的最大句柄数。 仅在内存超过 1 GB 的系统上创建读取缓存条目。 最小值为0，最大值为0xFFFFFFFF。

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    默认值为 1。 此参数控制由 NFS 文件服务器提供的句柄是否经过加密。 将其设置为0会禁用句柄签名。

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    默认值为60。 此参数是一个软超时，用于控制 NFS V3 不稳定写入数据缓存的持续时间。 最小值为1，最大值为600。 实际生存期约等于 RdWrNfsDeferredWritesFlushDelay 乘以 RdWrThreadSleepTime。

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    默认值为1（已启用）。 此参数控制在 NFS V2 和 V3 CREATE 期间打开的句柄是否在文件句柄缓存中保留。 将此值设置为0可禁止在 CREATE 和 MKDIR 代码路径中向缓存添加条目。

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    增加为指定工作队列创建的延迟工作线程数。 延迟的工作线程处理不被视为时间关键的工作项，并且可以在等待工作项时使其内存堆栈分页。 线程数不足会降低工作项的服务速率;如果值过高，则会不必要地消耗系统资源。

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    Windows Server 2012 和 Windows Server 2012 R2 中的默认值为2。 在 Windows Server 2012 之前的版本中，默认值为0。 此参数确定 NTFS 是否为长文件名和包含扩展字符集中的字符的文件名以8dot3 （MSDOS.SYS）命名约定生成短名称。 如果此项的值为0，则文件可以有两个名称：用户指定的名称和 NTFS 生成的短名称。 如果用户指定的名称遵循8dot3 命名约定，则 NTFS 不会生成短名称。 如果值为2，则表示可以按卷配置此参数。

    >[!NOTE]
    > 默认情况下，系统卷启用了8dot3。 Windows Server 2012 和 Windows Server 2012 R2 中的所有其他卷默认禁用8dot3。 更改此值不会更改文件的内容，但可以避免创建文件的短名称属性，这也会改变 NTFS 显示和管理文件的方式。 对于大多数文件服务器，建议设置为1（禁用）。


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    默认值为 1。 此系统全局交换机通过禁用对上一个文件或目录访问的日期和时间戳的更新来减少磁盘 i/o 负载和延迟时间。

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    MaxConcurrentConnectionsPerIp 参数的默认值为16。 最多可将此值增大到8192，以增加每个 IP 地址的连接数。
