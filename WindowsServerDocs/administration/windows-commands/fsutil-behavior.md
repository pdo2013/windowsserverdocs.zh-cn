---
title: Fsutil 行为
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 3ad6ad8d31bbcb4e9c70192efda37a9836eac14b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377020"
---
# <a name="fsutil-behavior"></a>Fsutil 行为

>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10，Windows Server 2012 R2，Windows 8.1，Windows Server 2012，Windows 8，Windows Server 2008 R2，Windows 7

查询或设置 NTFS 卷行为，其中包括：

-   创建8.3 字符长度的文件名

-   NTFS 卷上长度为8.3 个字符的短文件名的扩展字符使用

-   当目录在 NTFS 卷上列出时，最后一个访问时间戳的更新

-   配额事件写入系统日志和 NTFS 页面缓冲池和 NTFS 非分页池内存缓存级别所用的频率

-   主文件表区域的大小（MFT 区域）

-   当系统在 NTFS 卷上遇到损坏时，无提示删除数据。

-   文件删除通知（也称为修整或取消映射）

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<VolumePath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <Value> | [<VolumePath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <Frequency> | symlinkevaluation <SymbolicLinkType> | disabledeletenotify {1|0}}
```

## <a name="parameters"></a>Parameters

|                 参数                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   query                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            查询文件系统行为参数。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|                    设置                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            更改文件系统行为参数。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|          allowextchar {1&#124;0}           |                                                                                                                                                                                                                                                                                                                                                                                                                                  允许（**1**）或不允许使用在 NTFS 卷上的8.3 字符长度短文件名中使用的扩展**字符集（包括**音调符号字符）中的字符。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|        Bugcheckoncorrupt {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                      允许（**1**）或不允许（**0**）在 NTFS 卷损坏时生成 bug 检查。 此功能可用于在与自愈 NTFS 功能一起使用时防止 NTFS 无提示删除数据。<br /><br />您必须重新启动计算机才能使此参数生效。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。                                                                                                                                                                                                                                                                                                                                                                      |
|   disable8dot3 [<VolumePath>] {1&#124;0}   |                                                                                                                                                                                                                                                                                                                                                                                                                                                       禁用（**1**）或启用（**0**）在 FAT 和 NTFS 格式的卷上创建8.3 字符长度的文件名称。 （可选）使用指定为驱动器名称后跟冒号或 GUID 的*VolumePath*前缀。                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|       disablecompression {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 禁用 **（1）** 或启用 **（0）** NTFS 压缩。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|     disablecompressionlimit {1&#124;0}     |                                                                                                                                                                                                                                                                                   禁用 **（1）** 或启用 **（0）** ntfs 卷上的 ntfs 压缩限制。 压缩文件达到特定的碎片级别时，而不是扩展文件时，NTFS 将停止压缩文件的其他区。 这样做是为了允许压缩文件比平时更大。 如果将此值设置为 TRUE，则将禁用限制系统上压缩文件大小的此功能。 不建议禁用此功能。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                   |
|        disableencryption {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                禁用 **（1）** 或启用 **（0）** NTFS 卷上的文件夹和文件的加密。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| disablefilemetadataoptimization {1&#124;0} |                                                                                                                                                                                                                                                                                                                      禁用 **（1）** 或启用 **（0）** 文件元数据优化。 NTFS 对给定文件可以具有的区数有限制。 压缩文件和备件文件可能会产生很大的碎片。 默认情况下，NTFS 会定期压缩其内部元数据结构，以允许更多的碎片文件。 如果将此值设置为 TRUE，则将禁用此内部优化。 不建议禁用此功能。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                       |
|        disablelastaccess {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                       禁用（**1**）或启用（**0**）在 NTFS 卷上列出目录时，对每个目录上的最后一个访问时间戳进行更新。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|  disablespotcorruptionhandling {1&#124;0}  |                                                                                                                                                                                                                                                                                                                                                    禁用 **（1）** 或启用 **（0）** 点损坏处理。 Windows 8 和 Windows Server 2012 中引入的新功能之一是 CHKDSK 的一种新的高可用性形式。 此功能允许系统管理员运行 CHKDSK 来分析卷的状态，而无需使其脱机。 不建议禁用此功能。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                    |
|           disabletxf {1&#124;0}            |                                                                                                                                                                                                                                                                                                                                                                         禁用 **（1）** 或启用指定 NTFS 卷上的 **（0）** txf。 TxF 是一项 NTFS 功能，可将语义（如语义）提供给文件系统操作。 尽管该功能仍然可用，但现在还 deprected 了 TxF。 不建议在 C：卷上禁用此功能。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                          |
|     disablewriteautotiering {1&#124;0}     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                为分层卷禁用 ReFS v2 自动分层逻辑。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|        encryptpagingfile {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          加密（**1**）或不对 Windows 操作系统中的内存分页文件进行加密（**0**）。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|              mftzone <Value>               |                                                                                                                                                                                                                                                                                                                                                                                                                                           设置 MFT 区的大小，并将其表示为200MB 单元的倍数。 将 "值" 设置为**1** （默认*值*为 200 mb）到 " **4** " （最大值为 800 MB）。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|            memoryusage <Value>             |                                                                                                                                                                                                                                                                                   配置 NTFS 分页池内存和 NTFS 非分页缓冲池内存的内部缓存级别。 设置为**1**或**2**。 当设置为**1** （默认值）时，NTFS 将使用默认的分页池内存量。 设置为**2**时，NTFS 将增加其后备链表列表和内存阈值的大小。 （后备链表列表是固定大小内存缓冲区的池，内核和设备驱动程序将其创建为文件系统操作（如读取文件）的专用内存缓存。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                   |
|          quotanotify <Frequency>           |                                                                                                                                                                                                                                                                                                                                                                                                                                  配置在系统日志中报告 NTFS 配额冲突的频率。 的有效值范围是0–4294967295。 默认频率为3600秒（1小时）。<br /><br />您必须重新启动计算机才能使此参数生效。                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|    symlinkevaluation <SymbolicLinkType>    |                                                                                                                                                                                                                                                                                                                                                                                                   控制可以在计算机上创建的符号链接的种类。 有效选项包括：<br /><br />1.本地到本地符号链接，L2L： {0&#124;1}<br />2.本地到远程符号链接，L2R： {0&#124;1}<br />3.远程到本地符号链接，R2R： {0&#124;1}<br />4.远程到远程符号链接，R2L： {0&#124;1}                                                                                                                                                                                                                                                                                                                                                                                                   |
|            DisableDeleteNotify             | 禁用 \(**1**\) 或启用 \(**0**\) 删除通知。 删除通知 \(also 称为剪裁或取消映射 @ no__t 是一项功能，它将已被释放的群集的基础存储设备通知为文件删除操作。 此外：<br /><br />-对于使用 ReFS v2 的系统，默认情况下，修整处于禁用状态。 适用于 Windows Server 2016。<br />-对于使用 ReFS v1 的系统，默认情况下启用剪裁。 适用于 Windows Server 2012、Windows Server 2012 R2 和 Windows Server 2016。<br />-对于使用 NTFS 的系统，默认情况下会启用剪裁，除非管理员禁用了它。<br />-如果硬盘驱动器或 SAN 报告它不支持 trim，则硬盘驱动器和 San 不会获得剪裁通知。<br />-启用或禁用不需要重新启动。<br />-Trim 在发出下一个取消映射命令时有效。<br />-现有的即时 IO 不受注册表更改的影响。<br />-当你启用或禁用剪裁时，无需重新启动任何服务。<br /><br />此参数是在 Windows Server 2008 R2 和 Windows 7 中引入的。 |

## <a name="remarks"></a>备注

-   MFT 区域是一个保留区域，使主文件表（MFT）可以根据需要进行扩展以防止 MFT 碎片。 如果卷上的平均文件大小为 2 KB 或更小，则将**mftzone**值设置为2可能会很有用。 如果卷上的平均文件大小为 1 KB 或更小，则将**mftzone**值设置为4会很有用。

-   如果将**disable8dot3**设置为**0**，则每次创建具有较长文件名的文件时，NTFS 都会创建一个具有8.3 个字符长度的第二个文件项。 NTFS 在目录中创建文件时，必须查找与长文件名关联的8.3 字符长度的文件名。 此参数更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation**注册表项。

-   **Allowextchar**参数将更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name**注册表项。

-   **Disablelastaccess**参数可降低日志记录更新对文件和目录上最后访问时间戳的影响。 禁用 "**上次访问时间**" 功能可提高文件和目录访问的速度。 此参数更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate**注册表项。

    注意：

    -   即使所有磁盘上的值不是最新的，基于文件的**上次访问时间**查询也是准确的。 NTFS 为查询返回正确的值，因为准确的值存储在内存中。

    -   一小时是 NTFS 延迟更新磁盘上的**上次访问时间**的最长时间。 如果 NTFS 更新其他文件属性（如 "**上次修改时间**"），并且 "**上次访问时间**" 更新处于挂起状态，则 NTFS 将**上次访问时间**与其他更新进行更新，而不会对性能产生额外的影响。

    -   **Disablelastaccess**参数可能会影响依赖于此功能的备份和远程存储等程序。

-   增加物理内存并不总是增加 NTFS 可用的分页池内存量。 将**memoryusage**设置为**2**将引发分页池内存的限制。 如果系统打开并关闭同一文件集中的多个文件，并且没有为其他应用程序或缓存内存使用大量的系统内存，这可能会提高性能。 如果计算机已为其他应用程序或缓存内存使用了大量的系统内存，增加 NTFS 分页和非分页池内存的限制将减少其他进程的可用池内存。 这可能会降低整体系统性能。 此参数更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage**注册表项。

-   在**mftzone**参数中指定的值是在新卷上，mft 的初始大小加上 mft 区的近似值，并为每个文件系统在装入时设置。 由于使用的是卷上的空间，因此 NTFS 会调整保留的空间，以备今后 MFT 增长。 如果 MFT 区域已经很大，则不会再次保留完整的 MFT 区域大小。 因为 MFT 区域基于 MFT 末尾之后的连续范围，所以在使用空间时，将会收缩。

    在完全使用当前 MFT 区域之前，文件系统不会确定新的 MFT 区域位置。 请注意，此操作永远不会出现在典型的系统上。

-   当 "删除通知" 功能打开时，某些设备可能会遇到性能下降。 在这种情况下，请使用**disabledeletenotify**选项关闭通知功能。

### <a name="BKMK_examples"></a>示例
若要查询使用 GUID "{928842df-5a01-11de-a85c-806e6f6e6963}" 指定的磁盘卷的 "禁用8dot3 名称" 行为，请键入：

```
fsutil behavior query disable8dot3 Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

还可以使用**8dot3name**子命令查询8dot3 名称行为。

若要查询系统以查看是否已启用剪裁，请键入：

```
fsutil behavior query DisableDeleteNotify
```
这会生成类似于下面的输出：

    NTFS DisableDeleteNotify = 1
    ReFS DisableDeleteNotify is not currently set

若要重写 0disabledeletenotify @ no__t for ReFS v2 的默认 @no__t 行为，请键入：

```
fsutil behavior set DisableDeleteNotify ReFS 0
```

若要为 NTFS 和 ReFS v1 替代 TRIM \(disabledeletenotify @ no__t-1 的默认行为，请键入：
```
fsutil behavior set DisableDeleteNotify 1
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)


