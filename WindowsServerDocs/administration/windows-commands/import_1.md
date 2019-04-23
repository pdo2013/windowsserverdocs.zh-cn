---
title: import
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 379d5923a9355db2965b56c27cedd207b1b63006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885988"
---
# <a name="import"></a>import



将外部磁盘组导入到本地计算机的磁盘组。

## <a name="syntax"></a>语法

```
import [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   导入命令导入与选中的磁盘位于同一组中的每个磁盘。
-   若要成功执行此操作，必须选择外部磁盘组中的动态磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要导入到本地计算机的磁盘组选中的磁盘所在的磁盘组中的每个磁盘，请键入：
```
import
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

