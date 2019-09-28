---
title: 回到
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3243f13a4997824d9fff7c874ce26d56325fefa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371449"
---
# <a name="revert"></a>回到



将卷恢复到指定的卷影副本。 仅在 CLIENTACCESSIBLE 上下文中对卷影副本支持此项。 这些卷影副本是持久的，只能由系统提供程序进行。 如果不使用参数，则**revert**会在命令提示符下显示帮助。

## <a name="syntax"></a>语法

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ShadowCopyID >|指定卷的卷影副本 ID。|

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)