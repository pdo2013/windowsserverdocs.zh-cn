---
title: 掩盖
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2f0dc83d7d9f7204f56e95c62b7cfad991f539ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373711"
---
# <a name="mask"></a>掩盖



删除使用**import**命令导入的硬件卷影副本。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
mask <ShadowSetID>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|ShadowSetID|删除属于指定卷影副本集 ID 的卷影副本。|

## <a name="remarks"></a>备注

-   您可以使用现有的别名或环境变量来代替*ShadowSetID*。 使用**add**而不使用参数查看现有别名。

## <a name="BKMK_examples"></a>示例

若要删除导入的卷影副本% Import_1%，请键入：
```
mask %Import_1%
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)