---
title: 设置上下文
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f24e795f2d7c92d462cf822e70e4830b53827e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845848"
---
# <a name="set-contex"></a>设置上下文



设置卷影副本创建的上下文。 如果使用不带参数，**设置上下文**在命令提示符下显示的帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|clientaccessible|指定可供客户端版本的 Windows 卷影副本。|
|持久性|指定在程序退出时，重置或重新启动之间仍然存在卷影副本。|
|易失性|删除卷影复制退出，或重置。|
|nowriters|指定排除所有编写器。|

## <a name="remarks"></a>备注

-   *Clientaccessible*上下文是持久默认情况下。

## <a name="BKMK_examples"></a>示例

若要防止被删除时退出 DiskShadow 卷影副本，请键入：
```
set context persistent
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)