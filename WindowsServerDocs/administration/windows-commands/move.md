---
title: move
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d651e586c31ff64664079bdd10ffde3701ec317d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824988"
---
# <a name="move"></a>move



将一个或多个文件从一个目录移动到另一个目录。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/y|禁止提示确认你想要覆盖现有目标文件。|
|/-y|提示确认要覆盖现有目标文件。|
|\<源 >|指定的路径和要移动的文件的文件的名称。 如果你想要移动或重命名目录，*源*应为当前目录路径和名称。|
|\<Target>|指定的路径和名称，若要将文件移到。 如果你想要移动或重命名目录，*目标*应该是所需的目录路径和名称。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **/Y**命令行选项可能预设 COPYCMD 环境变量中。 可以重写与此 **/y**命令行上。 默认值是在覆盖文件，除非之前进行提示**复制**批处理脚本中运行命令。
-   将加密的文件移动到的卷，不支持加密文件系统 (EFS) 的结果中出现错误。 首先解密文件或将文件移到支持 EFS 的卷。

## <a name="BKMK_examples"></a>示例

若要将所有文件扩展名为.xls 从 \Data 目录都移到 \Second_Q\Reports 目录，键入：
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)