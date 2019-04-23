---
title: delete volume
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d25ed68077f594c765cf5630648ad52528d8fe3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872508"
---
# <a name="delete-volume"></a>delete volume



删除所选的卷。

## <a name="syntax"></a>语法

```
delete volume [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   你无法删除系统卷、启动卷或任何包含活动分页文件或崩溃转储（内存转储）的卷。
-   成功执行此操作，必须选择一个卷。 使用**选择卷**命令以选择一个卷并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要删除选中的卷，请键入：
```
delete volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

