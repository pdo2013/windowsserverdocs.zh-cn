---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: Fsutil reparsepoint
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fe274ad9a6dffc72607102d3430ba7527d3cc558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376851"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10，Windows Server 2012 R2，Windows 8.1，Windows Server 2012，Windows 8，Windows Server 2008 R2，Windows 7，Windows 2008，Windows Vista

查询或删除重新分析点。  **Fsutil reparsepoint**命令通常由支持专业人员使用。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Parameters

| 参数  |                                                                描述                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   query    |            检索与指定句柄标识的文件或目录关联的重新分析点数据。             |
|   delete   | 从文件或目录中删除由指定句柄标识的重新分析点，但不删除文件或目录。 |
| <FileName> |             指定文件的完整路径，包括文件名和扩展名，例如 C:\documents\filename.txt。             |

## <a name="remarks"></a>备注

-   重新分析点是 NTFS 文件系统对象，这些对象具有可定义的属性，其中包含用户定义的数据，并用于扩展输入/输出（i/o）子系统中的功能。

-   重新分析点用于目录交接点和卷装入点。 文件系统筛选器驱动程序也使用它们将特定文件标记为特定于该驱动程序。

-   当程序设置重新分析点时，它将存储此数据以及一个重新分析标记，该标记可唯一地标识其存储的数据。 当文件系统使用重新分析点打开文件时，它会尝试查找关联的文件系统筛选器。 如果找到文件系统筛选器，筛选器将按重新分析数据的指示处理该文件。 如果找不到文件系统筛选器，则文件打开操作将失败。

## <a name="BKMK_examples"></a>示例
若要检索与 C:\Server 关联的重新分析点数据，请键入：

```
fsutil reparsepoint query c:\server
```

若要从指定的文件或目录中删除重新分析点，请使用以下格式：

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


