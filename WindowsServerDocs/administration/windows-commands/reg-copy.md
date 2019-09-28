---
title: 注册副本
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a82b17b631d4242fa6affdec0ff67b5b09380550
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371780"
---
# <a name="reg-copy"></a>注册副本



将注册表项复制到本地或远程计算机上的指定位置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName1 >|指定要复制的子项的完整路径。 若要指定远程计算机，请包含计算机名称（格式为 \\ @ no__t-1ComputerName @ no__t-2 作为*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 将使操作默认为本地计算机。 *KeyName*必须包含有效的根密钥。 本地计算机的有效根密钥如下：HKLM、HKCU、HKCR、HKU 开头和 HKCC。 如果指定了远程计算机，则有效的根密钥为：HKLM 和 HKU 开头。|
|\<KeyName2 >|指定子项目标的完整路径。 若要指定远程计算机，请包含计算机名称（格式为 \\ @ no__t-1ComputerName @ no__t-2 作为*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 将使操作默认为本地计算机。 *KeyName*必须包含有效的根密钥。 本地计算机的有效根密钥如下：HKLM、HKCU、HKCR、HKU 开头和 HKCC。 如果指定了远程计算机，则有效的根密钥为：HKLM 和 HKU 开头。|
|/s|复制指定子项下的所有子项和项。|
|/f|复制子项，而不提示确认。|
|/?|在命令提示符下显示**reg** copy 的帮助。|

## <a name="remarks"></a>备注

-   Reg 在复制子项时不要求确认。
-   下表列出了**reg copy**操作的返回值。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要将项 MyApp 下的所有子项和值复制到 key SaveMyApp，请键入：
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
若要将名为天干/地支的计算机上的 MyCo 项下的所有值复制到当前计算机上的键 MyCo1，请键入：
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)