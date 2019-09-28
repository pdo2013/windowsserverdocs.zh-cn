---
title: 注册删除
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7156bf58b27da1602931f0dc1903de71d86764e7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384759"
---
# <a name="reg-delete"></a>注册删除



删除注册表中的一个或一些项。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName >|指定要删除的子项或项的完整路径。 若要指定远程计算机，请包含计算机名称（格式为 \\ @ no__t-1ComputerName @ no__t-2 作为*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 将使操作默认为本地计算机。 *KeyName*必须包含有效的根密钥。 本地计算机的有效根密钥如下：HKLM、HKCU、HKCR、HKU 开头和 HKCC。 如果指定了远程计算机，则有效的根密钥为：HKLM 和 HKU 开头。|
|/v \<ValueName >|删除子项下的特定项。 如果未指定任何项，则将删除子项下的所有项和子项。|
|/ve|指定仅删除没有值的条目。|
|/va|删除指定子项下的所有条目。 不会删除指定子项下的子项。|
|/f|删除现有的注册表子项或条目，而不要求确认。|
|/?|在命令提示符下显示**reg delete**的帮助。|

## <a name="remarks"></a>备注

下表列出了**reg delete**操作的返回值。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要删除注册表项超时及其所有子项和值，请键入：
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
若要在名为天干地支的计算机上的 HKLM\Software\MyCo 下删除注册表值 MTU，请键入：
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)