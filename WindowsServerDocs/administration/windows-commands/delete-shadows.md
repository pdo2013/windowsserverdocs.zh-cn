---
title: 删除阴影
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1a0945477bc4fce907b5ec4a697c7a2ec2f59557
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436116"
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
| 卷\<卷 >  |                                                            删除给定卷的所有卷影副本。                                                            |
| 最早\<卷 >  |                                                         删除给定卷的最旧的卷影副本。                                                          |
|   set \<SetID>    | 删除卷影副本中的卷影副本集的给定的 id。 您可以通过使用指定了别名 **%** 符号如果别名存在于当前环境中。 |
|  id \<ShadowID >   |              删除卷影副本的给定的 id。 您可以通过使用指定了别名 **%** 符号如果别名存在于当前环境中。               |
| 公开 {\<驱动器 > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)