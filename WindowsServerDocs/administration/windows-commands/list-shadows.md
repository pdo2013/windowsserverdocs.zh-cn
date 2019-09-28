---
title: 列表阴影
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2640c04aef34cd6433efe529ac08c0294ba1c3b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374757"
---
# <a name="list-shadows"></a>列表阴影



列出系统上持久的和现有的非持久卷影副本。

## <a name="syntax"></a>语法

```
list shadows {all | set <SetID> | id <ShadowID>}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|全部|列出所有卷影副本。|
|设置 \<SetID >|列出属于指定卷影副本集 ID 的卷影副本。|
|id \<ShadowID >|列出具有指定卷影副本 ID 的所有卷影副本。|

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)