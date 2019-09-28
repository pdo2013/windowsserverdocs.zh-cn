---
title: Cmd
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 032fbea2039faa09753ac0c2b51e4b62004d36ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379327"
---
# <a name="cmd"></a>Cmd

启动命令解释器 Cmd.exe 的新实例。 如果不使用参数， **cmd**将显示操作系统的版本和版权信息。

## <a name="syntax"></a>语法

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/c|执行*字符串*指定的命令，然后停止。|
|遇到|执行*String*指定的命令，然后继续。|
|/s|修改 **/c**或 **/K**之后的*字符串*处理。|
|/q|关闭 echo。|
|/d|禁止执行自动运行命令。|
|/a|将内部命令输出的格式设置为管道或文件美国国家标准学会（ANSI）。|
|/u|将内部命令输出的格式设置为作为 Unicode 的管道或文件。|
|/t： {\<B @ no__t-1 @ no__t-2F @ no__t-3 @ no__t @ no__t-5F @ no__t-6}|设置背景（*B*）和前景（*F*）颜色。|
|/e：开启|启用命令扩展。|
|/e： off|禁用命令扩展。|
|/f：开启|启用文件和目录名称完成。|
|/f： off|禁用文件和目录名称完成。|
|/v：开启|启用延迟环境变量扩展。|
|/v：关|禁用延迟的环境变量扩展。|
|\<String >|指定要执行的命令。|
|/?|在命令提示符下显示帮助。|

下表列出了可用作 \<B @ no__t 和 \<F @ no__t 的值的有效十六进制数字

|ReplTest1|颜色|
|-----|-----|
|0|黑色|
|1|蓝色|
|2|厚︹|
|3|水|
|4|红色|
|5|紫|
|6|独︹|
|7|白色|
|8|灰色|
|9|浅蓝色|
|a|浅绿色|
|b|浅浅绿色|
|c|浅红色|
|d|浅紫色|
|电邮|浅黄色|
|f|亮白色|

## <a name="remarks"></a>备注

-   使用多个命令

    若要将多个命令用于 @no__t 0String >，请使用命令分隔符分隔它们， **&&** 并将其括在引号中。 例如：

    ```
    "<Command>&&<Command>&&<Command>"
    ``` 
 
-   处理引号

    如果指定 **/c**或 **/k**，则**cmd**将处理字符串的其余部分 *，* 并且仅当满足以下所有条件时，才保留引号：  
    -   不要使用 **/s**。
    -   只使用一组引号。
    -   不使用引号内的任何特殊字符（例如： & < > （） @ ^ |）。
    -   在引号内使用一个或多个空白字符。
    -   引号中的*字符串*是可执行文件的名称。

    如果未满足上述条件，则将通过检查第一个字符来处理*字符串*，以验证它是否是左引号。 如果第一个字符是左引号，则将其与右引号一起去除。 保留右引号后面的任何文本。
-   执行注册表子项

    如果未指定 **/d** in *String*，cmd.exe 将查找以下注册表子项：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun\REG_EXPAND_SZ**

    如果存在一个或两个注册表子项，它们将在所有其他变量之前执行。

> [!CAUTION]
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。

-   启用和禁用命令扩展

    默认情况下，在 Windows XP 中启用命令扩展。 您可以使用 **/e： off**为特定进程禁用它们。 您可以通过设置以下**REG_DWORD**值为计算机或用户会话上的所有**cmd**命令行选项启用或禁用扩展：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    通过使用 Regedit.exe，将**REG_DWORD**值设置为**0 × 1** （已启用）或**0 × 0** （禁用）。 用户指定的设置优先于计算机设置，命令行选项优先于注册表设置。

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

-   启用延迟环境变量扩展

    如果启用延迟环境变量扩展，则可以使用感叹号字符来替换运行时环境变量的值。
-   启用文件和目录名称完成

    默认情况下，不启用文件和目录名称完成。 您可以使用 **/f：** {**on**|**off**} 启用或禁用**cmd**命令特定进程的文件名完成。 您可以通过设置以下**REG_DWORD**值为计算机上的**cmd**命令的所有进程启用或禁用文件和目录名完成，或为用户登录会话启用或禁用它们：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    若要设置**REG_DWORD**值，请运行 regedit.exe 并将控制字符的十六进制值用于特定函数（例如， **0 × 9**为 TAB， **0 × 08**表示退格符）。 用户指定的设置优先于计算机设置，命令行选项优先于注册表设置。

> [!CAUTION]
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。

如果通过使用 **/f： on**启用文件和目录名完成，请使用 Ctrl + D 来完成目录名称，并使用 Ctrl + f 来完成文件名。 若要在注册表中禁用特定的完成字符，请使用空白 [**0 × 20**] 的值，因为它不是有效的控制字符。

按 CTRL + D 或 CTRL + F 时， **cmd**会处理文件和目录名称的完成。 这些键组合函数将一个通配符追加到*字符串*（如果不存在），生成一个匹配的路径列表，然后显示第一个匹配的路径。 如果路径都不匹配，则文件和目录名称完成功能将发出嘟嘟声，并且不会更改显示。 若要在匹配路径列表中移动，请重复按 CTRL + D 或 CTRL + F。 若要向后移动列表，请同时按 SHIFT 键和 CTRL + D 或 CTRL + F。 若要放弃已保存的匹配路径列表并生成新列表，请编辑*字符串*，然后按 Ctrl + D 或 Ctrl + F。 如果在 "CTRL + D" 和 "CTRL + F" 之间切换，则会丢弃已保存的匹配路径列表，并生成一个新列表。 组合键 CTRL + D 和 CTRL + F 之间的唯一区别在于，CTRL + D 只匹配目录名称，CTRL + F 匹配文件和目录名称。 如果在任何内置目录命令（即**CD**、 **MD**或**RD**）上使用文件和目录名称完成，则会假定目录已完成。

如果用引号将引号括起来，则文件和目录名称完成会正确地处理包含空格或特殊字符的文件名。

以下特殊字符需要用引号引起来： & < > [] {} ^ =;！ "+，" ~ [空格]。

如果提供的信息包含空格，请使用引号将文本括起来（例如，"计算机名称"）。

如果从*字符串*内处理文件和目录名完成，则将丢弃光标右侧*路径*的任何部分（在*字符串*中处理完成的位置）。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
