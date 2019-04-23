---
title: cd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5f907b6162e6767820e23222e287b933397397d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861098"
---
# <a name="cd"></a>cd



显示的名称或更改当前目录。 如果使用带驱动器号 (例如， `cd C:`)， **cd**显示中指定的驱动器的当前目录名称。 如果使用不带参数， **cd**显示当前驱动器和目录。

> [!NOTE]
> 此命令等同于**chdir**命令。

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
|/d|更改当前驱动器，以及一个驱动器的当前目录。|
|\<驱动器 >:|指定要显示或更改 （如果不同于当前的驱动器） 的驱动器。|
|\<Path>|指定你想要显示或更改目录的路径。|
|[..]|指定你想要更改到父文件夹。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

如果启用了命令扩展，下列条件适用于**cd**命令：
-   当前目录字符串转换为使用该磁盘上的名称相同的大小写。 例如，`cd C:\TEMP`会将当前目录设置为 C:\Temp，如果出现这种情况在磁盘上的。
-   空格不被视为分隔符，因此*路径*可以包含空格，而无需封闭引号。 例如：  
    ```
    cd username\programs\start menu
    ```  
    是作为相同的：  
    ```
    cd "username\programs\start menu"
    ```  
    引号是必需的但是，如果已禁用扩展。

若要禁用命令扩展，请键入：
```
cmd /e:off
```

## <a name="BKMK_examples"></a>示例

根目录位于驱动器的目录层次结构的顶部。 若要返回到根目录，请键入：
```
cd\
```
若要更改所使用的一个不同的驱动器上的默认目录，请键入：
```
cd [<Drive>:\[<Directory>]]
```
若要验证对目录的更改，请键入：
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)