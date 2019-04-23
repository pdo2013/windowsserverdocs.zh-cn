---
title: ksetup:delenctypeattr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c2cc96e8156cafd3846422596abe62513e275b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838128"
---
# <a name="ksetupdelenctypeattr"></a>ksetup:delenctypeattr



删除域的加密类型属性。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /delenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DomainName>|你想要建立的连接的域的名称。 使用完全限定的域名或名称，例如 corp.contoso.com 或 contoso 的简单窗体。|

## <a name="remarks"></a>备注

若要查看的 Kerberos 票证授予票证 (TGT) 和会话密钥的加密类型，请运行**klist**命令并查看输出。

成功或失败完成后会显示状态消息。

若要设置你想要连接到并使用的域，请运行**ksetup /domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>示例

确定此计算机设置的当前加密类型：
```
klist
```
将域设置为 mit.contoso.com:
```
ksetup /domain mit.contoso.com
```
验证域的加密类型属性：
```
ksetup /getenctypeattr mit.contoso.com
```
删除域 mit.contoso.com 设置加密类型属性：
```
ksetup /delenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>其他参考

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [命令行语法解答](command-line-syntax-key.md)