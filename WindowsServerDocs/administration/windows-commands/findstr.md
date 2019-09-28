---
title: findstr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 547a0abf658ef826cca8c87d451144181f8dac7d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377188"
---
# <a name="findstr"></a>findstr

搜索文件中的文本模式。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/b|如果文本模式位于行的开头，则匹配它。|
|/e|如果文本模式位于行尾，则匹配它。|
|/l|按字面处理搜索字符串。|
|/r|将搜索字符串处理为正则表达式。 此为默认设置。|
|/s|搜索当前目录和所有子目录。|
|i|搜索字符串时忽略字符的大小写。|
|/x|打印完全匹配的行。|
|/v|仅打印不包含匹配项的行。|
|/n|打印每个匹配行的行号。|
|/m|如果文件包含匹配项，则仅打印文件名。|
|/o|打印每个匹配行前的字符偏移量。|
|/p|跳过包含不可打印字符的文件。|
|/off [line]|不会跳过设置了脱机属性的文件。|
|/f： \<File >|从指定的文件中获取文件列表。|
|/c： \<String >|使用指定的文本作为文本搜索字符串。|
|/g： \<File >|从指定的文件中获取搜索字符串。|
|/d： \<DirList >|搜索指定的目录列表。 必须用分号（;) 分隔每个目录，例如 `dir1;dir2;dir3`。|
|/a： \<ColorAttribute >|指定带有两个十六进制数字的颜色属性。 键入 `color /?` 获取其他信息。|
|\<Strings >|指定要在*FileName*中搜索的文本。 必需。|
|[@no__t >：][<Path>] <FileName> [...]|指定要搜索的位置和文件。 至少需要一个文件名。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 所有**findstr**命令行选项都必须在命令字符串中的*字符串*和*文件名*之前。
- 正则表达式使用原义字符和元字符查找文本模式，而不是精确的字符串。 文本字符是在正则表达式语法中不具有特殊含义的字符，它匹配该字符的匹配项。 例如，字母和数字是原义字符。 元字符是在正则表达式语法中具有特殊含义（运算符或分隔符）的符号。

  下表列出了**findstr**接受的元字符。  

  |元字符|ReplTest1|
  |-------------|-----|
  |.|通配符：任意字符|
  |*|重复：前一个字符或类的零个或多个匹配项|
  |^|行位置：行的开头|
  |$|行位置：行的末尾|
  |班级|字符类：集合中的任何一个字符|
  |[^ class]|反向类：不在集中的任何一个字符|
  |[x-y]|范围：指定范围内的任何字符|
  |\x|Escape：元字符 x 的文字用法|
  |\\ < 字符串|字位置：单词的开头|
  |string @ no__t-0|字位置：单词的结尾|

  正则表达式语法中的特殊字符在一起使用时，其功能最高。 例如，使用下面的通配符（.）和重复（*）字符与任何字符串匹配的组合：

  ```
  .*
  ``` 

  使用以下表达式作为更大的表达式的一部分，以匹配以 "b" 开头并以 "ing" 结尾的任何字符串： 

  ```
  b.*ing
  ```

## <a name="examples"></a>示例

使用空格分隔多个搜索字符串，除非参数使用 **/c**作为前缀。

若要在文件 a.x 中搜索 "hello" 或 "file"，请键入：

```
findstr "hello there" x.y 
```

若要在文件 a.x 中搜索 "hello"，请键入：

```
findstr /c:"hello there" x.y 
```

若要在文件 "建议 .txt" 中查找 "Windows" （带有初始大写字母 W）的所有匹配项，请键入：

```
findstr Windows proposal.txt 
```

若要搜索当前目录中的每个文件以及包含 word Windows 的所有子目录，无论字母大小写如何，请键入：

```
findstr /s /i Windows *.* 
```

若要查找所有以 "FOR" 开头的行，并以零个或多个空格开头（如在计算机程序循环中），并显示找到每个匹配项的行号，请键入：

```
findstr /b /n /r /c:"^ *FOR" *.bas 
```

若要在一组文件中搜索多个字符串，请在单独的行上创建一个包含每个搜索条件的文本文件。 还可以列出要在文本文件中搜索的具体文件。 例如，若要使用文件 Stringlist 中的搜索条件，请搜索 Filelist 中列出的文件，然后将结果存储在文件结果中，键入：

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

若要列出当前目录和所有子目录中包含 "computer" 一词的每个文件，请键入：

```
findstr /s /i /m "\<computer\>" *.*
```

若要列出每个包含单词 "computer" 的文件以及以 "comp" 开头的任何其他单词（如 "补充" 和 "竞争"），请键入：

```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)