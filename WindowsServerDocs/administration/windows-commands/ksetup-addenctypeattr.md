---
title: ksetup:addenctypeattr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32cc87d7-b9e1-4d14-9eb7-3b439c55aa3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 711da6a81269fc838ca091765ddbcac63c3fe6e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886388"
---
# <a name="ksetupaddenctypeattr"></a>ksetup:addenctypeattr



将加密类型属性添加到域的可能类型的列表。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addenctypeattr <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DomainName>|你想要建立的连接的域的名称。 使用完全限定的域名或名称，例如 corp.contoso.com 或 contoso 的简单窗体。|
|加密类型|必须是以下受支持的加密类型之一：</br>-   DES-CBC-CRC</br>-   DES-CBC-MD5</br>-   RC4-HMAC-MD5</br>-   AES128-CTS-HMAC-SHA1-96</br>-   AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>备注

若要查看的 Kerberos 票证授予票证 (TGT) 和会话密钥的加密类型，请运行**klist**命令并查看输出。

可以设置或用空格分隔命令中的加密类型来添加多个加密类型。 但是，您才可做到一个域一次。

如果该命令成功或失败，将显示一条状态消息。

若要设置你想要连接到并使用的域，请运行**ksetup /domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>示例

确定此计算机设置的当前加密类型：
```
klist
```
设置为 corp.contoso.com 的域：
```
ksetup /domain corp.contoso.com
```
将加密类型 AES-256-CTS-HMAC-SHA1-96 添加到域 corp.contoso.com 的可能类型的列表：
```
ksetup /addenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
将加密类型属性设置为 AES-256-CTS-HMAC-SHA1-96 为域 corp.contoso.com 中：
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
验证运行安装程序，为域设置的加密类型属性：
```
ksetup /getenctypeattr corp.contoso.com
```

#### <a name="additional-references"></a>其他参考

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [命令行语法解答](command-line-syntax-key.md)