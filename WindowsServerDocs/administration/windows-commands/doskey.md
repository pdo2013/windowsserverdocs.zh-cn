---
title: doskey
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d45a2ddfeba7ec136add07eac11c3a8522ef872b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377686"
---
# <a name="doskey"></a>doskey



调用 Doskey （回调先前输入的命令行命令）、编辑命令行并创建宏。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

## <a name="parameters"></a>Parameters

|       参数        |                                                                                                                          描述                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /reinstall       |                                                                                            安装 Doskey 的新副本并清除命令历史记录缓冲区。                                                                                            |
|   /listsize = \<Size >    |                                                                                                指定历史记录缓冲区中的最大命令数。                                                                                                 |
|        /macros         |                                        显示所有**doskey**宏的列表。 可以将重定向符号（ **>** ）与 **/macros**一起使用，将列表重定向到文件。 可以缩写 **/macros**到 **/m**。                                         |
|      /macros： all       |                                                                                                        显示所有可执行文件的**doskey**宏。                                                                                                         |
|   /macros： \<ExeName >   |                                                                                             显示*ExeName*指定的可执行文件的**doskey**宏。                                                                                              |
|        /history        |                                    显示存储在内存中的所有命令。 可以将重定向符号（ **>** ）与 **/history**一起使用，将列表重定向到文件。 可以将 **/history**缩写为 **/h**。                                    |
| /insert | 指定您键入的新文本将插入旧文本。 |
| /overstrike | 指定新文本覆盖旧文本。 |
|  /exename = \<ExeName >   |                                                                                        指定在其中运行**doskey**宏的程序（即可执行文件）。                                                                                         |
| /macrofile = \<FileName > |                                                                                              指定一个文件，其中包含要安装的宏。                                                                                               |
| \<MacroName > = [\<Text >]  | 创建一个宏，该宏执行*文本*指定的命令。 *MacroName*指定要分配给宏的名称。 *Text*指定要记录的命令。 如果*文本*保留为空白，则将清除任何已分配命令的*MacroName* 。 |
|           /?           |                                                                                                              在命令提示符下显示帮助。                                                                                                              |

## <a name="remarks"></a>备注

- 使用 Doskey

  Doskey 始终可用于所有基于字符的交互式程序（如程序调试器或文件传输程序），并且它将为其启动的每个程序保留一个命令历史记录缓冲区和宏。 不能从程序使用**doskey**命令行选项。 必须先运行**doskey**命令行选项，然后再启动程序。 程序密钥分配将覆盖**doskey**密钥分配。
- 撤回命令

  若要重新调用命令，你可以在启动 Doskey 后使用以下任何密钥。 如果您在程序中使用 Doskey，则优先使用该程序的密钥分配。  

  |    Key     |                              描述                              |
  |------------|-----------------------------------------------------------------------|
  |  向上键  |  撤回在显示的命令之前使用的命令。  |
  | 向下键 |  撤回在显示后使用的命令。   |
  |  Page Up   |    撤回在当前会话中使用的第一个命令。    |
  | Page Down  | 撤回当前会话中使用的最近使用的命令。 |


- 编辑命令行

  通过 Doskey，你可以编辑当前命令行。 如果您在程序中使用 Doskey，则该程序的密钥分配优先，一些 Doskey 编辑密钥可能不起作用。

  下表列出了**doskey**编辑密钥及其功能。  

  | 键或键组合 |                                                                                                                                                       描述                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       向左键       |                                                                                                                                      将插入点向后移动一个字符。                                                                                                                                      |
  |      向右键       |                                                                                                                                    将插入点向后移动一个字符。                                                                                                                                     |
  |    CTRL + 向左键     |                                                                                                                                        将插入点向后移动一个单词。                                                                                                                                         |
  |    CTRL + 向右键    |                                                                                                                                       将插入点向前移动一个单词。                                                                                                                                       |
  |          Home          |                                                                                                                                 将插入点移动到行首。                                                                                                                                 |
  |          End           |                                                                                                                                    将插入点移动到行的末尾。                                                                                                                                    |
  |          ESC           |                                                                                                                                          清除显示的命令。                                                                                                                                           |
  |           F1           |                                                                      将模板中的列中的一个字符复制到命令提示符窗口中的相同列。 （模板是存储所键入的最后一个命令的内存缓冲区。）                                                                       |
  |           F2           |                                                                 按下 F2 后，在模板中向前搜索你键入的下一个键。 Doskey 插入模板中的文本（最多，但不包括指定字符）。                                                                  |
  |           F3           |                                                 将模板的其余部分复制到命令行。 Doskey 从模板中的位置开始复制字符，该位置与命令行上的插入点所指示的位置相对应。                                                 |
  |           F4           |                                                                            在按 F4 后，删除当前插入点位置中的所有字符，直到下一次出现的字符。                                                                            |
  |           F5           |                                                                                                                                   将模板复制到当前命令行中。                                                                                                                                    |
  |           F6           |                                                                                                                    将文件尾字符（CTRL + Z）置于当前插入点位置。                                                                                                                    |
  |           F7           | 显示此程序存储在内存中的所有命令（在对话框中）。 使用向上键和向下键选择所需的命令，然后按 ENTER 运行该命令。 还可以记下命令前面的序列号，并将此数字与 F9 键结合使用。 |
  |         ALT + F7         |                                                                                                                          为当前历史记录缓冲区删除存储在内存中的所有命令。                                                                                                                          |
  |           F8           |                                                                                                           显示历史缓冲区中以当前命令中的字符开头的所有命令。                                                                                                            |
  |           F9           |                                             将提示您输入历史缓冲区命令编号，然后显示与您指定的号码关联的命令。 按 ENTER 运行该命令。 若要显示所有数字及其关联的命令，请按 F7。                                             |
  |        ALT + F10         |                                                                                                                                             删除所有宏定义。                                                                                                                                              |


- 在程序中使用**doskey**

  某些基于字符的交互式程序（如程序调试器或文件传输程序（FTP））会自动使用 Doskey。 若要使用 Doskey，程序必须是控制台进程并使用缓冲输入。 程序密钥分配将覆盖**doskey**密钥分配。 例如，如果程序对某个函数使用了 F7 键，则不能在弹出窗口中获取**doskey**命令历史记录。

  使用 Doskey，可以为启动或重复的每个程序维护命令历史记录。 你可以在程序的提示中编辑以前的命令，并启动为程序创建的**doskey**宏。 如果退出并重新启动同一个命令提示符窗口中的程序，则可使用上一个程序会话中的命令历史记录。

  必须先运行 Doskey，然后再启动程序。 你不能从程序的命令提示符使用**doskey**命令行选项，即使该程序具有 shell 命令也是如此。

  如果要自定义 Doskey 如何处理程序和为该程序创建**doskey**宏，可以创建一个批处理程序来修改 Doskey 并启动程序。
- 指定默认插入模式

  如果按 INSERT 键，则可以在现有文本中间的**doskey**命令行上键入文本，而无需替换文本。 但在按 ENTER 后，Doskey 会将键盘恢复为替换模式。 必须再次按 INSERT 才能返回到插入模式。

  每次按 ENTER 时，可使用 **/insert**将键盘切换到插入模式。 在使用 **/overstrike**之前，键盘会有效地保持在插入模式下。 可以通过按 INSERT 键来暂时返回到 Replace 模式，但在按 ENTER 后，Doskey 会将你的键盘返回到插入模式。

  当使用 INSERT 键从一种模式更改为另一种模式时，插入点会改变形状。
- 创建宏

  可以使用 Doskey 创建执行一个或多个命令的宏。 下表列出了可用于在定义宏时控制命令操作的特殊字符。  

  |   字符   |                                                                                                                                                                               描述                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G 或 $g    |                                                                                   重定向输出。 使用这两个特殊字符将输出发送到设备或文件而不是屏幕。 此字符等效于 output 的重定向符号（ **>** ）。                                                                                    |
  | $G $ G 或 $g $ g  |                                                         将输出追加到文件末尾。 使用这两个双字符将输出追加到现有文件，而不是替换文件中的数据。 这些双字符等效于 output 的追加重定向符号（ **>>** ）。                                                         |
  |   $L 或 $l    |                                                                                  重定向输入。 使用上述任一特殊字符可以从设备或文件而不是键盘读取输入。 此字符等效于输入的重定向符号（ **<** ）。                                                                                  |
  |   $B 或 $b    |                                                                                                                                    将宏输出发送到命令。 这些特殊字符等效于使用管道（\* @ no__t-1                                                                                                                                     |
  |   $T 或 $t    |                                                            分隔命令。 在**doskey**命令行上创建宏或类型命令时，可使用以下任一特殊字符分隔命令。 这些特殊字符等效于在命令行上使用与号（ **&** ）。                                                            |
  |      $$       |                                                                                                                                                              指定美元符号字符（ **$** ）。                                                                                                                                                               |
  | $1 到 $9 |             表示运行宏时要指定的任何命令行信息。 特殊字符 **$1**到 **$9**是批处理参数，可让你在每次运行宏时在命令行上使用不同的数据。 **Doskey**命令中的 **$1**字符与批处理程序中的 **% 1**字符类似。             |
  |      $\*      | 表示键入宏名时要指定的所有命令行信息。 特殊字符 **$ @ no__t-2**@no__t 为一个可替换参数，它与批处理参数 **$1**到 **$9**相似，但有一个重要区别：在命令行中键入宏名称后的所有内容都是替换宏中的 **$ @ no__t-8**\*。 |


- 运行**doskey**宏

  若要运行宏，请在命令提示符处键入宏名称，从第一个位置开始。 如果宏是使用 **$ @ no__t*** 或任意批处理参数 **$1**到 **$9**定义的，请使用空格分隔参数。 不能从批处理程序运行**doskey**宏。
- 创建与 Windows Server 2003 家族命令同名的宏

  如果始终使用特定命令和特定命令行选项，则可以创建一个与命令同名的宏。 若要指定是要运行宏还是运行命令，请遵循以下准则：  
  -   若要运行此宏，请在命令提示符处键入宏名称。 不要在宏名之前加空格。
  -   若要运行此命令，请在命令提示符处插入一个或多个空格，然后键入命令名称。
- 删除宏

  若要删除宏，请键入：  
  ```
  doskey <MacroName> =
  ```

## <a name="BKMK_examples"></a>示例

**/Macros**和 **/history**命令行选项可用于创建批处理程序以保存宏和命令。 例如，若要存储所有当前**doskey**宏，请键入：
```
doskey /macros > macinit 
```
若要使用存储在 Macinit 中的宏，请键入：
```
doskey /macrofile=macinit 
```
若要创建一个名为 Tmp 的批处理程序，其中包含最近使用过的命令，请键入：
```
doskey /history> tmp.bat 
```
若要定义包含多个命令的宏，请使用 **$t**分隔命令，如下所示：
```
doskey tx=cd temp$tdir/w $*
```
在前面的示例中，TX 宏将当前目录更改为 Temp，然后以宽显示格式显示目录列表。 在运行 TX 时，可以使用宏末尾的 **$ @ no__t*** 将其他命令行选项追加到**dir** 。

以下宏对新目录名称使用 batch 参数：
```
doskey mc=md $1$tcd $1
```
宏创建一个新目录，然后从当前目录更改到新目录。

若要使用上述宏创建并更改到名为书籍的目录，请键入：
```
mc books
```
若要为名为 cluster.exe 的程序创建**doskey**宏，请包括 **/exename** ，如下所示：
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
若要使用上述宏，请启动 FTP。 在 FTP 提示符下，键入：
```
go
```
FTP 运行**open**、 **mget**和**再见**命令。

若要创建快速、无条件地格式化磁盘的宏，请键入：
```
doskey qf=format $1 /q /u
```
若要快速而无条件地格式化驱动器 A 中的磁盘，请键入：
```
qf a:
```
若要删除名为 vlist 的宏，请键入：
```
doskey vlist =
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
