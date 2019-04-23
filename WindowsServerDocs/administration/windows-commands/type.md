---
title: type
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 4ceb7365d34a2aeca21d1a699730a589f98fd549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887398"
---
# <a name="type"></a>type


在 Windows 命令 shell 中，**类型**是内置命令用于显示文本的文件的内容。 使用**类型**命令，查看文本文件而无需修改它。


在 PowerShell 中，**类型**是对的内置别名**[Get-content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** 还会显示内容的文件，但具有不同的语法的 cmdlet。


有关如何使用此命令在 Windows 命令 shell (Cmd.exe) 中的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|指定的位置和你想要查看的文件的文件的名称。 用空格分隔多个文件的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如果*文件名*包含空格，将其括在引号 (例如，"文件名称包含 Spaces.txt")。
-   如果显示二进制文件或程序创建的文件，您可能会在屏幕上，包括换页符字符和转义序列符号出现奇怪的字符。 这些字符表示二进制文件中使用的控制代码。 一般情况下，应避免使用**类型**命令来显示二进制文件。

## <a name="BKMK_examples"></a>示例

若要显示一个名为 Holiday.mar 文件内容，请键入：
```
type holiday.mar 
```
若要显示名 Holiday.mar 一个屏幕为一次的耗时较长文件内容，请键入：
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
