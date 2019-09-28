---
title: tree
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 22875e63526dc3465021c9aa990f6cea388b81e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385616"
---
# <a name="tree"></a>tree



以图形方式显示驱动器路径或磁盘的目录结构。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive >：|指定包含要显示其目录结构的磁盘的驱动器。|
|\<Path >|指定要显示其目录结构的目录。|
|/f|显示每个目录中的文件的名称。|
|/a|指定**树**将使用文本字符而不是图形字符来显示链接子目录的行。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

**树**显示的结构取决于你在命令提示符处指定的参数。 如果未指定驱动器或路径，**树**将显示从当前驱动器的当前目录开始的树状结构。

## <a name="BKMK_examples"></a>示例

若要显示当前驱动器中磁盘上所有子目录的名称，请键入：
```
tree \
```
若要每次显示一个屏幕，驱动器 C 上的所有目录中的文件，请键入：
```
tree c:\ /f | more 
```
若要在驱动器 C 上打印所有目录的列表，请键入：
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)