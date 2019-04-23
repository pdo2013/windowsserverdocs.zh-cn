---
title: 联机卷
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bacba1e204f1eee2e3d4772ff9024aedbfc4fed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881998"
---
# <a name="online-volume"></a>联机卷



使卷处于脱机状态为联机状态

> [!IMPORTANT]
> 此命令不可在任何版本的 Windows Vista 中可用。

> [!IMPORTANT]
> 如果使用只读卷上，此命令将失败。

## <a name="syntax"></a>语法

```
online volume [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   此命令已失败、 失败，或处于失败的冗余状态的卷上进行操作。
-   若要成功执行此命令，必须选择卷。 使用**选择卷**命令以选择一个卷并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要使联机选中的卷，请键入：
```
online volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

