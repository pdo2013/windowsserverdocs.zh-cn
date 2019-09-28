---
title: forfiles
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fa2eb5c96dfbf3870705af41a0f27991084f816d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377075"
---
# <a name="forfiles"></a>forfiles



选择和执行一个文件或一组文件的命令。 此命令对于批处理很有用。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>Parameters

|                     参数                      |                                                                                                                                                                                                                                                                                                    描述                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<Path >                     |                                                                                                                                                                                                                                                 指定从其开始搜索的路径。 默认情况下，搜索从当前工作目录开始。                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask >                  |                                                                                                                                                                                                                                                           根据指定的搜索掩码搜索文件。 默认搜索掩码为 **\*. \\** \*。                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   指示**forfiles**命令以递归方式搜索子目录。                                                                                                                                                                                                                                                                    |
|                  /c "\<Command >"                   |                                                                                                                                                                                                                                  对每个文件运行指定的命令。 命令字符串应用引号引起来。 默认命令为 **"cmd/c echo @file"** 。                                                                                                                                                                                                                                   |
| /d @ no__t-0 [{+ \|-}]&#8288;[{\<Date > \|&#8288;@no__t | 选择在指定时间范围内具有上次修改日期的文件。</br>-选择上次修改日期晚于或等于（ **+** ）或早于或等于（ **-** ）指定日期的文件，其中*date*的格式为 MM/DD/YYYY。</br>-选择上次修改日期晚于或等于（ **+** ）的文件当前日期加上指定的天数，或早于或等于（ **-** ）当前日期减去指定的天数。</br>-*天数*的有效值包括0–32768范围内的任何数字。 如果未指定任何符号，则默认情况下使用 **+** 。 |
|                         /?                         |                                                                                                                                                                                                                                                                                        在命令提示符下显示帮助。                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>备注

-   **Forfiles**最常在批处理文件中使用。
-   **Forfiles/s**类似于**dir/s**
-   可以在命令字符串中使用由 **/c**命令行选项指定的以下变量。  

|变量|描述|
|--------|-----------|
|@FILE|文件名。|
|@FNAME|不带扩展名的文件名。|
|@EXT|文件扩展名。|
|@PATH|文件的完整路径。|
|@RELPATH|文件的相对路径。|
|@ISDIR|如果文件类型为目录，则计算结果为 TRUE。 否则，此变量的计算结果为 FALSE。|
|@FSIZE|文件大小（以字节为单位）。|
|@FDATE|文件中上次修改的日期戳。|
|@FTIME|文件中上次修改的时间戳。|

-   使用**forfiles**，可以运行命令，或将参数传递给多个文件。 例如，你可以对具有 .txt 文件扩展名的树中的所有文件运行**类型**命令。 或者，你可以在驱动器 C 上执行每个批处理文件（* .bat），文件名为 "Myinput"，作为第一个参数。
-   通过**forfiles**，你可以执行以下任一操作：  
    -   使用 **/d**参数按绝对日期或相对日期选择文件。
    -   使用 @FSIZE 和 @FDATE 等变量生成文件的存档树。
    -   使用 @ISDIR 变量来区分目录中的文件。
    -   在命令行中包含特殊字符（采用 0x*HH*格式（例如，选项卡的0x09）。
-   **Forfiles**的工作原理是在设计为仅处理一个文件的工具上实现 "**递归子目录**" 标志。

## <a name="BKMK_examples"></a>示例

若要列出驱动器 C 上的所有批处理文件，请键入：
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
若要列出驱动器 C 上的所有目录，请键入：
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
若要列出当前目录中至少一年的所有文件，请键入：
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
若要为当前目录中早于2007年1月1日的每个文件显示文本 "*文件*已过期"，请键入：
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
若要以列格式列出当前目录中所有文件的文件扩展名，并在扩展之前添加选项卡，请键入：
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
