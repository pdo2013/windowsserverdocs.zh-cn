---
title: Set 选项
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4756627d19d296d02fa11ac67ef80080ddf318
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441365"
---
# <a name="set-option"></a>Set 选项



设置创建卷影副本的选项。 如果使用不带参数，**设置选项**在命令提示符下显示的帮助。

## <a name="syntax"></a>语法

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Parameters

|     参数     |                                                                                                  描述                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [差异   |                                                                                                     plex]                                                                                                     |
|  [transportable]  |                       指定的卷影副本并不是尚未导入。 更高版本可以使用元数据的.cab 文件导入到相同或不同的计算机的卷影副本。                       |
| [rollbackrecover] |                     指示编写器使用*自动恢复*期间**PostSnapshot**事件。 这是卷影副本将用于回滚 （例如，使用数据挖掘） 的情况下很有用。                      |
|   [txfrecover]    |                                                               请求 VSS 在创建期间进行卷影副本的事务上一致。                                                                |
|  [noautorecover]  | 停止编写器和文件系统中执行事务一致的状态将卷影副本的任何恢复更改。 **Noautorecover**不能用于**txfrecover**或**rollbackrecover**。 |

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)