---
title: Md
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1396038410ecc5db5a124a1768038c4f8c8bea8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820838"
---
# <a name="md"></a>Md



创建一个目录或子目录。

> [!NOTE]
> 此命令等同于**mkdir**命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<驱动器 >:|指定你想要创建新目录的驱动器。|
|\<Path>|必需。 指定的名称和新的目录的位置。 任何单个路径的最大长度是由文件系统确定的。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

命令扩展，默认情况下启用，可以使用单个**md**命令以在指定路径中创建的中间目录。

## <a name="BKMK_examples"></a>示例

若要创建一个名 Directory1 为当前目录中，键入：
```
md Directory1
```
若要创建的根目录中的目录树 Taxes\Property\Current 具有启用了命令扩展，请键入：
```
md \Taxes\Property\Current
```
若要创建目录树 Taxes\Property\Current 如上一示例中，但禁用了命令扩展的根目录中，键入以下命令序列：
```
md \Taxes
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[Cmd](cmd.md)