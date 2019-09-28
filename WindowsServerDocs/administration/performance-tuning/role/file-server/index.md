---
title: 文件服务器的性能优化
description: 运行 Windows Server 的文件服务器的性能优化
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 5f772d2333acb2d48bf27168aca42754013dd8be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370220"
---
# <a name="performance-tuning-for-file-servers"></a>文件服务器的性能优化

考虑到平均负载、峰值负载、容量、增长计划和响应时间，应选择适当的硬件来满足要求的文件服务器负载。 硬件瓶颈会限制软件优化的有效性。

## <a name="general-tuning-parameters-for-clients"></a>客户端的常规优化参数

以下 REG\_DWORD 注册表设置可能会影响与 SMB 文件服务器交互的客户端计算机的性能：

-   **ConnectionCountPerNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerNetworkInterface
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    默认值为 1，强烈建议使用默认值。 有效范围为 1 - 16。 要与服务器建立的每个非 RSS 接口的连接数上限。


-   **ConnectionCountPerRssNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRssNetworkInterface
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    默认值为 4，强烈建议使用默认值。 有效范围为 1 - 16。 要与服务器建立的每个 RSS 接口的连接数上限。

-   **ConnectionCountPerRdmaNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRdmaNetworkInterface
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    默认值为 2，强烈建议使用默认值。 有效范围为 1 - 16。 要与服务器建立的每个 RDMA 接口的连接数上限。

-   **MaximumConnectionCountPerServer**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaximumConnectionCountPerServer
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    默认值为 32，有效范围为 1 - 64。 要与运行 Windows Server 2012 的单个服务器建立的所有接口的连接数上限。

-   **DormantDirectoryTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantDirectoryTimeout
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    默认值为 600 秒。 服务器目录句柄通过目录租用保持打开状态的最长时间。

-   **FileInfoCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheLifetime
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 10 秒。 文件信息缓存超时时间。

-   **DirectoryCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheLifetime
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 10 秒。 这是目录缓存超时。

    > [!NOTE]
    > 此参数控制在缺少目录租约时的目录元数据缓存。
     

-   **DirectoryCacheEntrySizeMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntrySizeMax
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 64 KB。 这是目录缓存条目的最大大小。

-   **FileNotFoundCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheLifetime
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 5 秒。 找不到文件缓存超时期限。

-   **CacheFileTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\CacheFileTimeout
    ```

    适用于 Windows 8.1、Windows 8、Windows Server 2012、Windows Server 2012 R2 和 Windows 7

    默认值为 10 秒。 此设置控制应用程序关闭文件的最后一个句柄后，重定向程序保留文件缓存数据的时长（以秒为单位）。

-   **DisableBandwidthThrottling**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 0。 默认情况下，SMB 重定向程序会限制高延迟网络连接的吞吐量，在某些情况下可以避免与网络相关的超时。 将此注册表值设置为 1 可禁用此限制，从而在高延迟网络连接上实现更高的文件传输吞吐量。

-   **DisableLargeMtu**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableLargeMtu
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    仅 Windows 8 的默认值为 0。 在 Windows 8 中，SMB 重定向程序为每个请求传输高达 1 MB 的有效负载，可提高文件传输速度。 将此注册表值设置为 1 会将请求大小限制为 64 KB。 应在应用此设置之前评估其影响。

-   **RequireSecuritySignature**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\RequireSecuritySignature
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 0，表示禁用 SMB 签名。 将此值更改为 1 可为所有 SMB 通信启用 SMB 签名，从而阻止 SMB 与禁用 SMB 签名的计算机进行通信。 SMB 签名虽然会增加 CPU 成本和网络往返次数，但有助于阻止中间人攻击。 如果不需要 SMB 签名，请确保所有客户端和服务器上的此注册表值均为 0。 
    
    有关详细信息，请参阅 [SMB 签名的基础知识](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

-   **FileInfoCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 64，有效范围为 1 - 65536。 此值用于确定可以由客户端缓存的文件元数据量。 当访问大量文件时，增加该值可以减少网络流量并提高性能。

-   **DirectoryCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 16，有效范围为 1 - 4096。 此值用于确定可以由客户端缓存的目录信息量。 在访问大量目录时，增加该值可以减少网络流量并提高性能。

-   **FileNotFoundCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 128，有效范围为 1 - 65536。 此值用于确定可由客户端缓存的文件名信息量。 当访问大量文件名时，增加该值可以减少网络流量并提高性能。

-   **MaxCmds**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaxCmds
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 15。 此参数限制会话上未完成的请求数。 增加该值会占用更多内存，但它可以通过启用更深层的请求管道来提高性能。 同时增加该值和 MaxMpxCt 的值还可以消除大量未完成的长期文件请求（例如 FindFirstChangeNotification 调用）所造成的错误。 此参数不会影响与 SMB 2.0 服务器的连接。

-   **DormantFileLimit**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit
    ```

    适用于 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    默认值为 1023。 此参数指定应用程序关闭文件后应在共享资源上保持打开状态的文件的数量上限。

### <a name="client-tuning-example"></a>客户端优化示例

客户端计算机的常规优化参数可以优化计算机，帮助其访问远程文件共享，特别是通过一些高延迟网络（例如，分支机构、跨数据中心通信、家庭办公室和移动宽带）进行的访问。 所有计算机上的这些设置都不是最佳或最合适的。 应在应用各个设置之前评估其影响。

| 参数                   | 值 | 默认 |
|-----------------------------|-------|---------|
| DisableBandwidthThrottling  | 1     | 0       |
| FileInfoCacheEntriesMax     | 32768 | 64      |
| DirectoryCacheEntriesMax    | 4096  | 16      |
| FileNotFoundCacheEntriesMax | 32768 | 128     |
| MaxCmds                     | 32768 | 15      |

 

从 Windows 8 开始，可使用 Set-SmbClientConfiguration 和 Set-SmbServerConfiguration Windows PowerShell cmdlet 配置其中的许多 SMB 设置   。 也可以使用 Windows PowerShell 配置只与注册表相关的设置。

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```
