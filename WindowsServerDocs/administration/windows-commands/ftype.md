---
title: ftype
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c29ca0aa027d11fa8f981134e5367021227d3096
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881678"
---
# <a name="ftype"></a>ftype



显示或修改文件扩展名关联中使用的文件类型。 如果使用而没有赋值运算符 (**=**)， **ftype**显示指定的文件类型的当前打开的命令字符串。 如果使用不带参数， **ftype**显示已定义的打开命令字符串的文件类型。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<FileType>|指定要显示或更改的文件类型。|
|\<OpenCommandString>|指定打开指定的文件类型的文件时要使用的打开命令字符串。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

下表描述了如何**ftype**将替换为打开命令字符串内的变量：

|变量|替换值|
|--------|-----------------|
|%0或 %1|获取替换启动通过关联的文件名称。|
|%*|获取所有参数。|
|%2, %3, ...|获取第一个参数 (%2)、 第二个参数 (%3) 等。|
|%~\<N>|获取所有其余参数开头*N*个参数，其中*N*可以是从 2 到 9 的任意数字。|

## <a name="BKMK_examples"></a>示例

若要显示打开命令已定义了字符串的当前文件类型，请键入：
```
ftype
```
若要显示的当前打开的命令字符串*txtfile*文件类型，类型：
```
ftype txtfile
```
此命令将生成类似于以下输出：
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
若要删除一个名为的文件类型的打开命令字符串*示例*，类型：
```
ftype example=
```
若要将.pl 文件扩展名与 PerlScript 文件类型相关联，并启用要运行 PERL 的 PerlScript 文件类型。EXE，键入以下命令：
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
若要无需键入.pl 文件扩展名调用 Perl 脚本时，键入：
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)