---
title: 联机磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c30d0853ff0ae065f02c0ee198c8cdcb90c950b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858058"
---
# <a name="online-disk"></a>联机磁盘



将处于脱机状态为联机状态的磁盘。

> [!IMPORTANT]
> 此命令不可在任何版本的 Windows Vista 中可用。

> [!IMPORTANT]
> 如果使用只读的磁盘上，此命令将失败。

有关如何使用此命令的说明，请参阅[重新激活丢失或脱机动态磁盘](https://go.microsoft.com/fwlink/?LinkId=207046)(https://go.microsoft.com/fwlink/?LinkId=207046)。

## <a name="syntax"></a>语法

```
online disk [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   如果不使用参数在 Windows Vista 中，此命令对磁盘组进行操作。 对于基本磁盘，是永远不会每个组的多个磁盘。 对于动态磁盘，组包括所有非外部动态磁盘。
-   对于基本磁盘，此命令将尝试联机所选的磁盘和所有卷的磁盘上。
-   对于动态磁盘，此命令将尝试使未标记为本地计算机上外的所有磁盘都处于联机状态。 它将尝试进行的动态磁盘集上联机的所有卷。
-   如果磁盘组中的动态磁盘联机，并且它是在组中唯一的磁盘，然后重新创建原始组，并且该磁盘移动到该组。 如果组中的其他磁盘，并且它们都处于联机状态，然后磁盘只需添加回组中。
-   如果所选磁盘的组包含镜像卷或 raid-5 卷，则此命令还重新同步这些卷。
-   若要成功执行此命令，必须选择一个磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要使联机选中的磁盘，请键入：
```
online disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

