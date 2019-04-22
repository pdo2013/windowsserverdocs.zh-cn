---
title: assoc
description: Windows 命令主题**assoc** -显示或修改文件扩展名关联。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d893167081b66c81366b59613c52182a4ddba370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816638"
---
# <a name="assoc"></a>assoc



显示或修改文件扩展名关联。 如果使用不带参数， **assoc**显示所有当前文件扩展名关联的列表。

> [!NOTE]
> 此命令仅支持在 cmd。EXE 和 PowerShell 中不可用。
>

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|<.ext>|指定的文件扩展名。|
|\<FileType>|指定要与指定的文件扩展名相关联的文件类型。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要删除的文件扩展名的文件类型关联，请按空格键在等号后添加空格。
-   若要查看当前已定义的打开命令字符串的文件类型，请使用**ftype**命令。
-   若要将输出重定向**assoc**到文本文件，请使用**>** 重定向运算符。

## <a name="BKMK_examples"></a>示例

若要查看文件扩展名为.txt 的当前文件类型关联，请键入：
```
assoc .txt
```
若要删除文件扩展名为.bak 文件类型关联，请键入：
```
assoc .bak= 
```

> [!NOTE]
> 请务必在等号后添加一个空格。

若要查看的输出**assoc**一个屏幕时，类型：
```
assoc | more
```
若要发送的输出**assoc**文件 assoc.txt 中，键入：
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
