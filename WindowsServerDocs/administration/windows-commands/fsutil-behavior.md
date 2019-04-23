---
title: Fsutil 行为
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 4593739f25c356e72ea39947c67f3e1301573137
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838268"
---
# <a name="fsutil-behavior"></a>Fsutil 行为

>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

查询或设置 NTFS 卷行为，包括：

-   8.3 字符长文件名的创建

-   8.3 在 NTFS 卷上的字符长度短文件名称中的扩展的字符使用

-   当 NTFS 卷上列出了目录的上次访问时间戳更新

-   使用的配额事件写入到系统日志并为 NTFS 的频率都分页池，并在 NTFS 非分页缓冲池内存缓存级别

-   主文件表区域 （MFT 区域） 的大小

-   无提示删除时的系统遇到 NTFS 卷上的损坏的数据。

-   文件删除通知 （也称为修整或取消映射）

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<VolumePath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <Value> | [<VolumePath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <Frequency> | symlinkevaluation <SymbolicLinkType> | disabledeletenotify {1|0}}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|查询|查询文件系统行为的参数。|
|设置|更改文件系统行为的参数。|
|allowextchar {1&#124;0}|允许 (**1**) 或不允许 (**0**) 中的扩展字符的字符设置 （包括音调符号的字符） 在 NTFS 卷上的 8.3 字符长度短文件名称中使用。<br /><br />必须重新启动计算机中的此参数才会生效。|
|Bugcheckoncorrupt {1&#124;0}|允许 (**1**) 或不允许 (**0**) 生成的 NTFS 卷上的损坏时的 bug 检查。 此功能可用于防止 NTFS 以无提示方式删除数据与自愈 NTFS 功能一起使用时。<br /><br />必须重新启动计算机中的此参数才会生效。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。|
|disable8dot3 [<VolumePath>] {1&#124;0}|禁用 (**1**) 或启用 (**0**) 8.3 FAT 和 NTFS 格式卷上的字符长度文件名称的创建。 （可选） 使用前缀*VolumePath*指定为驱动器名称后, 接冒号或 GUID。|
|disablecompression {1&#124;0}|禁用 **(1)** 或启用 **(0)** NTFS 压缩。<br /><br />必须重新启动计算机中的此参数才会生效。|
|disablecompressionlimit {1&#124;0}| 禁用 **(1)** 或启用 **(0)** NTFS 卷上的 NTFS 压缩限制。 当压缩的文件达到一定程度的碎片，而不会导致将该文件时，将停止 NTFS 压缩文件的其他扩展盘区。 这样做是为了允许不是它们通常是更大的压缩的文件。 将此值设置为 TRUE 将禁用此功能，它的大小限制将压缩文件系统上。 我们不建议禁用此功能。<br /><br />必须重新启动计算机中的此参数才会生效。|
|disableencryption {1&#124;0}|禁用 **(1)** 或启用 **(0)** NTFS 卷上的文件夹和文件加密。<br /><br />必须重新启动计算机中的此参数才会生效。|
|disablefilemetadataoptimization {1&#124;0}|禁用 **(1)** 或启用 **(0)** 文件元数据优化。 NTFS 对给定的文件可以有多少区数有限制。 压缩文件，备用文件可能会变得非常零碎。 默认情况下，NTFS 会定期压缩其内部元数据结构，以允许更零碎的文件。 此值设置为 TRUE 会禁用这种内部优化。 我们不建议禁用此功能。<br /><br />必须重新启动计算机中的此参数才会生效。|
|disablelastaccess {1&#124;0}|禁用 (**1**) 或启用 (**0**) 到最后一个更新每个目录的访问时间戳时 NTFS 卷上列出了目录。<br /><br />必须重新启动计算机中的此参数才会生效。|
|disablespotcorruptionhandling {1&#124;0}|禁用 **(1)** 或启用 **(0)** 发现损坏处理。 Windows 8 和 Windows Server 2012 中引入的新功能之一是新 CHKDSK 的高可用性形式。 此功能允许系统管理员运行 CHKDSK 以进行分析而使其脱机的卷的状态。 我们不建议禁用此功能。<br /><br />必须重新启动计算机中的此参数才会生效。|
|disabletxf {1&#124;0}|禁用 **(1)** 或启用 **(0)** txf 指定的 NTFS 卷上。 TxF 是 NTFS 功能，提供与文件系统操作的语义的事务。 TxF 目前是 deprected，尽管功能仍然可用。 我们不建议禁用此功能在 c： 卷上。<br /><br />必须重新启动计算机中的此参数才会生效。|
|disablewriteautotiering {1&#124;0}|禁用 ReFS v2 自动分层卷的分层逻辑。<br /><br />必须重新启动计算机中的此参数才会生效。|
|encryptpagingfile {1&#124;0}|加密 (**1**) 或不会加密 (**0**) Windows 操作系统中的内存分页文件。<br /><br />必须重新启动计算机中的此参数才会生效。|
|mftzone <Value>|设置 MFT 区的大小，并表示为 200 MB 单位的倍数。 设置*值*为介于**1** （默认值为 200 MB） 到**4** （最多为 800 MB）。<br /><br />必须重新启动计算机中的此参数才会生效。|
|memoryusage <Value>|配置 NTFS 分页缓冲池内存和 NTFS 非分页缓冲池内存的内部缓存级别。 设置为**1**或**2**。 如果设置为**1** （默认值），NTFS 使用默认的分页缓冲池内存量。 如果设置为**2**，NTFS 会增加其后备链列表和内存阈值的大小。 （后备链列表是内核和设备驱动程序创建，如专用内存中缓存的文件系统操作，例如，在读取文件的大小固定的内存缓冲区的池。）<br /><br />必须重新启动计算机中的此参数才会生效。|
|quotanotify <Frequency>|配置 NTFS 配额冲突的系统日志中报告的频率。 有效值为 0 – 4294967295 范围内。 默认频率为 3600 秒 （1 小时）。<br /><br />必须重新启动计算机中的此参数才会生效。|
|symlinkevaluation <SymbolicLinkType>|控制可以在计算机创建的符号链接的类型。 有效选项包括：<br /><br />1.本地到本地符号链接，L2L: {0&#124;1}<br />2.本地到远程符号链接，L2R: {0&#124;1}<br />3.远程连接到本地符号链接，R2R: {0&#124;1}<br />4.远程连接到远程符号链接，R2L: {0&#124;1}|
|DisableDeleteNotify|禁用\( **1** \)或启用\( **0** \)删除通知。 删除通知\(也称为修整或取消映射\)是一项功能，以通知基础存储设备的已释放的文件导致的群集删除操作。 此外：<br /><br />-对于使用 ReFS 的系统默认情况下禁用 v2，剪裁。 适用于 Windows Server 2016。<br />-对于使用 ReFS 的系统默认情况下启用 v1，剪裁。 适用于 Windows Server 2012、 Windows Server 2012 R2 和 Windows Server 2016。<br />-对于使用 NTFS 系统中，trim 除非管理员禁用它，否则默认情况下启用。<br />-如果你的硬盘驱动器或 SAN 报告，它不支持 trim，然后你的硬盘驱动器和 San 不获取剪裁通知。<br />-启用或禁用不需要重新启动。<br />-下一步取消映射命令后生效 trim 发出。<br />-现有的即时注册表更改不会影响 IO。<br />-当您启用或禁用剪裁不需要任何服务重新启动。<br /><br />在 Windows Server 2008 R2 和 Windows 7 中引入了此参数。 | 

## <a name="remarks"></a>备注

-   MFT 区域是保留的区域，使主文件表 (MFT) 以展开，以防止 MFT 碎片。 如果卷上的平均文件大小为 2 KB 或更低，可以将设置更有利**mftzone**为 2 的值。 如果卷上的平均文件大小为 1 KB 或更低，可以将设置更有利**mftzone**为 4 的值。

-   当**disable8dot3**设置为**0**、 每次具有长文件名称创建文件时，NTFS 将具有 8.3 字符长度文件名称的第二个文件条目。 当 NTFS 在目录中创建文件时，它必须查找 8.3 与长文件名的字符长度文件名称。 此参数更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation**注册表项。

-   **Allowextchar**参数更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name**注册表项。

-   **Disablelastaccess**参数减少对文件和目录的上次访问时间戳的日志记录更新的影响。 禁用**上次访问时间**功能提高了文件和目录访问的速度。 此参数更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate**注册表项。

    注意：

    -   基于文件的**上次访问时间**查询是准确的即使磁盘上的所有值不都是最新。 因为精确的值存储在内存中，NTFS 上的查询返回正确的值。

    -   一小时是 NTFS 延迟更新的时间的最长**上次访问时间**磁盘上。 如果 NTFS 更新其他文件属性，如**上次修改时间**，和一个**上次访问时间**更新正在等待 NTFS 更新**上次访问时间**使用而无需其他更新其他性能的影响。

    -   **Disablelastaccess**参数可能会影响备份和远程存储等依赖此功能的计划。

-   增加物理内存不会始终增加可用的 NTFS 页面缓冲的池内存量。 设置**memoryusage**到**2**引发页面缓冲的池内存的限制。 如果您的系统正在打开和关闭多个文件，同一文件中的设置和不已在使用大量的系统内存的其他应用程序或缓存内存的这可能会提高性能。 如果您的计算机的其他应用程序或缓存内存的已在使用大量的系统内存，NTFS 将限制增加分页和非页面缓冲池内存可减少其他进程的可用池内存。 这可能会降低整体系统性能。 此参数更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage**注册表项。

-   中指定的值**mftzone**参数是一个近似值的 MFT 和 MFT 区的新卷上的初始大小和设置在装入时为每个文件系统。 使用卷上的空间时，NTFS 将调整将来的 MFT 增加保留的空间。 如果 MFT 区已经很大，则不会再次保留完整的 MFT 区大小。 MFT 区根据超出末尾的 MFT 连续范围，因为它缩小由于使用的空间。

    文件系统不确定新 MFT 区的位置，直到完全使用当前的 MFT 区。 请注意，这绝对不会发生的典型系统上。

-   删除通知功能开启时，某些设备可能会遇到性能下降。 在这种情况下，使用**disabledeletenotify**选项，若要关闭通知功能。

### <a name="BKMK_examples"></a>示例
若要使用的 GUID，{928842df-5a01-11de-a85c-806e6f6e6963} 指定的磁盘卷禁用 8.3 文件名名称行为查询键入：

```
fsutil behavior query disable8dot3 Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

此外可以通过使用查询 8dot3 名称行为**8dot3name**子命令。

若要查询系统以查看是否启用 TRIM，请键入：

```
fsutil behavior query DisableDeleteNotify
```
这会生成类似于此的输出：

    NTFS DisableDeleteNotify = 1
    ReFS DisableDeleteNotify is not currently set

重写默认行为裁剪\(disabledeletenotify\)对于 ReFS v2，键入：

```
fsutil behavior set DisableDeleteNotify ReFS 0
```

重写默认行为裁剪\(disabledeletenotify\) NTFS 和 ReFS v1 中，键入：
```
fsutil behavior set DisableDeleteNotify 1
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)


