---
title: compact
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 068111b293a3eb3987b14744a1bfcf2fde26bced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826028"
---
# <a name="compact"></a>compact



显示或更改文件或目录在 NTFS 分区的压缩。 如果使用不带参数， **compact**显示当前目录和其包含的文件的压缩状态。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/c|将指定的目录或文件的压缩。|
|/u|指定的目录或文件解压缩。|
|/s[:\<Dir>]|适用**compact**命令到指定的目录 （或如果未指定的当前目录） 的所有子目录。|
|/a|显示隐藏文件或系统文件。|
|i|将忽略错误。|
|/f|强制压缩或解压缩的指定的目录或文件。 **/f**在一定程度上已压缩操作已中断通过在系统发生崩溃时的文件的情况下使用。 若要强制执行要压缩其完整的文件，请使用 **/c**并 **/f**参数并指定部分压缩的文件。|
|/q|报告最重要的信息。|
|\<FileName>|指定的文件或目录。 可以使用多个文件名称，并 **&#42;** 并 **？** 通配符字符。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Compact**命令是 NTFS 文件系统压缩功能的命令行版本。 目录的压缩状态指示添加到目录后是否自动压缩文件。 设置目录的压缩状态不一定是更改已经在目录中的文件的压缩状态。
-   不能使用**compact**到读取、 写入或使用磁盘空间管理或 DoubleSpace 已压缩的装载卷。
-   不能使用**compact**压缩文件分配表 (FAT) 或 FAT32 分区。

## <a name="BKMK_examples"></a>示例

若要设置的当前目录、 其子目录和现有文件的压缩状态，请键入：
```
compact /c /s 
```
若要设置的文件和子目录在当前目录中的压缩状态而无需更改当前目录本身的压缩状态，请键入：
```
compact /c /s *.*
```
若要压缩卷，请从该卷的根目录下键入：
```
compact /c /i /s:\
```

> [!NOTE]
> 此示例设置 （包括卷上的根目录） 的所有目录的压缩状态，并将压缩卷上的每个文件。 **/I**参数可防止错误消息会中断压缩进程。

若要压缩.bmp 文件扩展名 \Tmp 目录中的所有文件和所有子目录的 \Tmp，而无需修改目录的压缩的属性，请键入：
```
compact /c /s:\tmp *.bmp
```
若要强制完成压缩的文件 Zebra.bmp，部分系统崩溃期间压缩，请键入：
```
compact /c /f zebra.bmp
```
若要删除压缩的属性从目录 C:\Tmp，而无需更改该目录中的任何文件的压缩状态，请键入：
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)