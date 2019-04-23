---
title: ksetup:getenctypeattr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 302f94616f98eb350332b08ad37a58305a0a0be1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841988"
---
# <a name="ksetupgetenctypeattr"></a>ksetup:getenctypeattr



检索域的加密类型属性。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /getenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DomainName>|你想要建立的连接的域的名称。 使用完全限定的域名或名称，例如 corp.contoso.com 或 contoso 的简单窗体。|

## <a name="remarks"></a>备注

若要查看的 Kerberos 票证授予票证 (TGT) 和会话密钥的加密类型，请运行**klist**命令并查看输出。

如果命令成功或失败，则成功或失败完成时将显示一条状态消息。

若要设置你想要连接到并使用的域，请运行**ksetup /domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>示例

验证域的加密类型属性：
```
ksetup /getenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>其他参考

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [命令行语法解答](command-line-syntax-key.md)