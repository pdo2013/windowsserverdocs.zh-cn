---
title: Reg 负载
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebc75ad78b7334f4d48a085f6870a443b31fa2a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852188"
---
# <a name="reg-load"></a>Reg 负载



写入操作保存在注册表子项和到不同的子项的项。 使用适用于进行故障排除或编辑注册表项中使用的临时文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg load KeyName FileName
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName>|指定要加载的子项的完整路径。 用于指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。|
|\<FileName>|指定的名称和要加载的文件的路径。 通过使用必须提前创建此文件**reg 保存**操作和.hiv 扩展。|
|/?|显示的帮助**reg 负载**在命令提示符处。|

## <a name="remarks"></a>备注

下表列出的返回值**reg 负载**操作。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要加载到密钥 HKLM\TempHive 命名 TempHive.hiv 文件，请键入：
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)