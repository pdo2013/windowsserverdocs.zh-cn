---
title: color
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e1ee9b7c1ade184cf17b867b7fce10ab8885f0f
ms.sourcegitcommit: 4ff3d00df3148e4bea08056cea9f1c3b52086e5d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64771890"
---
# <a name="color"></a>color



更改前景色和背景颜色在命令提示符窗口中当前会话。 如果使用不带参数，**颜色**还原默认命令提示符窗口中的前景色和背景颜色。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
color [[<B>]<F>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<B>|指定背景色。|
|\<F>|指定前景色。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   下表列出了可以使用的值的形式的有效十六进制数字*B*并*F*。

|ReplTest1|颜色|
|-----|-----|
|0|黑色|
|1|蓝色|
|2|厚︹|
|3|浅绿色|
|4|红色|
|5|紫色|
|6|独︹|
|7|白色|
|8|灰色|
|9|浅蓝色|
|A|浅绿色|
|B|淡浅绿色|
|C|浅红色|
|D|淡紫色|
|E|淡黄色|
|F|亮白色|
    
-   不使用之间的空格字符*B*并*F*。
-   如果指定只有一个十六进制数字，对应的颜色用作的前景色和背景色设置为默认颜色。
-   若要设置的默认命令提示符窗口颜色，请单击命令提示符窗口的左上角，单击**默认值**，单击**颜色**卡，并单击你想要用于的颜色**屏幕上文本**并**屏幕上背景**。
-   如果*B*并*F*相同，**颜色**命令设置 ERRORLEVEL 为 1，并且为前景色或背景色进行任何更改。

## <a name="BKMK_examples"></a>示例

若要更改的命令提示符窗口背景色为灰色，为红色的前景颜色，请键入：
```
color 84
```
若要更改为淡黄色的命令提示符窗口前景色，请键入：
```
color e
```

> [!NOTE]
> 在此示例中，在后台因为只有一个十六进制数字指定为默认颜色设置。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
