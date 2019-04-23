---
title: revert
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5bc77b17317f602d642c7a9e025b67be10ad7256
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875108"
---
# <a name="revert"></a>revert



还原恢复到指定的卷影副本卷。 这是仅支持 CLIENTACCESSIBLE 上下文中的卷影副本。 这些卷影副本是永久性的只能由系统提供程序。 如果使用不带参数，**还原**在命令提示符下显示的帮助。

## <a name="syntax"></a>语法

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ShadowCopyID>|指定要将卷还原到的卷影副本 ID。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)