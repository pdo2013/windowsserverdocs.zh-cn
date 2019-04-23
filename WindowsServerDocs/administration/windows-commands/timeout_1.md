---
title: timeout
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3997399b732c494050797c83a0a52938574986bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830168"
---
# <a name="timeout"></a>timeout



暂停指定的秒数的命令处理器。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/t \<TimeoutInSeconds>|指定十进制 （-1 到 99999） 之间的秒数后的命令处理器将继续进行处理之前要等待。 值-1 将导致无限期地等待键击计算机。|
|/nobreak|指定要忽略用户键击。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **超时**命令通常用于在批处理文件中。
-   用户击键恢复命令处理器执行立即，即使未过期的超时期限。
-   当结合使用时**睡眠**命令，**超时**类似于**暂停**命令。

## <a name="BKMK_examples"></a>示例

若要为十秒暂停命令处理器，请键入：
```
timeout /t 10
```
若要暂停 100 秒命令处理器并忽略任何击键，键入：
```
timeout /t 100 /nobreak
```
若要无限期地直到按下某个键暂停命令处理器，请键入：
```
timeout /t -1
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
