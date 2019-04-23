---
title: secedit： 验证
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cca64f6b2904ed11f6b45e316c8e4da0093c373e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877908"
---
# <a name="seceditvalidate"></a>secedit： 验证



验证存储在安全模板 （.inf 文件） 中的安全设置。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|配置文件名称|必需。</br>指定要验证的安全模板的路径和文件。|

## <a name="remarks"></a>备注

验证安全模板可帮助您如果其中一个已损坏或不恰当地设置。

无效的安全模板将不会应用。

将不更新日志文件。

在 Windows Server 2008`Secedit /refreshpolicy`已替换为`gpupdate`。 有关如何刷新的安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>示例

上一种安全模板执行回滚后，你想要验证的回滚 inf 文件、 secRBKcontoso.inf，有效。
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>其他参考

-   [secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [命令行语法解答](command-line-syntax-key.md)