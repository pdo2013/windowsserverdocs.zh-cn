---
title: attrib
description: Windows 命令主题**attrib** -显示、 设置或删除属性分配到文件或目录。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f640af2c7957e43dd3f31dfa732bfc887112651
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841088"
---
# <a name="attrib"></a>attrib



显示、 集或删除分配到文件或目录的属性。 如果使用不带参数， **attrib**显示当前目录中的所有文件的属性。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|{+\|-}r|集 (**+**) 或清除 (**-**) 只读的文件属性。|
|{+\|-}a|集 (**+**) 或清除 (**-**) 存档文件属性。|
|{+\|-}s|集 (**+**) 或清除 (**-**) 系统文件属性。|
|{+\|-}h|集 (**+**) 或清除 (**-**) 隐藏文件属性。|
|{+\|-}i|集 (**+**) 或清除 (**-**) 未内容索引文件属性。|
|[\<Drive>:][<Path>][<FileName>]|指定的位置和目录、 文件或你想要显示或更改属性的文件组的名称。 可以使用 **？** 并 **&#42;** 中的通配符*FileName*参数来显示或更改的文件组的属性。|
|/s|适用**attrib**和到当前目录中的匹配文件的任何命令行选项和及其所有子目录。|
|/d|适用**attrib**和目录的任何命令行选项。|
|/l|适用**attrib**和符号链接，而不是符号链接的目标到任何命令行选项。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   可以使用通配符 (**？** 并 **&#42;**) 与*FileName*参数来显示或更改的文件组的属性。
-   如果文件系统 (**s**) 或隐藏 (**h**) 属性集，您必须清除该属性才能更改该文件的任何其他属性。
-   存档属性 () 将标记备份它们的上次更改的文件。 请注意， **xcopy**命令使用存档属性。

## <a name="BKMK_examples"></a>示例

若要显示名为 News86 位于当前目录中的文件的属性，请键入：
```
attrib news86 
```
若要将只读属性分配到名为 Report.txt 文件，请键入：
```
attrib +r report.txt 
```
若要从驱动器 B 中的磁盘上的公共目录及其子目录中的文件删除只读属性，请键入：
```
attrib -r b:\public\*.* /s 
```
若要设置为驱动器 A 中上的所有文件存档属性并清除具有.bak 扩展名的文件的存档属性，然后，键入：
```
attrib +a a:*.* & attrib -a a:*.bak 
```
