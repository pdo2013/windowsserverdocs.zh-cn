---
title: 添加别名
description: Windows 命令主题**将别名添加**-将别名添加到别名环境。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50de932ea0153546816face61f0852a08707ea85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862218"
---
# <a name="add-alias"></a>添加别名



将别名添加到别名环境。 如果使用不带参数，**将别名添加**在命令提示符下显示的帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<AliasName >|指定别名的名称。|
|\<AliasValue>|指定值的别名值。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   别名都保存在元数据文件，并且将会加载与**加载元数据**命令。

## <a name="BKMK_examples"></a>示例

若要列出所有阴影，包括各自的别名，请键入：
```
list shadows all
```
以下摘录显示了已向其分配的默认别名，VSS_SHADOW_x，卷影副本：
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
若要将新的别名名称系统 1 分配到此卷影副本，请键入：
```
add alias System1 %VSS_SHADOW_1%
```
或者，您可以通过使用卷影副本 ID 分配别名：
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)