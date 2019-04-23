---
title: 加载元数据
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b52b5040fc8c834b04cad83ca4b0cfab103fdc43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871328"
---
# <a name="load-metadata"></a>加载元数据



加载元数据.cab 文件导入可传送卷影副本之前，或加载在还原过程中的编写器元数据。 如果使用不带参数，**加载元数据**在命令提示符下显示的帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:][<Path>]|指定的元数据文件的位置。|
|MetaData.cab|指定要加载的元数据.cab 文件。|

## <a name="remarks"></a>备注

-   可以使用**导入**命令来导入可传送卷影副本基于指定的元数据**加载元数据**。
-   此命令需要之前**开始还原**命令加载所选编写器和还原组件。

## <a name="BKMK_examples"></a>示例

若要加载的默认位置名 metafile.cab 的元数据文件，请键入：
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)