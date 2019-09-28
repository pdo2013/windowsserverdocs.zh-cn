---
title: 脱机磁盘
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f28d473cdb557d6adb3aaf235bebdfbc4e78b24a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372597"
---
# <a name="offline-disk"></a>脱机磁盘



使联机磁盘具有焦点，使其进入脱机状态。

> [!IMPORTANT]
> 在任何版本的 Windows Vista 中，此 DiskPart 命令均不可用。

## <a name="syntax"></a>语法

```
offline disk [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   此命令对处于 SAN online 模式下的磁盘运行。 它将 SAN 模式更改为脱机模式。
-   如果磁盘组中的动态磁盘处于脱机状态，则磁盘的状态将更改为 "**丢失**"，组将显示处于脱机状态的磁盘。 缺少的磁盘将被移动到无效组。 如果动态磁盘是组中的最后一个磁盘，则磁盘的状态将更改为**脱机**，并且将删除空组。
-   必须选择磁盘，才能成功执行**脱机磁盘**命令。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要使具有焦点的磁盘脱机，请键入：
```
offline disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

