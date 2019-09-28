---
title: ksetup：域
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a4d9f09def32c7518046c25887f4154020c5d7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375125"
---
# <a name="ksetupdomain"></a>ksetup：域



设置所有 Kerberos 操作的域名。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DomainName >|要与之建立连接的域的名称。 使用完全限定的域名或名称的简单格式，如 contoso.com 或 contoso。|

## <a name="remarks"></a>备注

无。

## <a name="BKMK_Examples"></a>示例

使用/mapuser 子命令建立与有效域（如 Microsoft）的连接：
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
当连接成功时，你将收到一个新的 TGT，或者将刷新现有 TGT。

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法项](command-line-syntax-key.md)