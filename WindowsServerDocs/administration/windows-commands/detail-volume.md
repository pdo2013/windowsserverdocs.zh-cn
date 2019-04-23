---
title: 卷详细信息
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7cae6dc9b82992b58c4f94801f90c0b7072492b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856918"
---
# <a name="detail-volume"></a>卷详细信息



显示当前卷所驻留的磁盘。

## <a name="syntax"></a>语法

```
detail volume
```

## <a name="remarks"></a>备注

-   成功执行此操作，必须选择一个卷。 使用**选择卷**命令以选择一个卷并将焦点移到它。
-   卷详细信息不适用于只读卷，如 DVD ROM 或 CD-ROM 驱动器。

## <a name="BKMK_examples"></a>示例

若要查看当前卷所驻留的所有磁盘，请键入：
```
detail volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

