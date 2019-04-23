---
title: ren
description: 了解如何重命名文件或使用 ren 命令的目录。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c239dd1f1f8d03d761e45505634da10f19ed08cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842018"
---
# <a name="ren"></a>ren

重命名文件或目录。 此命令等同于**重命名**命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName1>|指定的位置和文件的名称或一组你想要重命名的文件。 *FileName1*可以包含通配符 (**&#42;** 并 **？**)。|
|\<FileName2>|指定的文件的新名称。 可以使用通配符来指定多个文件的新名称。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   重命名文件时，不能指定新的驱动器或路径。
-   不能使用**ren**命令以在驱动器重命名文件或将文件移到不同的目录。
-   可以使用通配符 (**&#42;** 并 **？**) 中任意一种*FileName*参数。 表示通过中的通配符字符的字符*FileName2*中的相应字符相同*FileName1*。
-   *FileName2*必须是唯一的文件名。 如果*FileName2*匹配现有的文件名称， **ren**会显示以下消息：  
    ```
    Duplicate file name or file not found
    ```

## <a name="BKMK_examples"></a>示例

若要更改当前目录中的所有.txt 文件名称扩展为.doc 扩展，请键入：
```
ren *.txt *.doc 
```
若要更改从 Chap10 Part10 到目录的名称，请键入：
```
ren chap10 part10 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)