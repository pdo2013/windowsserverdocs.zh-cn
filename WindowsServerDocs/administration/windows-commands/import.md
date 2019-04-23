---
title: import
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddef3958bc431519e3cb89b658a58d1f4dba6938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835258"
---
# <a name="import"></a>import



将可传送卷影副本加载元数据文件中导入到系统。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
import
```

## <a name="remarks"></a>备注

-   可传送卷影副本是不立即存储在系统中。 在备份组件文档 XML 文件，DiskShadow 会自动请求，并将保存在工作目录中的.cab 元数据文件中存储其详细信息。 您可以更改的路径和名称的此文件使用**设置元数据**命令。
-   可以使用之前**导入**，必须加载 DiskShadow 元数据文件使用**加载元数据**命令。

## <a name="BKMK_examples"></a>示例

下面是演示如何使用一个示例 DiskShadow 脚本**导入**命令：
```
#Sample DiskShadow script demonstrating IMPORT
SET CONTEXT PERSISTENT
SET CONTEXT TRANSPORTABLE
SET METADATA transHWshadow_p.cab
#P: is the volume supported by the Hardware Shadow Copy provider
ADD VOLUME P:
CREATE
END BACKUP
#The (transportable) shadow copy is not in the system yet.
#You can reset or exit now if you wish.

LOAD METADATA transHWshadow_p.cab
IMPORT
#The shadow copy will now be loaded into the system.
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)