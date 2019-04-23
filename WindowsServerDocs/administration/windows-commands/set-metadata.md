---
title: 设置元数据
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 82d2cd9ba447a0ea261f91dc01c11e45dfc0aa9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835988"
---
# <a name="set-metadata"></a>设置元数据



设置名称和用于从一台计算机的卷影副本传输到另一个卷影创建元数据文件的位置。 如果使用不带参数，**设置元数据**在命令提示符下显示的帮助。

## <a name="syntax"></a>语法

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:][<Path>]|指定要创建的元数据文件的位置。|
|\<MetaData.cab>|指定要存储卷影创建元数据的 cab 文件的名称。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)