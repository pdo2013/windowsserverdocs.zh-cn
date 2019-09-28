---
title: setx
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c206f36e2d0bc947329124b08fb797091e838bcd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384029"
---
# <a name="setx"></a>setx



在用户或系统环境中创建或修改环境变量，无需编程或编写脚本。 **Setx**命令还检索注册表项的值，并将它们写入文本文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> "<String>"} [/m] | /x} [/d <Delimiters>]
```

## <a name="parameters"></a>Parameters

|         参数          |                                                                                                                                              描述                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<Computer >       |                                                                                  指定远程计算机的名称或 IP 地址。 不要使用反斜杠。 默认值为本地计算机的名称。                                                                                  |
| /u [@no__t > \] @ no__t-2 |                                                                                           用指定用户帐户的凭据运行脚本。 默认值为 "系统权限"。                                                                                            |
|      /p [\<Password >]      |                                                                                                         指定在 **/u**参数中指定的用户帐户的密码。                                                                                                         |
|        \<Variable >         |                                                                                                                 指定要设置的环境变量的名称。                                                                                                                  |
|          \<Value >          |                                                                                                                指定要将环境变量设置为的值。                                                                                                                 |
|         /k \<Path >         | 指定根据注册表项中的信息设置变量。 P*a*使用以下语法：</br>`\\<HIVE>\<KEY>\...\<Value>`</br>例如，你可以指定以下路径：</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<File name >       |                                                                                                                               指定要使用的文件。                                                                                                                                |
|        /a \<X >，<Y>         |                                                                                                                    指定绝对坐标，偏移量作为搜索参数。                                                                                                                    |
|   /r \<X >，<Y> "<String>"   |                                                                                                            指定相对坐标和作为搜索参数的**字符串**的偏移量。                                                                                                            |
|             /m             |                                                                                                指定在系统环境中设置变量。 默认设置为本地环境。                                                                                                 |
|             /x             |                                                                                                       显示文件坐标，并忽略 **/a**、 **/r**和 **/d**命令行选项。                                                                                                        |
|      /d \<Delimiters >      |                    指定除四个内置分隔符外使用的分隔符（例如 " **、** " 或 " **\\** "）：空格、制表符、回车和换行。 有效的分隔符包含任何 ASCII 字符。 最大分隔符数为15，其中包括内置分隔符。                    |
|             /?             |                                                                                                                                 在命令提示符下显示帮助。                                                                                                                                  |

## <a name="remarks"></a>备注

-   **Setx**命令类似于 UNIX 实用工具 SETENV。
-   **Setx**提供唯一直接且永久地设置系统环境值的命令行或编程方式。 可以通过 **"控制面板"** 或通过注册表编辑器手动配置系统环境变量。 **Set**命令（在命令解释器（cmd.exe）内部）仅为当前控制台窗口设置用户环境变量。
-   可以使用**setx**命令从三个源（模式）之一设置用户和系统环境变量的值：命令行模式、注册表模式或文件模式。
-   **Setx**将变量写入注册表中的主环境。 使用**setx**变量设置的变量仅在以后的命令窗口中可用，而不能在当前的命令窗口中使用。
-   **HKEY_CURRENT_USER**和**HKEY_LOCAL_MACHINE**是唯一受支持的配置单元。 REG_DWORD、REG_EXPAND_SZ、REG_SZ 和 REG_MULTI_SZ 是有效的**RegKey**数据类型。
-   在注册表中访问**REG_MULTI_SZ**值后，只提取并使用第一项。
-   不能使用**setx**命令删除已添加到本地或系统环境的值。 您可以使用带变量名称和无值的**set**从本地环境中删除相应的值。
-   在十六进制模式中提取并使用 REG_DWORD 注册表值。
-   文件模式支持仅分析回车符和换行符（CRLF）文本文件。

## <a name="BKMK_examples"></a>示例

若要将本地环境中的计算机环境变量设置为 Brand1 值，请键入：
```
setx MACHINE Brand1
```
若要将系统环境中的计算机环境变量设置为值 Brand1 计算机，请键入：
```
setx MACHINE "Brand1 Computer" /m
```
若要将本地环境中的 MYPATH 环境变量设置为使用 PATH 环境变量中定义的搜索路径，请键入：
```
setx MYPATH %PATH%
```
若要在将 **~** 替换为 **@no__t**，请在本地环境中将 MYPATH 环境变量设置为使用 path 环境变量中定义的搜索路径，请键入：
```
setx MYPATH ~PATH~ 
```
若要在名为 Computer1 的远程计算机上将本地环境中的计算机环境变量设置为 Brand1，请键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
若要在本地环境中设置 MYPATH 环境变量以使用在名为 Computer1 的远程计算机上的 PATH 环境变量中定义的搜索路径，请键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
若要将本地环境中的 TZONE 环境变量设置为**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName**注册表项中的值，请键入：
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
若要将名为 Computer1 的远程计算机的本地环境中的 TZONE 环境变量设置为在**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName**注册表项中找到的值，请键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
若要在系统环境中将生成环境变量设置为在**HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber**注册表项中找到的值，请键入：
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
若要在名为 Computer1 的远程计算机的系统环境中将生成环境变量设置为**HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber**注册表项中找到的值，请键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
若要显示名为 Ipconfig. out 的文件的内容，以及内容的对应坐标，请键入：
```
setx /f ipconfig.out /x
```
若要将本地环境中的 IPADDR 环境变量设置为在文件 Ipconfig. out 中的坐标5、11处找到的值，请键入：
```
setx IPADDR /f ipconfig.out /a 5,11
```
若要将本地环境中的 OCTET1 环境变量设置为在坐标5中找到的值，请将文件中的值设置为 **"# $ \*"。**
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
若要将本地环境中的 IPGATEWAY 环境变量设置为在坐标0、7上找到的值（相对于文件中的 "网关" 坐标），请键入：
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
若要在名为 Computer1 的计算机上显示名为 Ipconfig. out 的文件的内容，并在其上显示内容的对应坐标，请键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)