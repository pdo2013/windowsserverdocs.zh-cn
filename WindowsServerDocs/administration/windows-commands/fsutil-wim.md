---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil wim
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c9186721ce4d3a549964e420cbc16d4893a1859d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826038"
---
# <a name="fsutil-wim"></a>Fsutil wim
>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

提供发现和管理 Windows 映像 WIM 备份文件的函数。

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
|enumfiles|枚举支持的 WIM 文件。|
|\<驱动器名称 >|指定的驱动器名称。|
|\<数据源 >|指定数据源。|
|enumwims|枚举支持 WIM 文件。|
|queryfile|如果该文件受 WIM，并且如果是这样，查询显示 WIM 文件的详细信息。|
|\<filename>|指定的文件名。|
|removewim|从备份文件中删除 WIM。|




### <a name="examples"></a>示例

若要枚举的文件的驱动器 c： 从数据源 0，请键入：

```
fsutil wim enumfiles C: 0
```

若要枚举为驱动器 c： 支持 WIM 文件，请键入：

```
fsutil wim enumwims C:
```

若要查看是否由 WIM 备份文件，请键入：

```
fsutil wim C:\Windows\Notepad.exe
```

若要从备份卷 c： 和数据源 2 文件中删除 WIM，请键入：

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)