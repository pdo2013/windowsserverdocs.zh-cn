---
title: wmic
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e9840bc20ddf6193241fe36055698e2bd3222496
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361885"
---
# <a name="wmic"></a>wmic



显示交互式命令行界面中的 WMI 信息。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
command </parameter>
```

## <a name="sub-commands"></a>子命令

以下子命令始终可用：

|子命令|描述|
|-----------|-----------|
|类|从 WMIC 的默认别名模式转义以直接访问 WMI 架构中的类。|
|path|从 WMIC 的默认别名模式进行转义，以直接访问 WMI 架构中的实例。|
|context|显示所有全局开关的当前值。|
|[quit \| exit]|退出 WMIC 命令行界面。|

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|</parameter>|@no__t 0Concise 说明，以谓词开头。 >|
|</param2>|@no__t 0Another 简明说明，以动词开头。 >|


## <a name="BKMK_examples"></a>示例

若要显示所有全局开关的当前值，请键入：
```
wmic context
```
类似于以下内容的输出：
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
若要将命令行使用的语言 ID 更改为英语（区域设置 ID 409），请键入：
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)