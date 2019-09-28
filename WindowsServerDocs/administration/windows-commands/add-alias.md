---
title: 添加别名
description: '**添加别名**的 Windows 命令主题-将别名添加到别名环境。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f2834376e497f54eadf1d9077e74f9c398202c5a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382824"
---
# <a name="add-alias"></a>添加别名



向别名环境添加别名。 如果在没有参数的情况下使用，则 "**添加别名**" 会在命令提示符下显示帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<AliasName >|指定别名的名称。|
|\<AliasValue >|指定别名的值。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   别名保存在元数据文件中，并将通过**加载元数据**命令加载。

## <a name="BKMK_examples"></a>示例

若要列出所有阴影（包括其别名），请键入：
```
list shadows all
```
以下摘录显示了已分配默认别名 VSS_SHADOW_x 的卷影副本：
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
若要将名为 System1 的新别名分配到此卷影副本，请键入：
```
add alias System1 %VSS_SHADOW_1%
```
或者，您可以使用卷影副本 ID 指定别名：
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)