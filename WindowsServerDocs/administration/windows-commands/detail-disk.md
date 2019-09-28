---
title: 详细信息磁盘
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff78a3f9e27cde35a7e19bdf1565c515a127261b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378584"
---
# <a name="detail-disk"></a>详细信息磁盘



显示所选磁盘的属性和该磁盘上的卷。

## <a name="syntax"></a>语法

```
detail disk
```

## <a name="remarks"></a>备注

-   若要成功执行此操作，必须选择磁盘。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。
-   如果所选磁盘是虚拟硬盘（VHD），**详细信息磁盘**会将磁盘的总线类型报告为 "虚拟"。

## <a name="BKMK_examples"></a>示例

若要查看所选磁盘的属性以及磁盘中的卷的相关信息，请键入：
```
detail disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

