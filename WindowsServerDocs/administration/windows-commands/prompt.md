---
title: prompt
description: 了解如何自定义命令提示符。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 320e12fd30deda30ccc0da1ad6e5bea6f9a19d8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818438"
---
# <a name="prompt"></a>prompt



更改 Cmd.exe 命令提示符。 如果使用不带参数，**提示符**将命令提示符重置为默认设置，这是当前的驱动器号和大于号后跟的目录 (**>**)。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
prompt [<Text>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Text>|指定的文本和想要包括在命令提示符中的信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

您可以自定义命令提示符下，若要显示任何所需的文本，包括当前目录、 时间和日期以及 Microsoft Windows 版本号等的信息。

下表列出了可以包含而非或除了中的一个或多个字符字符串的字符组合*文本*参数。 该列表包含的文本或每个字符组合将添加到命令提示符下的信息的简要说明。  
|字符|描述|
|---------|-----------|
|$q|= （等号）|
|$$|$ （美元符号）|
|$t|当前时间|
|$d|当前日期|
|$p|当前的驱动器和路径|
|$v|Windows 版本号|
|$n|当前驱动器|
|$g|> （大于号）|
|$l|< （小于登录）|
|$b|| （管道）|
|$_|回车符-换行符|
|$e|ANSI 转义代码 （代码 27）|
|$h|退格符 （若要删除已写入到命令行的字符）|
|$|& （与号）|
|$c|（（左的括号）|
|$f|) （右括号）|
|$s|空间|

启用了命令扩展 （默认值）**提示符**命令支持以下格式设置字符：  

|字符|描述|
|---------|-----------|
|$+|零个或多个加号 (**+**) 个字符，具体取决于的深度**pushd**目录堆栈 （一个字符为每个级别推送）。|
|$m|如果当前驱动器不是网络驱动器与当前的驱动器号或空字符串关联的远程名称。|

如果包括 **$p**字符文本参数中输入每个命令 （若要确定当前驱动器和路径） 后读取您的磁盘。 这可能需要额外的时间，尤其是对于软盘驱动器。

## <a name="BKMK_examples"></a>示例

若要设置的第一行和大于号上的下一步的行具有的当前时间和日期的两行代码命令提示符，键入：
```
prompt $d$s$s$t$_$g 
```
在提示符下，如下所示，其中的日期和时间是最新更改：
```
Fri 06/01/2007  13:53:28.91
>
```
若要设置要显示为一个箭头的命令提示符 (`-->`)，类型：
```
prompt --$g
```
若要手动更改为默认设置 （当前驱动器和通过使用大于号应遵循的路径） 的命令提示符处，键入：
```
prompt $p$g
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)