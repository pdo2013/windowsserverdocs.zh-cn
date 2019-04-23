---
title: reset
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0bd9b6735697cbcefdcf68dc3d4a53a6870a7a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850958"
---
# <a name="reset"></a>reset



将 DiskShadow.exe 重置为默认状态。 **重置**尤其有用，如分离复合 DiskShadow operations**创建**，**导入**，**备份**，或**还原**.

## <a name="syntax"></a>语法

```
reset
```

## <a name="remarks"></a>备注

-   当你使用**重置**命令时，您会丢失状态命令如**添加**，**设置**，**加载**，或**编写器**. **重置**还释放 IVssBackupComponent 接口并失去是非持久卷影副本。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)