---
title: set_2
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f5523646fddbfec31cb3900fc09230efc1c7813
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862608"
---
# <a name="set2"></a>set_2



设置上下文、 选项、 详细模式下，和卷影副本创建的元数据文件。 如果使用不带参数，**设置**列出了所有当前设置。

## <a name="syntax"></a>语法

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>设置子命令

|子命令|描述|
|-----------|-----------|
|上下文|设置卷影副本创建的上下文。 请参阅[设置上下文](set-context.md)语法和参数。|
|选项|设置创建卷影副本的选项。 请参阅[设置选项](set-option.md)语法和参数。|
|详细|打开或关闭，请启用详细输出模式。 请参阅[设置详细](set-verbose.md)语法和参数。|
|元数据|设置名称和卷影创建元数据文件的位置。 请参阅[设置元数据](set-metadata.md)语法和参数。|
|/?|在命令提示符下显示帮助。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)