---
title: 模拟还原
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 09b626939c13d4e38a983435b45d8c47ee2b93a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817248"
---
# <a name="simulate-restore"></a>模拟还原



在计算机上的还原会话而无需发出测试编写器参与**PreRestore**或**PostRestore**到编写器的事件。

## <a name="syntax"></a>语法

```
simulate restore
```

## <a name="remarks"></a>备注

-   **模拟还原**用于测试是否可以成功还原与编写器。
-   可以使用之前**模拟还原**，则必须使用加载 DiskShadow 元数据文件**加载元数据**命令。 这将加载所选编写器和还原的组件。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)