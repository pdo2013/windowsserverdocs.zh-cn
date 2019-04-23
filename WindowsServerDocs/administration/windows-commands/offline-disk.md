---
title: 脱机磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 617371583a3f0cb3d0cb739845208e4216573d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834618"
---
# <a name="offline-disk"></a>脱机磁盘



将具有焦点的联机磁盘带到脱机状态。

> [!IMPORTANT]
> 此 DiskPart 命令不可在任何版本的 Windows Vista 中可用。

## <a name="syntax"></a>语法

```
offline disk [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   此命令会在 SAN 联机模式下的磁盘上进行操作。 它会将其 SAN 模式更改为脱机。
-   如果磁盘组中的动态磁盘会进入脱机状态，磁盘的状态将更改为**缺少**和组显示磁盘处于脱机状态。 缺少磁盘移到无效的组。 如果动态磁盘是最后一个磁盘的组中，则磁盘的状态将更改为**脱机**，并且将删除空组。
-   磁盘必须为所选**脱机磁盘**命令才会成功。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要将选中的磁盘脱机，请键入：
```
offline disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

