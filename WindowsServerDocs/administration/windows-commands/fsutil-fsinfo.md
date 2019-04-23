---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: fsutil fsinfo
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 434dfde2286538367fb96d168b06983cb4357067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873038"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

列出所有驱动器、 查询驱动器类型、 查询卷信息、 查询特定于 NTFS 的卷信息，或查询文件系统统计数据。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|驱动器|列出计算机中的所有驱动器。|
|drivetype|查询一个驱动器，并列出其类型，例如 CD-ROM 驱动器。|
|ntfsinfo|列出指定的卷，如的扇区、 总群集、 免费的群集，以及开始和 MFT 区的最终数量的 NTFS 特定卷信息。|
|sectorinfo|列出有关的硬件扇区大小和对齐方式的信息。|
|统计信息|列出文件系统指定的卷，如元数据、 日志文件和 MFT 读取和写入操作的统计信息。|
|volumeinfo|列出指定的卷，如文件系统的信息以及是否卷支持区分大小写的文件名称和文件名称中的 unicode 磁盘配额，或 DirectAccess (DAX) 卷。|
|<"VolumePath">|指定 （后跟一个冒号） 的驱动器号。|
|<"RootPathname">|指定根驱动器的驱动器号 （后跟一个冒号）。|

## <a name="BKMK_examples"></a>示例
若要列出所有计算机中的驱动器，请键入：

```
fsutil fsinfo drives
```

类似于显示以下输出：

```
Drives: A:\ C:\ D:\ E:\       
```

若要查询的驱动器 C 的驱动器类型，请键入：

```
fsutil fsinfo drivetype c:
```

查询结果可能包括：

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

若要查询卷 E 的卷信息，请键入：

```
fsinfo volumeinfo e:\
```

类似于显示以下输出：

```
Volume Name :Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
.
.
.
Supports Named Streams
Is DAX Volume
```

若要为特定于 NTFS 的卷信息的查询驱动器 F，键入：

```
fsutil fsinfo ntfsinfo f:
```

类似于显示以下输出：

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

若要查询文件系统的基础硬件扇区的信息，请键入：

```
fsinfo sectorinfo d:
```

类似于显示以下输出：

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector :                                 4096
PhysicalBytesPerSectorForAtomicity :                    4096
.
.
.
Trim Not Supported
DAX capable
```

若要查询驱动器 E 的文件系统统计信息，请键入：

```
fsinfo statistics e:
```

类似于显示以下输出：

```
File System Type :     NTFS
Version :              1
UserFileReads :        75021
UserFileReadBytes :    1305244512
.
.
.
LogFileWriteBytes :    180936704       
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)
[Fsutil](Fsutil.md)


