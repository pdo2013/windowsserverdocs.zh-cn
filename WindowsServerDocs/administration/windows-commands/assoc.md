---
title: assoc
description: 相关的 Windows 命令**主题-显示**或修改文件扩展名关联。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e4a6fd700cbe66897a24f01f66387e76e07b568b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382666"
---
# <a name="assoc"></a>assoc



显示或修改文件扩展名关联。 如果在没有参数的情况下使用， **assoc**将显示所有当前文件扩展名关联的列表。

> [!NOTE]
> 只有 CMD 中支持此命令。EXE 和无法从 PowerShell 中获取。
>

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|< ext >|指定文件扩展名。|
|\<FileType >|指定与指定的文件扩展名关联的文件类型。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要删除文件扩展名的文件类型关联，请按空格键，在等号后面添加一个空格。
-   若要查看已定义打开的命令字符串的当前文件类型，请使用**ftype**命令。
-   若要将**assoc**的输出重定向到文本文件，请使用 **@no__t**重定向运算符。

## <a name="BKMK_examples"></a>示例

若要查看文件扩展名为 .txt 的当前文件类型关联，请键入：
```
assoc .txt
```
若要删除文件扩展名 .bak 的文件类型关联，请键入：
```
assoc .bak= 
```

> [!NOTE]
> 请确保在等号后面添加一个空格。

若要查看每次**显示一个屏幕的输出**，请键入：
```
assoc | more
```
若要将**assoc**的输出发送到文件 assoc，请键入：
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
