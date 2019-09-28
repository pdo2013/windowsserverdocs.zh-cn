---
title: choice
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e710b3813525647053365ebf4c764181fc38b3f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379368"
---
# <a name="choice"></a>choice



提示用户从批处理程序中的单字符选项列表中选择一项，然后返回选定选择的索引。 如果在没有参数的情况下使用，则**choice**将显示默认选择**Y**和**N**。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <"Text">]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/c @no__t > <Choice2> < 。>|指定要创建的选项的列表。 有效选项包括 a-z、a-z、0-9 和扩展的 ASCII 字符（128-254）。 默认列表为 "YN"，它显示为 `[Y,N]?`。|
|/n|隐藏选项列表，尽管仍将启用这些选项，并且仍显示消息文本（如果已指定 **）。**|
|/cs|指定选项区分大小写。 默认情况下，选择不区分大小写。|
|/t \<Timeout >|指定在使用 **/d**指定的默认选项之前要暂停的秒数。 可接受的值为**0**到**9999**。 如果 **/t**设置为**0**，则在返回默认选项之前，**选择**不暂停。|
|/d \<Choice >|指定在等待 **/t**指定的秒数后要使用的默认选项。 默认选项必须在 **/c**所指定的选项列表中。|
|/m < "Text" >|指定要在选项列表之前显示的消息。 如果未指定 **/m** ，则只显示选择提示。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   ERRORLEVEL 环境变量设置为用户从选项列表中选择的键的索引。 列表中的第一个选项返回值为1，第二个值为2，依此类推。 如果用户按下不是有效选择的密钥，则**choice**会出现警告提示音。 如果**choice**检测到错误条件，则返回 ERRORLEVEL 值255。 如果用户按 "CTRL + BREAK" 或 CTRL + C，则**choice**将返回 ERRORLEVEL 值0。

> [!NOTE]
> 在批处理程序中使用 ERRORLEVEL 值时，请按降序列出它们。

## <a name="BKMK_examples"></a>示例

若要显示 Y、N 和 C 选项，请在批处理文件中键入以下行：
```
choice /c ync
```
当批处理文件运行**choice**命令时，将显示以下提示：
```
[Y,N,C]?
```
若要隐藏选项 Y、N 和 C，但显示文本 "Yes，No 或 Continue"，请在批处理文件中键入以下行：
```
choice /c ync /n /m "Yes, No, or Continue?"
```
当批处理文件运行**choice**命令时，将显示以下提示：
```
Yes, No, or Continue?
```

> [!NOTE]
> 如果使用 **/n**参数，但不使用 **/m**，则在**选择**等待输入时不会提示用户。

若要显示在前面的示例中使用的文本和选项，请在批处理文件中键入以下行：
```
choice /c ync /m "Yes, No, or Continue"
```
当批处理文件运行**choice**命令时，将显示以下提示：
```
Yes, No, or Continue [Y,N,C]?
```
若要设置5秒的时间限制并指定**N**作为默认值，请在批处理文件中键入以下行：
```
choice /c ync /t 5 /d n
```
当批处理文件运行**choice**命令时，将显示以下提示：
```
[Y,N,C]?
```

> [!NOTE]
> 在此示例中，如果用户在5秒内未按某个键，则**choice**默认选择**N** ，并返回错误值2。 否则， **choice**返回对应于用户选择的值。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)