---
title: delete volume
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 35b22e1bfc6fbfca8ef7bd29bfe1b7e28d7d35d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378651"
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
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   你无法删除系统卷、启动卷或任何包含活动分页文件或崩溃转储（内存转储）的卷。
-   若要成功执行此操作，必须选择卷。 使用 "**选择音量**" 命令选择卷并将焦点移动到该卷。

## <a name="BKMK_examples"></a>示例

若要删除具有焦点的卷，请键入：
```
delete volume
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

