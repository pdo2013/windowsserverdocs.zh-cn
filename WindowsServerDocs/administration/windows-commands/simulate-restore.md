---
title: 模拟还原
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6652fea4e74c706fcc03b8a547fab771a7c0191
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370881"
---
# <a name="simulate-restore"></a>模拟还原



测试编写器参与计算机上的还原会话，而不向编写器发出**PreRestore**或**PostRestore**事件。

## <a name="syntax"></a>语法

```
simulate restore
```

## <a name="remarks"></a>备注

-   **模拟还原**用于测试编写器的还原是否成功。
-   在可以使用 "**模拟还原**" 之前，必须使用**加载元数据**命令加载 DiskShadow 元数据文件。 这将加载所选的编写器和组件以进行还原。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)