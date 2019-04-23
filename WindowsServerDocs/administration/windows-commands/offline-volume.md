---
title: 脱机卷
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd14492a40046472f43f37d79c393c9467fe4a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873738"
---
# <a name="offline-volume"></a>脱机卷



具有焦点的联机卷进入脱机状态。

> [!IMPORTANT]
> 此 DiskPart 命令不可在任何版本的 Windows Vista 中可用。

## <a name="syntax"></a>语法

```
offline volume [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   成功实现上述目的，必须选择一个卷。 使用**选择卷**命令选择某一磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要将选中的磁盘脱机，请键入：
```
offline volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

