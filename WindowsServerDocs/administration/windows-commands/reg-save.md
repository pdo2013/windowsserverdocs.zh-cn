---
title: 保存注册表
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a46dfe081421ed727bd7ffeeab364e6c23dd801
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841078"
---
# <a name="reg-save"></a>保存注册表



将指定的子项、 条目和注册表值的副本保存在指定的文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName>|指定的子项的完整路径。 用于指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。|
|\<FileName>|指定的名称和创建文件的路径。 如果未指定路径，则使用当前路径。|
|/y|覆盖现有文件具有名称*文件名*而不提示确认。|
|/?|显示的帮助**reg 保存**在命令提示符处。|

## <a name="remarks-optional-section"></a>备注\<可选部分 >

-   下表列出的返回值**reg 保存**操作。

|值|Description|
|-----|-----------|
|0|成功|
|1|失败|
-   在编辑任何注册表项，保存与父子项**reg 保存**操作。 如果编辑失败，还原与原始的子项**reg 还原**操作。

## <a name="BKMK_examples"></a>示例

若要作为一个名为 AppBkUp.hiv 文件保存到当前文件夹的 MyApp hive，请键入：
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)