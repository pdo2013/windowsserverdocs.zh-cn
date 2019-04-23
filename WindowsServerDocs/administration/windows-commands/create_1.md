---
title: 创建
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70a8f53d9cb90fc36a76b11de2f93da71874617a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881268"
---
# <a name="create"></a>创建



启动卷影副本创建过程，使用当前的上下文和选项设置。 需要在卷影副本集中的至少一个卷。

## <a name="syntax"></a>语法

```
create
```

## <a name="remarks"></a>备注

-   必须添加至少一个卷**添加卷**命令之前可以使用**创建**命令。
-   可以使用**开始备份**命令指定一个完整备份，而不是复制备份。
-   运行后**创建**命令时，可以使用**exec**命令以从卷影副本运行备份的重复脚本。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)