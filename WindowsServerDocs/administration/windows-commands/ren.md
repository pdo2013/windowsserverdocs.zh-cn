---
title: ren
description: 了解如何使用 ren 命令重命名文件或目录。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2ba3f6a13dc03c0b6a5561be9f0f692546a25149
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384576"
---
# <a name="ren"></a>ren

重命名文件或目录。 此命令与 "**重命名**" 命令相同。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[@no__t >：][@no__t >] \<FileName1 >|指定要重命名的文件或文件集的位置和名称。 *FileName1*可以包含通配符（ **&#42;** 和 **？** ）。|
|\<FileName2 >|指定文件的新名称。 您可以使用通配符来指定多个文件的新名称。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 在重命名文件时，不能指定新的驱动器或路径。
- 不能使用**ren**命令在驱动器之间重命名文件，或将文件移动到不同的目录。
- 可以在 FileName 参数中使用 **&#42;** 通配符（和 **？** ） 。 *FileName2*中由通配符表示的字符将与*FileName1*中的相应字符相同。
- *FileName2*必须是唯一的文件名。 如果*FileName2*与现有文件名相匹配，则**ren**会显示以下消息：  
  ```
  Duplicate file name or file not found
  ```

## <a name="BKMK_examples"></a>示例

若要将当前目录中的所有 .txt 文件扩展名更改为 .doc 扩展名，请键入：
```
ren *.txt *.doc 
```
若要将目录的名称从 Chap10 更改为 Part10，请键入：
```
ren chap10 part10 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)