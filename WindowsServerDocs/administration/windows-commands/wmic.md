---
title: wmic
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c68866fbe0c8f5b16dae77e2121331f06cdc726
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885838"
---
# <a name="wmic"></a>wmic



显示 WMI 内部的交互式命令行界面的信息。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
command </parameter>
```

## <a name="sub-commands"></a>子命令

以下子命令在任何时候都可用：

|子命令|描述|
|-----------|-----------|
|类|从 WMIC 直接访问 WMI 架构中的类的默认别名模式的转义符。|
|path|从 WMIC 直接访问 WMI 架构中的实例的默认别名模式的转义符。|
|上下文|显示所有全局开关的当前值。|
|[退出\|退出]|退出 WMIC 命令行界面。|

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|</parameter>|\<简单说明开头动词。 >|
|</param2>|\<另一个简明描述开头动词。 >|


## <a name="BKMK_examples"></a>示例

若要显示的所有全局开关的当前值，请键入：
```
wmic context
```
类似于显示以下输出：
```
NAMESPACE    : root\cimv2
ROLE         : root\cli
NODE(S)      : BOBENTERPRISE
IMPLEVEL     : IMPERSONATE
[AUTHORITY   : N/A]
AUTHLEVEL    : PKTPRIVACY
LOCALE       : ms_409
PRIVILEGES   : ENABLE
TRACE        : OFF
RECORD       : N/A
INTERACTIVE  : OFF
FAILFAST     : OFF
OUTPUT       : STDOUT
APPEND       : STDOUT
USER         : N/A
AGGREGATE    : ON
```
若要更改的语言 ID 使用命令行对英语 （区域设置 ID 409），类型：
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)