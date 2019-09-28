---
title: 详细信息卷
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f8cd5889bfd2aea835cb64ef1a4076faee0f39b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378469"
---
# <a name="detail-volume"></a>详细信息卷



显示当前卷所在的磁盘。

## <a name="syntax"></a>语法

```
detail volume
```

## <a name="remarks"></a>备注

-   若要成功执行此操作，必须选择卷。 使用 "**选择音量**" 命令选择卷并将焦点移动到该卷。
-   卷详细信息不适用于只读卷，如 DVD-ROM 驱动器或 CD-ROM 驱动器。

## <a name="BKMK_examples"></a>示例

若要查看当前卷所在的所有磁盘，请键入：
```
detail volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

