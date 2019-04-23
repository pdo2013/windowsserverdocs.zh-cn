---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: fsutil reparsepoint
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 940131a02a5cd3a6122022cf9b0dff3281d1dabf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847938"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7，Windows 2008 中，Windows Vista

查询或删除重分析点。  **Fsutil reparsepoint**命令通常由支持专业人员。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|查询|检索与指定句柄标识目录中的文件关联的重分析点数据。|
|“删除”|从文件或目录由指定句柄，但不会删除文件或目录中删除重分析点。|
|<FileName>|指定的文件包括文件名和扩展名，例如 C:\documents\filename.txt 的完整路径。|

## <a name="remarks"></a>备注

-   重新分析点 NTFS 文件系统对象，具有可定义属性包含用户定义的数据，以及使用它们来扩展输入/输出 (I/O) 子系统中的功能。

-   重新分析点用于目录交接点和卷装入点。 它们还用于通过文件系统筛选器驱动程序标记为特定于该驱动程序的某些文件。

-   程序集时的重分析点，它存储此数据，以及重分析标记，它可以唯一标识它存储的数据。 当文件系统打开文件时使用重分析点时，它会尝试查找关联的文件系统筛选器。 如果找到文件系统筛选器，则筛选器处理文件的重分析数据的指导。 如果不找到任何文件系统筛选器，则文件打开操作将失败。

## <a name="BKMK_examples"></a>示例
若要检索与 C:\Server 关联的重新分析点数据，请键入：

```
fsutil reparsepoint query c:\server
```

若要从指定的文件或目录中删除重分析点，请使用以下格式：

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


