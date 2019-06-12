---
title: setx
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0b2caceed6962bef22e7d546fa3b4469c9682b39
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441252"
---
# <a name="setx"></a>setx



创建或修改环境变量中的用户或系统环境中，而无需编程或脚本。 **Setx**命令还检索注册表项的值，并将其写入到文本文件。

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
|       /s\<计算机 >       |                                                                                  指定的名称或远程计算机的 IP 地址。 不要使用反斜杠。 默认值为本地计算机的名称。                                                                                  |
| /u [\<域 >\]<User name> |                                                                                           使用指定的用户帐户的凭据运行该脚本。 默认值为系统权限。                                                                                            |
|      /p [\<Password>]      |                                                                                                         指定在指定的用户帐户的密码 **/u**参数。                                                                                                         |
|        \<变量 >         |                                                                                                                 指定你想要设置的环境变量的名称。                                                                                                                  |
|          \<值 >          |                                                                                                                指定你想要设置环境变量的值。                                                                                                                 |
|         /k \<Path>         | 指定变量的信息从注册表项设置基于。 P*ath*使用以下语法：</br>`\\<HIVE>\<KEY>\...\<Value>`</br>例如，你可能指定以下路径：</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f\<文件名称 >       |                                                                                                                               指定你想要使用的文件。                                                                                                                                |
|        / a \<X >，<Y>         |                                                                                                                    作为搜索参数指定绝对坐标和偏移量。                                                                                                                    |
|   /r \<X >，<Y> "<String>"   |                                                                                                            指定相对坐标和偏移量**字符串**作为搜索参数。                                                                                                            |
|             /m             |                                                                                                指定在系统环境中设置该变量。 默认设置为在本地环境。                                                                                                 |
|             /x             |                                                                                                       显示文件坐标，忽略 **/a**， **/r**，并 **/d**命令行选项。                                                                                                        |
|      /d\<分隔符 >      |                    指定分隔符，例如" **，** " **\\** "要使用的四个内置分隔符除了 — 空间、 选项卡、 ENTER 和换行。 有效分隔符包括任何 ASCII 字符。 分隔符的最大数目为 15，包括内置的分隔符。                    |
|             /?             |                                                                                                                                 在命令提示符下显示帮助。                                                                                                                                  |

## <a name="remarks"></a>备注

-   **Setx**命令是类似于 UNIX 实用程序 SETENV。
-   **Setx**提供直接和永久设置系统环境值的只有命令行或编程方法。 系统环境变量是可通过手动配置**控制面板**或通过注册表编辑器。 **设置**命令，位于内部命令解释器 (Cmd.exe)，设置在当前控制台窗口的用户环境变量。
-   可以使用**setx**命令来设置值的用户和系统环境变量从三个源 （模式） 之一：命令行模式下，注册表模式或文件模式。
-   **Setx**写入到注册表中的主环境变量。 使用设置变量**setx**变量仅，将来的命令窗口中提供不是当前的命令窗口中。
-   **HKEY_CURRENT_USER**并**HKEY_LOCAL_MACHINE**是唯一受支持配置单元。 REG_DWORD、 REG_EXPAND_SZ、 REG_SZ 和 REG_MULTI_SZ 是有效**RegKey**数据类型。
-   当获取访问权限**REG_MULTI_SZ**提取并使用在注册表中，仅第一项的值。
-   不能使用**setx**命令删除已添加到本地或系统环境的值。 可以使用**设置**使用变量名称和要从本地环境中删除相应的值的值。
-   REG_DWORD 注册表值中提取，并在十六进制模式下使用。
-   文件模式支持分析回车符和换行符 (CRLF) 文本文件。

## <a name="BKMK_examples"></a>示例

若要将计算机环境变量设置为值 Brand1 在本地环境中，键入：
```
setx MACHINE Brand1
```
若要将计算机环境变量设置为值 Brand1 计算机系统环境中，键入：
```
setx MACHINE "Brand1 Computer" /m
```
若要将 MYPATH 环境变量设置要使用路径环境变量中定义的搜索路径的本地环境中，键入：
```
setx MYPATH %PATH%
```
若要设置要使用替换之后在 PATH 环境变量中定义的搜索路径的本地环境中的 MYPATH 环境变量 **~** 与 **%** ，类型：
```
setx MYPATH ~PATH~ 
```
若要将计算机环境变量设置 Brand1 到名为 Computer1 的远程计算机上的本地环境中，键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
要将 MYPATH 环境变量设置为使用名为 Computer1 的远程计算机上的 PATH 环境变量中定义的搜索路径在本地环境中，键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
在中找到的值为本地环境中设置 TZONE 环境变量**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName**注册表键，类型：
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
在中找到的值为 Computer1 的远程计算机的本地环境中设置 TZONE 环境变量**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName**注册表键，类型：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
若要在中找到的值在系统环境中设置生成的环境变量**HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber**注册表键，类型：
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
若要设置到中找到的值为 Computer1 的远程计算机的系统环境中的生成环境变量**HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber**注册表项类型：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
若要显示的内容的相应坐标，以及名为在文件的内容键入：
```
setx /f ipconfig.out /x
```
若要设置 IPADDR 环境变量中的文件在坐标 5,11 处找到的值在本地环境中，键入：
```
setx IPADDR /f ipconfig.out /a 5,11
```
若要在为分隔符的文件在坐标 5,3 处找到的值在本地环境中设置 OCTET1 环境变量 **"#$\*。"** ，类型：
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
若要设置 ip 网关的环境变量中的文件在坐标 0,7 相对于"网关"的坐标处找到的值在本地环境中，键入：
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
若要显示在名为的文件的内容 — 以及内容的相应坐标 — 在名为 Computer1 的计算机，键入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)