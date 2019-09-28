---
title: 脱机卷
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 71e507bde827233ce1f15aacd5e13523236a080e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372567"
---
# <a name="offline-volume"></a>脱机卷



使具有焦点的联机卷进入脱机状态。

> [!IMPORTANT]
> 在任何版本的 Windows Vista 中，此 DiskPart 命令均不可用。

## <a name="syntax"></a>语法

```
offline volume [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   若要成功，必须选择卷。 使用 "**选择卷**" 命令选择磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要使具有焦点的磁盘脱机，请键入：
```
offline volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

