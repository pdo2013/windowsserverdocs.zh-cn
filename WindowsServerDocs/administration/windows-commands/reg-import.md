---
title: reg 导入
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d1dd1b61848671b528c62fd22fe656e14fda7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861838"
---
# <a name="reg-import"></a>reg 导入



包含的文件的内容复制到本地计算机的注册表导出注册表子项、 条目和值。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Reg import FileName
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<FileName>|指定的名称和具有内容要复制到本地计算机的注册表文件的路径。 通过使用必须提前创建此文件**reg 导出**。|
|/?|显示的帮助**reg 导入**在命令提示符处。|

## <a name="remarks"></a>备注

下表列出的返回值**reg 导入**操作。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要从名为 AppBkUp.reg 的文件导入注册表项，请键入：
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)