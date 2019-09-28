---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: Fsutil 稀疏
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f9fb3cf46afb7e96c13fb623bc8f4fe67c1f3694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376815"
---
# <a name="fsutil-sparse"></a>Fsutil 稀疏
>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10，Windows Server 2012 R2，Windows 8.1，Windows Server 2012，Windows 8，Windows Server 2008 R2，Windows 7

管理稀疏文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil sparse [queryflag] <FileName>
fsutil sparse [queryrange] <FileName>
fsutil sparse [setflag] <FileName>
fsutil sparse [setrange] <FileName> <BeginningOffset> <Length>
```

## <a name="parameters"></a>Parameters

|     参数     |                                                    描述                                                    |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
|     queryflag     |                                                  查询稀疏。                                                  |
|    queryrange     |                        扫描文件并搜索可能包含非零数据的范围。                        |
|      setflag      |                                        将所指示的文件标记为稀疏。                                        |
|     setrange      |                                   使用零填充文件的指定范围。                                   |
|    <FileName>     | 指定文件的完整路径，包括文件名和扩展名，例如 C:\documents\filename.txt。 |
| <BeginningOffset> |                              指定文件中的偏移量，以将其标记为稀疏。                              |
|     <Length>      |                 指定文件中要标记为稀疏的区域的长度（以字节为单位）。                 |

## <a name="remarks"></a>备注

-   稀疏文件是一个文件，其中包含一个或多个未分配的数据区域。 程序会将这些未分配的区域视为包含值为零的字节，但不会使用磁盘空间来表示这些零。 所有有意义或非零的数据都将被分配，而不会分配所有 nonmeaningful 数据（由零组成的大型字符串）。 读取稀疏文件时，分配的数据将按存储的方式返回，且默认情况下，未分配的数据将根据 C2 安全要求规范返回零。 稀疏文件支持允许将数据从文件中的任何位置解除分配。

-   在稀疏文件中，大范围的零可能不需要磁盘分配。 在写入文件时，将根据需要分配非零数据的空间。

-   只有压缩文件或稀疏文件才能有操作系统已知的零范围。

-   如果文件是稀疏文件或压缩文件，NTFS 可能会释放文件中的磁盘空间。 这会将字节范围设置为零，而不会扩展文件大小。

## <a name="BKMK_examples"></a>示例
若要将名为 test.txt 的文件中的文件作为稀疏目录标记，请键入：

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


