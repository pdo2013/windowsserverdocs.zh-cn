---
title: reg 导出
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7aeddb4b069b1baf5b8f7aaea2730a2b25bdad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889648"
---
# <a name="reg-export"></a>reg 导出



将指定的子项、 条目和值的本地计算机到传输的文件复制到其他服务器。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName>|指定的子项的完整路径。 导出操作仅适用于本地计算机。 KeyName 必须包含有效的根键。 有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。|
|\<FileName>|指定的名称和要在操作期间创建的文件的路径。 该文件必须具有.reg 扩展名。|
|/y|覆盖任何现有的文件具有名称*文件名*而不提示确认。|
|/?|显示的帮助**reg 导出**在命令提示符处。|

## <a name="remarks"></a>备注

下表列出的返回值**reg 导出**操作。

|值|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要将所有子项的内容和值的 MyApp 的密钥导出到文件 AppBkUp.reg 中，键入：
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)