---
title: choice
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78d2816bff754ef04558cf37eaada7c7fafba823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841548"
---
# <a name="choice"></a>choice



提示用户从单字符在批处理程序中，选择列表中选择一个项，然后返回所选选项的索引。 如果使用不带参数，**选**将显示的默认选择**Y**并**N**。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <"Text">]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/c \<Choice1><Choice2><…>|指定要创建的选项的列表。 有效选项包括 a-z、 A-Z、 0-9 和扩展的 ASCII 字符 (128 254)。 默认列表是"YN"，将显示为`[Y,N]?`。|
|/n|选择仍处于启用状态虽然隐藏的选择，列表和消息文本 (如果指定，则 **/m**) 仍然会显示。|
|/cs|指定可用选项包括区分大小写。 默认情况下，不区分大小写选项。|
|/t \<Timeout>|指定要使用指定的默认选项之前暂停的秒数 **/d**。 可接受的值是从**0**到**9999**。 如果 **/t**设置为**0**，**选择**返回的默认选项之前不会暂停。|
|/d \<Choice>|指定等待指定的秒数之后，使用的默认选项 **/t**。 默认选择的由指定的选择列表中必须是 **/c**。|
|/m <"text">|指定要显示的选择列表之前的消息。 如果 **/m**未指定，则会显示仅选择提示。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   ERRORLEVEL 环境变量设置为用户从选项列表中选择的键的索引。 在列表中的第一个选择返回值为 1，第二个值为 2，依此类推。 如果用户按下并不是有效的选择，某个键**选择**听起来警告提示音。 如果**选择**检测到错误条件，它将返回的 ERRORLEVEL 值为 255。 如果用户按 CTRL + BREAK 或 CTRL + C**选择**返回的 ERRORLEVEL 值为 0。

> [!NOTE]
> 当在批处理程序中使用 ERRORLEVEL 的值时，以降序列出。

## <a name="BKMK_examples"></a>示例

若要显示选择 Y、 N 和 C，键入的批处理文件中的以下行：
```
choice /c ync
```
当批处理文件运行时，会出现以下提示**选择**命令：
```
[Y,N,C]?
```
若要隐藏选择 Y、 N 和 C，但显示文本"是、 否或继续"，在批处理文件中键入以下行：
```
choice /c ync /n /m "Yes, No, or Continue?"
```
当批处理文件运行时，会出现以下提示**选择**命令：
```
Yes, No, or Continue?
```

> [!NOTE]
> 如果您使用 **/n**参数，但不是使用 **/m**，该用户不是出现提示时**选择**正在等待输入。

若要显示的文本和上一示例中使用的选项，请在批处理文件中键入以下行：
```
choice /c ync /m "Yes, No, or Continue"
```
当批处理文件运行时，会出现以下提示**选择**命令：
```
Yes, No, or Continue [Y,N,C]?
```
若要设置为五秒的时间限制，并指定**N**作为默认值，在批处理文件中键入以下行：
```
choice /c ync /t 5 /d n
```
当批处理文件运行时，会出现以下提示**选择**命令：
```
[Y,N,C]?
```

> [!NOTE]
> 在此示例中，如果用户在 5 秒内未按某个键**选**选择**N**默认情况下，并返回错误值 2。 否则为**选择**返回与用户的选择相对应的值。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)