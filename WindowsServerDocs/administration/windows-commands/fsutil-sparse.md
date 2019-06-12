---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: fsutil 稀疏
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b1bc4e45ed2a2b06c72318e0999988ed8f016c40
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438977"
---
# <a name="fsutil-sparse"></a>fsutil 稀疏
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

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
|     queryflag     |                                                  稀疏的查询。                                                  |
|    queryrange     |                        扫描文件，并可能包含非零值的数据的范围中搜索。                        |
|      setflag      |                                        将标记为稀疏所指示的文件。                                        |
|     setrange      |                                   用零填充的文件的指定的范围。                                   |
|    <FileName>     | 指定的文件包括文件名和扩展名，例如 C:\documents\filename.txt 的完整路径。 |
| <BeginningOffset> |                              指定要将标记为稀疏文件中的偏移量。                              |
|     <Length>      |                 在要将标记为稀疏 （以字节为单位） 的文件中指定的区域的长度。                 |

## <a name="remarks"></a>备注

-   稀疏文件是具有一个或多个区域的未分配数据的文件。 程序会看到这些未分配的区域，包含字节的值为零，但没有磁盘空间用来代表这些零。 分配有意义或非零值的所有数据，而不都分配 nonmeaningful 的所有数据 （由组成的零的数据的大型字符串）。 稀疏文件中读取时，会返回已分配的数据，因为存储，并未分配的数据，默认情况下，作为返回零，根据 c2 安全需求。 稀疏文件支持允许为已取消分配从任意位置在文件中的数据。

-   在稀疏文件中，大范围的零可能不需要磁盘分配。 根据需要写入文件时，将分配为非零数据的空间。

-   仅压缩或稀疏文件可以具有零数据已知的操作系统的范围。

-   如果该文件是稀疏的还是压缩，NTFS 可能会释放文件中的磁盘空间。 这设置为零，而不必将扩展的文件大小的字节范围。

## <a name="BKMK_examples"></a>示例
若要将标记为稀疏 C:\Temp 目录中名为 Sample.txt 的文件，请键入：

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


