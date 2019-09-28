---
title: reset
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8903c300d12a019f8fb4aef6d367131a195d034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371458"
---
# <a name="reset"></a>reset



将 DiskShadow 重置为默认状态。 在分隔复合 DiskShadow 操作（如**创建**、**导入**、**备份**或**还原**）时，**重置**特别有用。

## <a name="syntax"></a>语法

```
reset
```

## <a name="remarks"></a>备注

-   使用**reset**命令时，**将**会丢失诸如 add、 **set**、 **load**或**writer**这样的命令的状态。 **Reset**还会释放 IVssBackupComponent 接口，并丢失非持久卷影副本。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)