---
title: xcopy
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: 6448c4c5940d286931f6d64ad51970bf577a28fd
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811023"
---
# <a name="xcopy"></a>xcopy

复制文件和目录，包括子目录。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<源 >|必需。 指定的位置和要复制的文件的名称。 此参数必须包括一个驱动器或路径。|
|[\<目标 >]|指定想要复制的文件的目标。 该参数可以包含驱动器号和冒号、 目录名称、 文件名或这些项的组合。|
|/w|显示以下消息并开始复制文件之前等待您的响应：</br>**按任意键以开始复制文件**|
|/p|提示您确认是否想要创建的每个目标文件。|
|/c|将忽略错误。|
|/v|在写入目标文件，以确保目标文件相同的源文件，请验证每个文件。|
|/q|禁止显示**xcopy**消息。|
|/f|复制时将显示源和目标文件的名称。|
|/l|显示要复制的文件的列表。|
|/g|创建解密*目标*文件时目标不支持加密。|
|/d [:MM-DD-YYYY]|复制源文件已更改或晚于指定日期。 如果不包括*MM DD YYYY*值， **xcopy**将所有复制*源*比现有文件*目标*文件。 此命令行选项可以更新已更改的文件。|
|/u|将文件从复制*源*上存在*目标*仅。|
|i|如果*源*是一个目录或包含通配符并*目标*不存在**xcopy**假定*目标*指定目录名称，并创建一个新目录。 然后， **xcopy**将所有指定的文件复制到新目录。 默认情况下**xcopy**将提示您指定是否*目标*是文件或目录。|
|/s|复制目录和子目录，除非它们为空。 如果省略 **/s**， **xcopy**在单个目录中工作。|
|/e|将复制所有子目录，即使它们为空。 使用 **/e**与 **/s**并 **/t**命令行选项。|
|/t|子目录结构 （树），而不是文件的副本。 若要将空的目录复制，必须包括 **/e**命令行选项。|
|/k|将文件复制和上保留只读属性*目标*文件如果位于*源*文件。 默认情况下**xcopy**删除只读属性。|
|/r|将只读的文件。|
|/h|将文件复制与隐藏和系统文件属性。 默认情况下**xcopy**复制隐藏文件或系统文件|
|/a|将仅复制*源*文件可以实现其存档的文件的属性集。 **/ a**不会修改源文件的存档文件属性。 有关如何使用设置的存档文件属性信息**attrib**，请参阅[其他参考](#additional-references)。|
|/m|副本*源*文件可以实现其存档的文件的属性集。 与不同 **/a**， **/m**关闭在源中指定的文件中的存档文件属性。 有关如何使用设置的存档文件属性信息**attrib**，请参阅[其他参考](#additional-references)。|
|/n|通过使用 NTFS 短文件名或目录名创建副本。 **/n**时将文件复制或需要从 NTFS 卷到 FAT 卷的目录或 FAT 文件系统命名约定 （即，8.3 个字符） 时是必需*目标*文件系统。 *目标*文件系统可以是 FAT 或 NTFS。|
|/o|复制文件所有权和随机访问控制列表 (DACL) 信息。|
|/x|复制文件的审核设置和系统访问控制列表 (SACL) 信息 (意味着 **/o**)。|
|/exclude:FileName1[+[FileName2][+[FileName3]( \)]|指定的文件的列表。 必须指定至少一个文件。 每个文件将包含搜索字符串，其中每个单独的文件中的行上的字符串。</br>时的任何字符串匹配要复制的文件的绝对任何的路径部分，将从复制中排除该文件。 例如，指定字符串**obj**将排除目录下的所有文件**obj**或使用的所有文件 **.obj**扩展。|
|/y|禁止提示确认你想要覆盖现有目标文件。|
|/-y|若要确认你想要覆盖现有目标文件的提示。|
|/z|在可重新启动模式下在网络上的副本。|
|/b|复制符号链接，而不是文件。 Windows Vista® 中引入了此参数。|
|/j|将文件复制而无需缓冲。 建议使用非常大的文件。 Windows Server 2008 R2 中新增了此参数。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 使用 **/z**

  如果丢失复制阶段 （例如，如果服务器脱机服务器连接） 连接，它将恢复后重新建立连接。 **/z**还会显示完成的每个文件的复制操作的百分比。

- 使用 **/y** COPYCMD 环境变量中。

  可以使用 **/y** COPYCMD 环境变量中。 可以通过使用此命令重写 **/y**命令行上。 默认情况下，系统会提示来覆盖。

- 复制加密的文件

  将加密的文件复制到的卷，不支持 EFS 结果中出现错误。 首先解密文件或将文件复制到支持 EFS 的卷。

- 追加文件

  若要附加文件，请指定单个文件的目标，但源的多个文件 (即，通过使用通配符或 file1 + file2 + file3 设置格式)。

- 默认值为*目标*

  如果省略*目标*，则**xcopy**命令将文件复制到当前目录。

- 指定是否*目标*是文件或目录

  如果*目标*不包含现有目录，并且不以反斜杠结尾 (\)，将显示以下消息：
  
  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```  
  
如果你想要复制到文件，请按 F。 如果你想要复制到目录的文件，请按 D。

  您可以使用禁止显示此消息 **/i**命令行选项，这会导致**xcopy**假定目标是一个目录，如果源是多个文件或目录。
- 使用**xcopy**命令设置存档特性*目标*文件

  **Xcopy**命令创建的文件设置了存档属性指示是否在源文件中设置此属性。 有关文件属性的详细信息和**attrib**，请参阅[其他参考](#additional-references)。

- 比较**xcopy**和**diskcopy**

  如果您有包含子目录中的文件的磁盘，并且你想要将其复制到磁盘具有不同的格式，请使用**xcopy**命令而不是**diskcopy**。 因为**diskcopy**命令将复制磁盘磁道地、 源和目标磁盘必须具有相同的格式。 **Xcopy**命令不具有此要求。 使用**xcopy**除非您需要完整的磁盘映像副本。

- 退出代码**xcopy**

  若要处理返回的退出代码**xcopy**，使用**ErrorLevel**上的参数**如果**命令行中的批处理程序。 有关进程退出代码使用的批处理程序示例**如果**，请参阅[其他参考](#additional-references)。 下表列出了每个退出代码和说明。  

  |退出代码|描述|
  |---------|-----------|
  |0|文件已复制不会出错。|
  |1|未找到文件复制。|
  |2|用户按下 CTRL + C 来终止**xcopy**。|
  |4|出现初始化错误。 没有足够的内存或磁盘空间，或在命令行上输入了无效的驱动器的名称或语法无效。|
  |5|磁盘写入错误出现。|

## <a name="examples"></a>示例

**1.** 若要将从驱动器的所有文件和子目录 （包括任何空子目录） 都复制到驱动器 B 中，键入：

```
xcopy a: b: /s /e 
```

**2.** 若要在前面的示例中包含的任何系统或隐藏的文件，添加<strong>/h</strong>命令行选项，如下所示：

```
xcopy a: b: /s /e /h
```

**3.** 若要使用自 1993 年 12 月 29 日以来更改过 \Rawdata 目录中的文件更新 \Reports 目录中的文件类型：

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** 若要更新在上一示例中，而不考虑日期，\Reports 中存在的所有文件类型：

```
xcopy \rawdata \reports /u
```

**5.** 若要获取的前一个命令复制的文件列表 (即，而不复制文件)，类型：

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

文件 xcopy.out 列出了要复制的每个文件。

**6.** 若要将 \Customer 目录和所有子目录复制到目录\\ \\Public\Address 网络上的提高 h:、 保留只读属性，并在 h： 类型上创建新文件时，系统会提示：

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** 若要发出前一个命令，请确保**xcopy**创建 \Address 目录，如果它不存在，和取消创建一个新目录时, 显示的消息添加 **/i**命令行选项，如下所示：

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** 您可以创建的批处理程序来执行**xcopy**操作和使用的 batch**如果**命令要处理的退出代码，如果发生错误。 例如，下面的批处理程序使用的可替换参数**xcopy**源和目标参数：

```
@echo off
rem COPYIT.BAT transfers all files in all subdirectories of
rem the source drive or directory (%1) to the destination
rem drive or directory (%2)
xcopy %1 %2 /s /e
if errorlevel 4 goto lowmemory
if errorlevel 2 goto abort
if errorlevel 0 goto exit
:lowmemory
echo Insufficient memory to copy files or
echo invalid drive or command-line syntax.
goto exit
:abort
echo You pressed CTRL+C to end the copy operation.
goto exit
:exit 
```

若要使用前面的批处理程序 C:\Prgmcode 目录及其子目录中的所有文件都复制到驱动器 B，请键入：

```
copyit c:\prgmcode b:
```

命令解释器替换**C:\Prgmcode**有关 *%1*并**b:** 有关 *%2*，然后使用**xcopy**与 **/e**并 **/s**命令行选项。 如果**xcopy**遇到的错误，批处理程序读取的退出代码，并指示在相应的标签将进入**如果 ERRORLEVEL**语句，然后显示相应的消息并退出批处理程序。

**9.** 此示例中所有非空的目录，加上其名称与中提供了星号符号的模式匹配的文件。

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

在前面的示例中，此特定的源参数值 **。\\toc\*.yml**复制相同的 3 个文件即使其两个路径字符 **。\\** 已删除。 但是，任何文件都如果星号通配符已删除源参数，使其只需从复制 **。\\toc.yml**。

#### <a name="additional-references"></a>其他参考

-   [复制](copy.md)
-   [移动](move.md)
-   [Dir](dir.md)
-   [Attrib](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [If](if.md)
-   [命令行语法项](command-line-syntax-key.md)
