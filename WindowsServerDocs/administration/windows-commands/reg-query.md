---
title: reg 查询
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d2f616fb33974df4327c7b2536b3143b75d116be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371716"
---
# <a name="reg-query"></a>reg 查询



返回位于注册表中指定子项下的子子项和条目的列表。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName >|指定子项的完整路径。 对于指定远程计算机，请包含计算机名称（格式为 \\ @ no__t-1ComputerName @ no__t-2 作为*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 将使操作默认为本地计算机。 *KeyName*必须包含有效的根密钥。 本地计算机的有效根密钥如下：HKLM、HKCU、HKCR、HKU 开头和 HKCC。 如果指定了远程计算机，则有效的根密钥为：HKLM 和 HKU 开头。|
|/v \<ValueName >|指定要查询的注册表值名称。 如果省略，则返回*KeyName*的所有值名称。 如果也使用了 **/f**选项，则此参数的*ValueName*是可选的。|
|/ve|为空值名称运行查询。|
|/s|指定以递归方式查询所有子项和值名称。|
|/se \<Separator >|指定要在值名称类型 REG_MULTI_SZ 中搜索的单个值分隔符。 如果未指定*Separator* ，则使用 **\ 0** 。|
|/f \<Data >|指定要搜索的数据或模式。 如果字符串包含空格，请使用双引号。 如果未指定，则使用通配符 **&#42;** （）作为搜索模式。|
|遇到|指定仅在项名称中搜索。|
|/d|指定仅搜索数据。|
|/c|指定查询区分大小写。 默认情况下，查询不区分大小写。|
|/e|指定仅返回完全匹配项。 默认情况下，将返回所有匹配项。|
|/t \<Type >|指定要搜索的注册表类型。 有效类型包括：REG_SZ、REG_MULTI_SZ、REG_EXPAND_SZ、REG_DWORD、REG_BINARY、REG_NONE。 如果未指定，则搜索所有类型。|
|/z|指定在搜索结果中包含注册表类型的等价数值。|
|/?|在命令提示符下显示**reg 查询**的帮助。|

## <a name="remarks-optional-section"></a>备注 @no__t 0optional 部分 >

下表列出了**reg query**操作的返回值。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要在 HKLM\Software\Microsoft\ResKit 项中显示 name 值版本的值，请键入：
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
若要在名为 ABC 的远程计算机上的 HKLM\Software\Microsoft\ResKit\Nt\Setup 项下显示所有子项和值，请键入：
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
若要使用 **#** 作为分隔符显示类型为 REG_MULTI_SZ 的所有子项和值，请键入：
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
若要在数据类型为 REG_SZ 的 HKLM 根下显示与系统区分大小写的密钥、值和数据，请键入：
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
显示与数据类型 REG_BINARY 的 HKCU 根密钥下的数据中的**0F**匹配的键、值和数据。
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
若要在 HKLM\SOFTWARE 下显示 null （默认值）的值名称和数据，请键入：
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)