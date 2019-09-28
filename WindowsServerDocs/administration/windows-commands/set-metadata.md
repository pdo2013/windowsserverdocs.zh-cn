---
title: 设置元数据
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac73a4131d3f4065cd1aeae873734b079ad664e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370952"
---
# <a name="set-metadata"></a>设置元数据



设置用于将卷影副本从一台计算机传输到另一台计算机的卷影创建元数据文件的名称和位置。 如果在没有参数的情况下使用，则**设置元数据将**在命令提示符下显示帮助。

## <a name="syntax"></a>语法

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[@no__t >：][<Path>]|指定元数据文件的创建位置。|
|\<MetaData >|指定用于存储卷影创建元数据的 cab 文件的名称。|

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)