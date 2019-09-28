---
title: 联机磁盘
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3d798bf34ec2f9d2f01b5470c4ec52f936674135
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372508"
---
# <a name="online-disk"></a>联机磁盘



将当前处于脱机状态的磁盘带入联机状态。

> [!IMPORTANT]
> 此命令在任何版本的 Windows Vista 中都不可用。

> [!IMPORTANT]
> 如果在只读磁盘上使用此命令，则此命令将失败。

有关如何使用此命令的说明，请参阅[重新激活丢失或脱机的动态磁盘](https://go.microsoft.com/fwlink/?LinkId=207046)（ https://go.microsoft.com/fwlink/?LinkId=207046) 。

## <a name="syntax"></a>语法

```
online disk [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   如果在 Windows Vista 中不带参数使用，则此命令将对磁盘组进行操作。 对于基本磁盘，每个组不会有多个磁盘。 对于动态磁盘，该组包括所有非外部动态磁盘。
-   对于基本磁盘，此命令将尝试使所选磁盘和该磁盘上的所有卷联机。
-   对于动态磁盘，此命令会尝试将未标记为 "外部" 的所有磁盘都置于本地计算机上。 它还将尝试使动态磁盘集上的所有卷联机。
-   如果磁盘组中的动态磁盘处于联机状态，并且它是组中的唯一磁盘，则会重新创建原始组，并将磁盘移到该组。 如果组中有其他磁盘并且它们处于联机状态，则磁盘只会重新添加到组中。
-   如果选定磁盘的组包含镜像卷或 RAID-5 卷，此命令还会重新同步这些卷。
-   必须选择磁盘才能使此命令成功。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要使具有焦点的磁盘联机，请键入：
```
online disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

