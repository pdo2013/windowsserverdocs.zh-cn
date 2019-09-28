---
title: 删除阴影
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c965af8b045c5ab3a110542d148b255f382a95c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378626"
---
# <a name="delete-shadows"></a>删除阴影



删除卷影副本。

## <a name="syntax"></a>语法

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Parameters

|     参数     |                                                                             描述                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        全部        |                                                                      删除所有卷影副本。                                                                      |
| 卷 \<Volume >  |                                                            删除给定卷的所有卷影副本。                                                            |
| 最早 \<Volume >  |                                                         删除给定卷的最旧的卷影副本。                                                          |
|   设置 \<SetID >    | 删除具有给定 ID 的卷影副本集中的卷影副本。 如果当前环境中存在别名，则可以通过使用 **%** 符号指定别名。 |
|  id \<ShadowID >   |              删除给定 ID 的卷影副本。 如果当前环境中存在别名，则可以通过使用 **%** 符号指定别名。               |
| 公开 {@no__t 0Drive > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)