---
title: ksetup:domain
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1f53e807891b434709b1a8faed7aae8e8d444f6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857448"
---
# <a name="ksetupdomain"></a>ksetup:domain



设置 Kerberos 的所有操作的域名。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DomainName>|你想要建立的连接的域的名称。 使用完全限定的域名或名称，例如 contoso.com 或 contoso 的简单窗体。|

## <a name="remarks"></a>备注

无。

## <a name="BKMK_Examples"></a>示例

建立与有效的域，如 Microsoft 使用 /mapuser 子命令：
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
连接成功后，你将收到一个新的 TGT 或现有的 TGT，则会刷新。

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)