---
title: reg 还原
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0025a37ed8ca50b47e7750501a7362659b500537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858768"
---
# <a name="reg-restore"></a>reg 还原



将保存子项和项备份到注册表。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName>|指定要还原的子项的完整路径。 还原操作仅适用于本地计算机。 KeyName 必须包含有效的根键。 有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。|
|\<FileName>|使用内容写入到注册表中指定的名称和文件的路径。 此文件必须使用预先创建**reg 保存**使用.hiv 扩展的操作。|
|/?|显示的帮助**reg 还原**在命令提示符处。|

## <a name="remarks"></a>备注

-   在编辑任何注册表项，保存与父子项**reg 保存**操作。 如果编辑失败，还原与原始的子项**reg 还原**操作。
-   下表列出的返回值**reg 还原**操作。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要还原到密钥 HKLM\Software\Microsoft\ResKit，名为 NTRKBkUp.hiv 的文件并覆盖现有项的内容，请键入：
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)