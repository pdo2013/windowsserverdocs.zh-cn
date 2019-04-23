---
title: 时间
description: 了解如何设置和显示系统时间。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5c1f43be98a19c4b150c247cc7fd48d62edeb5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861908"
---
# <a name="time"></a>时间



显示或设置系统时间。 如果使用不带参数，**时间**显示当前系统时间，并提示你输入新的时间。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<HH > [:\<MM > [:\<SS > [。\<NN >]]] [是\|pm]|系统时间设置为指定，其中的新时间*HH*以小时为单位 （必需）、 *MM*是以分钟为单位，并且*SS*以秒为单位。 *NN*可用于指定的百分之几秒。 如果**正在**或**pm**未指定，则**时间**默认情况下使用 24 小时格式。|
|/t|而不提示你输入新时间显示的当前时间。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要更改当前时间，必须具有管理凭据。
-   必须分隔为值*HH*， *MM*，并*SS*用冒号 （:）。 *SS*并*NN*必须用句点 （.） 分隔。
-   有效*HH*的值为 0 和 24 之间。
-   有效*MM*并*SS*的值为 0 到 59 之间。

## <a name="BKMK_examples"></a>示例

如果启用了命令扩展，若要显示当前系统时间，键入：
```
time /t
```
若要更改当前系统时间为下午 5:30，键入以下任一项：
```
time 17:30:00
time 5:30 pm
```
若要显示当前系统时间，跟在提示符下输入新的时间，请键入：
```
The current time is: 17:33:31.35
Enter the new time:
```
若要保留当前时间，并返回到命令提示符下，按 ENTER。 若要更改当前时间，键入新的时间，然后按 ENTER。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)