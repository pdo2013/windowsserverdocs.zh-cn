---
title: Cmd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 581e9a3bad8323c79839a4487b7da045e9cfec21
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811236"
---
# <a name="cmd"></a>Cmd

启动命令解释器，Cmd.exe 的新实例。 如果使用不带参数， **cmd**显示操作系统的版本和版权信息。

## <a name="syntax"></a>语法

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/c|执行指定的命令*字符串*，然后停止。|
|/k|执行指定的命令*字符串*并继续执行。|
|/s|修改的处理方式*字符串*后 **/c**或 **/k**。|
|/q|关闭回显。|
|/d|禁用自动运行命令的执行。|
|/a|设置管道或文件的内部命令输出的格式为美国美国国家标准协会 (ANSI)。|
|/u|设置为管道或文件的内部命令输出格式以 unicode 格式。|
|/t:{\<B\>\<F\>\|\<F\>}|将背景设置 (*B*) 和前景色 (*F*) 颜色。|
|/e： 上|启用命令扩展。|
|/e： 关闭|禁用命令扩展。|
|/f： 上|允许完成文件和目录名称。|
|/f:off|禁用完成文件和目录名称。|
|/v： 上|启用延迟的环境变量扩展。|
|/v： 关闭|禁用延迟的环境变量扩展。|
|\<String>|指定你想要执行的命令。|
|/?|在命令提示符下显示帮助。|

下表列出了可以使用的值的形式的有效十六进制数字\<B\>和\<F\>

|值|颜色|
|-----|-----|
|0|黑色|
|1|蓝色|
|2|厚︹|
|3|浅绿色|
|4|红色|
|5|紫色|
|6|独︹|
|7|白色|
|8|灰色|
|9|浅蓝色|
|a|浅绿色|
|b|淡浅绿色|
|c|浅红色|
|d|淡紫色|
|E|淡黄色|
|f|亮白色|

## <a name="remarks"></a>备注

-   使用多个命令

    若要使用的多个命令\<字符串 >，这些值分隔命令分隔符 **&&** 并将它们括在引号中。 例如：

    ```
    "<Command>&&<Command>&&<Command>"
    ``` 
 
-   处理引号

    如果指定 **/c**或 **/k**， **cmd**处理的其余部分*字符串，* 和引号保留仅当所有下列项满足条件：  
    -   不使用 **/s**。
    -   使用一组引号引起来。
    -   不使用任何特殊字符位于引号 (例如： & @ < > （) ^ |)。
    -   使用在引号内的一个或多个空白字符。
    -   *字符串*引号内是可执行文件的名称。

    如果不满足上述条件，*字符串*处理通过检查来验证它是否是一个左引号的第一个字符。 如果第一个字符是一个左引号，则将会去除以及右引号。 保留后结束引号引起来的所有文本。
-   执行注册表子项

    如果未指定 **/d**中*字符串*，Cmd.exe 查找以下注册表子项：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun\REG_EXPAND_SZ**

    如果存在一个或两个注册表子项时，也会执行所有其他变量之前。

> [!CAUTION]
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。

-   启用和禁用命令扩展

    默认情况下，在 Windows XP 中启用命令扩展。 可以通过使用禁用它们的特定进程 **/e： 关闭**。 可以启用或禁用扩展的所有**cmd**通过设置以下的计算机或用户会话上的命令行选项**REG_DWORD**值：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    **两 Processor\EnableExtensions\REG_DWORD**

    设置**REG_DWORD**值为**0 × 1** （启用） 或**0 × 0** （禁用） 在注册表中使用 Regedit.exe。 用户指定的设置优先于计算机设置和命令行选项优先于注册表设置。

> [!CAUTION]
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。

    When you enable command extensions, the following commands are affected:  
    -  **assoc**
    -  **call**
    -  **chdir (cd)**
    -  **color**
    -  **del (erase)**
    -  **endlocal**
    -  **for**
    -  **ftype**
    -  **goto**
    -  **if**
    -  **mkdir (md)**
    -  **popd**
    -  **prompt**
    -  **pushd**
    -  **set**
    -  **setlocal**
    -  **shift**
    -  **start** (also includes changes to external command processes)

-   启用延迟的环境变量扩展

    如果启用延迟的环境变量扩展，可用于在惊叹号字符替换为在运行时的环境变量的值。
-   启用文件和目录名称完成

    默认情况下不启用完成文件和目录名称。 可以启用或禁用特定的进程完成的文件名称**cmd**命令 **/f:** {**上**|**关闭**}. 可以启用或禁用文件和目录名称完成的所有进程**cmd**的计算机上或通过设置以下用户登录会话的命令**REG_DWORD**值：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    若要设置**REG_DWORD**值，运行 Regedit.exe，并为特定的函数中使用控制字符的十六进制值 (例如， **0 × 9**选项卡并**0 × 08**是退格符）。 用户指定的设置优先于计算机设置和命令行选项优先于注册表设置。

> [!CAUTION]
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。

如果使用启用文件和目录名称完成 **/f： 上**，完成文件名称中使用 CTRL + D 完成目录名称和 CTRL + F。 若要禁用注册表中的某个字符，使用值为空白 [**0 × 20**] 因为它不是有效的控制字符。

当您按 CTRL + D 或 CTRL + F **cmd**处理完成文件和目录名称。 这些键组合函数后面追加到一个通配符*字符串*（如果其中一个不存在），生成的路径匹配，并显示第一个匹配的路径的列表。 如果没有任何路径匹配，文件和目录名完成操作时发出提示音，并且不会更改显示。 若要移动到匹配路径的列表，请重复按 CTRL + D 或 CTRL + F。 若要在列表中向后移动，同时按下 SHIFT 键和 CTRL + D 或 CTRL + F。 若要放弃保存的匹配的路径的列表，并生成新列表，请编辑*字符串*，然后按 CTRL + D 或 CTRL + F。 如果您按 CTRL + D 和 CTRL + F 之间切换，保存的匹配的路径的列表将被丢弃并生成新列表。 组合键 CTRL + D，CTRL + F 之间的唯一区别是，CTRL + D 仅匹配的目录名称并按 CTRL + F 匹配文件和目录名称。 如果完成文件和目录名称使用任一内置目录命令 (即**CD**， **MD**，或**RD**)，假定目录完成。

完成文件和目录名称会正确地处理如果放置引号相匹配的路径包含空格或特殊字符的文件名。

以下特殊字符需要使用引号： & < > [] {} ^ =; ！ '+' ~ [空格]。

如果你提供的信息包含空格，则使用引号将文本 （例如，"计算机名称"）。

如果处理中的文件和目录名称完成*字符串*，则所有的一部分*路径*右侧的游标将被丢弃 (中的点处*字符串*其中完成已处理）。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
