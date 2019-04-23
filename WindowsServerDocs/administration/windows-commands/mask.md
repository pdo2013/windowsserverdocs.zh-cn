---
title: 掩码
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 353e6080d1f6c548bc907b58655f31d0bce6de8b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858018"
---
# <a name="mask"></a>掩码



删除已导入使用的硬件卷影副本**导入**命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
mask <ShadowSetID>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|ShadowSetID|删除卷影副本属于指定的卷影副本设置的 id。|

## <a name="remarks"></a>备注

-   可以使用现有别名或环境变量来代替*ShadowSetID*。 使用**添加**不带参数，以查看现有别名。

## <a name="BKMK_examples"></a>示例

若要删除导入的卷影副本 %import_1%，请键入：
```
mask %Import_1%
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)