---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: Fsutil 文件
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ffaf02f74f20f4eb94b94d8f0ffc51f26a62390e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828118"
---
# <a name="fsutil-file"></a>Fsutil 文件
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

按用户名查找文件 （如果已启用磁盘配额）、 查询的一个文件分配的范围中，设置文件的短名称、 设置文件的有效的数据长度、 设置为零的数据文件，或创建一个新文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil file [createnew] <filename> <length>
fsutil file [findbysid] <username> <directory>
fsutil file [optimizemetadata] [/A] <filename>
fsutil file [queryallocranges] offset=<offset> length=<length> <filename>
fsutil file [queryextents] [/R] <filename> [<startingvcn> [<numvcns>]]
fsutil file [queryfileid] <filename>
fsutil file [queryfilenamebyid] <volume> <fileid>
fsutil file [queryoptimizemetadata] <filename>
fsutil file [queryvaliddata] [/R] [/D] <filename>
fsutil file [seteof] <filename> <length>
fsutil file [setshortname] <filename> <shortname>
fsutil file [setvaliddata] <filename> <datalength>
fsutil file [setzerodata] offset=<offset> length=<length> <filename>

```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|createnew|将创建指定的名称和大小的文件具有由零组成的内容。|
|\<filename>|指定的文件包括文件名和扩展名，例如 C:\documents\filename.txt 的完整路径。|
|\<length>|指定文件的有效的数据长度。|
|findbysid|查找属于指定用户在 NTFS 卷上启用了磁盘配额的文件。|
|\<username>|指定用户的用户名或登录名。|
|\<directory>|指定的目录，例如 C:\users 的完整路径。|
|optimizemetadata|这会为给定文件执行的元数据的即时压缩。|
|/A|前和优化后，可分析文件元数据。|
|queryallocranges|查询的 NTFS 卷上的文件已分配的范围。 可用于确定文件是否包含稀疏区域。|
|offset=\<offset>|指定应设置为零的范围的起始位置。|
|length=\<length>|指定的长度 （以字节为单位） 的范围。|
|queryextents|查询扩展盘区的文件。|
|/R|如果<filename>是重新分析点，打开它而不是其目标。|
|\<startingvcn>|指定查询的第一个 VCN。 如果省略，则开始 VCN 0。|
|\<numvcns>|VCNs 查询数。 如果省略则为 0，直到 EOF 的查询。|
|queryfileid|查询 NTFS 卷上的文件的文件的 ID。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。|
|\<volume>|指定的卷作为驱动器名称后跟一个冒号。|
|queryfilenamebyid|显示 NTFS 卷上指定的文件 ID 的随机链接名称。 文件可以有多个指向该文件的链接名称，因为不保证将作为查询结果的文件名称提供哪些文件链接。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。|
|\<fileid>|指定的 NTFS 卷上的文件的 ID。|
|queryoptimizemetadata|查询文件的元数据状态。|
|queryvaliddata|查询文件的有效数据长度。|
|/D|显示详细的有效数据的信息。|
|seteof|设置给定文件的 EOF。|
|setshortname|设置为 NTFS 卷上的文件的短名称 （8.3 字符长度文件名称）。|
|\<shortname>|指定文件的短名称。|
|setvaliddata|设置 NTFS 卷上的文件的有效的数据长度。|
|\<datalength>|以字节为单位指定文件的长度。|
|setzerodata|一系列设置 (通过指定*偏移量*并*长度*) 为零的文件，会清空该文件。 如果文件是稀疏文件，将已回收的基础的分配单元。|

## <a name="remarks"></a>备注

-   在 NTFS 中，有的文件长度的两个重要概念： 最终文件 (EOF) 标记和有效的数据长度 (VDL)。 EOF 指示该文件的实际长度。 VDL 标识的磁盘上的有效数据的长度。 任何读取 VDL 和 EOF 之间自动返回 0，若要保留的 C2 对象重新使用要求。

-   **Setvaliddata**参数才可用于管理员，因为它需要执行卷维护任务 (SeManageVolumePrivilege) 特权。 此功能仅用于高级多媒体和系统区域网络方案。 **Setvaliddata**参数必须是大于当前 VDL，但小于当前文件大小的正值。

    它可用于程序设置 VDL 时：

    -   编写直接向通过硬件通道磁盘的原始群集。 这将允许程序以通知此范围包含可以返回给用户的有效数据的文件系统。

    -   创建大型文件时性能是一个问题。 这避免了用零填充该文件，创建或扩展的文件时所需的时间。

## <a name="BKMK_examples"></a>示例
若要查找所拥有的 scottb 驱动器 C 上的文件，请键入：

```
fsutil file findbysid scottb c:\users  
```

若要查询的 NTFS 卷上的文件已分配的范围，请键入：

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt  
```

若要优化的文件的元数据，请键入：

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

若要查询的文件的扩展盘区，请键入：

```
fsutil file queryextents C:\Temp\sample.txt
```

若要设置的文件 EOF，请键入：

```
fsutil file seteof C:\testfile.txt 1000
```

若要设置到 Longfile.txt 驱动器 C 上的文件 Longfilename.txt 的短名称，键入：

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

若要设置的有效数据长度为 4096 个字节的 NTFS 卷上名为 Testfile.txt 的文件，请键入：

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

若要设置为零以其为空的一系列 NTFS 卷上的文件，请键入：

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


