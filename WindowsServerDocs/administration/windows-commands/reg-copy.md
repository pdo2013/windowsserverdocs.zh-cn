---
title: reg 复制
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11cbfce39433ea18632bec16c524da191def2a07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867558"
---
# <a name="reg-copy"></a>reg 复制



将注册表项复制到本地或远程计算机上的指定位置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName1>|指定要复制的子项的完整路径。 若要指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。|
|\<KeyName2>|指定子项的目标的完整的路径。 若要指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。|
|/s|将复制所有子项和注册表项下指定的子项。|
|/f|将子项复制而不提示确认。|
|/?|显示的帮助**reg**在命令提示符下复制。|

## <a name="remarks"></a>备注

-   复制一个子项时 Reg 不要求进行确认。
-   下表列出的返回值**reg 复制**操作。

|值|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要将所有子项和项 MyApp 下的值都复制到项 SaveMyApp 中，键入：
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
若要复制到当前计算机上的项 MyCo1 命名 ZODIAC 的计算机上的键 MyCo 下的所有值，请键入：
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)