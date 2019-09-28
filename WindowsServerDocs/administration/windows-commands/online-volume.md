---
title: 联机卷
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 06a3c81313180b2880c1e47c3b6c12236fda4245
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372518"
---
# <a name="online-volume"></a>联机卷



使当前处于脱机状态的卷处于联机状态

> [!IMPORTANT]
> 此命令在任何版本的 Windows Vista 中都不可用。

> [!IMPORTANT]
> 如果在只读卷上使用此命令，则此命令将失败。

## <a name="syntax"></a>语法

```
online volume [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   此命令对已失败、发生故障或处于失败冗余状态的卷进行操作。
-   若要成功执行此命令，必须选择卷。 使用 "**选择音量**" 命令选择卷并将焦点移动到该卷。

## <a name="BKMK_examples"></a>示例

若要将焦点置于联机状态，请键入：
```
online volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

