---
title: ftype
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ce3f4c360269eb9cabd2cbef8abb89935923a595
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375825"
---
# <a name="ftype"></a>ftype



显示或修改在文件扩展名关联中使用的文件类型。 如果在没有赋值运算符（ **=** ）的情况下使用，则**ftype**将显示指定文件类型的当前打开命令字符串。 如果在没有参数的情况下使用，则**ftype**将显示已定义打开命令字符串的文件类型。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<FileType >|指定要显示或更改的文件类型。|
|\<OpenCommandString >|指定打开指定文件类型的文件时要使用的 open 命令字符串。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

下表说明了**ftype**如何在打开的命令字符串内替换变量：

|变量|替换值|
|--------|-----------------|
|% 0或% 1|替换为通过关联启动的文件名。|
|%*|获取所有参数。|
|% 2，% 3，。|获取第一个参数（% 2）、第二个参数（% 3）等。|
|%~ @ NO__T-1N >|获取以第*n*个参数开头的所有剩余参数，其中*N*可以是从2到9的任意数字。|

## <a name="BKMK_examples"></a>示例

若要显示已定义打开命令字符串的当前文件类型，请键入：
```
ftype
```
若要显示*txtfile*文件类型的当前打开的命令字符串，请键入：
```
ftype txtfile
```
此命令生成类似于以下内容的输出：
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
若要删除名为 " *Example*" 的文件类型的 "打开命令字符串"，请键入：
```
ftype example=
```
若要将 pl 文件扩展名与 PerlScript 文件类型相关联，并启用 PerlScript 文件类型以运行 PERL。EXE，键入以下命令：
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
若要在调用 Perl 脚本时不再需要键入 pl 文件扩展名，请键入：
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)