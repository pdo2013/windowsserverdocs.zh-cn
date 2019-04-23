---
title: forfiles
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 127f715620321354792d46f024ee12a06925d866
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881308"
---
# <a name="forfiles"></a>forfiles



选择并文件或文件组上执行的命令。 此命令可用于批处理的处理。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/p \<Path>|指定从其开始搜索的路径。 默认情况下，搜索当前工作目录中启动。|
|/m \<SearchMask>|根据指定的搜索掩码搜索文件。 默认搜索掩码 **\*。\***.|
|/s|指示**forfiles**命令到子目录以递归方式搜索。|
|/c "\<Command>"|在每个文件上运行指定的命令。 命令字符串应括在引号中。 默认命令是 **"cmd /c echo @file"**。|
|/d&nbsp;[{+\|-}]&#8288;[{\<Date>\|&#8288;\<Days>}]|选择具有指定的时间范围内的上次修改日期的文件。</br>-选择文件的上次修改日期晚于或等于 (**+**) 或早于或等于 (**-**) 指定的日期，其中*日期*采用格式 MM/DD/YYYY。</br>-选择文件的上次修改日期晚于或等于 (**+**) 的当前日期加上指定，天内或早于或等于 (**-**) 的当前日期减去天数指定。</br>有效值*天*范围 0-32，768 中包含任意数量。 如果指定没有登录，则**+** 默认情况下使用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Forfiles**最常用于在批处理文件中。
-   **Forfiles /s**类似于**dir /s。**
-   可以在由指定的命令字符串中使用以下变量 **/c**命令行选项。  

|变量|描述|
|--------|-----------|
|@FILE|文件的名称。|
|@FNAME|不带扩展名的文件名。|
|@EXT|文件扩展名。|
|@PATH|文件的完整路径。|
|@RELPATH|文件的相对路径。|
|@ISDIR|如果文件类型是一个目录，计算结果为 TRUE。 否则，此变量的计算结果为 FALSE。|
|@FSIZE|文件大小 （字节）。|
|@FDATE|对该文件的上次修改的日期戳。|
|@FTIME|对该文件的上次修改的时间戳。|

-   与**forfiles**，可以运行命令，也可以将参数传递到多个文件。 例如，可以运行**类型**命令上具有文件扩展名为.txt 的树中的所有文件。 也可以执行的每个批处理文件 (*.bat) 在驱动器 C 上，使用该文件命名为"Myinput.txt"作为第一个参数。
-   与**forfiles**，可以执行下列任一操作：  
    -   使用的绝对日期或相对日期选择文件 **/d**参数。
    -   通过使用变量，如生成文件的存档树@FSIZE和@FDATE。
    -   将文件与目录区分开来使用@ISDIR变量。
    -   通过使用十六进制代码 0 x 中的字符，在命令行中包含特殊字符*HH*格式 （例如，一个选项卡的 0x09）。
-   **Forfiles**的工作原理是实现**对子目录执行递归**标志来处理单个文件的工具。

## <a name="BKMK_examples"></a>示例

若要列出所有驱动器 C 上的批处理文件，请键入：
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
若要列出所有目录位于驱动器 C 上，请键入：
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
若要列出所有当前目录中至少一个年前的旧的文件，请键入：
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
若要显示文本"*文件*已过时"早于 2007 年 1 月 1 日，当前目录中的文件的每个类型：
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
若要列出的列格式中的当前目录中的所有文件的文件扩展名，添加在扩展名之前的选项卡，键入：
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
