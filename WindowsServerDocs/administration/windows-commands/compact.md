---
title: compact
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5e73369d69912437875a0151b1d9cfcfc85a30da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379176"
---
# <a name="compact"></a>compact



显示或更改 NTFS 分区上的文件或目录的压缩。 如果在没有参数的情况下使用， **compact**将显示当前目录及其包含的文件的压缩状态。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/c|压缩指定的目录或文件。|
|/u|Uncompresses 指定的目录或文件。|
|/s [： \<Dir >]|将**compact**命令应用于指定目录的所有子目录（如果指定了 "无"，则为当前目录）。|
|/a|显示隐藏文件或系统文件。|
|i|忽略错误。|
|/f|强制对指定目录或文件进行压缩或解压缩。 如果文件是在系统崩溃中断操作中断的情况下进行部分压缩的，则使用 **/f** 。 若要强制完全压缩文件，请使用 **/c**和 **/f**参数，并指定部分压缩的文件。|
|/q|仅报告最重要的信息。|
|\<文件名 >|指定文件或目录。 您可以使用多个文件名，并使用 **&#42;** 和 **？** 通配符。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Compact**命令是 NTFS 文件系统压缩功能的命令行版本。 目录的压缩状态指示将文件添加到目录时是否自动对文件进行压缩。 设置目录的压缩状态不一定会更改目录中已有文件的压缩状态。
-   不能使用**compact**来读取、写入或装入使用 "磁盘空间管理" 或 DoubleSpace 进行压缩的卷。
-   不能使用**compact**压缩文件分配表（FAT）或 FAT32 分区。

## <a name="BKMK_examples"></a>示例

若要设置当前目录、其子目录和现有文件的压缩状态，请键入：
```
compact /c /s 
```
若要设置当前目录中的文件和子目录的压缩状态，而不改变当前目录本身的压缩状态，请键入：
```
compact /c /s *.*
```
若要压缩卷，请在卷的根目录中键入：
```
compact /c /i /s:\
```

> [!NOTE]
> 此示例设置所有目录（包括卷上的根目录）的压缩状态，并压缩卷上的每个文件。 **/I**参数可防止错误消息中断压缩进程。

若要压缩 \Tmp 目录和 \Tmp 的所有子目录中扩展名为 .bmp 的所有文件，而不修改目录的压缩属性，请键入：
```
compact /c /s:\tmp *.bmp
```
若要强制对在系统崩溃期间部分压缩的文件斑马进行完全压缩，请键入：
```
compact /c /f zebra.bmp
```
若要从目录 C:\Tmp 中删除压缩属性，但不更改该目录中任何文件的压缩状态，请键入：
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)