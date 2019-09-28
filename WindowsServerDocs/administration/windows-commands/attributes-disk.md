---
title: 属性磁盘
description: 用于 "磁盘"**属性**的 Windows 命令主题-显示、设置或清除磁盘的属性。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 415125208b13d82adeed736107f59fda9489a953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382567"
---
# <a name="attributes-disk"></a>属性磁盘



显示、设置或清除磁盘的属性。

> [!IMPORTANT]
> 此参数在任何版本的 Windows Vista 中都不可用。

## <a name="syntax"></a>语法

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|设置|设置具有焦点的磁盘的指定属性。|
|clear|清除具有焦点的磁盘的指定属性。|
|只读|指定磁盘为只读。|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   当**属性 "磁盘**" 用于显示磁盘的当前属性时，"启动磁盘" 属性表示用于启动计算机的磁盘。 对于动态镜像，将为包含启动卷的启动丛的磁盘显示。
-   必须选择磁盘，"属性"**磁盘**命令才能成功。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要查看所选磁盘的属性，请键入：
```
attributes disk
```
若要将所选磁盘设置为只读，请键入：
```
attributes disk set readonly
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

