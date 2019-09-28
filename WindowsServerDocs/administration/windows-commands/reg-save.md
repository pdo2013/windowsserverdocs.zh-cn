---
title: 注册保存
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6ae07cd3c90c51e7bd494bc6c35919680cde912a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371704"
---
# <a name="reg-save"></a>注册保存



在指定的文件中保存指定子项、项和注册表值的副本。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName >|指定子项的完整路径。 对于指定远程计算机，请包含计算机名称（格式为 \\ @ no__t-1ComputerName @ no__t-2 作为*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 将使操作默认为本地计算机。 *KeyName*必须包含有效的根密钥。 本地计算机的有效根密钥如下：HKLM、HKCU、HKCR、HKU 开头和 HKCC。 如果指定了远程计算机，则有效的根密钥为：HKLM 和 HKU 开头。|
|\<文件名 >|指定创建的文件的名称和路径。 如果未指定路径，则使用当前路径。|
|/y|使用名称*文件名*覆盖现有文件，而不提示确认。|
|/?|在命令提示符下显示**reg save**的帮助。|

## <a name="remarks-optional-section"></a>备注 @no__t 0optional 部分 >

-   下表列出了**reg save**操作的返回值。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|
-   在编辑任何注册表项之前，请用**reg save**操作保存父子项。 如果编辑失败，请将原始子项还原为**reg restore**操作。

## <a name="BKMK_examples"></a>示例

要将 hive MyApp 保存到当前文件夹中作为名为 AppBkUp 的文件，请键入：
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)