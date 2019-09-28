---
title: cd
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed0942232eb205a8198d4b3d366ca9482af1f4b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379706"
---
# <a name="cd"></a>cd



显示或更改当前目录的名称。 如果仅用于驱动器号（例如 `cd C:`）， **cd**将显示指定驱动器中当前目录的名称。 如果在没有参数的情况下使用， **cd**将显示当前驱动器和目录。

> [!NOTE]
> 此命令与**chdir**命令相同。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/d|更改当前驱动器以及驱动器的当前目录。|
|\<Drive >：|指定要显示或更改的驱动器（如果不同于当前驱动器）。|
|\<Path >|指定要显示或更改的目录的路径。|
|[..]|指定要更改为父文件夹。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

如果启用了命令扩展，则以下条件适用于**cd**命令：
- 当前目录字符串被转换为使用与磁盘上的名称相同的大小写。 例如，如果磁盘上出现这种情况，`cd C:\TEMP` 会将当前目录设置为 C：\Temp。
- 空格不被视为分隔符，因此*路径*可以包含空格而无需用引号引起来。 例如：  
  ```
  cd username\programs\start menu
  ```  
  与相同：  
  ```
  cd "username\programs\start menu"
  ```  
  但如果禁用了扩展，则需要引号。

若要禁用命令扩展，请键入：
```
cmd /e:off
```

## <a name="BKMK_examples"></a>示例

根目录是驱动器的目录层次结构的顶部。 若要返回到根目录，请键入：
```
cd\
```
若要更改与你所在的驱动器不同的驱动器上的默认目录，请键入：
```
cd [<Drive>:\[<Directory>]]
```
若要验证对目录所做的更改，请键入：
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)