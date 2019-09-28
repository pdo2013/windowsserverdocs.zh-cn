---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil wim
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fc79b70e8dedb9ecad5e8c6e89f51ece3279faa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376656"
---
# <a name="fsutil-wim"></a>Fsutil wim
>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10

提供用于发现和管理支持 Windows 映像（WIM）的文件的函数。

## <a name="syntax"></a>语法

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|enumfiles|枚举支持 WIM 的文件。|
|\<drive name >|指定驱动器名称。|
|@no__t 源 >|指定数据源。|
|enumwims|枚举后备 WIM 文件。|
|queryfile|查询文件是否由 WIM 支持，如果是，则显示有关 WIM 文件的详细信息。|
|\<filename >|指定文件名。|
|removewim|从备份文件中删除 WIM。|




### <a name="examples"></a>示例

要从数据源0枚举驱动器 C：的文件，请键入：

```
fsutil wim enumfiles C: 0
```

若要枚举驱动器 C：的后备 WIM 文件，请键入：

```
fsutil wim enumwims C:
```

若要查看某个文件是否由 WIM 支持，请键入：

```
fsutil wim C:\Windows\Notepad.exe
```

若要从卷 C：和数据源2的备份文件中删除 WIM，请键入：

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)