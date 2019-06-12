---
title: findstr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8d080420d250deee9bef701272e936d33733a9d6
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811196"
---
# <a name="findstr"></a>findstr

搜索模式的文件中的文本。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/b|如果它位于行首，则匹配的文本模式。|
|/e|如果它位于行尾，匹配的文本模式。|
|/l|进程按原义搜索字符串。|
|/r|进程作为正则表达式搜索字符串。 这是默认设置。|
|/s|搜索当前目录及所有子目录。|
|i|搜索字符串时，将忽略字符大小写。|
|/x|打印完全匹配的行数。|
|/v|打印不包含匹配的行。|
|/n|打印每个匹配的行的行号。|
|/m|如果文件包含匹配项，将输出的文件名。|
|/o|每个匹配行之前打印字符偏移量。|
|/p|将跳过非打印字符的文件。|
|/off[line]|不跳过已设置了脱机属性的文件。|
|/f:\<文件 >|从指定的文件中获取文件列表。|
|无\<字符串 >|使用指定的文本作为文本搜索字符串。|
|/g:\<文件 >|获取搜索从指定的文件的字符串。|
|/d:\<DirList>|搜索指定的目录列表。 每个目录必须以用分号 （;），例如`dir1;dir2;dir3`。|
|/a:\<ColorAttribute >|指定具有两个十六进制数字的颜色属性。 类型`color /?`有关其他信息。|
|\<Strings>|指定要在中搜索的文本*文件名*。 必需。|
|[\<Drive>:][<Path>]<FileName>[ ...]|指定的位置和文件或要搜索的文件。 至少一个文件的名称是必需的。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 所有**findstr**命令行选项必须位于之前*字符串*并*文件名*命令字符串中。
- 正则表达式使用原义字符和元字符查找模式的文本，而不是精确的字符串。 原义字符是正则表达式语法中没有特殊含义的字符 — 它匹配该字符一次。 例如，字母和数字是原义字符。 元字符是在正则表达式语法中具有特殊含义 （操作员或分隔符） 的符号。

  下表列出了元字符， **findstr**接受。  

  |元字符|ReplTest1|
  |-------------|-----|
  |.|任何字符通配符：|
  |*|重复： 零个或多个匹配项的前一个字符或类|
  |^|行位置： 行首|
  |$|行位置： 行尾|
  |[class]|字符类： 一组中的任何一个字符|
  |[^class]|反向类： 一组不在任何一个字符|
  |[x-y]|范围： 指定范围内任何字符|
  |\x|元字符 x 文字用法转义：|
  |\\<string|字位置： 的单词的开头|
  |string\>|字位置： 的单词结尾|

  正则表达式语法中的特殊字符一起使用时具有最高的能力。 例如，使用以下组合的通配符字符 （.） 和重复 （*） 字符匹配任何字符的字符串：

  ```
  .*
  ``` 

  使用下面的表达式作为较大表达式的一部分来匹配任何字符串开头为"b"和"ing"结尾： 

  ```
  b.*ing
  ```

## <a name="examples"></a>示例

使用空格来分隔多个搜索字符串，除非参数以前缀 **/c**。

若要搜索的"你好"或"there"在文件 x.y 键入：

```
findstr "hello there" x.y 
```

若要搜索"你好那里"在文件 x.y，请键入：

```
findstr /c:"hello there" x.y 
```

若要在文件 Proposal.txt 中查找所有出现的单词"Windows"（与初始大写字母 W），键入：

```
findstr Windows proposal.txt 
```

若要搜索的每个文件在当前目录和所有子目录中包含单词 Windows，而不考虑字母大小写，键入：

```
findstr /s /i Windows *.* 
```

若要查找所有出现的行的开头"FOR"后跟零个或多个空格 （如计算机程序中的循环），并显示每个匹配项所在的行号，请键入：

```
findstr /b /n /r /c:"^ *FOR" *.bas 
```

若要搜索的一组文件中的多个字符串，请创建包含单独的行上每个搜索条件的文本文件。 此外可以列出你想要在文本文件中搜索的确切文件。 例如，若要使用的搜索条件文件 Stringlist.txt 中，搜索 Filelist.txt 中, 列出的文件，然后将结果存储在文件 Results.out，类型：

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

若要列出每个文件包含单词"计算机"中的当前目录及所有子目录，而不考虑的情况下，键入：

```
findstr /s /i /m "\<computer\>" *.*
```

若要列出包含单词"computer"和任何其他单词开头 （例如"补充"和"相互谦让"），"完成"类型的每个文件：

```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)