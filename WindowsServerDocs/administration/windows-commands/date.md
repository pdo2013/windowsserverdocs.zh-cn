---
title: date
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e774f7bfabb9b574255691dd97d2cfff36f034e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877948"
---
# <a name="date"></a>date



显示或设置系统日期。 如果使用不带参数，**日期**显示当前系统日期设置，并提示你输入新的日期。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Month-Day-Year>|设置日期的指定，其中*月*为 month （一个或两个数字），*天*为天 （一个或两个数字） 和*年*是年份 （两个或四个数字）。|
|/t|显示当前日期而不提示你输入新的日期。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要更改当前日期，必须具有管理凭据。
-   必须分隔为值*月*，*天*，并*年*使用句点 （.）、 连字符 （-） 或斜杠标记 （/）。
-   有效*月*的值为 1 到 12。
-   有效*天*的值为 1 到 31 之间。
-   有效*年*值为 00 到 99，或者通过 2099年 1980年。 如果使用两个数字，80 到 99 之间的值对应于 1980 通过 1999年年。

## <a name="BKMK_examples"></a>示例

如果启用了命令扩展，若要显示当前系统日期，键入：
```
date /t
```
若要将当前系统日期更改为 2007 年 8 月 3 日，可以键入以下任一：
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
若要显示当前系统日期，在提示符下输入新的日期，然后键入：
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
若要保留当前日期并返回到命令提示符下，按 ENTER。 若要更改当前日期，键入新的日期，然后按 ENTER。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)