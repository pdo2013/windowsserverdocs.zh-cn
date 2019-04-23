---
title: recover
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 261bfd79d74323ad324246e21b84a5eb798ebcdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867178"
---
# <a name="recover"></a>recover



刷新磁盘组中的所有磁盘的状态，尝试恢复在无效的磁盘组中，磁盘和重新同步镜像的卷和 raid-5 卷，包含陈旧的数据。

> [!IMPORTANT]
> 此 DiskPart 命令不可在任何版本的 Windows Vista 中可用。

## <a name="syntax"></a>语法

```
recover [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   此命令对磁盘组进行操作。
-   此命令仅适用于组的动态磁盘。 如果组使用基本磁盘上使用此命令，则它不会返回错误，但不会执行任何操作。
-   此命令运行失败的磁盘或失败。 它还会运行失败，失败，卷上或处于失败的冗余状态。
-   若要成功执行此命令，必须选择是磁盘组的一部分的磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要恢复包含具有焦点的磁盘的磁盘组，请键入：
```
recover
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

