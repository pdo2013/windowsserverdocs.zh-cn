---
title: tree
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de22de1c9d62ba79c1aa68248109cca88009703a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872628"
---
# <a name="tree"></a>tree



以图形方式显示在驱动器中的目录结构或磁盘的路径。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<驱动器 >:|指定包含你想要显示的目录结构的磁盘的驱动器。|
|\<Path>|指定你想要显示的目录结构的目录。|
|/f|显示每个目录中的文件的名称。|
|/a|指定的**树**是使用文本字符而非图形字符显示链接子目录的行。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

显示的结构**树**取决于在命令提示符下指定的参数。 如果未指定驱动器或路径，**树**显示当前驱动器的当前目录开头的树结构。

## <a name="BKMK_examples"></a>示例

若要显示当前驱动器中的磁盘上的所有子目录的名称，请键入：
```
tree \
```
若要显示一个屏幕时，所有目录中的文件在驱动器 C，键入：
```
tree c:\ /f | more 
```
若要在驱动器 C 上打印的所有目录的列表，请键入：
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)