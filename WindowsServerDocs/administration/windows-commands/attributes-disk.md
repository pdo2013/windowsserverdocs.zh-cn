---
title: 属性磁盘
description: Windows 命令主题**属性磁盘**-显示、 设置或清除的磁盘的属性。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7cacc2fb6b47d095f5e452ca470c89f228949594
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890348"
---
# <a name="attributes-disk"></a>属性磁盘



显示、 设置，或清除的磁盘的属性。

> [!IMPORTANT]
> 此参数不在任何版本的 Windows Vista 中可用。

## <a name="syntax"></a>语法

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|设置|设置具有焦点的磁盘的指定的特性。|
|clear|清除选中的磁盘的指定的属性。|
|只读|指定该磁盘是只读的。|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   当**属性磁盘**是用来显示当前磁盘的属性，启动磁盘属性表示用于启动计算机的磁盘。 对于动态镜像，它会显示包含启动丛的启动卷的磁盘。
-   磁盘必须为所选**属性磁盘**命令才会成功。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要查看所选的磁盘的属性，请键入：
```
attributes disk
```
若要将所选的磁盘设置为只读的请键入：
```
attributes disk set readonly
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

