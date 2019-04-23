---
title: ksetup:dumpstate
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e5e8f20188fc27cc08dfd37c5fdbd811925f476
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863118"
---
# <a name="ksetupdumpstate"></a>ksetup:dumpstate



显示计算机定义的所有领域的领域设置的当前状态。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /dumpstate
```

### <a name="parameters"></a>Parameters

无

## <a name="remarks"></a>备注

此命令输出中包括的默认领域 （在计算机所在的域） 和此计算机定义的所有领域。 下面是为每个领域包括：
-   所有密钥分发中心 (Kdc) 与此领域
-   所有**集领域**此领域的标志
-   KDC 密码

此命令不显示通过 DNS 检测或该命令指定的域名**ksetup /domain**。

此命令不会显示使用该命令设置的计算机密码**ksetup /setcomputerpassword**。

**Ksetup**生成相同的输出**ksetup /dumpstate**。

## <a name="BKMK_Examples"></a>示例

查找计算机上的 Kerberos 领域配置大多数：
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)