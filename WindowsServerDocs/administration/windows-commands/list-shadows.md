---
title: 列表阴影
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 50e4c4b8c7ea97ec65cecb6b8e904abd8c6d98eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848768"
---
# <a name="list-shadows"></a>列表阴影



列出系统上的持久性和现有的非永久性卷影副本。

## <a name="syntax"></a>语法

```
list shadows {all | set <SetID> | id <ShadowID>}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|全部|列出所有卷影副本。|
|set \<SetID>|列出了卷影副本属于指定的卷影副本设置的 id。|
|id \<ShadowID >|列出与指定的卷影副本 id。 任何卷影副本|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)