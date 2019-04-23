---
title: Reg delete
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 369ef3bda37ab8e143a14f0f9707b9bbf14bd5f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877078"
---
# <a name="reg-delete"></a>Reg delete



从注册表中删除项或子项。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName>|指定的子项或要删除的项的完整路径。 若要指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。|
|/v \<ValueName>|删除子项下的特定项。 如果不指定了任何项，然后将删除所有项和子项下的子项。|
|/ve|指定没有值的条目将被删除。|
|/va|删除指定子项下的所有条目。 不会删除指定的子项下的子项。|
|/f|而不要求确认删除现有注册表子项或条目。|
|/?|显示的帮助**reg 删除**在命令提示符处。|

## <a name="remarks"></a>备注

下表列出的返回值**reg 删除**操作。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要删除超时的注册表项及其所有子项和值，请键入：
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
若要删除名为 ZODIAC 的计算机上的注册表值下 HKLM\Software\MyCo MTU，请键入：
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)