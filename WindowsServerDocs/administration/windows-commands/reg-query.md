---
title: reg 查询
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e239184cc5d118a858d012528fd8135f0b834e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827748"
---
# <a name="reg-query"></a>reg 查询



返回的下一层子项和注册表项都位于注册表中指定的子项下的列表。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName>|指定的子项的完整路径。 用于指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。|
|/v \<ValueName>|指定是要查询的注册表值名称。 如果省略，所有值都名称*KeyName*返回。 *ValueName*此参数是可选的如果 **/f**还使用选项。|
|/ve|在运行查询的值为空的名称。|
|/s|指定要查询的所有子项和值名称以递归方式。|
|/se \<Separator>|指定要搜索的值类型 REG_MULTI_SZ 中的单个值分隔符。 如果*分隔符*未指定，则**\0**使用。|
|/f\<数据 >|指定的数据或要搜索模式。 如果字符串包含空格，请使用双引号引起来。 如果未指定通配符 (**&#42;**) 用作搜索模式。|
|/k|指定要搜索密钥只在名称中。|
|/d|指定要在仅限数据中搜索。|
|/c|指定查询是区分大小写。 默认情况下，查询不区分大小写。|
|/e|指定要返回仅完全匹配项。 默认情况下，返回所有匹配项。|
|/t \<Type>|指定要搜索的注册表类型。 有效类型包括：REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. 如果未指定，则搜索所有类型。|
|/z|指定要包括在搜索结果中，注册表类型的数字等效项。|
|/?|显示的帮助**reg 查询**在命令提示符处。|

## <a name="remarks-optional-section"></a>备注\<可选部分 >

下表列出的返回值**reg 查询**操作。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要显示的版本中的名称值的值中 HKLM\Software\Microsoft\ResKit 密钥，请键入：
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
若要显示名为 ABC 在远程计算机上的所有子项和项 HKLM\Software\Microsoft\ResKit\Nt\Setup 下的值，请键入：
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
若要显示的所有子项和值的使用类型 REG_MULTI_SZ **#** 作为分隔符，键入：
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
若要显示的键，值和数据用于确切和区分大小写匹配系统的 HKLM 根下的数据类型为 REG_SZ，类型：
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
若要显示键、 值和匹配的数据**0F**中数据的 HKCU 根项下的数据类型 REG_BINARY。
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
若要显示的值和值名称为 null （默认值） 在 HKLM\SOFTWARE 下的数据，请键入：
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)