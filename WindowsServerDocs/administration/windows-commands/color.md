---
title: color
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ed792e4626897945e688f1c54767d7680ade6d99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379237"
---
# <a name="color"></a>color



更改当前会话的命令提示符窗口中的前景色和背景色。 如果在没有参数的情况下使用，则**color**会还原默认的 "命令提示符" 窗口前景色和背景色。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
color [[<B>]<F>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<B >|指定背景色。|
|\<F >|指定前景色。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   下表列出了可以使用的值的形式的有效十六进制数字*B*并*F*。

|ReplTest1|颜色|
|-----|-----|
|0|黑色|
|1|蓝色|
|2|厚︹|
|3|水|
|4|红色|
|5|紫|
|6|独︹|
|7|白色|
|8|灰色|
|9|浅蓝色|
|A|浅绿色|
|B|浅浅绿色|
|C|浅红色|
|D|浅紫色|
|E|浅黄色|
|F|亮白色|
    
-   不要在*B*和*F*之间使用空格字符。
-   如果只指定一个十六进制数字，则将使用相应的颜色作为前景色，并将背景色设置为默认颜色。
-   若要设置默认的 "命令提示符" 窗口颜色，请单击 "命令提示符" 窗口的左上角，单击 "**默认值**"，单击 "**颜色**" 选项卡，然后单击要用于**屏幕文本**和**屏幕背景的颜色。** .
-   如果*B*和*F*相同，则**color**命令会将 ERRORLEVEL 设置为1，并且不会对前景或背景色进行任何更改。

## <a name="BKMK_examples"></a>示例

若要将 "命令提示符" 窗口的背景色更改为灰色，将前景色更改为红色，请键入：
```
color 84
```
若要将 "命令提示符" 窗口前景色更改为浅黄色，请键入：
```
color e
```

> [!NOTE]
> 在此示例中，背景设置为默认颜色，因为只指定了一个十六进制数字。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
